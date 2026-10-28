load("//learning/gemini/agents/skill_rules:skills.bzl", "agent_skill")
load("//learning/gemini/agents/skills/skill_creator/scripts:skill_defs.bzl", "validate_skill_test")

package(default_visibility = [
    "//experimental/users/<user>:__subpackages__",
    "//learning/agents/jetski:__subpackages__",
    "//learning/gemini/agents/skills:__subpackages__",
])

agent_skill(
    name = "cr",
    srcs = [
        "SKILL.md",
        "prompts/architecture_reviewer.md",
        "prompts/design_reviewer.md",
        "prompts/logic_reviewer.md",
        "prompts/security_reviewer.md",
        "prompts/test_reviewer.md",
        "prompts/walkthrough_generator.md",
    ],
)

validate_skill_test(
    name = "validate_cr_skill_test",
)
