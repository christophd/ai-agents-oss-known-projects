# AI Agents OSS Helper — Known Projects

Centralized project configuration rules consumed by [ai-agents-oss-helper](https://github.com/Open-Harness-Engineering/ai-agents-oss-helper) commands.

## Purpose

This repository is an opt-in alternative to shipping `.oss-ai-helper-rules/` inside each project. Projects that prefer not to host AI-agent metadata in their own source tree (or that cannot ship dot-directories for policy reasons) can have their rules hosted here instead. Users install the rules locally with the `/oss-install-info` command from the helper.

## Layout

Each project has its own top-level directory containing the three rule files the helper expects:

```
ai-agents-oss-known-projects/
├── <project>/
│   ├── project-info.md          # Repository URLs, issue trackers, related repos
│   ├── project-standards.md     # Build tools, commands, code style restrictions
│   └── project-guidelines.md    # Branch naming, commit formats, PR policies
└── generic-github/              # Catch-all fallback for any GitHub project
    ├── project-info.md
    ├── project-standards.md
    └── project-guidelines.md
```

The `<project>` directory name is the slug the helper uses internally (for example `camel-core` for `apache/camel`).

## Adding a New Project

Open a pull request that adds a new `<project>/` directory with the three required files. Use any existing project (for example `wanaku/`) as a template. The `generic-github/` directory should remain as the catch-all fallback.

Every rule file must include a `## Version` section at the bottom with the git SHA of the project being configured — `/oss-install-info` and `.oss-init.md` use this to detect when local copies are out of date.

## Installing Rules Locally

From any project directory:

```
/oss-install-info <project>
```

For example:

```
/oss-install-info camel-core
```

The command fetches the three rule files for `<project>` from this repository and writes them under the helper's agent-specific rules directory (`~/.claude/rules/<project>/`, `~/.bob/rules/<project>/`, `~/.gemini/rules/<project>/`, `~/.config/opencode/rules/<project>/`, or `~/.codex/oss-helper/rules/<project>/`).

## Project-Local Rules Still Take Precedence

If a project repository already ships its own `.oss-ai-helper-rules/` directory, the helper uses those files first and ignores both this repository and any locally installed copies. See the helper's `.oss-init.md` for the full priority order.

## License

Apache License 2.0
