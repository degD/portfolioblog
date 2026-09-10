---
date: "2026-09-10T22:34:24+03:00"
draft: false
title: "GYS"
---

[GYS](https://gitlab.com/den.ege.der/gys) stands for "Income Management System".
An integrated system designed to manage corporate expenses, approvals, and
budget tracking. The system utilizes a role-based access control (RBAC) model
with four primary roles.

| Role               | Responsibilities                                                                               |
| :----------------- | :--------------------------------------------------------------------------------------------- |
| **Employee**       | Submits expense reports.                                                                       |
| **Manager**        | Reviews, approves, or rejects expense submissions.                                             |
| **Accountant**     | Modifies expense items prior to approval and processes reimbursements post-approval.           |
| **Senior Manager** | Defines budgets and thresholds, analyzes financial charts, and monitors expenditure forecasts. |

## Key Logic

- **Hierarchy:** All users possess an Employee account; Managers inherit Employee privileges.
- **Budgeting:** Budget checks are automated. Reimbursements are processed based on remaining 
- budget limits, and employees are notified upon budget overrun.
- **Customization:** All users can update their passwords and personalize the UI theme.

## Tech Stack

- **Backend:** Python Flask
- **Frontend:** HTML, CSS, JavaScript (Styled with Pico.css, Data visualization via Chart.js)
- **Database:** SQLite

### Security

Authentication is based on SBA (Session-Based Authentication). Passwords are hashed using `bcrypt`.
Current session key is hardcoded for demonstration purposes. The database is unencrypted.
