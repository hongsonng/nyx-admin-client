# Nyx project skills

These project-local skills follow the [Agent Skills specification](https://agentskills.io/specification). Each skill is an independently discoverable directory whose required entrypoint is `SKILL.md`.

```text
skills/
├── nyx-git-commit-quality/
│   └── SKILL.md          # Required: YAML frontmatter + Markdown instructions
└── nyx-reusable-components/
    └── SKILL.md          # Required: YAML frontmatter + Markdown instructions
```

Every `SKILL.md` must:

- use a frontmatter `name` that exactly matches its parent directory;
- use a lowercase hyphenated name no longer than 64 characters;
- include a non-empty `description` no longer than 1024 characters that explains what the skill covers and when to use it; and
- keep the instructions in Markdown, moving only genuinely heavy references or reusable tools into optional `references/`, `scripts/`, or `assets/` directories.

Keep each skill focused. The root `skills/README.md` is only this project catalog; it is not a skill entrypoint.

Validate an individual skill with the reference validator:

```bash
skills-ref validate ./skills/nyx-git-commit-quality
skills-ref validate ./skills/nyx-reusable-components
```
