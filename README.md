# Agent Skills

This repository is the source of truth for skills that are useful in more
than one project or agent host. Project-specific skills stay in the project
that owns their commands, paths, and operating history.

## Layout

Each immediate child of `skills/` is one skill:

```text
skills/
  skill-name/
    SKILL.md
    agents/       optional host metadata
    scripts/      optional deterministic helpers
    references/   optional instructions loaded on demand
    assets/       optional output resources
```

The shared contract belongs in `SKILL.md`. Host-specific metadata may be
added alongside it, but do not maintain separate Claude and Codex copies of
the same instructions.

## Included skills

- `delegate-with-subagents` — decide when and how to split work across
  bounded subagents.
- `audit-merges` — inspect a combined merge range for defects that emerge
  only when otherwise-correct branches interact.

## Add a skill

Add `skills/<skill-name>/SKILL.md`, then run:

```sh
python3 scripts/check.py
```

The checker validates every skill's required frontmatter, directory name,
and non-empty instructions.

## Make skills discoverable

For a personal installation, link each skill directory from this checkout
into both user catalogs:

```text
~/.claude/skills/<skill-name> -> <checkout>/skills/<skill-name>
~/.agents/skills/<skill-name> -> <checkout>/skills/<skill-name>
```

Link individual skills rather than the whole catalog so each host can also
keep tool-specific skills in its native directory.

For a repository-pinned installation, vendor this repository into the
consumer as `.shared-skills` using a Git subtree or submodule, then expose
the selected skills with internal relative links:

```text
.claude/skills/<skill-name> -> ../../.shared-skills/skills/<skill-name>
.agents/skills              -> ../.claude/skills
```

Internal relative links survive clones and worktrees. Do not check in links
to a sibling checkout outside the consumer repository.

## Scope rule

A skill belongs here when its behavior is shared. Exact package names,
build commands, artifact locations, and repository history normally belong
in the consuming project's `AGENTS.md`, `CLAUDE.md`, or local skill.
