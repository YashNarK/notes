---
description: "Use when creating, editing, or reviewing markdown notes files in this repository. Enforces note structure, relative link correctness, README reachability, and content standards."
applyTo: "**/*.md"
---

# Notes Repository Standards

## File Structure (required in every note)

```markdown
# Title

## Table of contents
- [Title](#title)
  - [Section 1](#section-1)
  ...

---

## Section 1
...
```

Every note MUST have:
- A single `#` title
- `## Table of contents` with anchor links to all `##` sections
- At least one `---` separator before the first section
- Substantive content with code examples where practical

## Relative Link Rules

File depth determines how many `../` levels are needed:

| File location | To reach `language/` | To reach `prompt_engineering/` | To reach `cybersecurity/` |
|---|---|---|---|
| `full stack web development/MERN/frontend/*.md` | `../../../language/` | `../../../prompt_engineering/` | `../../../cybersecurity/` |
| `full stack web development/MERN/frontend/React/*.md` | `../../../../language/` | `../../../../prompt_engineering/` | `../../../../cybersecurity/` |
| `README.md` (root) | `language/` | `prompt_engineering/` | `cybersecurity/` |

**Always verify** relative paths resolve to real files. A path like `../../language/` from a 3-level-deep file is WRONG (too shallow).

## Reachability Rule

Every `.md` file MUST be linked from either:
1. `README.md` directly, OR
2. A file that is itself reachable from `README.md`

Frontend files should be linked through `full stack web development/MERN/frontend/frontend_basics.md`.

## Content Standards

- **Generic only** — no job-specific, role-specific, or person-specific content
- **Code examples** — every concept section should have a working code snippet
- **No duplication** — if a concept exists in another file, link to it instead of repeating
- **Heading hierarchy** — never skip levels (`#` → `##` → `###`, never `#` → `###`)

## Before Creating a New File

Ask: does this topic have a natural home in an existing file?
- JS language features → `language/JavaScript.md`
- CSS features/preprocessors → `full stack web development/MERN/frontend/CSS.md`
- React features → `full stack web development/MERN/frontend/React/React.md` (if React.md is < ~300KB)
- AI/prompting → `prompt_engineering/PromptEngineering.md`

Only create a new standalone file when the topic is substantial (> ~10KB of content) AND has no natural existing home.
