# ansible-role-rabbitmq - CLAUDE.md

## Project Overview

Infrastructure automation bootstrap repository. The `ansible/` directory contains roles, playbooks, inventory, and collection configuration for infrastructure automation. Linting is enforced via pre-commit (yamllint + ansible-lint).

## Current Status
- **Last Updated**: 2026-04-16
- **Current Phase**: Initial setup
- **Health**: Green

## Workflow — Agent Gates

These agents are mandatory gates, not optional tools. Do not skip them.

> **Issue tracker**: This repo is hosted on Bitbucket (`bitbucket.org/digitalisio/bootstrap`). All issues and work items are managed in **Jira**. Never create GitHub issues for this project.

### Before creating any Jira issue:

Run the **issue-writer** agent. Every issue must have: summary, detailed requirements, numbered acceptance criteria, specific testing requirements (named tests, not "add tests"), documentation requirements, dependencies, and labels. If any section is missing or vague, rewrite it before creating.

Then use the **atlassian:triage-issue** skill to search for duplicates before filing.

### When starting any new role or significant feature

1. **`agent-skills:source-driven-development`** — verify module signatures, parameter names, and deprecations against the current Ansible docs before writing any tasks
2. **`agent-skills:test-driven-development`** — write the molecule scenario (`molecule.yml`, `converge.yml`, `verify.yml`) before writing role tasks

### Before committing any Ansible content:

1. **ansible-specialist** — correctness, idempotency, FQCN usage, variable hygiene, molecule coverage, ansible-lint compliance. Do not commit if any ❌ issues remain.
2. **`agent-skills:security-and-hardening`** — on any tasks touching TLS, credentials, secrets, file permissions, or external input

### After completing any feature:

1. **ansible-specialist** — on all changed Ansible files
2. **ansible-devops-reviewer** — final review of role structure, handlers, and molecule tests
3. **docs-quality-reviewer** — on any README or user-facing documentation changes

### Commit messages and branch names:

- **Branch name** must include the Jira ticket key: `feat/BOOT-42-add-prometheus-role`
- **PR/merge commit title** must include the Jira ticket key: `feat(BOOT-42): add Prometheus node exporter role`
- Individual WIP commits do not need the ticket key — the branch and PR title are the canonical reference
- Commit messages must be self-describing without the ticket (readable in `git log` without Jira access)

### Before creating any pull request:

1. **`agent-skills:ci-cd-and-automation`** — verify the Bitbucket/GitHub pipeline runs `molecule test` across all six required platforms
2. **ansible-specialist** — one final time on the full diff

A PR must not be opened until:
- All ❌ issues are resolved
- CI pipeline exists and covers all six distros (`rockylinux9`, `rockylinux10`, `ubuntu2204`, `ubuntu2404`, `debian12`, `debian13`)
- Documentation is current (role READMEs, examples)
- Molecule tests cover all new behaviour
- `CHANGELOG.md` is updated with all changes included in the PR

## Active Tasks

1. [NOT STARTED] Define first role
   - Status: Not Started

## Architecture & Key Decisions

- **Linting**: yamllint + ansible-lint (profile: production) via pre-commit
- **Config location**: `ansible/.yamllint`, `ansible/.pre-commit-config.yaml`
- **Role pattern**: `defaults/` → `tasks/main.yml` (orchestrator) → subtask imports → `molecule/default/` tests
- **Molecule driver**: Docker with `geerlingguy/docker-*-ansible` images

## Useful Commands

```bash
# Install pre-commit hooks
cd ansible && pre-commit install

# Run linting manually against staged files
pre-commit run --files <file1> <file2>

# Run all linting
pre-commit run --all-files

# Run ansible-lint directly
ansible-lint ansible/

# Run yamllint directly
yamllint -c ansible/.yamllint ansible/

# Run molecule tests for a role
cd ansible/roles/<name> && molecule test
```

## Dependencies

- Python 3.11+
- ansible-core >= 2.15
- ansible-lint >= 24.12.2
- yamllint >= 1.35.1
- molecule[docker] for role testing
- pre-commit

```bash
pip install ansible-core ansible-lint yamllint pre-commit molecule[docker]
```

## Known Limitations & Tech Debt

None currently.

## Next Steps & Roadmap

1. Define inventory structure
2. Add first role
3. Configure molecule CI
