# Treasury Management System — Backend

Backend API for the **Treasury Management System (TMS)**, built with **Node.js, Express.js, TypeScript, MariaDB/MySQL, and Prisma**.

This project is developed collaboratively by 8 teams. Each team owns a separate feature module to reduce conflicts and keep the codebase organized.

## Tech Stack

* Node.js
* Express.js
* TypeScript
* MariaDB / MySQL
* Prisma ORM
* `tsx` for development
* Git + GitHub

---

# Getting Started

## 1. Fork the repository

Open the main repository:

https://github.com/kawsaramin101/tms-be

Click **Fork** in the top-right corner of GitHub.

Create the fork under your own GitHub account.

You will now have your own copy:

```text
https://github.com/<your-username>/tms-be
```

**Do not push directly to the main repository.**

---

## 2. Clone your fork

Clone **your fork**, not the original repository:

```bash
git clone https://github.com/<your-username>/tms-be.git
cd tms-be
```

---

## 3. Install dependencies

```bash
npm install
```

---

## 4. Configure the database

This project uses MariaDB/MySQL.

Create a database named:

```text
tms
```

Create a database user:

```text
Username: tms
Password: hello_world
Host: localhost
Port: 3306
```

Configure the `.env` file:

```env
DATABASE_URL="mysql://tms:hello_world@localhost:3306/tms"
```

Do not commit real production credentials.

---

## 5. Generate Prisma Client

```bash
npx prisma generate
```

---

## 6. Run database migrations

```bash
npx prisma migrate dev
```

If you are creating the first migration:

```bash
npx prisma migrate dev --name init
```

---

## 7. Start the development server

```bash
npm run dev
```

The development server uses `tsx` and automatically reloads when source files change.

---

# Project Structure

The backend uses a **feature-based architecture**. Each team owns one module.

```text
src/
├── modules/
│   ├── auth/             # Team 1
│   ├── transactions/     # Team 2
│   ├── vouchers/         # Team 3
│   ├── calculations/     # Team 4
│   ├── members/          # Team 5
│   ├── fees/             # Team 6
│   ├── reports/          # Team 7
│   └── notifications/    # Team 8
│
├── middleware/           # Shared Express middleware
├── lib/                  # Shared utilities/services
├── config/               # Application configuration
├── app.ts                # Express app configuration
└── server.ts             # Server entry point

prisma/
├── schema.prisma         # Database schema
└── migrations/           # Database migrations

uploads/
└── vouchers/             # Voucher files/images

tests/                    # Tests
```

Each module normally contains:

```text
module/
├── *.controller.ts
├── *.service.ts
├── *.routes.ts
├── *.validation.ts
└── *.types.ts
```

**Before changing code, ask your LLM to inspect the existing project and follow its architecture. Do not let it redesign the project or modify another team's module unnecessarily.**

### Team ownership

| Team | Folder                       |
| ---- | ---------------------------- |
| 1    | `src/modules/auth/`          |
| 2    | `src/modules/transactions/`  |
| 3    | `src/modules/vouchers/`      |
| 4    | `src/modules/calculations/`  |
| 5    | `src/modules/members/`       |
| 6    | `src/modules/fees/`          |
| 7    | `src/modules/reports/`       |
| 8    | `src/modules/notifications/` |

Shared infrastructure (`middleware`, `lib`, `config`, `app.ts`, `server.ts`, and `prisma/`) should be changed only when necessary and coordinated with the other teams.

---

# Development Rules

## 1. Work inside your team's module

If you are Team 5, for example:

```text
src/modules/members/
```

should be your primary workspace.

Avoid modifying another team's module unless you have coordinated with that team.

## 2. Reuse shared code

Before creating a new:

* middleware
* utility
* Prisma client
* authentication helper
* upload handler

check whether one already exists in:

```text
src/middleware/
src/lib/
src/config/
```

## 3. Keep controllers thin

Prefer:

```text
Route
  ↓
Controller
  ↓
Service
  ↓
Prisma
  ↓
Database
```

Avoid putting large amounts of business logic directly inside controllers.

## 4. Keep business logic inside services

For example:

```text
transaction.controller.ts
        ↓
transaction.service.ts
        ↓
Prisma
```

The controller handles HTTP.

The service handles the actual business operation.

## 5. Do not create duplicate database clients

Use the shared Prisma client from:

```text
src/lib/prisma.ts
```

---

# Working With an LLM

Every team may use an LLM such as ChatGPT, Claude, Gemini, or GitHub Copilot.

However, the LLM should **not be allowed to freely redesign the project architecture**.

Before asking an LLM to modify code, give it the relevant project context.

## Recommended prompt

```text
You are working on the TMS backend.

Project:
Treasury Management System

Stack:
- Node.js
- Express.js
- TypeScript
- MariaDB/MySQL
- Prisma

Architecture:
Feature-based modular architecture.

src/modules/
├── auth/
├── transactions/
├── vouchers/
├── calculations/
├── members/
├── fees/
├── reports/
└── notifications/

Shared infrastructure:
src/middleware/
src/lib/
src/config/
src/app.ts
src/server.ts

Rules:
1. Work only on the module I specify.
2. Do not redesign the project architecture.
3. Do not create duplicate Prisma clients.
4. Reuse existing shared middleware and utilities.
5. Keep HTTP logic in controllers.
6. Keep business logic in services.
7. Keep routes inside the module's routes file.
8. Keep module-specific validation in the validation file.
9. Keep module-specific TypeScript types in the types file.
10. Do not modify another team's module unless explicitly requested.
11. Do not replace entire files when only a small change is required.
12. Preserve existing code and make the smallest necessary change.
13. Do not invent APIs, database fields, or existing functions.
14. If required information is missing, ask before making architectural changes.
15. Follow the existing coding style.

My team/module:
[PUT YOUR MODULE HERE]

Task:
[DESCRIBE THE TASK HERE]

Before changing code:
- Inspect the relevant existing files.
- Explain which files need to change.
- Then provide the implementation.
```

Replace:

```text
[PUT YOUR MODULE HERE]
```

with your team's module, for example:

```text
My team/module:
members
```

---

# LLM Rules for Database Changes

Database changes affect the entire project.

If an LLM suggests changing:

```text
prisma/schema.prisma
```

do not blindly apply it.

First check:

```text
Does this model already exist?
Does another team use this model?
Will this change break another module?
Does this require a migration?
```

Ask the LLM:

```text
Before modifying prisma/schema.prisma, inspect the existing schema and explain:

1. What models are affected?
2. Whether an existing model can be reused.
3. What relationships will change.
4. What migration will be required.
5. Whether another module could be affected.

Do not modify the schema until I confirm.
```

---

# Git & GitHub Workflow

Everyone works directly from the main repository as a GitHub collaborator.

Repository:

```text
https://github.com/kawsaramin101/tms-be
```

## 1. Clone the repository

```bash
git clone https://github.com/kawsaramin101/tms-be.git
cd tms-be
```

## 2. Create your own branch

**Do not work directly on `main`.**

```bash
git checkout -b feature/<your-feature>
```

Examples:

```bash
git checkout -b feature/member-management
git checkout -b feature/voucher-upload
git checkout -b feature/transaction-api
```

## 3. Work on your feature

Make your changes inside your team's module.

For example:

```text
src/modules/members/
```

## 4. Commit your changes

```bash
git status
git add .
git commit -m "feat: add member management"
```

Use clear commit messages:

```text
feat: add member management
feat: add voucher upload
fix: validate transaction amount
refactor: simplify transaction service
docs: update API documentation
```

## 5. Push your branch

```bash
git push -u origin feature/<your-feature>
```

Example:

```bash
git push -u origin feature/member-management
```

## 6. Create a Pull Request

After pushing your branch, open the repository on GitHub.

Create a Pull Request:

```text
your branch
     ↓
main
```

Explain:

* What you changed
* Which module you worked on
* Whether you changed the database
* How you tested it

Wait for the project maintainer/team lead to review and merge the Pull Request.

## 7. Keep your branch updated

Before starting new work, update your local `main`:

```bash
git checkout main
git pull origin main
```

Then create a new branch:

```bash
git checkout -b feature/<your-feature>
```

If you are already working on a branch and `main` has received important changes, coordinate before merging `main` into your branch.

## Important Rules

* **Never push directly to `main`.**
* One feature/task = one branch.
* Use descriptive branch names.
* Keep your changes focused on your assigned module.
* Don't modify another team's module unnecessarily.
* Don't commit `.env` secrets or production credentials.
* Database/schema changes must be communicated to the team before merging.


---

# Important Principle

The goal of this architecture is:

```text
8 teams
   ↓
8 independent feature modules
   ↓
shared infrastructure
   ↓
shared Prisma/database
   ↓
single Express application
```

Each team owns its feature, while the whole class maintains one consistent architecture and database.
