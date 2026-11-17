# Contributing

## Naming

- Database tables: plural, snake_case (`locations`)
- Columns: snake_case (`aisle`, `created_at`)
- Eloquent models: singular, PascalCase (`Location`)
- Routes and endpoints: plural, kebab-case where multi-word
- React components: PascalCase, one component per file

## Commits

Format: `<type>(<scope>): <description>`

Types:

- `feat` — new functionality
- `fix` — bug fix
- `refactor` — code changes, behaviour does not
- `test` — tests added or changed
- `docs` — documentation
- `chore` — dependencies, configuration, setup

Scope is optional and names the area touched: `backend`, `frontend`, `docker`, or a domain area such as `locations`, `picking`.

Description rules:

- imperative present tense: `add location model`, not `added` or `adds`
- lowercase initial, no trailing period, under ~50 characters
- states what the commit does, not which files it touches
- no manual numbering: git already orders the history

One commit is one complete logical unit. If the description needs an "and", it is probably two commits.

For non-trivial commits, a blank line after the title followed by a body explaining why the change was made — the what is readable from the diff.

## Daily loop

All commands run inside the containers. Service names are the ones declared in `docker-compose.yml`: `backend`, `frontend`, `mysql`.

1. Pull the current branch.
2. Write code.
3. Format the files touched by the current work.
4. Lint the client.
5. Verify in non-destructive mode.
6. Commit.

## Commands

Backend (PHP, Laravel):

    docker compose exec --workdir /var/www/html backend composer lint
    docker compose exec --workdir /var/www/html backend composer lint:test

Frontend (TypeScript, React):

    docker compose exec frontend npm run format
    docker compose exec frontend npm run format:check
    docker compose exec frontend npm run lint

## When each step runs

- Before every commit: format the touched files, then lint, then verify.
- Before every push: verify both sides in non-destructive mode.
- Never commit with a failing linter.

## Code style

- Style-only changes go in their own commit, never mixed with behaviour.
- `backend/pint.json`, `frontend/eslint.config.js` and `frontend/.prettierrc` are the single source of truth. Editor settings must not override them.
- PHP files declare strict types; Pint adds the declaration.

## Editor

The client is edited from inside its container, through the Dev Containers extension and `.devcontainer/devcontainer.json`: the TypeScript server needs the dependencies, which live in the container volume and are not visible on the host. Git runs on the host, since the container only mounts `frontend/`.

## Docker notes

- Frontend dependencies are installed inside the container: `docker compose exec frontend npm install -D <package>`. Installing from the host puts them where the container cannot see them.
- After rebuilding the frontend image, recreate the anonymous volume that holds `node_modules`, otherwise the container keeps the old one: `docker compose up -d --build --renew-anon-volumes`.
- The production Dockerfile, still to be written, uses `npm ci` instead of `npm install`, so builds install the versions locked in `package-lock.json`.