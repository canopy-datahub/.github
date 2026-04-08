# Contributing to Canopy

Thank you for your interest in contributing to **Canopy** — an open-source platform for FAIR-aligned scientific data hubs. We welcome contributions of all kinds: bug fixes, features, documentation, tests, infrastructure improvements, and more.

Please read this guide before opening an issue or pull request. It helps us review and merge your contribution quickly.

---

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Ways to Contribute](#ways-to-contribute)
   - [Suggested Contributions](#suggested-contributions)
3. [Reporting Bugs & Requesting Features](#reporting-bugs--requesting-features)
4. [Development Setup](#development-setup)
   - [Frontend (Next.js / React)](#frontend-nextjs--react)
   - [Backend (Spring Boot)](#backend-spring-boot)
5. [Branch Naming](#branch-naming)
6. [Commit Messages](#commit-messages)
7. [Pull Request Process](#pull-request-process)
8. [Code Style](#code-style)
9. [Testing](#testing)
10. [Documentation](#documentation)
11. [Repository Map](#repository-map)

---

## Code of Conduct

All contributors are expected to uphold a welcoming and respectful environment. We follow the [Contributor Covenant Code of Conduct](https://www.contributor-covenant.org/version/2/1/code_of_conduct/). By participating, you agree to abide by its terms.

Please report unacceptable behavior to the maintainers via the repository's issue tracker.

---

## Ways to Contribute

| Type | How |
|------|-----|
| Bug fix | Open an issue first, then a PR referencing it |
| New feature | Open a discussion/issue first to align on approach |
| Documentation | Direct PR is fine for small fixes; open an issue for larger rewrites |
| Tests | Always welcome — PRs adding coverage can be opened directly |
| Infrastructure | Changes to CloudFormation or deployment scripts require extra review |
| Security | **Do not open a public issue.** Email the maintainers directly. |

---

### Suggested Contributions

The items below are known platform gaps documented in [LIMITATIONS.md](https://github.com/canopy-datahub/datahub-docs/blob/main/LIMITATIONS.md). They are good candidates for new API endpoints and/or admin UI features that would let operators manage content **without direct database access**.

#### Content Management API & Admin UI

Several content types (Funding Opportunities, News, Events, Newsletters) are currently managed by running raw SQL against the database. There are already read-only `GET` endpoints in `datahub-service-entity`, but no write endpoints or admin interface exist.

**Potential contributions:**
- Add `POST`, `PUT`, and `DELETE` endpoints to `datahub-service-entity` for each content type:
  - `POST /api/entity/v1/funding` — create a funding opportunity (DB table: `public.funding`)
  - `PUT /api/entity/v1/funding/{id}` — update a funding opportunity
  - `DELETE /api/entity/v1/funding/{id}` — remove a funding opportunity
  - Equivalent CRUD endpoints for `public.news` / `public.news_link`, `public.events` / `public.event_link`, and `public.newsletter`
- Build an **Admin UI** in `datahub-ui-main` (behind a role-restricted route) with forms to create, edit, and delete each content type, calling the new endpoints above.

#### Resource Center Content Management

The Resource Center page, homepage footer links, and social media links are currently **hardcoded in React JSX** and require a code change + redeployment to update.

**Potential contributions:**
- Move Resource Center cards and footer link data into database tables (new tables in `datahub-development/db/postgres/`).
- Add `GET`/`POST`/`PUT`/`DELETE` endpoints to `datahub-service-entity` to serve and manage this content.
- Update the `datahub-ui-main` Resource Center and footer components to fetch from the API instead of importing hardcoded constants.
- Expose editing in the Admin UI described above.

#### Lookup Table Admin Interface

Lookup tables (`lkup_*`) — which control entity types, roles, statuses, and other enumerated values — can currently only be updated by editing and re-running SQL seed scripts.

**Potential contributions:**
- Add read and write endpoints for `lkup_*` tables to `datahub-service-entity`.
- Build an admin UI panel (under the same role-restricted admin route) allowing operators to view and manage lookup values without database access.

#### Per-User / Per-Study File Access Control

All approved data files are currently **publicly accessible** to any user who can reach the platform. There is no per-user or per-study enforcement at the download level.

**Potential contributions:**
- Design and implement a permissions model in `datahub-service-download` that checks a user's data access approvals before serving a file.
- Add the necessary database schema (access request/approval tables) and corresponding API endpoints for requesting and granting file access.

#### Customizable Metadata Forms and Controlled Vocabularies

The study registration form and data file metadata template are **fixed and hardcoded**. Projects with domain-specific metadata needs cannot adapt them.

**Potential contributions:**
- Design a schema for configurable metadata templates stored in the database.
- Build a UI for administrators to define, edit, and publish custom metadata forms.
- Extend CDE/codebook support so projects can maintain their own Common Data Elements alongside or instead of the Global Codebook.

#### Additional File Format Support

The platform only supports CSV (data), JSON-LD/YAML (metadata), data-dictionary CSV, and PDF. Images, code notebooks (`.ipynb`, `.py`, `.R`), and other tabular formats are not supported.

**Potential contributions:**
- Extend the file validation logic in `datahub-service-submission` to accept additional material types.
- Update the data model and frontend Study Overview pages to display or link new file types appropriately.

---

## Reporting Bugs & Requesting Features

Before opening a new issue:

- Search existing issues to avoid duplicates.

When filing a bug, include:
- A clear, concise title
- Steps to reproduce
- Expected vs. actual behavior
- Environment details (browser, OS, Java/Node version, deployment environment)
- Relevant logs or screenshots

When requesting a feature, describe:
- The problem you are trying to solve
- Your proposed solution (if any)
- Any alternatives you have considered

---

## Development Setup
[Deployment Guide](https://github.com/canopy-datahub/datahub-docs/blob/feature/aws/DEPLOYMENT_GUIDE.md)

---

## Branch Naming

Use the following prefixes, followed by a short kebab-case description:

| Prefix | Purpose |
|--------|---------|
| `feature/` | New feature or enhancement |
| `fix/` | Bug fix |
| `docs/` | Documentation only |
| `refactor/` | Code restructuring without behavior change |
| `test/` | Adding or updating tests |
| `chore/` | Dependency updates, build changes, tooling |
| `hotfix/` | Urgent fix targeting a release branch |

**Examples:**
```
feature/study-export-csv
fix/banner-white-gap
docs/update-deployment-guide
chore/bump-spring-boot-3.2
```

---

## Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <short summary>

[optional body]

[optional footer: closes #<issue-number>]
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `ci`

**Scope** (optional): the affected repo or component, e.g. `ui`, `user-service`, `search`, `cloudformation`

**Examples:**
```
feat(ui): add warning banner to all pages
fix(search): handle empty query string in OpenSearch call
docs: update CONTRIBUTING with backend setup instructions
chore(deps): upgrade Spring Boot to 3.2.4
```

Keep the summary line under **72 characters**. Use the body to explain *why*, not *what*.

---

## Pull Request Process

1. **Fork** the repository and create your branch from `main` (or the active development branch).
2. **Make your changes** following the code style guidelines below.
3. **Add or update tests** where applicable.
4. **Run the full test suite** locally and ensure it passes.
5. **Update documentation** (README, inline comments) if your change affects behavior or configuration.
6. **Open a PR** against `main` with:
   - A clear title following the commit message format
   - A description of *what* changed and *why*
   - Reference to any related issues (`Closes #123`)
   - Screenshots or logs for UI or behavioral changes
7. **Address review feedback** — maintainers may request changes before merging.
8. PRs require **at least one approving review** before merge.
9. Squash commits before merging unless a multi-commit history is intentional.

> **Infrastructure changes** (CloudFormation, ECS task definitions, RDS, OpenSearch config) require additional review and must be tested in a non-production environment first.

---

## Code Style

### Frontend (JavaScript / React)

- **ESLint** is configured and enforced — run `npm run lint` before committing.
- Use **functional components** and React hooks. Avoid class components.
- Use **CSS Modules** (`.module.scss`) for component-scoped styling; avoid inline styles.
- Keep components small and focused. Extract reusable logic into custom hooks under `/lib/hooks/`.
- Use **Redux** only for global state (user session, toast notifications). Component-local state belongs in `useState`.
- Prop types must be declared with `PropTypes` for all components.
- Avoid committing `console.log` statements.

### Backend (Java / Spring Boot)

- Follow standard **Java naming conventions** (PascalCase for classes, camelCase for methods and variables, SCREAMING_SNAKE_CASE for constants).
- Keep controllers thin — business logic belongs in service classes.
- Use constructor injection over field injection (`@Autowired` on fields).
- DTOs should be used for all API request/response payloads — do not expose entity classes directly.
- New endpoints must include appropriate Javadoc and be registered in the service's API documentation.
- Do not hardcode secrets, URLs, or environment-specific values — use Spring profiles and AWS Secrets Manager keys defined in `application.yml`.

### Infrastructure (CloudFormation / Scripts)

- All CloudFormation templates must be linted and validated with `aws cloudformation validate-template` before submission.
- Use parameters and mappings instead of hardcoded values.
- Python scripts must target **Python 3.8+** and include a `requirements.txt`.

---

## Documentation

- Every repository has a `README.md` — keep it up to date when you change setup steps, environment variables, or endpoints.
- The org-level deployment guide lives in [`datahub-docs`](https://github.com/canopy-datahub/datahub-docs). Update it if your change affects deployment or infrastructure.
- Inline code comments should explain **why**, not what. Avoid comments that simply restate the code.
- For significant new features, consider adding or updating the relevant section in `datahub-docs/DEPLOYMENT_GUIDE.md`.

---

## Repository Map

| Repository | Stack | Purpose |
|---|---|---|
| [datahub-ui-main](https://github.com/canopy-datahub/datahub-ui-main) | Next.js / React | Frontend web application |
| [datahub-service-entity](https://github.com/canopy-datahub/datahub-service-entity) | Spring Boot / Java 17 | Entity retrieval service |
| [datahub-service-search](https://github.com/canopy-datahub/datahub-service-search) | Spring Boot / Java 17 | OpenSearch-based search service |
| [datahub-service-user](https://github.com/canopy-datahub/datahub-service-user) | Spring Boot / Java 17 | User management and auth service |
| [datahub-service-submission](https://github.com/canopy-datahub/datahub-service-submission) | Spring Boot / Java 17 | Data and study ingest service |
| [datahub-service-report](https://github.com/canopy-datahub/datahub-service-report) | Spring Boot / Java 17 | Metrics and reporting service |
| [datahub-service-download](https://github.com/canopy-datahub/datahub-service-download) | Spring Boot / Java 17 | File download service |
| [datahub-service-email](https://github.com/canopy-datahub/datahub-service-email) | Spring Boot / Lambda | SQS-triggered email notifications |
| [datahub-lib-keycloak-auth](https://github.com/canopy-datahub/datahub-lib-keycloak-auth) | Java 17 | Shared Keycloak auth library |
| [datahub-project](https://github.com/canopy-datahub/datahub-project) | Maven | Parent POM for all Java services |
| [datahub-cloud-replication](https://github.com/canopy-datahub/datahub-cloud-replication) | CloudFormation | AWS infrastructure as code |
| [datahub-development](https://github.com/canopy-datahub/datahub-development) | SQL / Python / Docker | DB schema, OpenSearch tooling, Keycloak |
| [datahub-docs](https://github.com/canopy-datahub/datahub-docs) | Markdown | Deployment guide and operator docs |
| [datahub-cli](https://github.com/canopy-datahub/datahub-cli) | CLI | Developer tooling |

---

*Thank you for contributing to Canopy!*
