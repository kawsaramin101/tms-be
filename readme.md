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

## 1. Clone the repository

```bash
git clone https://github.com/kawsaramin101/tms-be.git
cd tms-be
```

## 2. Install dependencies

```bash
npm install
```

## 3. Configure the database

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

The database connection should be configured in `.env`:

```env
DATABASE_URL="mysql://tms:hello_world@localhost:3306/tms"
```

Do not commit real production credentials.

## 4. Generate Prisma Client

```bash
npx prisma generate
```

## 5. Run database migrations

```bash
npx prisma migrate dev
```

If you are creating the first migration:

```bash
npx prisma migrate dev --name init
```

## 6. Start the development server

```bash
npm run dev
```

The development server uses `tsx` and automatically reloads when source files change.

---

# Project Structure

```text
tms-be/
│
├── prisma/
│   ├── schema.prisma
│   └── migrations/
│
├── src/
│   │
│   ├── modules/
│   │   ├── auth/
│   │   ├── transactions/
│   │   ├── vouchers/
│   │   ├── calculations/
│   │   ├── members/
│   │   ├── fees/
│   │   ├── reports/
│   │   └── notifications/
│   │
│   ├── middleware/
│   ├── lib/
│   ├── config/
│   │
│   ├── app.ts
│   └── server.ts
│
├── uploads/
│   └── vouchers/
│
├── tests/
│
├── .env
├── .gitignore
├── package.json
├── package-lock.json
├── prisma.config.ts
└── tsconfig.json
```

---

# Team Modules

There are 8 feature teams.

| Team   | Module          | Responsibility                        |
| ------ | --------------- | ------------------------------------- |
| Team 1 | `auth`          | Authentication & access control       |
| Team 2 | `transactions`  | Income and expense transactions       |
| Team 3 | `vouchers`      | Voucher creation, tracking and photos |
| Team 4 | `calculations`  | Automatic calculations and balances   |
| Team 5 | `members`       | Member management                     |
| Team 6 | `fees`          | Member fee management                 |
| Team 7 | `reports`       | Reports and financial status          |
| Team 8 | `notifications` | Notifications and reminders           |

Each team should primarily work inside its own module.

---

# Module Structure

Each feature module follows the same basic structure.

Example:

```text
src/modules/members/
├── member.controller.ts
├── member.service.ts
├── member.routes.ts
├── member.validation.ts
└── member.types.ts
```

## Controller

```text
member.controller.ts
```

Handles HTTP requests and responses.

The controller should:

* Read request data
* Call the appropriate service
* Return the HTTP response
* Handle request/response-related logic

Business logic should not normally live here.

## Service

```text
member.service.ts
```

Contains the actual business logic.

For example:

* Create member
* Update member
* Find member
* Delete member
* Calculate member-related information

Database operations should normally be called from the service layer.

## Routes

```text
member.routes.ts
```

Defines the API endpoints for the module.

Example:

```text
GET    /members
GET    /members/:id
POST   /members
PATCH  /members/:id
DELETE /members/:id
```

Routes should connect HTTP endpoints to controllers.

## Validation

```text
member.validation.ts
```

Contains request validation rules.

For example:

* Required fields
* Data types
* Valid formats
* Business input constraints

## Types

```text
member.types.ts
```

Contains TypeScript types/interfaces that are specific to the module.

---

# Shared Folders

These folders are shared by all teams.

## `src/middleware/`

Contains Express middleware used across the application.

```text
src/middleware/
├── auth.middleware.ts
├── error.middleware.ts
└── upload.middleware.ts
```

Examples:

* Authentication checks
* Authorization checks
* Error handling
* File upload handling

Teams should reuse existing middleware instead of creating duplicate middleware inside their modules.

---

# `src/lib/`

Contains shared libraries and infrastructure.

```text
src/lib/
├── prisma.ts
├── jwt.ts
└── logger.ts
```

Examples:

* Prisma client
* JWT utilities
* Logging

Do not create another Prisma client inside your feature module.

Use the shared Prisma client.

---

# `src/config/`

Application configuration.

```text
src/config/
└── env.ts
```

Environment variables and application configuration should be handled here.

---

# `src/app.ts`

Creates and configures the Express application.

This is where application-level configuration belongs, such as:

* Express initialization
* Middleware registration
* API route registration
* Error middleware

Feature business logic does **not** belong here.

---

# `src/server.ts`

Application entry point.

It starts the HTTP server.

Keep server startup logic here rather than putting business logic into this file.

---

# Prisma

## `prisma/schema.prisma`

This is the central database schema.

All database models are defined here.

For example:

```prisma
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  createdAt DateTime @default(now())
}
```

### Important

Because all 8 teams use the same database, **do not independently redesign or duplicate database models** inside individual modules.

If your feature requires a database change:

1. Discuss the model with the team responsible for the database.
2. Update `schema.prisma`.
3. Create a Prisma migration.
4. Test the migration.
5. Commit the migration with the schema change.

## `prisma/migrations/`

Contains generated database migrations.

Do not manually edit an already-applied migration unless the team has specifically agreed to do so.

---

# Uploads

```text
uploads/
└── vouchers/
```

Voucher images/files are stored here during development.

The vouchers team owns the voucher upload functionality, while the shared upload middleware handles common upload behavior.

---

# Tests

```text
tests/
```

Contains automated tests for the application.

Feature-specific tests should be organized clearly so another developer can identify which module they belong to.

---

# Environment Variables

Development configuration belongs in `.env`.

Example:

```env
DATABASE_URL="mysql://tms:hello_world@localhost:3306/tms"
```

Never hard-code passwords, tokens, JWT secrets, or other credentials inside TypeScript source files.

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

Copy this into your LLM:

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

# Git Workflow

Before starting work:

```bash
git pull origin main
```

Create a branch for your work:

```bash
git checkout -b feature/<your-feature>
```

Example:

```bash
git checkout -b feature/member-management
```

After making changes:

```bash
git status
git add .
git commit -m "feat: add member management"
git push -u origin feature/member-management
```

Then create a Pull Request on GitHub.

---

# Commit Message Format

Use simple conventional commit messages.

Examples:

```text
feat: add member creation
feat: add voucher upload
fix: validate transaction amount
fix: handle invalid login
refactor: simplify transaction service
docs: update API documentation
```

---

# Before Opening a Pull Request

Run:

```bash
npm run build
```

Then verify that:

```bash
npx prisma generate
```

works correctly.

If your change includes a database schema change, also verify the Prisma migration.

Make sure you have not accidentally modified another team's module.

---

# Team Boundaries

The intended ownership is:

```text
Team 1 → src/modules/auth/
Team 2 → src/modules/transactions/
Team 3 → src/modules/vouchers/
Team 4 → src/modules/calculations/
Team 5 → src/modules/members/
Team 6 → src/modules/fees/
Team 7 → src/modules/reports/
Team 8 → src/modules/notifications/
```

Shared infrastructure:

```text
src/middleware/
src/lib/
src/config/
src/app.ts
src/server.ts
prisma/
```

Changes to shared infrastructure should be discussed with the other teams before merging.

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

Each team should own its feature, while the whole class maintains one consistent architecture and database.
