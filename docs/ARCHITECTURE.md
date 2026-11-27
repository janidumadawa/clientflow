
# ClientFlow - System Architecture

## 1. Architecture Overview

ClientFlow follows a monorepo architecture with independently organised frontend and backend applications.

The backend follows a modular monolith architecture. Business modules are separated logically while operating within a single backend application.

## 2. High-Level Architecture

```text
                CLIENT USERS
                     |
                     v
             NEXT.JS FRONTEND
                     |
                REST API
                     |
                     v
             EXPRESS BACKEND
                     |
       +-------------+-------------+
       |             |             |
       v             v             v
  Authentication   Business     File Storage
                   Modules
                     |
       +-------------+-------------+
       |             |             |
       v             v             v
   PostgreSQL    Prisma ORM    External Services
```

## 3. Technology Stack

### Frontend
- Next.js
- React
- TypeScript
- Tailwind CSS
- shadcn/ui
- Recharts

### Backend
- Node.js
- Express.js
- TypeScript
- Zod
- Prisma ORM

### Database
- PostgreSQL
- Shared database and shared schema
- Workspace-level tenant isolation

### Authentication
- Better Auth
- Session-based authentication
- Role-based access control

### Infrastructure
- Frontend: Vercel
- Backend: Render
- Database: Neon PostgreSQL
- File storage: Cloudinary or S3-compatible storage

## 4. Backend Modules

The backend will be organised into the following modules:

1. Authentication
2. Workspaces
3. Memberships and Permissions
4. Clients
5. Services
6. Projects
7. Tasks
8. Quotations
9. Invoices
10. Payments
11. Expenses
12. Notifications
13. Attachments
14. Reports and Analytics
15. Audit Logs

## 5. Multi-Tenant Architecture

ClientFlow supports multiple independent workspaces.

Each workspace has its own:
- Clients
- Projects
- Team members
- Tasks
- Quotations
- Invoices
- Payments
- Expenses
- Settings

Tenant-owned database records must be associated with a workspace.

The application must verify workspace membership and permissions before accessing tenant-owned resources.

PostgreSQL Row-Level Security (RLS) will provide an additional tenant-isolation layer.

## 6. Role-Based Access Control

Supported workspace roles:

- OWNER
- ADMIN
- PROJECT_MANAGER
- ACCOUNTANT
- TEAM_MEMBER

A user may belong to multiple workspaces and hold different roles in each workspace.

## 7. API Design

The backend will expose REST APIs under:

`/api/v1`

API requests and responses will use JSON.

## 8. Security Considerations

- Validate incoming requests.
- Enforce authentication and authorisation.
- Verify workspace ownership and membership.
- Prevent cross-tenant data access.
- Store passwords and authentication data securely.
- Protect environment variables and credentials.
- Apply appropriate rate limiting.
- Record important business activities in audit logs.

## 9. Architecture Decisions

The architecture is a planning baseline. Detailed implementation decisions, package versions and deployment configurations will be confirmed before implementation.
