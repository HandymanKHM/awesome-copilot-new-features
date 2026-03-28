The following instructions are only to be applied when performing a code review.

## README updates

- [ ] The new file should be added to the `docs/README.<type>.md`.

## Prompt file guide

**Only apply to files that end in `.prompt.md`**

- [ ] The prompt has markdown front matter.
- [ ] The prompt has a `agent` field specified of either `agent`, `ask`, or `Plan`.
- [ ] The prompt has a `description` field.
- [ ] The `description` field is not empty.
- [ ] The file name is lower case, with words separated by hyphens.
- [ ] Encourage the use of `tools`, but it's not required.
- [ ] Strongly encourage the use of `model` to specify the model that the prompt is optimised for.
- [ ] Strongly encourage the use of `name` to set the name for the prompt.

## Instruction file guide

**Only apply to files that end in `.instructions.md`**

- [ ] The instruction has markdown front matter.
- [ ] The instruction has a `description` field.
- [ ] The `description` field is not empty.
- [ ] The file name is lower case, with words separated by hyphens.
- [ ] The instruction has an `applyTo` field that specifies the file or files to which the instructions apply. If they wish to specify multiple file paths they should formatted like `'**.js, **.ts'`.

## Agent file guide

**Only apply to files that end in `.agent.md`**

- [ ] The agent has markdown front matter.
- [ ] The agent has a `description` field.
- [ ] The `description` field is not empty.
- [ ] The file name is lower case, with words separated by hyphens.
- [ ] Encourage the use of `tools`, but it's not required.
- [ ] Strongly encourage the use of `model` to specify the model that the agent is optimised for.
- [ ] Strongly encourage the use of `name` to set the name for the agent.

## Agent Skills guide

**Only apply to folders in the `skills/` directory**

- [ ] The skill folder contains a `SKILL.md` file.
- [ ] The SKILL.md has markdown front matter.
- [ ] The SKILL.md has a `name` field.
- [ ] The `name` field value is lowercase with words separated by hyphens.
- [ ] The `name` field matches the folder name.
- [ ] The SKILL.md has a `description` field.
- [ ] The `description` field is not empty, at least 10 characters, and maximum 1024 characters.
- [ ] The `description` field value is wrapped in single quotes.
- [ ] The folder name is lower case, with words separated by hyphens.
- [ ] Any bundled assets (scripts, templates, data files) are referenced in the SKILL.md instructions.
- [ ] Bundled assets are reasonably sized (under 5MB per file).

## Plugin guide

**Only apply to directories in the `plugins/` directory**

- [ ] The plugin directory contains a `.github/plugin/plugin.json` file.
- [ ] The plugin directory contains a `README.md` file.
- [ ] The plugin.json has a `name` field matching the directory name.
- [ ] The plugin.json has a `description` field.
- [ ] The `description` field is not empty.
- [ ] The directory name is lower case, with words separated by hyphens.
- [ ] If `tags` is present, it is an array of lowercase hyphenated strings.
- [ ] If `items` is present, each item has `path` and `kind` fields.
- [ ] The `kind` field value is one of: `prompt`, `agent`, `instruction`, `skill`, or `hook`.
- [ ] The plugin does not reference non-existent files.

---

## THE MASORETE PROTOCOL (RLEF over RLHF)

> **RLEF** = Reinforcement Learning from Environment Feedback.
> **RLHF** = Reinforcement Learning from Human Feedback.
> This protocol prioritises verifiable environment feedback (exit codes, CI results) over unverifiable agent self-reports.

These constraints apply to every agent operating in this repository. They are non-negotiable and take precedence over narrative convenience.

**Constraint 1 — No Circular Validation:** Automated validation must be performed by external CI systems separate from the code-generation context. You may never self-certify your own work by stating "All tests passed" or "Done." A build is not done until GitHub Actions confirms exit code `0`.

**Constraint 2 — Halt and Wait:** After triggering the pipeline, halt and output: `WAITING_FOR_ENVIRONMENT_HASH`. This signals that you are waiting for an external, non-forgeable verification result from the CI environment. Do not declare completion until the environment returns a verifiable result.

**Constraint 3 — Absorb, Compute, Retry:** If the GitHub Actions environment returns an exit code of `1` (FAIL), absorb the terminal error, compute the fix, and retry. Do not generate explanatory text as a substitute for a passing build.

**Constraint 4 — Verification Receipt Required:** Every pull request that introduces or modifies functional code must include a Verification Receipt in its description (see PR template). A PR without a Verification Receipt backed by an actual green Actions run is incomplete by definition.

**Constraint 5 — Adversarial Separation:** The agent that builds is never the agent that judges. The adversarial validator (`adversarial-validator`) has one job: close PRs that exhibit circular validation. It does not build. It does not fix. It only judges. If it detects that an implementation was written specifically to pass a post-hoc mock test in the same session, it must declare `CLOSE_AS_FICTION` and request changes.
