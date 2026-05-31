# SyntraAid API Documentation

**Group 12 | Capstone 2026**

This document describes every API endpoint available in the SyntraAid backend. It is the reference guide for the frontend and mobile teams. Every field name, status code, and response format in this document matches the actual backend code exactly.

---

## Base URLs

| Environment | URL |
|---|---|
| Local Development | `http://localhost:5000/api` |
| Production | `https://your-railway-url.railway.app/api` |

---

## Authentication

All protected endpoints require a JWT token in the request header:

```
Authorization: Bearer YOUR_JWT_TOKEN
```

You get this token by calling the login endpoint. Include it in every request that requires authentication.

---

## General Rules

- All request and response bodies use **JSON format**
- All timestamps are in **ISO 8601 format** (e.g. `2026-05-27T10:00:00.000Z`)
- All IDs are **MongoDB ObjectId strings**

---

## User Roles

The following roles exist in the system. Every endpoint specifies which roles are allowed to use it.

| Role | Description |
|---|---|
| `admin` | Full access to all features and management tools |
| `coordinator` | Manages projects, tasks, and volunteers |
| `volunteer` | Views and updates their own tasks and attendance |
| `donor` | Read-only access to funded projects only |

---

## Endpoint Groups

- [Authentication](#1-authentication-endpoints)
- [Users](#2-user-endpoints)
- [Volunteers](#3-volunteer-endpoints)
- [Projects](#4-project-endpoints)
- [Tasks](#5-task-endpoints)
- [Attendance](#6-attendance-endpoints)
- [Activity Logs](#7-activity-log-endpoints)
- [Notifications](#8-notification-endpoints)
- [Reports](#9-report-endpoints)
- [Donors](#10-donor-endpoints)
- [Contact](#11-contact-endpoint)
- [Important Rules](#12-important-rules)

---

## 1. Authentication Endpoints

Base path: `/api/auth`

These endpoints handle registration, login, and password management. No token is required for these endpoints.

---

### POST /api/auth/register

Registers a new user using a valid invite token. The invite token identifies the user record — the email address and role are looked up from the token, not supplied in the request body.

**Access:** Public (invite token required)

**Request Body:**

```json
{
  "inviteToken": "uuid-invite-token-here",
  "password": "yourpassword"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `inviteToken` | String | Yes | Single-use token sent by admin. Becomes null after use. |
| `password` | String | Yes | Will be hashed before storing. |

**Success Response — 201 Created:**

```json
{
  "message": "Registration successful",
  "userId": "64f1a2b3c4d5e6f7a8b9c0d1"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 400 | Missing required fields or invalid invite token |
| 409 | Email already exists |

---

### POST /api/auth/login

Logs in an existing user and returns a JWT token.

**Access:** Public

**Request Body:**

```json
{
  "email": "volunteer@example.com",
  "password": "yourpassword"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `email` | String | Yes | Registered email address |
| `password` | String | Yes | Account password |

**Success Response — 200 OK:**

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "email": "volunteer@example.com",
    "role": "volunteer",
    "isActive": true
  }
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 400 | Missing email or password |
| 401 | Invalid email or password |
| 403 | Account not yet activated by admin |

---

### POST /api/auth/forgot-password

Sends a password reset link to the user's email address.

**Access:** Public

**Request Body:**

```json
{
  "email": "volunteer@example.com"
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Password reset email sent"
}
```

> **Security Note:** This endpoint always returns 200 regardless of whether the email exists in the system. This prevents user enumeration attacks.

---

### POST /api/auth/reset-password

Resets the user's password using a valid reset token.

**Access:** Public (reset token required)

**Request Body:**

```json
{
  "resetToken": "uuid-reset-token-here",
  "newPassword": "yournewpassword"
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Password reset successful"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 400 | Invalid or expired reset token |

---

### GET /api/auth/invite/:token

Validates an invite token and returns the email address and role associated with it. The registration page uses this to confirm the token is valid and to pre-fill the user's details before the password is set.

**Access:** Public

**Success Response — 200 OK:**

```json
{
  "email": "newuser@example.com",
  "role": "volunteer"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 400 | Invalid or expired invite token |

---

## 2. User Endpoints

Base path: `/api/users`

These endpoints manage user accounts. All endpoints in this group require authentication.

---

### GET /api/users

Returns a list of all users in the system.

**Access:** Admin only

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "email": "admin@example.com",
    "role": "admin",
    "isActive": true,
    "createdAt": "2026-01-01T00:00:00.000Z"
  }
]
```

---

### GET /api/users/:id

Returns a single user by their ID.

**Access:** Admin only

**Success Response — 200 OK:**

```json
{
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
  "email": "volunteer@example.com",
  "role": "volunteer",
  "isActive": true,
  "invitedBy": "64f1a2b3c4d5e6f7a8b9c0d2",
  "createdAt": "2026-01-01T00:00:00.000Z"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 404 | User not found |

---

### POST /api/users/invite

Admin creates an invite token and sends it to a new user's email address.

**Access:** Admin only

**Request Body:**

```json
{
  "email": "newuser@example.com",
  "role": "volunteer"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `email` | String | Yes | Email address to send the invite to |
| `role` | String | Yes | Must be one of: `admin`, `coordinator`, `volunteer`, `donor` |

**Success Response — 201 Created:**

```json
{
  "message": "Invite sent successfully",
  "inviteToken": "uuid-token-here"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 400 | Missing fields or invalid role |
| 409 | Email already registered |

---

### PATCH /api/users/:id/activate

Activates or deactivates a user account. Pass `isActive: true` to activate or `isActive: false` to deactivate.

**Access:** Admin only

**Request Body:**

```json
{
  "isActive": true
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `isActive` | Boolean | Yes | Pass `true` to activate or `false` to deactivate |

**Success Response — 200 OK:**

```json
{
  "message": "User updated successfully"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 404 | User not found |
| 403 | Access denied |

---

### GET /api/users/:id/notification-preferences

Returns the notification preferences for the specified user.

**Access:** The user themselves

**Success Response — 200 OK:**

```json
{
  "userId": "64f1a2b3c4d5e6f7a8b9c0d1",
  "taskAssigned": true,
  "deadlineReminder": true,
  "taskBlocked": true,
  "milestoneCompleted": true,
  "preferredChannel": "in_app"
}
```

---

### PUT /api/users/:id/notification-preferences

Updates notification preferences for the specified user.

**Access:** The user themselves

**Request Body:** Include only the fields you want to update.

```json
{
  "deadlineReminder": false,
  "preferredChannel": "in_app"
}
```

| Field | Type | Description |
|---|---|---|
| `taskAssigned` | Boolean | Notify when a task is assigned |
| `deadlineReminder` | Boolean | Notify before a task deadline |
| `taskBlocked` | Boolean | Notify when a task is blocked |
| `milestoneCompleted` | Boolean | Notify when a milestone is completed |
| `preferredChannel` | String | Must be: `in_app` |

**Success Response — 200 OK:**

```json
{
  "message": "Notification preferences updated"
}
```

---

## 3. Volunteer Endpoints

Base path: `/api/volunteers`

These endpoints manage volunteer profiles. All endpoints require authentication.

---

### GET /api/volunteers

Returns a list of all volunteer profiles.

**Access:** Admin, Coordinator

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "userId": "64f1a2b3c4d5e6f7a8b9c0d2",
    "firstName": "Jane",
    "lastName": "Doe",
    "skills": ["teaching", "first aid"],
    "availabilityDays": ["MON", "WED"],
    "availabilityTimes": ["morning"],
    "recognitionBadges": ["50 Hours"]
  }
]
```

---

### GET /api/volunteers/:id

Returns a single volunteer profile by ID.

**Access:** Admin, Coordinator, the Volunteer themselves

**Success Response — 200 OK:**

```json
{
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
  "userId": "64f1a2b3c4d5e6f7a8b9c0d2",
  "firstName": "Jane",
  "lastName": "Doe",
  "bio": "Passionate about community work.",
  "skills": ["teaching", "first aid"],
  "availabilityDays": ["MON", "WED", "FRI"],
  "availabilityTimes": ["morning", "afternoon"],
  "recognitionBadges": ["50 Hours", "Project Champion"]
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 404 | Volunteer profile not found |

---

### POST /api/volunteers/:id/profile

Creates a volunteer profile for a registered volunteer user.

**Access:** Volunteer (own profile), Admin

**Request Body:**

```json
{
  "userId": "64f1a2b3c4d5e6f7a8b9c0d2",
  "firstName": "Jane",
  "lastName": "Doe",
  "bio": "Passionate about community work.",
  "skills": ["teaching", "first aid"],
  "availabilityDays": ["MON", "WED", "FRI"],
  "availabilityTimes": ["morning", "afternoon"]
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `userId` | String | Yes | Must match a registered user with role `volunteer` |
| `firstName` | String | Yes | Volunteer's first name |
| `lastName` | String | Yes | Volunteer's last name |
| `bio` | String | No | Short personal description |
| `skills` | Array of Strings | No | e.g. `["teaching", "first aid"]` |
| `availabilityDays` | Array of Strings | No | e.g. `["MON", "WED", "FRI"]` |
| `availabilityTimes` | Array of Strings | No | e.g. `["morning", "afternoon"]` |

**Success Response — 201 Created:**

```json
{
  "message": "Volunteer profile created",
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1"
}
```

---

### PUT /api/volunteers/:id/profile

Updates an existing volunteer profile.

**Access:** Volunteer (own profile), Admin

**Request Body:** Include only the fields you want to update.

```json
{
  "bio": "Updated bio text.",
  "skills": ["teaching", "logistics"],
  "availabilityDays": ["TUE", "THU"]
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Volunteer profile updated"
}
```

---

## 4. Project Endpoints

Base path: `/api/projects`

These endpoints manage projects, milestones, and volunteer assignments.

---

### GET /api/projects

Returns all projects. Soft-deleted projects (where `deletedAt` is not null) are excluded.

**Access:** Admin, Coordinator

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "title": "Community Clean-Up Drive",
    "description": "Monthly clean-up event.",
    "status": "active",
    "startDate": "2026-06-01T00:00:00.000Z",
    "endDate": "2026-08-31T00:00:00.000Z",
    "coordinatorId": "64f1a2b3c4d5e6f7a8b9c0d2",
    "volunteers": ["64f1a2b3c4d5e6f7a8b9c0d3"],
    "milestones": []
  }
]
```

---

### GET /api/projects/:id

Returns a single project by ID including its milestones and assigned volunteers.

**Access:** Admin, Coordinator, Volunteer (assigned to the project)

**Success Response — 200 OK:**

```json
{
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
  "title": "Community Clean-Up Drive",
  "description": "Monthly clean-up event.",
  "goals": "Clean 10 neighbourhoods by August.",
  "status": "active",
  "startDate": "2026-06-01T00:00:00.000Z",
  "endDate": "2026-08-31T00:00:00.000Z",
  "coordinatorId": "64f1a2b3c4d5e6f7a8b9c0d2",
  "createdBy": "64f1a2b3c4d5e6f7a8b9c0d4",
  "milestones": [
    {
      "_id": "64f1a2b3c4d5e6f7a8b9c0d5",
      "title": "Phase 1 Complete",
      "dueDate": "2026-07-01T00:00:00.000Z",
      "isCompleted": false
    }
  ],
  "volunteers": ["64f1a2b3c4d5e6f7a8b9c0d3"]
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 404 | Project not found |

---

### POST /api/projects

Creates a new project. Status defaults to `planning`.

**Access:** Admin, Coordinator

**Request Body:**

```json
{
  "title": "Community Clean-Up Drive",
  "description": "Monthly clean-up event.",
  "goals": "Clean 10 neighbourhoods by August.",
  "startDate": "2026-06-01T00:00:00.000Z",
  "endDate": "2026-08-31T00:00:00.000Z"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `title` | String | Yes | Project title |
| `description` | String | No | Project description |
| `goals` | String | No | Project goals |
| `startDate` | Date | No | ISO 8601 date string |
| `endDate` | Date | No | ISO 8601 date string |

**Success Response — 201 Created:**

```json
{
  "message": "Project created",
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1"
}
```

---

### PATCH /api/projects/:id

Updates project details or changes project status.

**Access:** Admin, Coordinator (assigned to the project)

**Valid status values:** `planning`, `active`, `on_hold`, `completed`, `cancelled`

> Note: A project cannot be set to `active` unless a `coordinatorId` is assigned.

**Request Body:** Include only the fields you want to update.

```json
{
  "status": "active",
  "coordinatorId": "64f1a2b3c4d5e6f7a8b9c0d2"
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Project updated"
}
```

---

### POST /api/projects/:id/volunteers

Assigns a volunteer to a project.

**Access:** Admin, Coordinator

**Request Body:**

```json
{
  "volunteerId": "64f1a2b3c4d5e6f7a8b9c0d3"
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Volunteer assigned to project"
}
```

---

### DELETE /api/projects/:id/volunteers/:volunteerId

Removes a volunteer from a project.

**Access:** Admin, Coordinator

**Success Response — 200 OK:**

```json
{
  "message": "Volunteer removed from project"
}
```

---

### POST /api/projects/:id/milestones

Adds a milestone to a project.

**Access:** Admin, Coordinator

**Request Body:**

```json
{
  "title": "Phase 1 Complete",
  "description": "All phase 1 tasks finished.",
  "dueDate": "2026-07-01T00:00:00.000Z",
  "sortOrder": 1
}
```

**Success Response — 201 Created:**

```json
{
  "message": "Milestone added"
}
```

---

### PATCH /api/projects/:id/milestones/:milestoneId

Updates or marks a milestone as completed.

**Access:** Admin, Coordinator

**Request Body:**

```json
{
  "isCompleted": true
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Milestone updated"
}
```

---

### DELETE /api/projects/:id

Soft deletes a project by setting `deletedAt`. The project is not permanently removed.

**Access:** Admin only

**Success Response — 200 OK:**

```json
{
  "message": "Project deleted"
}
```

---

## 5. Task Endpoints

Base path: `/api/tasks`

These endpoints manage tasks within projects.

---

### GET /api/tasks

Returns all tasks. Can be filtered by project.

**Access:** Admin, Coordinator

**Query Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `projectId` | String | Filter tasks by project ID |

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "projectId": "64f1a2b3c4d5e6f7a8b9c0d2",
    "title": "Set up equipment",
    "status": "not_started",
    "dueDate": "2026-06-15T00:00:00.000Z",
    "assignees": ["64f1a2b3c4d5e6f7a8b9c0d3"]
  }
]
```

---

### GET /api/tasks/:id

Returns a single task by ID.

**Access:** Admin, Coordinator, Volunteer (assigned to the task)

**Success Response — 200 OK:**

```json
{
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
  "projectId": "64f1a2b3c4d5e6f7a8b9c0d2",
  "milestoneId": null,
  "title": "Set up equipment",
  "description": "Prepare all tools before the event.",
  "status": "in_progress",
  "dueDate": "2026-06-15T00:00:00.000Z",
  "notes": "",
  "assignees": ["64f1a2b3c4d5e6f7a8b9c0d3"],
  "completedAt": null,
  "createdBy": "64f1a2b3c4d5e6f7a8b9c0d4"
}
```

---

### POST /api/tasks

Creates a new task inside a project.

**Access:** Admin, Coordinator

**Request Body:**

```json
{
  "projectId": "64f1a2b3c4d5e6f7a8b9c0d2",
  "title": "Set up equipment",
  "description": "Prepare all tools before the event.",
  "dueDate": "2026-06-15T00:00:00.000Z",
  "milestoneId": null,
  "assignees": ["64f1a2b3c4d5e6f7a8b9c0d3"]
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `projectId` | String | Yes | ID of the project this task belongs to |
| `title` | String | Yes | Task title |
| `description` | String | No | Task description |
| `dueDate` | Date | No | ISO 8601 date string |
| `milestoneId` | String | No | Link to a project milestone. Use `null` if not linked. |
| `assignees` | Array of Strings | No | Array of user IDs |

**Success Response — 201 Created:**

```json
{
  "message": "Task created",
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1"
}
```

---

### PATCH /api/tasks/:id

Updates task details or changes task status.

**Access:** Admin, Coordinator, Volunteer (status update only)

**Valid status values:** `not_started`, `in_progress`, `blocked`, `completed`

**Request Body:** Include only the fields you want to update.

```json
{
  "status": "completed",
  "notes": "All equipment set up successfully."
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Task updated"
}
```

---

## 6. Attendance Endpoints

Base path: `/api/attendance`

These endpoints manage volunteer attendance logs.

> **INSERT ONLY** — Attendance records can never be edited or deleted. There are no PUT, PATCH, or DELETE endpoints for attendance. See Important Rules section.

---

### GET /api/attendance

Returns all attendance records. Can be filtered by volunteer or project.

**Access:** Admin, Coordinator

**Query Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `volunteerId` | String | Filter by volunteer ID |
| `projectId` | String | Filter by project ID |

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "volunteerId": "64f1a2b3c4d5e6f7a8b9c0d2",
    "projectId": "64f1a2b3c4d5e6f7a8b9c0d3",
    "sessionDate": "2026-06-01T00:00:00.000Z",
    "hoursLogged": 4,
    "notes": "",
    "loggedBy": "64f1a2b3c4d5e6f7a8b9c0d2",
    "loggedAt": "2026-06-01T18:00:00.000Z"
  }
]
```

---

### POST /api/attendance

Logs a new attendance record. This is an insert-only operation.

**Access:** Volunteer, Coordinator

**Request Body:**

```json
{
  "volunteerId": "64f1a2b3c4d5e6f7a8b9c0d2",
  "projectId": "64f1a2b3c4d5e6f7a8b9c0d3",
  "sessionDate": "2026-06-01T00:00:00.000Z",
  "hoursLogged": 4,
  "notes": ""
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `volunteerId` | String | Yes | ID of the volunteer |
| `projectId` | String | Yes | ID of the project |
| `sessionDate` | Date | Yes | The date the session took place |
| `hoursLogged` | Number | Yes | Must be greater than 0 |
| `notes` | String | No | Use for corrections referencing the original entry |

**Success Response — 201 Created:**

```json
{
  "message": "Attendance logged",
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 400 | Missing required fields or hoursLogged is 0 or less |

---

### GET /api/attendance/export

Exports attendance records as a CSV or PDF file. Used by the admin attendance screen to download records for donor reporting.

**Access:** Admin

**Query Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `projectId` | String | Filter by project ID |
| `dateRangeStart` | String | ISO 8601 date string |
| `dateRangeEnd` | String | ISO 8601 date string |

**Success Response — 200 OK:**

Returns the generated file as a download.

---

## 7. Activity Log Endpoints

Base path: `/api/activity-logs`

Activity logs are written by the system automatically. There is no endpoint to create them directly.

---

### GET /api/activity-logs

Returns all activity log entries. Most recent entries are returned first.

**Access:** Admin, Coordinator

**Query Parameters:**

| Parameter | Type | Description |
|---|---|---|
| `actorId` | String | Filter by the user who performed the action |
| `targetId` | String | Filter by the affected document ID |
| `activityType` | String | Filter by activity type |

**Valid activityType values:** `project_created`, `project_status_changed`, `task_created`, `task_status_changed`, `volunteer_assigned`, `attendance_logged`, `milestone_completed`, `report_generated`, `donor_linked`

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "activityType": "task_status_changed",
    "actorId": "64f1a2b3c4d5e6f7a8b9c0d2",
    "targetType": "task",
    "targetId": "64f1a2b3c4d5e6f7a8b9c0d3",
    "description": "Jane marked Task X as completed",
    "metadata": {},
    "createdAt": "2026-06-01T10:00:00.000Z"
  }
]
```

---

### GET /api/activity-logs/:id

Returns a single activity log entry by ID.

**Access:** Admin, Coordinator

**Success Response — 200 OK:**

```json
{
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
  "activityType": "project_created",
  "actorId": "64f1a2b3c4d5e6f7a8b9c0d2",
  "targetType": "project",
  "targetId": "64f1a2b3c4d5e6f7a8b9c0d3",
  "description": "Admin created project Community Clean-Up Drive",
  "metadata": {},
  "createdAt": "2026-06-01T09:00:00.000Z"
}
```

---

## 8. Notification Endpoints

Base path: `/api/notifications`

These endpoints manage in-app notifications. User notification preferences are managed under the [Users](#2-user-endpoints) section.

---

### GET /api/notifications

Returns all notifications for the currently logged-in user. Most recent first.

**Access:** All authenticated users

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "recipientId": "64f1a2b3c4d5e6f7a8b9c0d2",
    "notificationType": "task_assigned",
    "referenceType": "task",
    "referenceId": "64f1a2b3c4d5e6f7a8b9c0d3",
    "message": "You have been assigned a new task: Set up equipment",
    "isRead": false,
    "deliveryChannel": "in_app",
    "createdAt": "2026-06-01T10:00:00.000Z"
  }
]
```

---

### PATCH /api/notifications/:id/read

Marks a single notification as read.

**Access:** The notification recipient only

**Success Response — 200 OK:**

```json
{
  "message": "Notification marked as read"
}
```

---

### PATCH /api/notifications/read-all

Marks all notifications as read for the currently logged-in user.

**Access:** All authenticated users

**Success Response — 200 OK:**

```json
{
  "message": "All notifications marked as read"
}
```

---

## 9. Report Endpoints

Base path: `/api/reports`

These endpoints generate and retrieve impact reports.

---

### GET /api/reports

Returns a list of all previously generated reports.

**Access:** Admin only

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "reportType": "impact_report",
    "generatedBy": "64f1a2b3c4d5e6f7a8b9c0d2",
    "projectId": "64f1a2b3c4d5e6f7a8b9c0d3",
    "dateRangeStart": "2026-01-01T00:00:00.000Z",
    "dateRangeEnd": "2026-06-01T00:00:00.000Z",
    "fileUrl": "https://storage.example.com/reports/report-001.pdf",
    "generatedAt": "2026-06-01T12:00:00.000Z"
  }
]
```

---

### POST /api/reports

Generates a new report and returns the file URL.

**Access:** Admin only

**Request Body:**

```json
{
  "reportType": "impact_report",
  "projectId": "64f1a2b3c4d5e6f7a8b9c0d3",
  "dateRangeStart": "2026-01-01T00:00:00.000Z",
  "dateRangeEnd": "2026-06-01T00:00:00.000Z"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `reportType` | String | Yes | Must be one of: `impact_report`, `attendance_export`, `kpi_summary` |
| `projectId` | String | No | Leave null for an organisation-wide report |
| `dateRangeStart` | Date | No | ISO 8601 date string |
| `dateRangeEnd` | Date | No | ISO 8601 date string |

**Success Response — 201 Created:**

```json
{
  "message": "Report generated",
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
  "fileUrl": "https://storage.example.com/reports/report-001.pdf"
}
```

---

## 10. Donor Endpoints

Base path: `/api/donors`

These endpoints manage donor access to funded projects.

> Donor responses never include volunteer personal contact details such as email addresses or phone numbers.

---

### GET /api/donors/:donorId/projects

Returns all projects linked to a specific donor.

**Access:** Admin, the Donor themselves

**Success Response — 200 OK:**

```json
[
  {
    "projectId": "64f1a2b3c4d5e6f7a8b9c0d1",
    "title": "Community Clean-Up Drive",
    "status": "active",
    "visibilitySettings": {
      "showMilestoneProgress": true,
      "showVolunteerCount": true,
      "showAttendanceHours": true,
      "showActivityFeed": true,
      "showProgramHealth": true
    }
  }
]
```

---

### POST /api/donors/link

Links a donor to a project. Only admins can create this link.

**Access:** Admin only

**Request Body:**

```json
{
  "donorId": "64f1a2b3c4d5e6f7a8b9c0d1",
  "projectId": "64f1a2b3c4d5e6f7a8b9c0d2"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `donorId` | String | Yes | Must be a user with role `donor` |
| `projectId` | String | Yes | ID of the project to link |

**Success Response — 201 Created:**

```json
{
  "message": "Donor linked to project successfully"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 400 | Donor is already linked to this project |
| 404 | Donor or project not found |

---

### PATCH /api/donors/link/:linkId

Updates the visibility settings for a donor-project link or deactivates the link.

**Access:** Admin only

**Request Body:**

```json
{
  "isActive": true,
  "visibilitySettings": {
    "showMilestoneProgress": true,
    "showVolunteerCount": false,
    "showAttendanceHours": true,
    "showActivityFeed": false,
    "showProgramHealth": true
  }
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Donor link updated"
}
```

---

## 11. Contact Endpoint

Base path: `/api/contact`

This endpoint handles contact requests from organisations or individuals who want to use SyntraAid.

---

### POST /api/contact

Submits a contact request.

**Access:** Public (no authentication required)

**Request Body:**

```json
{
  "name": "John Smith",
  "email": "john@ngo.org",
  "organizationName": "Help Africa Foundation",
  "role": "NGO Admin",
  "message": "We would like to use SyntraAid for our volunteer programme."
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `name` | String | Yes | Full name of the person submitting |
| `email` | String | Yes | Contact email address |
| `organizationName` | String | Yes | Name of the organisation |
| `role` | String | No | e.g. `NGO Admin`, `Coordinator`, `Donor` |
| `message` | String | No | Additional message or context |

**Success Response — 201 Created:**

```json
{
  "message": "Contact request submitted successfully"
}
```

**Error Responses:**

| Status Code | Meaning |
|---|---|
| 400 | Missing required fields |

---

### GET /api/contact

Returns all contact requests submitted through the platform.

**Access:** Admin only

**Success Response — 200 OK:**

```json
[
  {
    "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
    "name": "John Smith",
    "email": "john@ngo.org",
    "organizationName": "Help Africa Foundation",
    "role": "NGO Admin",
    "message": "We would like to use SyntraAid.",
    "status": "new",
    "submittedAt": "2026-05-27T10:00:00.000Z"
  }
]
```

---

### PATCH /api/contact/:id

Updates the status of a contact request.

**Access:** Admin only

**Valid status values:** `new`, `reviewed`, `actioned`

**Request Body:**

```json
{
  "status": "reviewed"
}
```

**Success Response — 200 OK:**

```json
{
  "message": "Contact request updated"
}
```

---

## 12. Important Rules

These rules apply across the entire API. Every developer working with this API must read and follow them.

---

### Attendance Records Are Insert-Only

Attendance records can never be edited or deleted. The POST `/api/attendance` endpoint is the only attendance endpoint that writes data. There are no PUT, PATCH, or DELETE endpoints for attendance. If a correction is needed, a new attendance entry must be created with a note referencing the original entry.

---

### Activity Logs Are Written by the System Only

There is no endpoint that allows a client to create activity log entries directly. All activity logs are written automatically by the backend when significant events occur such as project creation, task status changes, and volunteer assignments.

---

### Total Hours Are Always Computed

The total hours logged for a volunteer are always computed by summing the `hoursLogged` field across all attendance records for that volunteer. This value is never stored as a separate field on the volunteer profile.

---

### Donor Responses Never Include Personal Contact Details

All API responses returned to donor users are filtered to remove volunteer personal contact information. Email addresses and phone numbers belonging to volunteers are never included in any donor-facing response.

---

### Enum Values Must Match Exactly

All enum fields must use the exact values listed in this document. Sending an unlisted value will result in a validation error. The valid enum values for each field are:

| Field | Valid Values |
|---|---|
| `user.role` | `admin`, `coordinator`, `volunteer`, `donor` |
| `project.status` | `planning`, `active`, `on_hold`, `completed`, `cancelled` |
| `task.status` | `not_started`, `in_progress`, `blocked`, `completed` |
| `report.reportType` | `impact_report`, `attendance_export`, `kpi_summary` |
| `notification.notificationType` | `task_assigned`, `deadline_reminder`, `task_blocked`, `milestone_completed`, `project_status_changed`, `donor_milestone_alert` |
| `notification.deliveryChannel` | `in_app` |
| `activityLog.activityType` | `project_created`, `project_status_changed`, `task_created`, `task_status_changed`, `volunteer_assigned`, `attendance_logged`, `milestone_completed`, `report_generated`, `donor_linked` |
| `contactRequest.status` | `new`, `reviewed`, `actioned` |

---

*SyntraAid | API Documentation | Group 12 | Capstone 2026*
