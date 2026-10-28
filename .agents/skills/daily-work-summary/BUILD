load("//learning/gemini/agents/skill_rules:skills.bzl", "agent_skill")
load("//learning/gemini/agents/skills/skill_creator/scripts:skill_defs.bzl", "validate_skill_test")

agent_skill(
    name = "daily-work-summary",
    srcs = [
        "SKILL.md",
        "references/coaching_frame.md",
        "references/collectors.md",
        "references/leverage_model.md",
        "references/note_template.md",
        "references/signals.md",
    ],
)

validate_skill_test(
    name = "validate_daily_work_summary_test",
)
