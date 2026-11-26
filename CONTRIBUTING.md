# Contributing to PayLink

## Rules

1. Never push directly to `main`. It is protected.
2. Every change goes through a pull request.
3. Each pull request needs **1 teammate approval** and a **passing CI check** (`build-test`).
4. Never commit secrets. Use `.env.example` for variable names only.

## Branch names

Use a prefix and, where possible, the requirement ID:

- `feature/fr-10-idempotency`
- `fix/fr-18-daily-limit`
- `docs/class-diagram`
- `chore/update-ci`

## Workflow

```bash
git checkout main
git pull
git checkout -b feature/fr-XX-short-name
# ...make changes...
git add .
git commit -m "FR-XX: short description"
git push -u origin feature/fr-XX-short-name
```

Then open a pull request on GitHub, fill in the template, and ask a teammate to review.

## Before opening a pull request

```bash
cd backend
npm run lint
npm run test:cov
```

## Commit messages

Start with the requirement ID when there is one, for example `FR-10: return the first result for a repeated idempotency key`.

## AI prompt log

Record the prompts the team actually used, what the AI returned, and what the team changed, in `docs/prompt-log.md`. Keep it honest.

## Shared names

Use the same actor names, payment states and business rules as the requirements pack in `docs/`.
