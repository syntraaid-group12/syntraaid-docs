# SyntraAid Admin User Manual

**Version:** MVP v1.0
**Audience:** System Administrators
**Project:** SyntraAid | Group 12 | Capstone 2026

---

## Table of Contents

1. [Your Role as an Administrator](#section-1-your-role-as-an-administrator)
2. [The Admin Dashboard](#section-2-the-admin-dashboard)
3. [Inviting and Managing Users](#section-3-inviting-and-managing-users)
4. [Generating Reports](#section-4-generating-reports)
5. [Managing Donor Access](#section-5-managing-donor-access)
6. [The Transparency Dashboard](#section-6-the-transparency-dashboard)

---

## Section 1: Your Role as an Administrator

As an Administrator on SyntraAid, you have the highest level of access in the system. You are responsible for keeping the platform running smoothly for everyone — coordinators, volunteers, and donors.

Your key responsibilities include:

- **User Management** — You invite new users to the platform and assign them their roles. You can also remove users who should no longer have access.
- **Report Generation** — You can generate reports on project activity, volunteer hours, and funding impact. These reports are used internally and shared with donors where appropriate.
- **Donor Access Control** — You decide what information donors are allowed to see. You control their visibility settings.
- **System Oversight** — You can view the transparency dashboard, which shows a full log of activity across the platform.

You are the only role that can do all of the above. Coordinators and volunteers cannot access these settings.

---

## Section 2: The Admin Dashboard

When you log into SyntraAid, you are taken directly to the Admin Dashboard. This is your central control panel.

### What You Will See

**Summary Cards at the Top**
These cards give you a quick overview of the system at a glance:

- Total number of active users
- Total number of ongoing projects
- Total volunteer hours logged across all projects
- Total number of donors with active access

**Navigation Menu**
On the left side of the screen (or accessible via the menu icon on smaller screens), you will find links to all admin sections:

- Users
- Reports
- Donor Access
- Transparency Log

**Recent Activity Feed**
Below the summary cards, you will see a list of the most recent actions taken on the platform — such as new users joining, projects being updated, or attendance being logged. This feed is read-only. You cannot edit or delete entries here.

---

## Section 3: Inviting and Managing Users

All new users on SyntraAid must be invited by an Administrator. Users cannot sign up on their own.

### How to Invite a New User

1. From the Admin Dashboard, click **Users** in the navigation menu.
2. Click the **Invite User** button.
3. Enter the new user's **email address**.
4. Select their **role** from the dropdown. The available roles are:
   - Coordinator
   - Volunteer
   - Donor
5. Click **Send Invitation**.

The user will receive an email with a link to complete their registration. Once they accept the invitation and set up their profile, they will appear in your users list.

> **Note:** You do not set passwords for users. Each user creates their own password when they accept the invitation.

---

### How to View All Users

1. Click **Users** in the navigation menu.
2. You will see a list of all users on the platform, with their name, email address, role, and account status.
3. You can filter the list by role using the filter options at the top of the page.

---

### How to Remove a User

1. Click **Users** in the navigation menu.
2. Find the user you want to remove. You can use the search bar to find them by name or email.
3. Click on their name to open their profile.
4. Click the **Remove User** button.
5. Confirm the action when prompted.

> **Important:** Removing a user will revoke their access immediately. Their historical records — such as attendance logs and activity history — will remain in the system. This data is not deleted.

---

### How to Activate or Deactivate a User

If a user should temporarily lose access without being fully removed, you can deactivate their account.

1. Click **Users** in the navigation menu.
2. Find the user and click on their name to open their profile.
3. Click **Deactivate User** to suspend their access, or **Activate User** to restore it.
4. Confirm the action when prompted.

> **Note:** A deactivated user cannot log in, but their records remain intact in the system. This is different from removing a user entirely.

---

## Section 4: Generating Reports

Reports in SyntraAid give you a structured summary of activity across the platform. You can generate reports for internal use or to share with donors.

### Types of Reports Available

- **Impact Report** — A formatted summary of how funded projects are performing. This is the report donors can be given access to view.
- **Attendance Export** — Shows hours logged per volunteer, across all projects.
- **KPI Summary** — Shows the status of each project, key performance indicators, and volunteer participation metrics.

---

### How to Generate a Report

1. From the Admin Dashboard, click **Reports** in the navigation menu.
2. Select the report type you want to generate.
3. Use the filter options to narrow your results:
   - Date range
   - Specific project (optional)
   - Specific volunteer (optional, for activity reports)
4. Click **Generate Report**.
5. The report will appear on screen. Review it to make sure the information looks correct.
6. To save it, click **Export**. You can export the report as a **PDF** or **CSV** file.

> **Note:** Total volunteer hours shown in reports are always calculated directly from attendance records. These figures cannot be manually changed.

---

### Before Generating Any Report

Please verify the following before generating a report:

- Attendance records are complete
- Project milestone progress is updated
- Task completion data is accurate
- Volunteer participation records are correct

---

### How to Generate an Impact Report

1. From the Admin Dashboard, click **Reports** in the navigation menu.
2. Select **Impact Report** as the report type.
3. Select a project or date range using the filter options.
4. Click **Generate Report**.
5. Review the report on screen.
6. Click **Export** and choose **PDF** to download.

The Impact Report includes:
- Project progress summaries
- Volunteer activity summaries
- Task completion information
- Attendance contribution data
- Project milestone progress

---

### How to Generate an Attendance Export

1. From the Admin Dashboard, click **Reports** in the navigation menu.
2. Select **Attendance Export** as the report type.
3. Select a project or date range using the filter options.
4. Click **Generate Report**.
5. Review the report on screen.
6. Click **Export** and choose **PDF** or **CSV** to download.

The Attendance Export includes:
- Attendance records
- Volunteer hours logged
- Participation records
- Project attendance summaries

---

### How to Generate a KPI Summary

1. From the Admin Dashboard, click **Reports** in the navigation menu.
2. Select **KPI Summary** as the report type.
3. Select a project or reporting range using the filter options.
4. Click **Generate Report**.
5. Review the KPI information on screen.
6. Click **Export** and choose **PDF** or **CSV** to download.

The KPI Summary includes:
- Total volunteers
- Total hours logged
- Task completion rates
- Project status information

---

### Report Generation Requirement

Impact report generation must complete within two minutes of clicking Generate Report.

---

## Section 5: Managing Donor Access

Donors on SyntraAid have a restricted view of the platform. They can only see what you allow them to see. It is your responsibility as an Administrator to manage these settings carefully.

### What Donors Can See (by default)

When a donor account is created, they can see:

- A list of projects they have funded
- The current status of those projects
- Volunteer participation numbers (not volunteer personal details)
- Funding summaries and impact metrics

### What Donors Can Never See

Regardless of your settings, the following are always hidden from donors:

- Volunteer personal contact information (email addresses, phone numbers)
- Internal notes or coordinator communications
- Detailed task breakdowns not related to their funded project

---

### How to Adjust a Donor's Visibility Settings

1. Click **Donor Access** in the navigation menu.
2. Find the donor account you want to update.
3. Click on their name to open their access profile.
4. You will see a list of visibility toggles. Turn each setting on or off depending on what you want that donor to see.
5. Click **Save Settings**.

The donor's view will update immediately.

---

### How to Revoke Donor Access

1. Click **Donor Access** in the navigation menu.
2. Find the donor and click their name.
3. Click **Revoke Access**.
4. Confirm when prompted.

The donor will no longer be able to log in. Their account is suspended, not deleted.

---

## Section 6: The Transparency Dashboard

The Transparency Dashboard is a full record of everything that has happened on the SyntraAid platform. Every significant action — a project being created, a volunteer logging attendance, a report being exported — is automatically recorded here with a timestamp.

### What You Can See on the Dashboard

- **Activity Type** — What happened (e.g. "Attendance Logged", "User Invited", "Report Exported")
- **User** — Who performed the action
- **Project** — Which project the action is linked to (where applicable)
- **Timestamp** — The exact date and time the action occurred

### Important Rules About the Activity Log

- **The log is read-only.** No one — including administrators — can edit or delete entries in this log.
- **All timestamps are recorded automatically** by the system. They cannot be adjusted.
- Donors with the appropriate visibility setting can see a limited version of this log for projects they have funded.

### How to Use the Dashboard

1. Click **Transparency Log** in the navigation menu.
2. Use the filters at the top to search by:
   - Date range
   - User
   - Project
   - Activity type
3. Scroll through the results to review activity.
4. To export a copy of the log, click **Export Log** and choose PDF or CSV.

---

*SyntraAid | Admin User Manual | Group 12 | Capstone 2026*
*This document is part of the SyntraAid documentation pack maintained in the syntraaid-docs repository.*
