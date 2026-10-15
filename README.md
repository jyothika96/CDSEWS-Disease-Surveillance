# Community Disease Surveillance and Early-Warning System (CDSEWS)

> **Educational Prototype Notice:**
> This full-stack application is an educational prototype designed for academic demonstration, epidemiologist training, and public health surveillance simulation. It never utilizes real patient-identifiable healthcare records. All datasets are synthetically generated.
> Automated early-warning alerts represent statistical pattern deviations (such as threshold breaches and weekly acceleration spikes) intended to prompt human epidemiological review, and do not constitute confirmed clinical diagnoses or official epidemic declarations.

---

## Tech Stack Overview

- **Frontend:** React 18, Vite, Chart.js, react-chartjs-2, Lucide React Icons
- **Backend:** Node.js (v18+ or v24+), Express 4, RESTful APIs, CORS, centralized error handling
- **Database:** MySQL 8.0, parameterized SQL queries via `mysql2/promise`, relational foreign keys, performance indexes
- **Styling:** Custom healthcare analytics theme (clean blue, white, and slate palette with responsive layouts, metric cards, interactive charts, and modal dialogs)

---

## System Architecture & Features

1. **Epidemiological Dashboard**
   - Real-time aggregate indicators (total cumulative cases, hospitalized count, 7-day incidence with week-over-week growth percentage).
   - Monitored infrastructure coverage (facilities across geographic regions).
   - High-urgency alert queue with one-click investigation review.
   - Interactive 30-day epidemiological incidence curve (Chart.js Line) and disease proportion chart (Chart.js Doughnut).
   - Regional surveillance matrix with attack rates per 100,000 population and risk levels.

2. **Case Reporting Form**
   - Input validation for disease category, region, facility, report date, case count, clinical severity, and suspected transmission vector.
   - Dynamic cascading facility dropdown filtered by the chosen surveillance region.
   - Instant threshold evaluation: if reported cases breach the weekly threshold, an automatic early-warning banner is immediately displayed.

3. **Multi-Criteria Search & Filtering**
   - Search across field notes and suspected transmission sources.
   - Filter by disease category, region, clinical severity (Mild, Moderate, Severe), and date intervals.
   - Tabular view with pagination, inline editing, and deletion.

4. **Early-Warning Threshold Alerts Engine**
   - Automated 7-day rolling evaluation engine comparing regional cases against disease-specific thresholds and rapid spike percentage triggers.
   - Status filters: `All`, `New`, `Investigating`, `Resolved`, `False Alarm`.
   - Severity categorization: `Critical`, `High`, `Medium`, `Low`.

5. **Alert Management & Investigation Tracking**
   - Formal transition workflow: review alert, mark as `Investigating`, `Resolved`, or `False Alarm`.
   - Audit trail capturing reviewer name, timestamp, previous status, new status, specific containment actions taken, and clinical notes.
   - Dedicated cross-system Investigation Tracker feed for monitoring public health field responses.

6. **Reports & Data Export**
   - Custom analytical reporting filtered by date range, disease, region, and severity.
   - Summary statistics (hospitalization rate, distinct pathogens, affected regions).
   - One-click RFC-4180 compliant CSV export with proper download headers.

7. **Configurable Alert Thresholds**
   - Interactive settings modal allowing epidemiologists to tune weekly alert thresholds (cases) and spike sensitivity (%) per disease category.

---

## Directory Structure

```text
community-disease-surveillance/
├── database/
│   └── schema.sql                # Complete MySQL 8.0 schema, indexes, FKs, & sample data
├── server/
│   ├── .env                      # Database credentials and port config
│   ├── .env.example              # Template environment variables
│   ├── package.json              # Backend dependencies
│   └── src/
│       ├── config/
│       │   ├── db.js             # MySQL connection pool & health diagnostics
│       │   └── setupDb.js        # Automated DB creation and seed script
│       ├── controllers/          # Business logic (Dashboard, Cases, Alerts, Reports)
│       ├── middleware/           # Centralized error handler & validators
│       ├── routes/               # Express REST API route definitions
│       ├── services/
│       │   └── thresholdService.js # Automated threshold & spike detector
│       └── server.js             # Express app entry point
├── client/
│   ├── package.json              # Frontend dependencies (React, Vite, Chart.js)
│   ├── vite.config.js            # Vite configuration with API reverse proxy
│   ├── index.html                # HTML entry point with medical typography
│   └── src/
│       ├── api.js                # Centralized REST API client
│       ├── App.jsx               # Master state & view navigation manager
│       ├── index.css             # Healthcare UI design system
│       ├── main.jsx              # React 18 bootstrap
│       └── components/           # Dashboard, Forms, Tables, Alerts, Tracker
├── package.json                  # Root runner scripts
└── README.md                     # Documentation & setup guide
```

---

## Prerequisites

1. **Node.js**: v18.0.0 or later (Tested with Node v24.11.0 and npm 11.6.1).
2. **MySQL 8.0**: MySQL Server 8.0 running locally on port `3306` (service `MySQL80`).

---

## Step-by-Step Installation & Setup

### Step 1: Open the Project Directory
Open PowerShell or your preferred terminal in the project directory:
```powershell
cd C:\Users\Dell\.gemini\antigravity\scratch\community-disease-surveillance
```

### Step 2: Configure Database Credentials in `server/.env`
Open `server/.env` in any text editor and specify your MySQL root password:
```env
# Server Configuration
PORT=5000
NODE_ENV=development
CLIENT_URL=http://localhost:5173

# MySQL 8 Database Configuration
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_actual_mysql_password
DB_NAME=disease_surveillance
```

*(Replace `your_actual_mysql_password` with your local MySQL password).*

---

### Step 3: Initialize and Seed the Database
Run the automated database setup script from the root folder:
```powershell
npm run db:setup
```
*Or from inside the `server` folder:*
```powershell
cd server
npm run db:setup
```

**What this command does:**
1. Connects to your MySQL 8.0 server.
2. Executes `database/schema.sql`, creating the `disease_surveillance` database.
3. Builds all 8 relational tables (`users`, `regions`, `facilities`, `disease_categories`, `threshold_settings`, `case_reports`, `alerts`, `alert_reviews`) with indexes and foreign key constraints.
4. Populates realistic synthetic epidemiological seed data (40+ case reports, regional matrices, and historical alert reviews with relative dates).
5. Prints a table verification summary.

> **Tip:** You can also import `database/schema.sql` directly into MySQL Workbench or via the MySQL CLI:
> ```powershell
> & "C:\Program Files\MySQL\MySQL Server 8.0\bin\mysql.exe" -u root -p < database/schema.sql
> ```

---

### Step 4: Start the Backend API Server
In terminal 1:
```powershell
npm run server
```
*Output confirmation:*
```text
======================================================
 CDSEWS Backend API server running on port 5000
 Endpoint: http://localhost:5000/api/health
======================================================
 MySQL 8.0 connected to database: 'disease_surveillance' at localhost:3306
```

---

### Step 5: Start the Frontend Application
In terminal 2 (open a new tab or window):
```powershell
npm run client
```
*Vite will start the client:*
```text
  VITE v6.1.0  ready in 280 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

Open **`http://localhost:5173`** in your browser.

---

## How to Verify Everything Works

1. **Verify Backend Health Endpoint:**
   - In browser or terminal, visit `http://localhost:5000/api/health`.
   - You should see `status: "healthy"` and `database.connected: true`.
2. **Verify Database Connection in Header:**
   - On the top right of the web app, notice the green pill: `● MySQL 8 Connected`.
3. **Verify Dashboard Metrics & Charts:**
   - Confirm the 30-Day Epidemiological Case Incidence Curve renders smoothly with Chart.js.
   - Confirm disease breakdown doughnut chart and regional attack rates per 100k display data.
4. **Test Logging a Case & Threshold Alert Trigger:**
   - Navigate to **"Report New Case"**.
   - Select:
     - Disease: **Dengue Fever**
     - Region: **Coastal Delta Zone**
     - Facility: **Delta Port General Hospital**
     - Cases: **30**
   - Click **Submit Surveillance Report**.
   - Notice the instant **AUTOMATIC EARLY-WARNING ALERT TRIGGERED!** banner appearing immediately above the form, indicating that weekly cases exceeded the threshold.
5. **Test Alert Review & Investigation Tracking:**
   - Click **"Threshold Alerts"** on the sidebar.
   - Click **"Review & Track"** on any alert.
   - Switch status to `Investigating` or `Resolved`, enter field notes (e.g., *"Completed vector larvicide treatment in Ward 4"*), and click **Record Investigation Entry**.
   - Notice the status immediately updates and the note is chronologically appended to the audit history.
6. **Test Investigation Tracker:**
   - Click **"Investigation Tracker"** on the sidebar to view the global timeline of all past field responses and containment notes.
7. **Test Filtered CSV Export:**
   - Navigate to **"Reports & Export"**.
   - Select a date range or disease filter.
   - Click **Export CSV Archive** to download a clean, structured `.csv` file.
8. **Test Threshold Configuration:**
   - Click **"Threshold Config"** at the bottom of the sidebar.
   - Adjust the weekly threshold limit or spike sensitivity for any disease and click **Save**.

---

## REST API Reference

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/api/health` | Service health check and MySQL connection diagnostic |
| `GET` | `/api/dashboard/summary` | Aggregated surveillance metrics, regional matrix, and timeline data |
| `GET` | `/api/meta/all` | Reference metadata (regions, facilities, diseases, users) |
| `GET` | `/api/meta/thresholds` | Configured alert thresholds for each disease |
| `PUT` | `/api/meta/thresholds/:diseaseId` | Update weekly threshold limit or spike sensitivity |
| `GET` | `/api/cases` | Paginated case reports with disease, region, severity, and date filters |
| `POST` | `/api/cases` | Log a new case report and trigger automated threshold evaluation |
| `PUT` | `/api/cases/:id` | Update an existing case report and re-evaluate threshold |
| `DELETE` | `/api/cases/:id` | Remove a case report by ID |
| `GET` | `/api/alerts` | List early-warning alerts filtered by status and severity |
| `GET` | `/api/alerts/:id` | Get alert details, recent related cases, and investigation history |
| `PUT` | `/api/alerts/:id/status` | Transition status (`Investigating`, `Resolved`, `False Alarm`) with notes |
| `GET` | `/api/investigations` | Holistic timeline of all outbreak investigations and audit records |
| `POST` | `/api/alerts/:alertId/reviews` | Append a field investigation progress note to an alert |
| `GET` | `/api/reports/cases` | Analytical summary and tabular dataset for custom date intervals |
| `GET` | `/api/reports/export-csv` | Stream RFC-4180 formatted CSV download with custom filters |

---

## Educational Disclaimer
*This software is intended solely for educational, research, and technical evaluation purposes. It does not replace established epidemiological reporting guidelines from agencies such as the WHO, CDC, or national ministries of health. Do not input protected health information (PHI) or real patient records.*
