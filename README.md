
# ClientFlow

### Manage Clients. Manage Projects. Grow Your Business.

ClientFlow is a multi-tenant SaaS business management platform designed for freelancers and small digital agencies.

It provides a centralised workspace to manage clients, projects, tasks, quotations, invoices, payments, expenses and business activities.

## Project Goals

- Simplify client and project management.
- Organise business operations in one platform.
- Manage quotations, invoices and payments.
- Support multiple businesses through isolated workspaces.
- Provide role-based access control.
- Offer useful business insights through dashboards and reports.

## Target Users

- Freelancers
- Independent professionals
- Small digital agencies
- Growing service-based businesses

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js, React, TypeScript |
| Styling | Tailwind CSS, shadcn/ui |
| Backend | Node.js, Express.js, TypeScript |
| Database | PostgreSQL |
| ORM | Prisma |
| Authentication | Better Auth |
| Validation | Zod |
| Charts | Recharts |
| Testing | Vitest, Supertest, Playwright |

## Repository Structure

```text
clientflow/
├── frontend/
├── backend/
├── docs/
│   ├── diagrams/
│   ├── decisions/
│   ├── SRS.md
│   ├── ARCHITECTURE.md
│   ├── DATABASE_DESIGN.md
│   ├── API_DOCUMENTATION.md
│   └── PROJECT_ROADMAP.md
├── README.md
└── .gitignore
```

## Architecture

ClientFlow follows a monorepo structure with a separate frontend and backend.

The backend will use a modular monolith architecture with a shared PostgreSQL database and workspace-level tenant isolation.

## Currency Support

Each workspace can configure its preferred currency. LKR is the default currency. Other supported currencies may include USD and EUR.

Financial documents retain their currency information when created.

## Development Status

<badge color="info">Planning and foundation phase</badge>

The project is currently in its planning and architecture phase. Application development will begin after the foundational documentation and database design have been reviewed.

## Documentation

Detailed project documentation is available in the `docs/` directory.

## License

MIT License.
