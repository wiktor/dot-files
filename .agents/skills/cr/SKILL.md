---
name: cr
description: "Perform a comprehensive, high-quality code review of a Piper Changelist (CL) and draft comments in Critique. Usage: /cr [cl_number]"
metadata:
  icon: 🔍
---

<!-- disableFinding(LINE_OVER_80) -->

# Comprehensive CL Review (/cr)

Perform a detailed, high-quality code review on a specific Google3 Piper Changelist (CL) and draft Critique comments using parallel specialized subagents.

## General Review Guidelines

These guidelines apply to the overall consolidation, prompt creation, and review plan creation:

- **Tone & Directness by Comment Type**: Phrasing of proposed comments must match the nature of the issue:
  - **Bugs, Logic Errors, Security Flaws, Missing Tests, and Build Blockers (Obvious Wins)**: Be **direct, clear, and assertive**. State the issue and the concrete fix directly (e.g., *"This reflection loop terminates prematurely when ... Please return true to continue scanning."*, *"The argument normalization is currently untested. Please add a unit test in ... to assert ..."*). Do not phrase bugs or broken constraints as tentative questions (avoid "Should we...?" or "Could we...?").
  - **Architecture, Refactoring, Alternative Designs, and Non-Critical Optimizations**: Be **suggestive, polite, and open-ended**. Phrase these as constructive ideas or questions to invite discussion (e.g., *"Could we use Go protobuf reflection here to reduce boilerplate?"*, *"Consider extracting this server component into a separate file to keep ... clean. What do you think?"*).
- **Deployment Context**: Analyze the CL description and codebase context to determine if the changes target a production service (Borg, Cloud) or local development tools, CLI utilities, or desktop harnesses. If it is a local tool, suppress generic production-only warnings (like DoS limits, strict socket timeouts that would kill long-running LLM calls) unless there is a genuine local security/correctness risk.
- **Nit Suppression**: Prioritize logic correctness, performance, safety, and testing. Suppress minor cosmetic style nits (like redundant `pass` statements, trailing spaces, minor naming preferences) that automated formatters can handle. However, **high-value readability refactoring** (e.g., refactoring methods that are becoming too large/complex, extracting helpers, or applying guard clauses/early returns to flatten control flow) is highly encouraged and should not be suppressed as a nit.
- **Thorough & Deep-Dive Explanations**: Avoid generic or shallow findings (e.g., "This could be cleaner"). For both the walkthrough and proposed comments, provide deep technical details of *why* the issue occurs, what happens under the hood (e.g., step-by-step logic tracing), and how the remediation fixes it. All comments and explanations must be self-contained and clear, without forcing the reviewer to guess the motivation.
- **Review Scope (Strictly Diff-bound)**: You **MUST ONLY** propose comments on lines that are explicitly added or modified in the `{CL_DIFF}`. Do not review or propose comments on unchanged lines, even if they are in the same files. If you read the full file from the Critique snapshot path for context, use it only to understand the modified lines, and do not flag issues in unchanged surrounding code or in code that belongs to parent CLs in the stack. All proposed comments must target lines that are in the diff.

## Phase 1: Context Gathering

1. **Extract CL Number**:
   - Extract the CL number from the user's message. It is usually a 9-digit number. If not present, ask the user to provide it.

2. **Retrieve Metadata & Diff**:
   - Define the Critique CLI command: `CRITIQUE=/google/bin/releases/gemini-agents-critique/critique`
   - Retrieve CL information (metadata, description, etc.):
     
     ```bash
     $CRITIQUE readonly info --cl=<cl_number>
     ```
   - Retrieve the full diff:
     
     ```bash
     $CRITIQUE readonly diff --cl=<cl_number>
     ```
     *(Note on Stacked CLs & Diffbase: For stacked CLs, this command returns the diff relative to the CL's diffbase. Any files listed under "Protected changes omitted from diff" or files without diff blocks have zero changes in this specific CL relative to its parent. `{CL_DIFF}` is the strict source of truth for what changed).*
   - Retrieve existing Tricorder findings:
     
     ```bash
     $CRITIQUE readonly findings --cl=<cl_number> --all-lines --include-non-actionable
     ```
   - Retrieve existing comments (both published and drafts):
     
     ```bash
     $CRITIQUE readonly list-comments --cl=<cl_number> --include-drafts
     ```

3. **Read Description & Existing Comments**:
   - Read the CL description from the `info` command output. Understand the change's goal, the linked bug, and any relevant background context. Save the CL description as `{CL_DESC}` and the CL diff as `{CL_DIFF}`.
   - When gathering context, if there is a need to save temporary artifacts, utilize the scratch directory (`{ARTIFACT_DIR}/scratch/`). This allows subagents to easily access this gathered context if it is needed.
   - Read the existing comments from the `list-comments` output. Save this context as `{CL_COMMENTS}`.

## Phase 2: Define and Invoke Subagents

1. **Define Subagents**:
    -   Read the following prompt files from
        `google3/learning/agents/jetski/skills/cr/prompts/` (or the `prompts/`
        sibling directory inside the `cr/` skill folder if resolving relative
        paths) and define the subagents using `define_subagent` with the read
        contents as their system prompt. Ensure you use absolute paths when
        reading these files
        (`google3/learning/agents/jetski/skills/cr/prompts/<filename>`) so
        subagent definitions resolve reliably from any current working
        directory:
        - `architecture-reviewer`: use `google3/learning/agents/jetski/skills/cr/prompts/architecture_reviewer.md`
        -   `design-reviewer`: use
            `google3/learning/agents/jetski/skills/cr/prompts/design_reviewer.md`
        - `security-reviewer`: use `google3/learning/agents/jetski/skills/cr/prompts/security_reviewer.md`
        - `logic-reviewer`: use `google3/learning/agents/jetski/skills/cr/prompts/logic_reviewer.md`
        - `test-reviewer`: use `google3/learning/agents/jetski/skills/cr/prompts/test_reviewer.md`
        - `walkthrough-generator`: use `google3/learning/agents/jetski/skills/cr/prompts/walkthrough_generator.md`

2. **Invoke Subagents in Parallel**:
    - Invoke the defined subagents in parallel using `invoke_subagent`.
    - For each reviewer subagent, replace the `{CL_DESC}` and `{CL_DIFF}` placeholders in the prompt with the actual CL description and CL diff retrieved in Phase 1.
    - For `walkthrough-generator`, replace `{CL_DESC}` and `{CL_DIFF}` with the actual CL description and CL diff, and replace `{OUTPUT_PATH}` with `{ARTIFACT_DIR}/cl_walkthrough.md` (where `{ARTIFACT_DIR}` is the current conversation's artifact directory, e.g., `/usr/local/google/home/$USER/.gemini/jetski/brain/<conversation-id>`).
    -   Launch all 6 subagents concurrently.

3. **Wait and Gather Responses**:
    -   Wait for all 6 subagents to complete their execution and return
        responses.

## Phase 3: Consolidate, Filter, and Deduplicate Findings

1. **Extract Findings**:
    - From each subagent's response, extract the fenced `json-findings` code block.
    - Parse the JSON array. Each finding must contain: `file`, `line`, `severity`, `confidence`, `finding`, `remediation`.
    -   Combine all findings from the 5 reviewer subagents into a single unified
        list.

2. **Confidence Filtering**:
   - Filter out any findings that have a `confidence` score below 75.
   - *Exception*: If a finding has a severity of `Critical` or `High`, keep it if its `confidence` is at least 50.

3. **Deduplicate & Scope Filter Findings**:
   - **Filter Out Unchanged Lines**: Check `{CL_DIFF}` and discard any finding that targets a file or line number not explicitly added or modified in the diff blocks of `{CL_DIFF}` (e.g., discard findings on files listed under "Protected changes omitted from diff" or lines unchanged from the diffbase).
   - Remove any findings that duplicate issues already flagged in the Tricorder findings retrieved in Phase 1.
   - Remove any findings that target a file and line range where a comment (published or draft) has already been left by a reviewer in `{CL_COMMENTS}`.
   - Group findings by file.
   - Within each file, sort the findings by line number.
   - Cluster findings that are within 3 lines of each other (proximity deduplication):
     - For each cluster, keep only the highest-severity finding.
     - If severities are equal, keep the finding with the higher `confidence` score.
     - If both severity and confidence are equal, keep the first one but merge any unique remediation/finding details into it.

## Phase 4: Construct the Review Plan

Before presenting the proposed comments, you **MUST** present the CL walkthrough.

1. **Write Review Plan to Artifact File**:
   - Format the plan using a **List format with code blocks** (do NOT use a markdown table, as it makes code blocks unreadable).
   - Write the formatted plan to `{ARTIFACT_DIR}/review_plan.md` using `write_to_file`.
   - Set the `ArtifactMetadata` to:
     
     ```json
     {
       "RequestFeedback": true,
       "Summary": "Proposed Critique comments for CL <cl_number> review plan.",
       "UserFacing": true
     }
     ```
   
   The Review Plan structure in `review_plan.md` must be:
   
   ```markdown
   # Review Plan for CL [CL_NUMBER]
   
   Detailed list of proposed Critique comments:
   
   ### Comment [ID]: [Short Title]
   - **File**: `//depot/google3/path/to/file.py`
   - **Line(s)**: [Line number(s)]
   - **Code Context**:
     ```[language]
     [code snippet showing the target code]
     ```
   - **Proposed Comment**: [Exact text to post, phrased constructively as a question/suggestion]
   - **Rationale & Impact (Deep Dive)**: [A detailed, self-contained explanation of the issue, what happens under the hood, and why it needs fixing]
   ```

2. **Present the Plan and Walkthrough in Chat**:
   Once both `cl_walkthrough.md` and `review_plan.md` are written, output a message in the chat pointing the user to them:
   - "I have generated the CL Walkthrough and the Review Plan."
   - Link to walkthrough: `[CL Walkthrough](file://{ARTIFACT_DIR}/cl_walkthrough.md)`
   - Link to review plan: `[Review Plan](file://{ARTIFACT_DIR}/review_plan.md)`
   - Output this exact verbatim prompt to request review:
     > Please review the proposed Critique comments in the [Review Plan](file://{ARTIFACT_DIR}/review_plan.md) and click the **Proceed** button or select comments you want me to post.

3. **Await Approval**:
   Stop executing and wait for the user to approve the plan or provide feedback using the artifact's feedback button. **DO NOT** make any write calls to Critique until the user has approved the plan.

## Phase 5: Posting Draft Comments (Only after approval)

Once the user has approved the comments:

1. **Identify Approved Comments**: Filter the list based on the user's selection.
2. **Post as Drafts**:
   - For each approved comment, write the message to a temporary file in `/tmp/` (`/tmp/scratch_review_comment_${cl}_${id}.txt` or `$TMPDIR/scratch_review_comment_${cl}_${id}.txt`) to prevent shell escaping issues. Because `$CRITIQUE mutate post-comment` runs in a shell subprocess, `/tmp/` guarantees exact, reliable bash file expansion for every user across all workspace types.
   - Run the Critique `post-comment` command:
     
     ```bash
     $CRITIQUE mutate post-comment \
       --cl=<cl_number> \
       --file=<file_path> \
       --start-line=<start_line> \
       --end-line=<end_line> \
       --message="$(cat /tmp/scratch_review_comment_${cl}_${id}.txt)"
     ```
     *(Omit `--end-line` if it is a single-line comment)*.
   - **CRITICAL**: Do **NOT** use the `--is-human=true` flag.
   - **CRITICAL**: Do **NOT** run `$CRITIQUE mutate publish-comments`. The user wants to manually review and publish draft comments themselves.
3. **Report Status**:
   - Confirm to the user that the comments have been successfully posted as drafts in Critique. Provide a link to the Critique CL review interface: `https://critique.corp.google.com/cl/<cl_number>`.
