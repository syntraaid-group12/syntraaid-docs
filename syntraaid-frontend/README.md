# SyntraAid Frontend

The web application for **SyntraAid** — a transparency-focused NGO and volunteer management platform built by Group 12, Capstone 2026.

This is the browser-based interface built with plain HTML, Tailwind CSS, and Vanilla JavaScript. It is used by NGO Administrators, Project Coordinators, Volunteers, and Donors to manage volunteers, track projects, and generate impact reports.

---

## What This Application Does

The frontend provides:

- A login and registration flow with role-based access for all user types
- An Admin Dashboard showing organization-wide KPIs, project health, and volunteer activity
- A Project and Task management interface for coordinators to create, assign, and monitor work
- A Transparency Dashboard with real-time project progress feeds and tamper-proof activity logs
- A Reports Dashboard where admins can generate and export donor-ready PDF and CSV reports
- A Donor Dashboard giving funders a read-only view of the programs they support
- Volunteer profile pages with participation history and auto-calculated hours
- Attendance logging screens for volunteers and coordinators
- Notification preferences and in-app alert management

---

## Tech Stack

| Layer | Technology |
|---|---|
| Markup | HTML5 |
| Styling | Tailwind CSS |
| Logic | Vanilla JavaScript |
| Routing | Custom router in router.js |
| HTTP Client | Fetch API |
| Hosting | Netlify (free tier) |

> **Note for contributors:** If the tech stack changes during development, update this table before the Week 4 handoff.

---

## Getting Started

### Prerequisites

All you need is:

- A code editor (VS Code recommended)
- A browser (Chrome or Firefox)
- Git
- Live Server extension in VS Code (for local development)

### Installation

1. Clone the repository:

```bash
git clone https://github.com/syntraaid-group12/syntraaid-frontend.git
cd syntraaid-frontend
```

2. Open the project in VS Code:

```bash
code .
```

3. Install the **Live Server** extension in VS Code if you haven't already

4. Right-click on `index.html` and select **"Open with Live Server"**

The app will open in your browser automatically 

> There is no npm install or build step required for this project.

---

## Environment Variables

There are no `.env` files on the frontend. The API base URL is configured directly in:

```
js/config.js
```

Update the API URL in that file to point to your local or deployed backend.

---

## Folder Structure

```
syntraaid-frontend/
├── auth/              # Login and registration pages
├── admin/             # NGO Admin dashboard and management pages
├── coordinator/       # Project Coordinator pages
├── volunteer/         # Volunteer dashboard and task pages
├── donor/             # Donor dashboard and project view pages
├── shared/            # Shared components used across all roles
├── js/
│   ├── config.js      # API base URL and global configuration
│   ├── auth.js        # Authentication logic and token handling
│   ├── api.js         # All API call functions using Fetch API
│   └── router.js      # Client-side routing logic
└── README.md
```

---

## Key Screens

| Screen | Primary User | Description |
|---|---|---|
| Login and Registration | All roles | Entry point with invite link support and password reset |
| Admin Dashboard | NGO Administrator | KPI overview, project health cards, volunteer activity summary |
| Volunteer Dashboard | Volunteer | Assigned tasks, personal hours, participation history, recognition |
| Project List | Coordinator, Admin | Project cards with status, milestone progress, coordinator name |
| Task Detail Page | Volunteer, Coordinator | Description, status selector, notes, due date, assignees |
| Attendance Screen | Volunteer, Coordinator | Date, hours input, project selector, cumulative total |
| Transparency Dashboard | Admin, Coordinator | Activity feed, contribution tracker, volunteer impact summaries |
| Reports Dashboard | NGO Administrator | KPI charts, project summaries, PDF and CSV export |
| Donor Home Dashboard | Donor | Funded projects with status indicators and progress highlights |

---

## Role-Based Folder Guide

Each user role has its own folder containing the HTML pages for that role:

- **`auth/`** — Shared login and registration pages used by all roles
- **`admin/`** — Pages only accessible to the NGO Administrator
- **`coordinator/`** — Pages for Project Coordinators managing projects and volunteers
- **`volunteer/`** — Pages for Volunteers viewing tasks and logging attendance
- **`donor/`** — Read-only pages for Donors viewing funded projects
- **`shared/`** — Components like navigation bars, modals, and notification panels used across roles

---

## Connecting to the Backend

All API calls are made using the Fetch API. The base URL for the backend is set in `js/config.js`.

For local development update `js/config.js` to:

```javascript
const API_BASE = "http://localhost:5000/api";
```

For production update it to the deployed backend URL on Railway or Render.

---

## Acceptance Criteria

Before the capstone demonstration, the following must pass on the web application:

- All four user roles can log in and land on their correct dashboard
- A coordinator can search volunteers by skill and assign them to a project
- A project can be created, assigned, and tracked through to completion
- The transparency dashboard activity log displays all significant events with timestamps
- An impact report PDF can be generated and downloaded in under 2 minutes
- A donor can log in and see only the projects they are linked to

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
| Product | SyntraAid |
| Group | Group 12 — Capstone 2026 |
| Track | Frontend Development |
| Deadline | End of Week 3 |
| Deployment | Netlify |