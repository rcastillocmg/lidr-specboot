# Specboot: Augmented Spec-driven development

![Specboot](https://drive.google.com/uc?export=view&id=1yic8hwSzSyQE6Zmf6__YNBcBvqiHMGEr)

Boot OpenSpec's Spec-Driven in any project and give superpowers to any coding agent.

This repository contains a comprehensive set of development rules, standards, and AI agent configurations designed to work seamlessly with multiple AI coding copilots. The setup is portable and can be imported into any project to provide consistent, high-quality AI-assisted development.

It's highly recommended to be used along with Spec-Driven Development frameworks like [OpenSpec](https://github.com/Fission-AI/OpenSpec)

## 🆕 What's New on This Branch (`tighten-decision-workflow`)

Plain-language summary of what this branch changed, for review before merging:

1. **This repo now has its own OpenSpec setup.** Previously, this toolkit told *other* projects how to set up OpenSpec, but never set it up on itself. It now does — that's what made everything below possible to build using the toolkit's own formal change-tracking process.
2. **No more vague language allowed in specs or task lists.** Words like "if applicable," "if needed," "maybe," and "preferred" are now banned in AI-generated plans. Every gap either becomes a clear decision or an explicitly flagged question — never a silent guess.
3. **A new "map of the project" file, kept always up to date.** `docs/architecture.md` is a new living document describing where everything lives. A new skill (`update-architecture-doc`) automatically keeps it current every time something changes, so future AI sessions don't have to re-read the entire codebase to get oriented.
4. **The "clarify a vague idea" step (`enrich-us`) got stricter.** It now reads the project's architecture first, checks against the real code, and — this is the important part — will stop and ask you a direct question instead of guessing when something is a genuine blocker (like not knowing which system a feature belongs to), rather than quietly producing a "finished-looking" answer that's actually incomplete.
5. **The independent code review step (`adversarial-review`) no longer just reports problems and stops.** If it finds something serious, it now automatically loops back to get it fixed and re-checks before signing off — and it will no longer take someone's word that something was fixed without seeing the actual updated work.
6. **Pull request write-ups (`commit`) are now more complete by default** — always including what changed by area, what was tested, the review outcome, and any follow-ups, instead of a short note that might skip details.
7. **Real bugs were caught and fixed during this process, not just found and left.** While double-checking this branch's own work, two real inconsistencies were found and fixed before anything was finalized — see `openspec/changes/archive/2026-07-30-tighten-decision-workflow/reports/2026-07-30-step-13-adversarial-review-tighten-decision-workflow.md` for exactly what those were.

Every step above was tested with real before/after comparisons (not just written and assumed to work) — full detail is saved under `openspec/changes/archive/2026-07-30-tighten-decision-workflow/`.

## 📝 Open Proposals (Planned, Not Yet Built)

These two OpenSpec changes have a complete proposal/design/spec/tasks plan under `openspec/changes/`, but nothing has been implemented yet — no skill file, symlink, or doc edit from either one exists in the repo yet:

1. **[`add-ai-governance-copilot-default-skill`](openspec/changes/add-ai-governance-copilot-default-skill/proposal.md)** — embeds CMG's AI Governance Co-Pilot skill into this project by default (a project-local copy, symlinked into `.claude/skills` and `.cursor/skills`, declared a standing default in `docs/base-standards.md`), so governance coverage doesn't depend on any one person's personal global Claude settings.
2. **[`require-addition-consistency-check`](openspec/changes/require-addition-consistency-check/proposal.md)** — adds a standing rule requiring every future addition to this project (a skill, agent, capability, doc section, or symlink) to be checked against what already exists before it's finalized, plus a new mandatory step in `docs/openspec-tasks-mandatory-steps.md` enforcing that check on every future change.

## 📁 Repository Structure

```
.
├── docs/                        # Development standards and specifications
│   ├── base-standards.md        # Core development rules (single source of truth)
│   ├── backend-standards.md
│   ├── frontend-standards.md
│   ├── documentation-standards.md
│   ├── api-spec.yml             # OpenAPI specification
│   ├── data-model.md            # Database and domain models
│   ├── development_guide.md
│   ├── architecture.md          # Living system map, updated with every change
├── ai-specs/
│   ├── agents/                  # Agent role definitions (backend, frontend, analyst, etc.)
│   └── skills/                  # Reusable skill prompts/workflows
│
├── AGENTS.md                    # Generic agent configuration
├── CLAUDE.md                    # Claude-specific configuration
├── codex.md                     # GitHub Copilot/Codex configuration
└── GEMINI.md                    # Gemini-specific configuration
```

## 🤖 Multi-Copilot Support

This repository uses **symbolic links** or **naming conventions** to support multiple AI coding copilots without duplication:

- **`AGENTS.md`** → Generic agent rules (works with most copilots)
- **`CLAUDE.md`** → Optimized for Claude/Cursor
- **`codex.md`** → Optimized for GitHub Copilot/Codex
- **`GEMINI.md`** → Optimized for Google Gemini

All these files reference the same core rules in `docs/base-standards.md`, ensuring consistency across different AI tools while allowing copilot-specific customizations.

### Why This Approach?

✅ **Single Source of Truth**: Core rules maintained in one place (`base-standards.md`)  
✅ **Copilot Compatibility**: Each AI tool finds its configuration using its preferred naming convention  
✅ **Zero Configuration**: Import into a new project and it works immediately  
✅ **Easy Updates**: Update rules once, all copilots benefit  
✅ **Portable**: Copy this structure to any project  

## 🚀 Quick Start

### 1) Install and Initialize OpenSpec

OpenSpec works great with this repository and is recommended for a spec-driven workflow.

Quick Start requirements from OpenSpec official docs:

- Node.js `20.19.0` or higher

Install OpenSpec globally (this repo's own OpenSpec setup was last verified against `1.7.0`):

```bash
npm install -g @fission-ai/openspec@latest
```

Then navigate to your project and initialize:

```bash
cd your-project
openspec init
```

### 2) Import Into Your Project

Copy this repository into your project first, so the `docs/` and `ai-specs/` paths already exist when you configure OpenSpec:

```bash
# Clone or copy this repository into your project (`-n`: do not overwrite existing files so you keep project's original README)
cp -rn lidr-specboot/* your-project/
```

Alternative for step 2 (Claude Code users):

- You can alternatively install the Claude plugin and use it as the coding agent for this import step.
- This only changes **how** you install Specboot. It does **not** install OpenSpec, does **not** update OpenSpec config, and does **not** customize `docs/`.

Quick install:

```bash
npx @lidr/lidr-specboot
```

This copies all files into your project and recreates the symlink structure automatically. Safe to re-run: existing files are never overwritten.


### 3) Customize `docs/` for Your Project (Mandatory)

This step is required. If you skip it, your AI assistant will use generic technical context instead of your real project context.

Update the files in `docs/` to match your stack, architecture patterns, domain language, API contracts, and data model.

For detailed guidance and ready-to-use prompt examples, see [Customization](#-customization).

### 4) Point OpenSpec Config to Your `docs/` and `ai-specs/`

After `openspec init` and after copying this repository, update your project's `openspec/config.yaml` (OpenSpec also accepts `config.yml`) to include your technical context from `docs`.

Prompt example to automate this with your copilot:

```text
Update my openspec config.yml context to reference this repository's docs and ai-specs structure.

Requirements:
- Use docs/base-standards.md as the single source of truth.
- Include docs/backend-standards.md, docs/frontend-standards.md, docs/documentation-standards.md.
- Include docs/api-spec.yml and docs/data-model.md.
- Tell the agent to adopt ai-specs/agents/backend-developer.md for backend work and ai-specs/agents/frontend-developer.md for frontend work.
- Mention ai-specs/skills as workflow guidance.
- Keep all paths relative to the project root.
```

Example (`openspec/config.yaml`):

```yaml
context: |
  Tech stack: TypeScript, Node.js, Express, Prisma, Domain-Driven Design (DDD)
  Architecture: Clean Architecture with Domain, Application, and Presentation layers
  We use conventional commits
  Domain: LTI (Leadership. Technology. Impact) ATS platform
  All code, comments, documentation, and technical artifacts must be in English

  Project specs (single source of truth): All artifact creation and implementation MUST follow the project's technical context in ai-specs/. Read and apply these when creating or implementing:
  - docs/base-standards.md — core principles, TDD, language standards, links to backend/frontend/docs standards
  - docs/backend-standards.md — API, database, testing, security (backend changes)
  - docs/frontend-standards.md — React, UI/UX (frontend changes)
  - docs/api-spec.yml — API contracts and endpoint definitions
  - docs/data-model.md — domain and data model
  - docs/documentation-standards.md — docs structure and maintenance
  - docs/architecture.md — living system map, updated with every change
  For implementation: adopt the relevant agent from ai-specs/agents/ (e.g. backend-developer.md for backend, frontend-developer.md for frontend). Use ai-specs/skills/ for workflow guidance when applicable.

# Per-artifact rules (optional)
# Keys must match real OpenSpec artifact IDs (proposal, specs, design, tasks) --
# an invented key like `_global` won't match anything and only produces a CLI warning.
rules:
  proposal:
    - Before creating any artifact, read and apply docs/base-standards.md
  tasks:
    - Follow docs/openspec-tasks-mandatory-steps.md for structure, mandatory steps, and the verification checklist
    - For backend-related artifacts, read docs/backend-standards.md and adopt guidelines from ai-specs/agents/backend-developer.md
    - For frontend-related artifacts, read docs/frontend-standards.md and adopt guidelines from ai-specs/agents/frontend-developer.md
    - Use docs/api-spec.yml and docs/data-model.md for API and data consistency in specs and tasks
```

## ✅ Verify Configuration (Required)

Do this after completing the setup steps above.

Your AI copilot should automatically load:

- **Claude/Cursor**: `CLAUDE.md` → `docs/base-standards.md`
- **GitHub Copilot**: `codex.md` → `docs/base-standards.md`
- **Gemini**: `GEMINI.md` → `docs/base-standards.md`

All paths and rules are configured to work seamlessly without manual adjustments.

## 💡 Usage: Official OpenSpec Workflow

The recommended workflow in this repository uses official OpenSpec commands:

The recommended workflow in this repository uses official OpenSpec commands:

1. **`/enrich-us`** (optional): refine a vague user story or idea
2. **`/new`**: create a new OpenSpec change (currently a copy of `/ff`)
3. **`/ff`**: create all required OpenSpec artifacts (feature file, tasks, etc.)
   - Running `/new` followed by `/ff` is equivalent to the new `/propose` command
4. **`/apply`**: implement tasks one by one
5. **`/verify`**: validate implementation against the change artifacts
6. **`/adversarial-review`**: independent red-team code review before archiving
7. **`/archive`**: archive the completed change
8. **`/commit`**: create focused commit(s) and manages Pull Request after verification
Workflow reference image:

![OpenSpec custom workflow reference](https://drive.google.com/uc?export=view&id=1Bu8hysVBlpBZgH3SVgRh3knHS7W4X5ud)

### Optional: MCP Integrations (Jira + Playwright)

This workflow is enhanced with MCP servers that are mentioned in the workflow. These are optional and you can skip them entirely, or replace them with equivalent tools.

- **Jira MCP (recommended in `/enrich-us`)**: lets the agent read Jira tickets directly to enrich user stories without copy/paste.
- **Playwright MCP (recommended for E2E testing)**: lets the agent run browser-based E2E checks for user workflows.

Recommended installation approach:

- **Cursor**: enable/configure MCP servers in Cursor settings (add your Jira and Playwright MCP servers, then provide credentials such as Jira API tokens as required).
- **Other agents/IDEs**: follow your tool’s MCP installation docs and configure Jira/Playwright there.

If you don’t use Jira, or you don’t want automated E2E testing in this workflow, just update the skills and keep using the same OpenSpec command flow.

### Example: End-to-End Flow

Use these commands in sequence:

Optional first step (recommended): create a dedicated worktree before running the command flow, then clean it up when done. The `using-git-worktrees` skill can automate this.

```bash
/enrich-us SCRUM-10
/ff SCRUM-10
/apply SCRUM-10
/verify SCRUM-10
/adversarial-review SCRUM-10
/archive SCRUM-10
/commit
```

Artifacts are managed through OpenSpec directories during this flow, including testing reports created. 

### Useful Skills

Skills live in `ai-specs/skills/` and are mirrored into `.claude/skills/` and `.cursor/skills/` via relative symlinks, so any copilot can discover them. The agent loads a skill automatically when a request matches its description (per `AGENTS.md` §4). The most useful ones in day-to-day work are **`enrich-us`**, **`using-git-worktrees`**, **`writing-skills`**, **`code-auditing`**, and **`update-architecture-doc`**:

- **`enrich-us`** — Analyze and enhance a vague Jira user story (or raw idea) into an implementation-ready ticket with closed decisions and acceptance criteria, reading `docs/architecture.md` and the existing codebase first, and asking a direct question rather than finalizing when a gap is genuinely blocking. Use **before** planning to make sure the team and the AI agree on scope. Output: `Original` / `Scope` / `Closed Decisions` / `Expected Behavior`.
- **`using-git-worktrees`** — Set up an isolated workspace before starting feature work or executing a plan, with safe creation, baseline checks, copying of local Claude settings, and a complete cleanup workflow when the work is done.
- **`writing-skills`** — Author and verify new skills (or refactor existing ones) following TDD-style validation before deployment. Use when adding a skill to `ai-specs/skills/` or editing an existing `SKILL.md`.
- **`code-auditing`** — Run a systematic 6-phase code quality audit covering security, performance, type safety, dead code, and library best practices, ending with a prioritized action plan. Use for pre-release reviews, technical-debt sweeps, and dependency audits.
- **`update-architecture-doc`** — Keep `docs/architecture.md` (the project's living "shared memory" document, see `docs/base-standards.md` §11) current and consistently formatted after any capability, module, or cross-cutting decision changes. Invoked as a mandatory step of the OpenSpec task checklist.
- **`cmg-ai-governance-copilot`** — Unlike the skills above, this one is a **standing default** (see `docs/base-standards.md` §12), not something you invoke by request: it proactively watches for recurring AI/LLM-assisted building in this repo (a new skill, agent, or automation, not a one-off task) and prompts for the CMG AI Policy intake before the build is treated as finished. A project-local copy of CMG's company-wide governance skill, kept current manually rather than auto-synced.

Other active skills in this repository: `commit` (now requires per-layer, per-test-type, and evaluations detail in every PR description — see `docs/base-standards.md` §10-§11 for the closed-decision and architecture rules behind these changes), `adversarial-review` (now loops back into `/apply` on any Blocker/Major finding instead of stopping at a verdict), `explain`, `meta-prompt`, `update-docs`. See each `ai-specs/skills/<name>/SKILL.md` for the full instructions.

## 📖 Core Development Rules

All development follows principles defined in `docs/base-standards.md`:

### Key Principles

1. **Small Tasks, One at a Time**: Baby steps, never skip ahead
2. **Test-Driven Development (TDD)**: Write failing tests first
3. **Type Safety**: Fully typed code (TypeScript)
4. **Clear Naming**: Descriptive variables and functions
5. **English Only**: All code, comments, documentation, and messages in English
6. **90%+ Test Coverage**: Comprehensive testing across all layers
7. **Incremental Changes**: Focused, reviewable modifications

### Specific Standards

- **Backend Standards**: `docs/backend-standards.md`
  - API development patterns
  - Database best practices
  - Security guidelines
  - Testing requirements
- **Frontend Standards**: `docs/frontend-standards.md`
  - React component patterns
  - UI/UX guidelines
  - State management
  - Component testing
- **Documentation Standards**: `docs/documentation-standards.md`
  - Technical documentation structure
  - API documentation (OpenAPI)
  - Code documentation
  - Maintenance guidelines
- **OpenSpec Workflow (Standing Rule)**: `docs/base-standards.md` §8
  - When the agent must run the full OpenSpec workflow automatically vs. when a quick direct edit is fine
- **Git Workflow**: `docs/base-standards.md` §9
  - Session start/end checks, branch naming, commit style, and safety confirmations before destructive actions
- **Closed-Decision Language**: `docs/base-standards.md` §10
  - Banned vague qualifiers ("if applicable", "if needed", "maybe", etc.) in specs/tasks; every gap must become an explicit decision or a flagged Open Question
- **Architecture as Shared Memory**: `docs/base-standards.md` §11 and `docs/architecture.md`
  - The living system map every change must update before archiving, maintained via the `update-architecture-doc` skill
- **CMG AI Governance Co-Pilot (Standing Default Skill)**: `docs/base-standards.md` §12
  - `cmg-ai-governance-copilot` triggers proactively by default in this repo; see [Useful Skills](#useful-skills) above
- **Consistency Check Before Any Addition (Standing Rule)**: `docs/base-standards.md` §13
  - Every new skill, agent, capability, doc section, or symlink must be checked against what already exists before it's finalized; enforced as a mandatory `tasks.md` step in `docs/openspec-tasks-mandatory-steps.md`

## 🎯 Benefits

### For Developers

- ✅ **Consistent Code Quality**: AI follows the same standards every time
- ✅ **Comprehensive Testing**: Automatic 90%+ coverage across all layers
- ✅ **Complete Documentation**: API specs updated automatically
- ✅ **Faster Onboarding**: New team members reference the same rules
- ✅ **Reduced Review Time**: Code follows established patterns

### For Teams

- ✅ **Copilot Flexibility**: Team members can use their preferred AI tool
- ✅ **Knowledge Preservation**: Standards documented, not in people's heads
- ✅ **Quality Consistency**: Same standards regardless of who (or what) writes code
- ✅ **Easier Code Reviews**: Clear expectations and patterns
- ✅ **Scalable Practices**: Standards scale with the team

### For Projects

- ✅ **Maintainable Codebase**: Clean architecture and clear separation of concerns
- ✅ **Production-Ready Code**: TDD, error handling, and validation built-in
- ✅ **Living Documentation**: API specs and data models always current
- ✅ **Faster Feature Development**: Autonomous AI implementation from plans
- ✅ **Lower Technical Debt**: Best practices enforced from day one

## 🔧 Customization

### Adapting to Your Project

1. **Update technical context**: Find the different files in `docs` and modify core principles, coding standards, business rules and technical documentation to match your needs:
  - backend/frontend/testing/documentation standards
  - installation guide
  - data model
  - API docs
  - ...
2. **Adapt agents in `ai-specs/agents`**: Adjust agent definitions to your project's roles and workflows
3. **Extend skills in `ai-specs/skills`**: Define battle-tested prompts and workflows in reusable skills
4. **Link Resources**: Reference your project's specific documentation or tasks using MCPs
5. **Keep the symlink structure**: Remember to create relative symlinks from `.claude` and `.cursor` to the corresponding `ai-specs/agents` and `ai-specs/skills` entries to keep it consistent

### Prompt Example: Adapt Technical Context

Use this prompt with your copilot to adapt the `docs/` folder while preserving the same baseline structure:

```text
Following the same base structure already present in docs/, update all technical context documents according to this project's specifics.

Requirements:
- Keep the same document set and file names in docs/.
- Replace generic content with this project's real stack, architecture patterns, coding conventions, and domain terminology.
- Update backend, frontend, and documentation standards to reflect actual practices used by this team.
- Update docs/api-spec.yml and docs/data-model.md so they match the real endpoints and entities of this project.
- Ensure all references are internally consistent and aligned across docs/.
- Keep everything in English and make guidance implementation-ready for AI agents.
```

### Maintaining Standards

- **Single Source of Truth**: Always update `base-standards.md` first
- **Version Control**: Track changes to standards like code
- **Team Review**: Standards changes should be reviewed like pull requests
- **Documentation**: Keep examples current with actual implementation
- **Symlink Integrity**: After file renames/moves/suffix changes, verify and update all impacted symlinks
- **Canonical Placement**: Prefer `ai-specs` as canonical source and expose through symlinks for `.claude`/`.cursor` compatibility

## 📚 Technical context

### Reference Examples (from LIDR Project)

The following files are included as **reference examples** from the LIDR project. You should create your own versions tailored to your specific project:

- **API Specification**: `docs/api-spec.yml` (OpenAPI 3.0 format)
  - *Create your own API spec documenting your project's endpoints*
- **Data Models**: `docs/data-model.md` (Database schemas, domain models)
  - *Document your database structure and domain entities*
- **Development Guide**: `docs/development_guide.md` (Setup, workflows)
  - *Write setup instructions specific to your tech stack*

## 🤝 Contributing

When contributing to the standards:

1. Update `base-standards.md` (single source of truth)
2. Test with multiple AI copilots to ensure compatibility
3. Update generated examples in `changes/` if needed
4. Document breaking changes clearly
5. Follow the same standards you're defining!

## 📄 License

Copyright (c) 2025 LIDR.co
Licensed under the MIT License

**English:**

The content of this repository is part of the AI4Devs program by LIDR.co. If you want to learn to code with AI like the pros and get more templates and resources like these, you can find all the information on the official website: [https://lidr.co/ia-devs](https://lidr.co/ia-devs)

**Español:**

El contenido de este repositorio es parte del programa AI4Devs de LIDR.co. Si quieres aprender a programar con IA como los pros, y obtener más plantillas y recursos como estos, puedes encontrar toda la información en la página oficial: [https://lidr.co/ia-devs](https://lidr.co/ia-devs)

---

## 🙏 Acknowledgements

Some workflows and skill patterns in this repository are inspired by the Superpowers framework, especially around:

- `using-git-worktrees`
- `writing-skills`

Superpowers project: [obra/superpowers](https://github.com/obra/superpowers/tree/main)

Additional inspiration/source acknowledgements:

- `code-auditing` skill: inspired by and adapted from [jeffrigby/somepulp-agents](https://github.com/jeffrigby/somepulp-agents/tree/main)

**Made with 🤖 by the LIDR community**

For questions, issues, or suggestions, visit [LIDR.co](https://lidr.co/ia-devs)