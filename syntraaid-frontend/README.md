# Syntraaid Frontend

The web application for **Volunteer_NGO_App** — a transparency-focused NGO and volunteer management platform built by Group 12, Capstone 2026.

This is the browser-based interface used by NGO Administrators, Project Coordinators, and Donors to manage volunteers, track projects, and generate impact reports.

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
| Framework | React.js |
| Styling | Tailwind CSS |
| State Management | React Context / Redux |
| HTTP Client | Axios |
| Routing | React Router |
| Hosting | Vercel (free tier) |

> **Note for contributors:** If the tech stack changes during development, update this table before the Week 4 handoff.

---

## Getting Started

### Prerequisites

Make sure you have the following installed:

- Node.js (v18 or higher)
- npm (comes with Node.js)
- Git

### Installation

1. Clone the repository:

```bash
git clone https://github.com/syntraaid-group12/syntraaid-frontend.git
cd syntraaid-frontend
```

2. Install dependencies:

```bash
npm install
```

3. Set up your environment variables (see Environment Variables section below)

4. Start the development server:

```bash
npm start
```

The app will open at `http://localhost:3000`

---

## Environment Variables

Create a `.env` file in the root of the project and add the following:

```
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_ENV=development
```

> **Important:** Never commit your `.env` file to GitHub. It is already listed in `.gitignore`.

---

## Folder Structure

```
syntraaid-frontend/
├── public/                  # Static assets and index.html
├── src/
│   ├── components/          # Reusable UI components (buttons, cards, modals)
│   ├── pages/               # Full page views (Dashboard, Projects, Reports)
│   ├── layouts/             # Shared page layouts per user role
│   ├── context/             # Global state management
│   ├── services/            # API call functions
│   ├── hooks/               # Custom React hooks
│   └── utils/               # Helper functions and constants
├── .env.example             # Sample environment variable file
├── package.json
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
| Donor Project Detail | Donor | Program progress, participation indicators, recent activity |
| Donor Funding Summary | Donor | Program health across funded projects, contact admin option |
| Donor Project History | Donor | Chronological milestones, participation summaries, progress over time |

---

## Design System

All UI design, wireframes, and high-fidelity prototypes are maintained in Figma by the Design team.

- Figma prototype link: *(To be added by the Design Lead at end of Week 2)*
- Design principles: Simple, Mobile First, Minimal Learning Curve, Clear Visibility
- All screens are designed mobile-first and adapted for desktop

---

## Connecting to the Backend

The frontend communicates with the `syntraaid-backend` service via REST API calls. Make sure the backend server is running locally before starting the frontend in development.

Set the `REACT_APP_API_URL` in your `.env` file to point to your local backend:

```
REACT_APP_API_URL=http://localhost:5000/api
```

For production, this will point to the deployed backend URL on Railway or Render.

---

## Running Tests

```bash
npm test
```

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
| Product | Volunteer_NGO_App |
| Group | Group 12 — Capstone 2026 |
| Track | Frontend Development |
| Deadline | End of Week 3 |
| Deployment | Vercel |