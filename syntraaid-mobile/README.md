# SyntraAid Mobile

The mobile application for **SyntraAid_App** — a transparency-focused NGO and volunteer management platform built by Group 12, Capstone 2026.

This app is designed primarily for volunteers who need quick, on-the-go access to their task assignments, attendance logging, and project updates from their smartphones.

---

## What This Application Does

The mobile app provides:

- A simple login flow for volunteers and project coordinators
- A personal task list showing all assigned tasks with due dates and status
- Task detail view where volunteers can update their progress
- Attendance Logging and hour logging after each volunteer session
- in-app notifications for new task assignments and upcoming deadlines
- A personal impact summary showing total hours and project contributions
- Project progress updates for coordinators on the move

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | React Native |
| Navigation | React Navigation |
| State Management | React Context |
| HTTP Client | Fetch API |
| Notifications | Expo Notifications |
| Platform | iOS and Android |
| Hosting | Expo / EAS Build |

> **Note for contributors:** If the tech stack changes during development, update this table before the Week 4 handoff.

---

## Getting Started

### Prerequisites

Make sure you have the following installed:

- Node.js (v18 or higher)
- npm (comes with Node.js)
- Expo CLI: `npm install -g expo-cli`
- Git
- Expo Go app on your phone (for testing) — available on the App Store and Google Play

### Installation

1. Clone the repository:

```bash
git clone https://github.com/syntraaid-group12/syntraaid-mobile.git
cd syntraaid-mobile
```

2. Install dependencies:

```bash
npm install
```

3. Start the development server:

```bash
npx expo start
```

4. Scan the QR code shown in your terminal with the **Expo Go** app on your phone to open the app

---

## API Configuration

There are no `.env` files for API configuration on mobile. The backend base URL is set as a constant directly in:

```
src/config/api.js
```

The constant is named `API_BASE`. Update it to point to your local or deployed backend:

```javascript
export const API_BASE = "http://your-backend-url:5000/api";
```

All API calls throughout the app use the Fetch API and reference this constant.

> **Important:** For local development use your computer's local IP address instead of `localhost` so your phone can reach the backend on the same network.

---

## Folder Structure

```
syntraAid-mobile/
├── assets/                  # Images, icons, and fonts
├── src/
│   ├── components/          # Reusable UI components
│   ├── screens/             # Individual app screens
│   ├── navigation/          # Stack and tab navigation setup
│   ├── context/             # Global state management
│   ├── config/
│   │   └── api.js           # API_BASE constant and configuration
│   └── utils/               # Helper functions and constants
├── app.json                 # Expo configuration
├── package.json
└── README.md
```

---

## Key Screens

| Screen | Primary User | Description |
|---|---|---|
| Login | Volunteer, Coordinator | Simple login with email and password |
| Task List | Volunteer | All assigned tasks with status and due dates |
| Task Detail | Volunteer | Full task description, status update, and notes |
| Attendance Logging | Volunteer, Coordinator | Log date, hours, and project for each session |
| Notifications | Volunteer | In-app alerts for task assignments and deadlines |
| Personal Summary | Volunteer | Total hours logged, projects contributed to, recognition earned |

---

## Notifications

The app sends in-app notifications to volunteers for:

- New task assignments — delivered within 60 seconds of assignment
- Deadline reminders — sent 24 hours before a task is due
- Project updates from their coordinator

Users can manage notification preferences within the app settings.

---

## Running Tests

```bash
npm test
```

---

## Acceptance Criteria

Before the capstone demonstration, the following must pass on a real mobile device:

- A volunteer can log in and see their assigned tasks
- A volunteer can update a task status from Not Started to In Progress to Completed
- A volunteer can log attendance and hours for a session
- A volunteer receives a push notification within 60 seconds of a task being assigned
- The full volunteer flow runs end to end on both iOS and Android

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
| Product | SyntraAid_App |
| Group | Group 12 — Capstone 2026 |
| Track | Mobile Development |
| Deadline | End of Week 3 |
| Target Platforms | iOS and Android |
| Deployment | Expo / EAS Build |