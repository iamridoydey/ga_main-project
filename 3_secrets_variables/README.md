# Secrets And Variables

Short description
- GitHub Actions workflow that demonstrates using repository variables and secrets across two environments: `staging` and `production`.
- Trigger: manual (`workflow_dispatch`).


What this workflow does
- Defines two jobs: `staging-environment` and `production-environment`.
- Maps repository variables and secrets into job environment variables.
- Prints the mapped values to the runner (note: secrets are masked in logs by GitHub).

Workflow summary
```yaml
name: Secrets And Variables
on:
  workflow_dispatch:

jobs:
  staging-environment:
    runs-on: ubuntu-24.04
    environment: staging
    env:
      REPO_USERNAME: ${{ vars.REPO_USER_NAME }}
      REPO_PASS:     ${{ secrets.REPO_USER_PASS }}
      ENV_DB_NAME:   ${{ vars.DB_NAME }}
      ENV_DB_PASS:   ${{ secrets.DB_PASS }}

    steps:
      - name: Printing all staging variables
        run: |
          echo "REPOSITORY USERNAME: $REPO_USERNAME"
          echo "REPOSITORY PASSWORD(masked): $REPO_PASS"
          echo "ENV DATABASE NAME: $ENV_DB_NAME"
          echo "ENV DATABASE PASSWORD(masked): $ENV_DB_PASS"
  production-environment:
    runs-on: ubuntu-24.04
    environment: production
    env:
      REPO_USERNAME: ${{ vars.REPO_USER_NAME }}
      REPO_PASS:     ${{ secrets.REPO_USER_PASS }}
      ENV_PROD_DB_NAME: ${{ vars.PROD_DB_NAME }}
      ENV_PROD_DB_PASS: ${{ secrets.PROD_DB_PASS }}

    steps:
      - name: Printing all production variables
        run: |
          echo "REPOSITORY USERNAME: $REPO_USERNAME"
          echo "REPOSITORY PASSWORD(masked): $REPO_PASS"
          echo "ENV PRODUCTION DATABASE NAME: $ENV_PROD_DB_NAME"
          echo "ENV PRODUCTION DATABASE PASSWORD(masked): $ENV_PROD_DB_PASS"
```

Required repository settings
- Repository variables (Settings → Secrets and variables → Actions → Variables):
  - `REPO_USER_NAME`
  - `DB_NAME`
  - `PROD_DB_NAME`
- Repository secrets (Settings → Secrets and variables → Actions → Secrets):
  - `REPO_USER_PASS`
  - `DB_PASS`
  - `PROD_DB_PASS`

How to run
- Open the repository on GitHub → Actions → choose "Secrets And Variables" → Run workflow (select branch if needed).

Security notes
- GitHub masks secrets in logs, but avoid printing secrets where possible.
- Use environment protection rules for `staging` and `production` if required (Settings → Environments).
- Prefer using secrets directly in steps (e.g., as inputs to scripts) rather than echoing them.

Expected output (example)
- REPOSITORY USERNAME: your_user
- REPOSITORY PASSWORD(masked): ***
- ENV DATABASE NAME: staging_db
- ENV DATABASE PASSWORD(masked): ***

Troubleshooting
- If a variable shows empty, confirm name spelling in repository Variables.
- If a secret is not available, confirm it's added under Secrets and that you have access to the repository.

License
- No license specified.

Contact
- For issues, open an issue in the repository.