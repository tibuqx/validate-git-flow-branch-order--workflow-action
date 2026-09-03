# AI Agents Orientation

This repository is governed by the rules in the
[swe-playbook--docs](https://github.com/tibuqx/swe-playbook--docs) repository.
Please read the root `README.md` for architectural context and component boundaries.

## Repository Profile

| Field | Value |
|---|---|
| **Name** | validate-git-flow-branch-order--workflow-action |
| **Description** | Composite GitHub Action — enforces GitHub Flow (ADR-0046) branch conventions on PRs. |
| **Type** | library |
| **System** | ci-cd-tooling |
| **Owner** | group:default/architecture |
| **Lifecycle** | production |

## Stack & Technologies

- **CI/CD Platform**: GitHub Actions reusable workflows and composite actions (ADR-0046)

## Key Directories

| Directory | Purpose |
|---|---|
| `.antigravity/` | Antigravity agents and rules |
| `.claude/` | Claude Code skills, agents, and slash commands |
| `.cursor/` | Project folder: .cursor |
| `.github/` | GitHub Actions workflows, prompts, and instructions |
| `rules/` | Coding and architectural rules |

## Build & Test Commands

| Task | Command |
|---|---|
| Run checks | Inspect `action.yml` for syntax and step validation |

## Conventions & Boundaries

- **Workflow Tagging**: SemVer release tags via versioning workflow (ADR-0021 / ADR-0031).
- **Zero Secrets**: Never commit secrets or credentials. Use AWS Secrets Manager or GitHub Secrets.
- **Observability**: Structured logs, OpenTelemetry traces, and dual SLI latency/error metrics across all service workflows.
- **Conventional Commits**: Format commit messages as `feat:`, `fix:`, `chore:`, etc. (ADR-0005).
- **PR-Only Merges**: Direct pushes to `main` are restricted. All changes require Pull Requests (ADR-0024, ADR-0046).

## ADR Compliance

- This repo complies with **ADR-0006** (AI-friendly documentation), **ADR-0035** (mandatory README.md with C4 diagram + AGENTS.md), and **ADR-0046** (Standardize on GitHub Flow only).
- Check applicable ADRs in `swe-playbook--docs` before proposing architectural or structural changes.
- Cite ADR IDs (`// See ADR-NNNN`) in code decisions and PR descriptions.
- Only ADRs with `status: accepted` define current practice.

