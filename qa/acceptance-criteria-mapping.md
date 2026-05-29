# SyntraAid Acceptance Criteria Mapping Document

## Overview

This document maps SyntraAid product requirements to measurable acceptance criteria and verification methods.

Each acceptance criterion is based on the approved PRD, user flows, dashboard structure, and MVP feature requirements.

The product is considered ready for demonstration only after all criteria below are verified successfully.

---

# Acceptance Criteria Mapping

| Feature Area                | Acceptance Criteria                                                                                                                                             | Verification Method                         |
| --------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| Authentication              | All user roles can log in and access only the views and actions permitted for their role.                                                                       | Manual role-based access test               |
| Volunteer Management        | Coordinators can search volunteers by skill and availability and assign them to projects.                                                                       | End-to-end volunteer assignment walkthrough |
| Volunteer Hours             | Volunteer profile contribution totals match submitted attendance records.                                                                                       | Verify cumulative attendance totals         |
| Project Management          | Coordinators can create projects with goals, milestones, timelines, and status tracking.                                                                        | Full project lifecycle walkthrough          |
| Task Tracking               | Coordinators can create, assign, update, and complete tasks.                                                                                                    | End-to-end task management test             |
| Attendance Logging          | Volunteers can submit attendance records by selecting a project and entering hours worked.                                                                      | Attendance submission and verification test |
| Transparency Dashboard      | The dashboard displays activity feed updates, contribution tracking, timestamps, and volunteer impact summaries.                                                | Dashboard activity verification             |
| Reporting and Impact Export | The system generates donor-ready reports with KPI summaries and supports PDF and CSV export.                                                                    | Timed export generation test                |
| Notifications               | Volunteers receive task assignment notifications and deadline reminders, while coordinators receive blocked task alerts and milestone completion notifications. | Notification delivery verification          |
| Mobile Access               | Volunteers can access tasks, attendance logging, and notifications on mobile devices.                                                                           | Mobile device walkthrough                   |
| Donor Dashboard Access      | Donors can access only projects linked to their funded programs.                                                                                                | Donor access restriction test               |
| Donor Project Visibility    | Donors can view project progress summaries and impact indicators for funded projects.                                                                           | Donor dashboard review walkthrough          |
| Donor Funding Summary       | Donors can review overall programme health and project progress indicators.                                                                                     | Funding summary verification                |

---

# Verification Scenarios

## Authentication Verification

### Expected Result

* Users can log in successfully.
* Users only see role-permitted dashboards and actions.

### Pass Condition

Role restrictions function correctly for:

* Volunteer
* Coordinator
* NGO Administrator
* Donor

---

## Volunteer Assignment Verification

### Expected Result

* Coordinators can locate volunteers using the searchable roster.
* Volunteers can be assigned to projects successfully.

### Pass Condition

* Assigned volunteers appear within the project workspace immediately after assignment.

---

## Task Management Verification

### Expected Result

* Tasks can be created and assigned.
* Task status updates appear in real time.

### Pass Condition

* Coordinators can monitor all task progress from the task board.

---

## Attendance Verification

### Expected Result

* Volunteers submit attendance entries against projects.
* Attendance records store hours worked and optional notes.

### Pass Condition

* Attendance totals update correctly within contribution tracking.

---

## Reporting Verification

### Expected Result

* KPI summaries are generated successfully.
* Reports export correctly in PDF and CSV formats.

### Pass Condition

* Report generation completes within two minutes.

---

## Donor Dashboard Verification

### Expected Result

* Donors can view funded project progress and impact summaries.
* Donors cannot access unrelated projects.

### Pass Condition

* Dashboard visibility matches assigned donor permissions.

---

# Pass and Fail Criteria

## Pass

The product passes acceptance testing when:

* All mapped features operate successfully
* Role permissions function correctly
* Project and task flows complete without failure
* Attendance records calculate accurately
* Dashboard data updates correctly
* Reporting exports successfully
* Donor visibility restrictions operate correctly

---

## Fail

The product fails acceptance testing if:

* Users access unauthorized areas
* Task or attendance data is lost or inaccurate
* Reporting exports fail
* Dashboard activity does not update correctly
* Donor visibility restrictions fail
* Core project workflows cannot be completed

---

# Demonstration Evidence

The following evidence should be prepared before the capstone demonstration:

* Role-based login walkthrough
* Volunteer assignment demonstration
* Project lifecycle walkthrough
* Task tracking demonstration
* Attendance logging verification
* Transparency dashboard walkthrough
* Report export demonstration
* Donor dashboard access verification
* Mobile access walkthrough
