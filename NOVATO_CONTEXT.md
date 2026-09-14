# NOVATO — PROJECT CONTEXT

## 1. Project Identity

Novato is a reusable web-first SaaS platform for groups, associations, clubs, cooperatives, and organizations to manage:

- Funds
- Member contributions
- Contribution schedules
- Money-in / money-out transactions
- Expenses
- Financial accountability
- Meetings
- Attendance
- Decisions
- Action items
- Reports
- Organization governance
- Transparent progress dashboards

Novato must be designed as a reusable multi-organization SaaS, not as a one-off application for a single group.

Pilot organization:

**Trail Blazers**

Pilot currency:

**NGN**

---

## 2. Core Architecture

The fundamental architecture is:

User
→ Organization Membership
→ Role
→ Permissions
→ Organization Resources

Every organization-owned resource must be protected by organization membership and Supabase Row Level Security (RLS).

Backend financial data is the source of truth.

The browser/frontend must NOT become the financial source of truth.

Do not use localStorage for financial records.

Do not maintain two competing financial data stores.

---

## 3. Technology

Current stack:

- HTML/CSS/JavaScript frontend
- Supabase
- PostgreSQL
- Supabase Auth
- Supabase RLS
- Supabase RPC/database functions
- GitHub repository
- Browser-based deployment/testing
- AI coding agents may be used for implementation

Repository:

`cinobentech-ai/novato-phone`

Primary branch:

`main`

GitHub is the source of truth for application code, documentation, migrations, tests, and project instructions.

Supabase is the source of truth for live application data.

Do NOT move live Trail Blazers financial records into GitHub.

---

## 4. Current Database

The Supabase public schema currently contains at least these tables:

- audit_logs
- contribution_schedules
- fund_balances
- funds
- invitations
- org_balances
- organization_members
- organizations
- profiles
- transactions

The `organizations` table contains the Trail Blazers organization.

There are also existing database functions/triggers for:

- organization creation
- role/permission checks
- posting transactions
- reversals
- balance handling
- audit logging
- triggers and integrity protection

IMPORTANT:

Do NOT recreate existing tables or database functions simply because they are not visible in the SQL Editor.

The live database already exists.

Before changing database structure, inspect the existing structure first.

---

## 5. Roles

Current organization roles:

- Owner
- Admin
- Treasurer
- Secretary
- Member
- Viewer

Permissions must be enforced server-side through RLS/database functions.

Never rely only on hiding buttons in the frontend for security.

---

# ROADMAP

## v0.2 MVP

Status: COMPLETE

Basic product foundation established.

---

## Sprint 0 — Audit

Status: COMPLETE

Existing implementation and database reviewed.

---

## Sprint 1 — Organization Foundation

Status: BUILT / NEARLY COMPLETE

Scope:

- Authentication
- Profiles
- Organizations
- Organization memberships
- Invitations
- Roles
- RLS foundation

No production financial transaction functionality should be introduced as part of Sprint 1.

---

## Sprint 2 — Financial Engine

Status: BUILT / APPROXIMATELY 90% COMPLETE

Scope:

- Funds
- Contribution schedules
- Transactions
- Ledger
- Fund balances
- Organization balances
- Backend financial logic
- RLS
- Financial integrity
- Reversal/correction mechanism
- Audit logging
- Backend/frontend connection
- Testing

Current remaining work is primarily live authentication/session verification and frontend/backend integration verification.

DO NOT start Sprint 3 until Sprint 2 is verified.

---

## Sprint 3 — Financial Accountability

NOT STARTED.

Planned scope:

- Expenses
- Expense records
- Reversals
- Approval workflows
- Expanded audit trail
- Financial accountability controls

Do not implement Sprint 3 unless explicitly authorized.

---

## Sprint 4 — Governance

NOT STARTED.

Planned scope:

- Meetings
- Meeting scheduling
- Attendance
- Meeting records
- Decisions
- Action items
- Governance history

Do not implement Sprint 4 unless explicitly authorized.

---

## Pilot

NOT STARTED.

Pilot target:

Approximately 82 Trail Blazers members.

Pilot begins only after the core financial and governance systems are sufficiently tested.

---

# CURRENT AUTHENTICATION WORK

The current frontend is `index.html`.

Supabase JavaScript v2 is being used.

Authentication uses Supabase magic-link authentication.

The application should use:

- `detectSessionInUrl: true`
- `persistSession: true`
- `autoRefreshToken: true`

Authentication should be handled through:

`supabase.auth.onAuthStateChange()`

The application must correctly process the session after a magic-link redirect.

The URL should be cleaned after authentication so access tokens are not left visible in the browser address bar.

The magic-link redirect should use the clean application origin/path rather than carrying an old authentication fragment.

Current redirect approach:

`window.location.origin + window.location.pathname`

Never expose access tokens, refresh tokens, service-role keys, passwords, or other credentials in GitHub.

---

# CURRENT AUTH BUG / UI ISSUE

The authentication callback was previously producing a URL containing:

- access_token
- refresh_token

Those tokens must be treated as exposed.

Never copy, reuse, log, commit, or display authentication tokens.

Request a fresh magic link when testing authentication.

The current application can reach the authenticated state.

The page currently shows:

- Novato
- Step 1 — Connect project
- Step 2 — Email login
- Step 3 — You are in — Create organization

The authenticated application section appears, but Steps 1 and 2 may remain visible after successful login.

Expected authenticated behavior:

- Hide the project setup box.
- Hide the email authentication box.
- Show the authenticated application.
- Display the authenticated user's email.
- Load organization membership.
- Allow organization creation only when appropriate.

Expected signed-out behavior:

- Do not display the authenticated application.
- Display the appropriate connection/authentication interface.

---

# CURRENT FRONTEND AUTH STRUCTURE

The frontend currently contains functions similar to:

- `client()`
- `cleanAuthUrl()`
- `loadApp(session)`
- `refresh()`
- `initAuth()`

There are HTML inputs with these IDs:

- `url`
- `key`

IMPORTANT:

Do not change them to `sb-url` or `sb-key` unless the HTML itself is deliberately changed.

The current JavaScript save handler should use:

```javascript
const url = $("url").value.trim();
const key = $("key").value.trim();