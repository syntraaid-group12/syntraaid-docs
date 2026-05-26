# Syntraaid Backend

The server-side application for **Volunteer_NGO_App** — a transparency-focused NGO and volunteer management platform built by Group 12, Capstone 2026.

This service handles all business logic, data storage, authentication, and API endpoints that power the web and mobile frontends.

---

## What This Service Does

The backend is responsible for:

- Authenticating users and enforcing role-based access control (Volunteer, Project Coordinator, NGO Admin, Donor, Platform Admin)
- Managing volunteer profiles, skills, and availability data
- Creating and tracking projects, milestones, and tasks
- Logging attendance and calculating volunteer hours automatically
- Generating impact reports and exporting them as PDF or CSV
- Serving the Transparency Dashboard with tamper-proof activity logs
- Powering the Donor Dashboard with funded project visibility
- Sending in-app and email notifications for task assignments and deadlines

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express.js |
| Database | PostgreSQL |
| Authentication | JWT (JSON Web Tokens) |
| File Export | PDF and CSV generation libraries |
| Hosting | Railway / Render (free tier) |

> **Note for contributors:** If the tech stack changes during development, update this table before the Week 4 handoff.

---

## Getting Started

### Prerequisites

Make sure you have the following installed before running the project:

- Node.js (v18 or higher)
- npm (comes with Node.js)
- PostgreSQL (v14 or higher)
- Git

### Installation

1. Clone the repository:

```bash
git clone https://github.com/syntraaid-group12/syntraaid-backend.git
cd syntraaid-backend
```

2. Install dependencies:

```bash
npm install
```

3. Set up your environment variables (see Environment Variables section below)

4. Set up the database:

```bash
npm run db:migrate
```

5. Start the development server:

```bash
npm run dev
```

The server will start on `http://localhost:5000`

---

## Environment Variables

Create a `.env` file in the root of the project and add the following:

```
PORT=5000
DATABASE_URL=your_postgresql_connection_string
JWT_SECRET=your_jwt_secret_key
EMAIL_SERVICE=your_email_service_api_key
NODE_ENV=development
```

> **Important:** Never commit your `.env` file to GitHub. It is already listed in `.gitignore`.

---

## Folder Structure

```
syntraaid-backend/
├── src/
│   ├── controllers/       # Handles incoming requests and responses
│   ├── routes/            # Defines all API endpoint paths
│   ├── models/            # Database schema and queries
│   ├── middleware/        # Authentication and role-based access checks
│   ├── services/          # Business logic (reports, notifications, etc.)
│   └── utils/             # Helper functions
├── database/
│   └── migrations/        # Database setup and versioning scripts
├── tests/                 # Automated tests
├── .env.example           # Sample environment variable file
├── package.json
└── README.md
```

---

## API Overview

The backend exposes REST API endpoints across the following modules:

| Module | Base Path | Description |
|---|---|---|
| Authentication | `/api/auth` | Login, registration, password reset |
| Volunteers | `/api/volunteers` | Volunteer profiles, skills, availability |
| Projects | `/api/projects` | Project creation, milestones, status |
| Tasks | `/api/tasks` | Task assignment, status updates |
| Attendance | `/api/attendance` | Log hours, view history, export |
| Reports | `/api/reports` | KPI dashboard, PDF and CSV export |
| Donors | `/api/donors` | Donor dashboard, funded project views |
| Notifications | `/api/notifications` | In-app and email alerts |

Full API documentation is maintained separately in the `syntraaid-docs` repository.

---

## User Roles and Access

| Role | Access Level |
|---|---|
| Volunteer | View own assignments, update task status, log attendance |
| Project Coordinator | Manage projects and volunteers within assigned projects |
| NGO Admin | Full access to all projects, volunteers, and reporting |
| Donor | Read-only access to funded projects only |
| Platform Admin | System-wide oversight and configuration |

---

## Database Schema

Core tables in the database:

- `users` — All user accounts and roles
- `volunteers` — Volunteer profiles, skills, and availability
- `projects` — Projects with goals, milestones, and timelines
- `tasks` — Tasks linked to projects and assigned to volunteers
- `assignments` — Records linking volunteers to projects
- `attendance_logs` — Individual attendance and hour entries
- `activity_logs` — Tamper-proof record of all significant system events
- `notifications` — In-app and email notification records

---

## Running Tests

```bash
npm test
```

---

## Acceptance Criteria

Before the capstone demonstration, the following must pass:

- All five user roles can log in and see only their permitted views
- Volunteers can be searched by skill and assigned to a project
- A project can be created and tracked through to completion
- Attendance entries are saved correctly and cumulative totals are accurate
- An impact report PDF is generated and available for download in under 2 minutes
- A volunteer receives a task notification within 60 seconds of assignment
- Donors can only see projects they are linked to

---

## Contributing

1. Branch from `main` using the naming format: `SAI-[ticket-number]-short-description`
2. Make your changes
3. Commit with a clear message: `git commit -m "feat: description of change"`
4. Push your branch and open a pull request
5. Tag the Team Lead for review before merging

---

## Project Info

| Detail | Info |
|---|---|
| Product | Volunteer_NGO_App |
| Group | Group 12 — Capstone 2026 |
| Track | Backend Development |
| Deadline | End of Week 3 |
| Deployment | Railway / Render |