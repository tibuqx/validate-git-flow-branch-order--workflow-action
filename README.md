# validate-git-flow-branch-order--workflow-action

Composite GitHub Action that enforces GitFlow branch promotion order on pull requests:

```
<feature-branch>  ──►  develop  ──►  staging  ──►  main
```

Fails the step (exit code 1) and emits a GitHub Actions error annotation when a pull request attempts to bypass required promotion stages.

---

## Architecture & Component Flow

```mermaid
C4Component
    title Component diagram for validate-git-flow-branch-order--workflow-action

    Container_Boundary(action_boundary, "Composite Action: validate-git-flow-branch-order") {
        Component(regex_validator, "Branch Pattern Validator", "Bash / ERE Regex", "Validates source branch prefix (feat, fix, chore, docs, etc.)")
        Component(flow_validator, "Promotion Chain Validator", "Bash Case Evaluator", "Validates allowed promotion target (develop, staging, main)")
        Component(annotation_reporter, "Error Annotator", "GitHub Workflow Commands", "Emits GitHub Actions error annotations on violation")
    }

    System_Ext(caller_pr, "Pull Request Workflow", "Triggers validation on pull_request events")

    Rel(caller_pr, flow_validator, "Passes head-ref and base-ref", "Action Inputs")
    Rel(flow_validator, regex_validator, "Checks head against pattern when targeting develop", "Bash ERE")
    Rel(flow_validator, annotation_reporter, "Emits failure message if invalid chain", "exit 1 / ::error")
```

---

## Usage

In your repository's `.github/workflows/` directory, add the action to your PR validation workflow:

```yaml
name: Validate PR Branch Order

on:
  pull_request:
    branches: [develop, staging, main]

jobs:
  validate-branch:
    runs-on: ubuntu-latest
    steps:
      - name: Validate GitFlow branch order
        uses: tibuqx/validate-git-flow-branch-order--workflow-action@v1
        with:
          head-ref: ${{ github.head_ref }}
          base-ref: ${{ github.base_ref }}
```

---

## Allowed Merge Paths

| Base (Target) | Allowed Head (Source) Pattern | Description |
|---|---|---|
| `develop` | `feat/*`, `fix/*`, `chore/*`, `docs/*`, `refactor/*`, `test/*`, `ci/*`, `build/*` | Feature and fix branches |
| `staging` | `develop` | Release candidate integration |
| `main` | `staging` | Production release |

---

## Inputs

| Name | Required | Default | Description |
|---|---|---|---|
| `head-ref` | **Yes** | `N/A` | Source branch of the pull request (pass `github.head_ref`). |
| `base-ref` | **Yes** | `N/A` | Target branch of the pull request (pass `github.base_ref`). |
| `feature-branch-pattern` | No | `^(feat|feature|fix|bugfix|hotfix|chore|docs|refactor|test|ci|build)/.+` | ERE regex matched against head ref when targeting develop. |

---

## Backstage Integration

This repository is registered in Backstage as `validate-git-flow-branch-order--workflow-action` (`catalog-info.yaml`).\n