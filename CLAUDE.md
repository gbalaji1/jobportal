# CLAUDE.md

This file provides guidance to Claude Code (`claude.ai/code`) when working with code in this repository.

## Commands

- `npm run dev` — Start Vite dev server
- `npm run build` — Production build
- `npm run lint` — ESLint (flat config, JS/JSX only)
- `npm run preview` — Preview production build

## Architecture

React 19 SPA using Vite 7, Tailwind CSS 4, and React Router 7. No TypeScript — plain JSX throughout.

### State Management

Two layers of React Context:

- `src/context/` — Core contexts: `AuthContext` (auth + dummy users + localStorage persistence), `JobContext` (applications, saved jobs, employer job CRUD), `ThemeContext`
- `src/contexts/` — Data-fetching contexts: `JobsDataContext` (cached job list with 5-min TTL), `CompaniesContext`

Provider nesting order (in `App.jsx`): AuthProvider → JobsDataProvider → JobProvider → CompaniesProvider → ThemeProvider

### Data Layer

Currently uses **mock data** with localStorage persistence — no real backend. Services in `src/services/` simulate async API calls using `delay()` from `src/utils/delay.js`. Data originates from `src/data/mockData.js`.

Key localStorage keys: `jobPortalUser`, `authToken`, `registeredUsers`, `globalPostedJobs`, `jobApplications_{userId}`, `savedJobs_{userId}`, `postedJobs_{userId}`.

### Routing & Roles

Three roles with route protection via `ProtectedRoute` component:

- `ROLE_JOB_SEEKER` — profile, applied-jobs, saved-jobs
- `ROLE_EMPLOYER` — post-job, employer/jobs, job-applicants/:jobId
- `ROLE_ADMIN` — admin/*, admin pages in `src/pages/admin/`

### Key Libraries

- Font Awesome + Lucide React for icons
- react-toastify for notifications

### ESLint

Flat config (`eslint.config.js`). The `no-unused-vars` rule ignores variables starting with uppercase or underscore (`varsIgnorePattern: '^[A-Z_]'`).

## Git Workflow

Use the installed Claude Code `commit-commands` plugin for Git commit and Pull Request workflows whenever its commands are available.

### Branching Rules

- Never commit directly to `main` or `master`.
- Start new work on a descriptive branch.
- Use `feature/<description>` for new features.
- Use `bugfix/<description>` for bug fixes.
- Use `hotfix/<description>` for urgent fixes.
- Before creating a branch, inspect `git status` and preserve all existing user changes.
- If the working tree has unrelated changes, do not include them in the current commit.
- Do not switch branches in a way that risks losing uncommitted changes.

### Before Committing

1. Review the changed files using `git status` and `git diff`.
2. Check for secrets, API keys, passwords, `.env` files, and generated artifacts.
3. Run `npm run lint` and `npm run build` when practical.
4. Fix issues caused by the current changes before proceeding.
5. Stage only files related to the requested task.

### Commit

- Use the `commit-commands` plugin's commit command when available.
- Write a concise, descriptive commit message.
- Review the staged diff before creating the commit.
- Never amend an existing commit unless explicitly requested.
- Never use destructive commands such as `git reset --hard` to clean up changes without explicit approval.

### Push to GitHub

- Push the feature branch to the configured `origin` remote.
- Use the current branch name; do not push feature work directly to `main` or `master`.
- Never force-push or overwrite remote history without explicit authorization.
- If GitHub authentication or the remote is not configured, explain the required setup and stop.

### Create a Pull Request

- Use the `commit-commands` plugin's Pull Request command when available.
- Otherwise, use GitHub CLI (`gh pr create`) if installed and authenticated.
- Target `main` as the base branch unless the repository specifies another target.
- Include a meaningful title, a summary of changes, and relevant test results.
- Do not merge the Pull Request unless explicitly requested.
- Report the Pull Request URL after successful creation.

### Approval and Safety

- Inspect and summarize the intended changes before committing or pushing.
- Ask for approval before committing, pushing, or creating a Pull Request.
- Do not commit secrets, unrelated files, or generated build output.
- If a command fails, explain the error rather than assuming success.
- Never claim a commit, push, or Pull Request succeeded without verifying the result.

## Expected Git Workflow

1. Inspect the repository and working tree.
2. Create a feature or bugfix branch.
3. Implement the requested code changes.
4. Run lint and build checks.
5. Review and stage relevant files.
6. Commit using `commit-commands`.
7. Push the branch to GitHub.
8. Create a Pull Request using `commit-commands` or GitHub CLI.
9. Report the branch, commit hash, test results, and Pull Request URL.