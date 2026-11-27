# ClientFlow
## Software Requirements Specification (SRS)

| Document field | Value |
|---|---|
| Product | ClientFlow |
| Document | Software Requirements Specification |
| Version | 1.0 |
| Status | Approved baseline |
| Last updated | 27 November 2026 |
| Product type | Multi-tenant SaaS business management platform |
| Primary audience | Freelancers and small digital agencies |

---

## 1. Introduction

### 1.1 Purpose

This document defines the functional and non-functional requirements for ClientFlow. It establishes the agreed product scope, user roles, business rules, workflows, constraints and acceptance conditions. It will guide system design, implementation and testing.

### 1.2 Product overview

ClientFlow is a web-based, multi-tenant SaaS platform for freelancers and small digital agencies. It brings client management, project delivery and business administration into one workspace.

A user may create or join multiple workspaces. Each workspace maintains its own business data, members, roles, settings and financial records. A freelancer may operate a workspace independently, while an agency may invite team members and assign responsibilities.

**Product tagline:** Manage Clients. Manage Projects. Grow Your Business.

### 1.3 Objectives

- Centralise client and business information.
- Organise projects, tasks and team responsibilities.
- Prepare and manage quotations and invoices.
- Record payments and business expenses.
- Provide useful operational and financial summaries.
- Support multiple businesses through isolated workspaces.
- Provide access according to workspace roles and permissions.
- Offer a clear, responsive and approachable user experience.

### 1.4 Intended audience

This specification is intended for the product owner, developers, designers, testers and other project stakeholders.

### 1.5 Definitions

| Term | Meaning |
|---|---|
| User | A person with a ClientFlow account |
| Workspace | An isolated business environment within ClientFlow |
| Membership | A user's association with a workspace and assigned role |
| Tenant | A workspace and its associated business data |
| Client | A customer or organisation served by a workspace |
| Service | A reusable service or product offered by a workspace |
| Quotation | A commercial offer sent to a client |
| Invoice | A request for payment for supplied or agreed work |
| Payment | A recorded receipt against an invoice |
| RBAC | Role-Based Access Control |
| RLS | PostgreSQL Row-Level Security |

---

## 2. Product Scope

### 2.1 In scope

ClientFlow v1 includes:

1. Account registration, authentication and session management.
2. Workspace creation, settings and switching.
3. Workspace memberships, invitations and role-based access.
4. Client relationship management.
5. Services and products catalogue.
6. Project management and project members.
7. Task management, assignment and comments.
8. Quotation creation and management.
9. Invoice creation and management.
10. Payment recording and invoice payment tracking.
11. Expense categories and expense tracking.
12. Dashboard summaries and business analytics.
13. In-app notifications.
14. Reports and data exports.
15. File attachments.
16. Audit logs for important business activities.

### 2.2 Out of scope for the initial release

The following are not part of the initial agreed scope unless approved through a later requirements change:

- Native Android or iOS applications.
- Full payroll and employee attendance management.
- Full accounting, tax filing or statutory financial reporting.
- Inventory, warehouse and procurement management.
- Automated bank reconciliation.
- A public client self-service portal.
- Automated payment-gateway collection.
- AI-based business recommendations.
- Multi-currency conversion and exchange-rate automation.

The workspace currency is configurable. Financial documents retain their own currency snapshot. This does not imply automatic currency conversion.

### 2.3 Product assumptions

- Users have access to a modern web browser and an internet connection.
- Each workspace represents one business or independent operating environment.
- A user can belong to more than one workspace.
- A workspace has one configured default currency.
- LKR is the default currency for newly created workspaces.
- Workspace owners are responsible for inviting members and assigning appropriate roles.
- Financial and business information is entered and maintained by authorised workspace users.
- The platform is initially designed for freelancers and small digital agencies.

### 2.4 Constraints

- All tenant-owned records must be associated with a workspace.
- Access to tenant-owned records must be checked against the active workspace membership.
- Financial values must use precise decimal storage rather than floating-point types.
- Historical financial documents must preserve their currency and relevant line-item details.
- Important business actions must be traceable.
- Application secrets and credentials must not be committed to source control.

---

## 3. User Classes and Roles

### 3.1 User classes

**Freelancer:** Operates a workspace independently and manages clients, work and finances.

**Workspace owner:** Owns a workspace, controls its settings and has full authority over workspace membership and administration.

**Agency administrator:** Helps manage workspace operations and team access.

**Project manager:** Coordinates projects, members and tasks within the permissions granted by the workspace.

**Accountant:** Manages quotations, invoices, payments and expenses within the permissions granted.

**Team member:** Works on assigned projects and tasks and accesses permitted workspace information.

### 3.2 Workspace roles

ClientFlow defines the following workspace roles:

- OWNER
- ADMIN
- PROJECT_MANAGER
- ACCOUNTANT
- TEAM_MEMBER

A user may have a different role in each workspace.

### 3.3 Permission principles

| Capability | OWNER | ADMIN | PROJECT_MANAGER | ACCOUNTANT | TEAM_MEMBER |
|---|---:|---:|---:|---:|---:|
| Manage workspace settings | Yes | Yes, subject to restrictions | No | No | No |
| Manage members and invitations | Yes | Yes, subject to restrictions | No | No | No |
| Manage clients | Yes | Yes | Yes, as permitted | View as permitted | View assigned/relevant |
| Manage services | Yes | Yes | View | View | View |
| Manage projects | Yes | Yes | Yes | View | View assigned |
| Manage tasks | Yes | Yes | Yes | View | Assigned tasks |
| Manage quotations | Yes | Yes | As permitted | Yes | No |
| Manage invoices | Yes | Yes | No | Yes | No |
| Record payments | Yes | Yes | No | Yes | No |
| Manage expenses | Yes | Yes | No | Yes | No |
| View financial reports | Yes | Yes | Restricted | Yes | No |
| View audit logs | Yes | Yes, as permitted | No | No | No |

This is the initial permission baseline. Fine-grained permissions and exceptional cases must be enforced by the backend, not merely hidden in the user interface. The owner role must not be removable through ordinary member-management actions.

---

## 4. Functional Requirements

Requirements are grouped by module. Each requirement has a unique identifier for traceability.

### 4.1 Authentication and account management

- **FR-001:** The system shall allow a new user to register an account using the supported registration method.
- **FR-002:** The system shall allow registered users to authenticate securely.
- **FR-003:** The system shall allow users to end their active session by signing out.
- **FR-004:** The system shall provide the supported account verification and password recovery flows.
- **FR-005:** The system shall allow users to view and update permitted profile information.
- **FR-006:** The system shall maintain authenticated sessions and prevent unauthenticated access to protected resources.

### 4.2 Workspace management

- **FR-007:** The system shall allow an eligible user to create a workspace.
- **FR-008:** The system shall create the workspace owner membership when a workspace is created.
- **FR-009:** The system shall allow a user to view the workspaces to which they belong.
- **FR-010:** The system shall allow a user to switch between their authorised workspaces.
- **FR-011:** The system shall allow authorised users to update workspace profile and business settings.
- **FR-012:** The system shall allow a workspace to configure its default currency, with LKR as the initial default.
- **FR-013:** The system shall prevent users from accessing workspace data unless they have valid membership and permission.

### 4.3 Memberships, invitations and access control

- **FR-014:** The system shall allow authorised workspace users to invite people to join their workspace.
- **FR-015:** The system shall support invitation acceptance and rejection or expiry handling.
- **FR-016:** The system shall associate each accepted workspace member with a role.
- **FR-017:** The system shall allow authorised users to change a member's role, subject to owner-protection rules.
- **FR-018:** The system shall allow authorised users to remove a member from a workspace.
- **FR-019:** The system shall enforce role-based permissions for protected operations.
- **FR-020:** The system shall prevent a user from gaining access to another workspace by changing an identifier or request parameter.

### 4.4 Client relationship management

- **FR-021:** The system shall allow authorised users to create client records.
- **FR-022:** The system shall allow authorised users to view a client's details.
- **FR-023:** The system shall allow authorised users to update client information.
- **FR-024:** The system shall allow authorised users to archive or deactivate a client without breaking historical business records.
- **FR-025:** The system shall allow users to search and filter clients using supported client fields.
- **FR-026:** The system shall allow users to view relevant projects and financial documents associated with a client.
- **FR-027:** The system shall allow authorised users to add and view internal notes associated with a client.

### 4.5 Services and products catalogue

- **FR-028:** The system shall allow authorised users to create service or product catalogue entries.
- **FR-029:** The system shall allow authorised users to view catalogue entries.
- **FR-030:** The system shall allow authorised users to update catalogue entry details and default pricing.
- **FR-031:** The system shall allow authorised users to deactivate catalogue entries that are no longer offered.
- **FR-032:** The system shall allow quotation and invoice line items to refer to catalogue entries where applicable.
- **FR-033:** The system shall preserve historical descriptions, quantities, rates and amounts on issued financial documents even if a catalogue entry later changes.

### 4.6 Project management

- **FR-034:** The system shall allow authorised users to create projects for a workspace client.
- **FR-035:** The system shall allow authorised users to view project details and status.
- **FR-036:** The system shall allow authorised users to update project information and status.
- **FR-037:** The system shall allow authorised users to archive or complete projects.
- **FR-038:** The system shall allow authorised users to assign workspace members to projects.
- **FR-039:** The system shall allow users to view projects according to their workspace permissions and project involvement.
- **FR-040:** The system shall allow users to search and filter projects using supported fields such as client, status and date.

### 4.7 Task management

- **FR-041:** The system shall allow authorised users to create tasks within a project.
- **FR-042:** The system shall allow authorised users to update task details.
- **FR-043:** The system shall allow authorised users to assign a task to an eligible workspace member.
- **FR-044:** The system shall allow permitted users to update task status.
- **FR-045:** The system shall support task priority and due-date information.
- **FR-046:** The system shall allow permitted users to view tasks associated with a project or assigned member.
- **FR-047:** The system shall allow permitted users to add and view task comments.
- **FR-048:** The system shall allow users to filter tasks by supported attributes such as status, priority, assignee and due date.

### 4.8 Quotations

- **FR-049:** The system shall allow authorised users to create a quotation for a workspace client.
- **FR-050:** The system shall allow users to add, update and remove quotation line items while the quotation is editable.
- **FR-051:** The system shall calculate quotation subtotals, applicable adjustments and totals using decimal-safe arithmetic.
- **FR-052:** The system shall assign a unique quotation number within the workspace.
- **FR-053:** The system shall support quotation status tracking, including draft, sent, accepted, rejected and expired states as applicable.
- **FR-054:** The system shall allow authorised users to view and update quotations according to their status and permissions.
- **FR-055:** The system shall preserve the quotation's currency and line-item snapshots.
- **FR-056:** The system shall support conversion of an eligible quotation into an invoice without losing its relevant commercial details.

### 4.9 Invoices

- **FR-057:** The system shall allow authorised users to create an invoice for a workspace client.
- **FR-058:** The system shall allow users to add, update and remove invoice line items while the invoice is editable.
- **FR-059:** The system shall calculate invoice totals using decimal-safe arithmetic.
- **FR-060:** The system shall assign a unique invoice number within the workspace.
- **FR-061:** The system shall support invoice lifecycle states, including draft, issued, partially paid, paid, overdue and cancelled as applicable.
- **FR-062:** The system shall store issue date, due date, client details and line-item snapshots on the invoice.
- **FR-063:** The system shall preserve the invoice currency and relevant financial values after issue.
- **FR-064:** The system shall allow authorised users to view, issue, update or cancel invoices according to invoice state and business rules.
- **FR-065:** The system shall provide a printable or downloadable invoice representation.

### 4.10 Payments

- **FR-066:** The system shall allow authorised users to record a payment against an invoice.
- **FR-067:** The system shall store payment amount, payment date and supported payment reference or method information.
- **FR-068:** The system shall update the invoice's paid and outstanding amounts based on valid recorded payments.
- **FR-069:** The system shall reflect partial and full payment states on the associated invoice.
- **FR-070:** The system shall preserve confirmed payment records and use a reversal or adjustment process to correct them rather than silently overwriting financial history.

### 4.11 Expenses

- **FR-071:** The system shall allow authorised users to create and manage workspace expense categories.
- **FR-072:** The system shall allow authorised users to record an expense with its description, amount, date and category.
- **FR-073:** The system shall allow authorised users to update or archive expense records according to applicable business rules.
- **FR-074:** The system shall support expense records in the workspace's configured currency and retain the currency used for each record.
- **FR-075:** The system shall allow permitted users to view and filter expenses by supported fields such as date and category.

### 4.12 Dashboard and analytics

- **FR-076:** The system shall provide a workspace dashboard for authorised users.
- **FR-077:** The dashboard shall display relevant summaries of clients, projects and tasks.
- **FR-078:** The dashboard shall display relevant financial summaries, including invoice and payment information, according to the user's permissions.
- **FR-079:** The system shall provide date-based summaries or trends for supported business information.

### 4.13 Notifications

- **FR-080:** The system shall create in-app notifications for supported workspace events.
- **FR-081:** The system shall allow a user to view notifications and mark them as read.
- **FR-082:** The system shall ensure that notifications are visible only to their intended recipient and within the relevant authorised workspace context.

### 4.14 Reports, attachments and audit logs

- **FR-083:** The system shall provide permitted users with access to supported business reports.
- **FR-084:** The system shall allow authorised users to export supported report data in an agreed format.
- **FR-085:** The system shall allow authorised users to upload and associate attachments with supported workspace resources.
- **FR-086:** The system shall allow permitted users to view and download attachments only when they have access to the associated resource.
- **FR-087:** The system shall record important workspace and financial actions in an audit log.
- **FR-088:** Audit records shall identify the action, actor where known, affected resource and timestamp.
- **FR-089:** The system shall restrict audit-log access to authorised roles.
- **FR-090:** The system shall preserve tenant boundaries across reports, exports, attachments and audit records.

---

## 5. Non-Functional Requirements

- **NFR-001 - Performance:** Common user-facing operations should complete within an acceptable response time under normal operating conditions. Performance targets will be measured and refined during testing.
- **NFR-002 - Scalability:** The system architecture shall support growth in users, workspaces and business records without requiring a complete architectural redesign.
- **NFR-003 - Security:** Authentication, authorisation, input validation and secure session handling shall be applied to protected operations.
- **NFR-004 - Tenant isolation:** A user shall never access another workspace's data without valid membership and permission. Tenant isolation shall be enforced in application logic and strengthened through database controls.
- **NFR-005 - Data integrity:** Database constraints and transactional operations shall protect relationships and financial consistency.
- **NFR-006 - Availability:** The deployed service should remain available during normal operating periods, with infrastructure and recovery targets defined before production release.
- **NFR-007 - Usability:** The interface shall use clear navigation, consistent terminology and predictable interaction patterns for non-technical business users.
- **NFR-008 - Responsiveness:** Core workflows shall be usable on desktop, tablet and mobile-sized browser screens.
- **NFR-009 - Accessibility:** The interface should follow recognised accessibility practices for keyboard navigation, labels, contrast and semantic structure.
- **NFR-010 - Maintainability:** The codebase shall use modular organisation, consistent conventions and documented interfaces.
- **NFR-011 - Testability:** Business rules and critical workflows shall be testable through automated unit, integration and end-to-end tests.
- **NFR-012 - Backup and recovery:** Production data shall be backed up using the selected database provider's supported backup and recovery capabilities. Recovery procedures shall be documented.
- **NFR-013 - Auditability:** Important actions affecting workspace access and financial records shall be traceable.
- **NFR-014 - Compatibility:** The application shall support current major versions of commonly used modern browsers.
- **NFR-015 - Data privacy:** The system shall limit personal and business data access to authorised purposes and users.
- **NFR-016 - Reliability:** Financial calculations, document numbering and payment updates shall behave consistently under concurrent requests and recover safely from failed transactions.

---

## 6. Business Rules

### 6.1 Workspace and membership rules

- **BR-001:** Every workspace is an independent tenant boundary.
- **BR-002:** A user may belong to multiple workspaces.
- **BR-003:** A user's role is assigned per workspace and may differ between workspaces.
- **BR-004:** A workspace must have an owner membership.
- **BR-005:** Ordinary member-management operations must not remove or demote the only owner.
- **BR-006:** Every tenant-owned record must be associated with the workspace that owns it.
- **BR-007:** The backend must derive and verify workspace access from authenticated membership. A client-supplied workspace identifier alone is not proof of access.

### 6.2 Client and project rules

- **BR-008:** A project belongs to one workspace and is associated with a client in that same workspace.
- **BR-009:** Project members and task assignees must be valid members of the relevant workspace.
- **BR-010:** Archiving a client or project must not silently destroy historical financial records or audit history.

### 6.3 Financial rules

- **BR-011:** Monetary values shall use a precise decimal representation.
- **BR-012:** Each workspace has a configurable default currency. New workspaces default to LKR.
- **BR-013:** A quotation, invoice or expense record shall retain the currency applicable when it was created. Changing the workspace default currency must not rewrite historical records.
- **BR-014:** Issued financial documents shall preserve their descriptions, quantities, rates, adjustments, totals and currency snapshots.
- **BR-015:** Quotation and invoice numbers shall be unique within their workspace.
- **BR-016:** Document number generation shall be safe under concurrent requests and shall not rely on a non-transactional read-then-increment sequence.
- **BR-017:** A quotation may be converted into an invoice only when it is in an eligible state.
- **BR-018:** A quotation conversion shall not create duplicate invoices for the same conversion.
- **BR-019:** Confirmed payment records shall not be silently edited or deleted. Corrections shall use a traceable reversal or adjustment.
- **BR-020:** Invoice paid and outstanding amounts shall be derived consistently from valid payment records and invoice totals.
- **BR-021:** Invoice status shall reflect its lifecycle and payment position. An unpaid invoice past its due date may be classified as overdue.
- **BR-022:** Financial records shall not be hard-deleted when doing so would break auditability or historical reporting.
- **BR-023:** Service catalogue changes shall not alter historical line-item snapshots.

### 6.4 Security and audit rules

- **BR-024:** Every protected operation shall enforce authentication, workspace membership and role permissions.
- **BR-025:** Cross-workspace relationships shall be rejected.
- **BR-026:** Sensitive actions, including role changes and important financial actions, shall be auditable.
- **BR-027:** File access shall be authorised against the workspace and resource to which the file is associated.

---

## 7. Main User Workflows

### 7.1 New user and workspace setup

1. A person registers and verifies their account through the supported flow.
2. The user creates a workspace.
3. The system creates the workspace and its owner membership.
4. The owner enters business details and selects the workspace currency.
5. The owner can begin adding clients and business records.

### 7.2 Inviting a team member

1. An authorised user opens workspace member management.
2. The user sends an invitation.
3. The invited person receives the invitation through the supported delivery method.
4. The person accepts the invitation.
5. The system creates the membership with the assigned role.
6. The new member can access only the workspace resources allowed by that role.

### 7.3 Managing a client project

1. An authorised user creates or selects a client.
2. The user creates a project associated with that client.
3. The project manager assigns eligible workspace members.
4. Tasks are created and assigned.
5. Members update task progress and add comments.
6. Authorised users review project status and completion.

### 7.4 Quotation to invoice

1. An authorised user selects a client.
2. The user creates a quotation and adds line items.
3. The system calculates and stores the quotation totals and currency snapshot.
4. The quotation is sent and its status is updated as appropriate.
5. When eligible, the quotation is converted into an invoice.
6. The invoice receives a unique workspace-scoped number and preserves the relevant commercial details.

### 7.5 Invoice payment

1. An authorised user opens an issued invoice.
2. The user records a payment.
3. The system validates the payment and updates the invoice's paid and outstanding amounts transactionally.
4. The invoice reflects partial or full payment as appropriate.
5. The payment action is recorded for audit purposes.

### 7.6 Expense tracking

1. An authorised user selects or creates an expense category.
2. The user records an expense with amount, date and description.
3. The system stores the expense with its currency and workspace association.
4. Permitted users can review and filter expenses and view them in supported summaries.

---

## 8. Data and Integration Requirements

### 8.1 Data management

- PostgreSQL shall be the primary relational database.
- Prisma shall be used as the planned ORM and database access layer.
- UUIDs shall be used for application entity identifiers unless a specific integration requires otherwise.
- Monetary values shall use decimal database types.
- Timestamps shall be stored consistently in UTC.
- Calendar-only dates, such as invoice due dates, shall use date semantics rather than being treated as arbitrary timestamps.
- Tenant-owned records shall carry workspace ownership and use tenant-aware relationship constraints where appropriate.

### 8.2 External services

The implementation may use the following services, subject to final configuration:

- Better Auth for authentication.
- Cloudinary or S3-compatible storage for file attachments.
- Vercel for frontend hosting.
- Render for backend hosting.
- Neon for managed PostgreSQL hosting.

The exact service configuration and versions are implementation decisions and must be documented when selected.

---

## 9. Acceptance Criteria

The initial product baseline is considered functionally acceptable when the following conditions are demonstrated:

- [ ] A user can register, authenticate and sign out.
- [ ] A user can create a workspace and becomes its owner.
- [ ] A user can belong to multiple workspaces and switch between them.
- [ ] Workspace data is isolated and cross-tenant access attempts are rejected.
- [ ] Workspace roles are enforced by backend authorisation.
- [ ] Authorised users can create, update, search and archive client records.
- [ ] Users can manage service catalogue entries without changing historical document snapshots.
- [ ] Projects can be associated with clients and assigned to workspace members.
- [ ] Tasks can be created, assigned, updated, filtered and commented on.
- [ ] Quotations can be created, totalled, tracked and converted when eligible.
- [ ] Invoices can be created, numbered, issued and represented for printing or download.
- [ ] Payments update invoice balances consistently, including partial payments.
- [ ] Confirmed payments have a traceable correction process.
- [ ] Expenses can be recorded, categorised and reviewed.
- [ ] Workspace currency can be configured, with LKR as the default for new workspaces.
- [ ] Historical financial records retain their original currency and values.
- [ ] Dashboard summaries respect user permissions.
- [ ] Notifications are restricted to intended recipients.
- [ ] Reports and exports respect workspace boundaries and permissions.
- [ ] Attachments are accessible only to authorised users of the associated resource.
- [ ] Important actions are represented in audit logs.
- [ ] Critical business rules have automated tests.
- [ ] The application is usable on supported desktop and mobile browser sizes.

---

## 10. Requirements Change Management

This SRS is the approved ClientFlow v1.0 requirements baseline.

Any change to product scope, roles, financial behaviour, tenant isolation or core workflows must be reviewed before implementation. Approved changes shall be reflected in this document with an updated version or change history entry.

Implementation details may evolve without changing the product requirements, provided the approved behaviour and constraints remain satisfied.

### Change history

| Version | Date | Description |
|---|---|---|
| 1.0 | 27 November 2026 | Initial approved requirements baseline documented in the project repository |

---

## 11. Traceability

The functional requirements use stable identifiers from FR-001 to FR-090. Non-functional requirements use NFR-001 to NFR-016. Business rules use BR-001 onwards.

These identifiers should be reused in:
- Database design notes where a data constraint supports a requirement.
- API documentation where an endpoint implements a requirement.
- Development issues and pull requests.
- Unit, integration and end-to-end test cases.
- Release acceptance reviews.

This traceability will help ensure that implementation remains aligned with the approved product scope.
