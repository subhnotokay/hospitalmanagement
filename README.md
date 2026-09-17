# CityCare Hospital – Automated Appointment Management System (RPA)

An end-to-end Robotic Process Automation (RPA) workflow developed using **UiPath Studio Web**, **Google Workspace**, and **GitHub Pages**. This system automates outpatient appointment intake, performs regex-based data validation, dispatches responsive HTML appointment confirmation cards via Gmail, and maintains structured audit trails in Google Sheets.

---

## 📌 Project Overview
Manual healthcare appointment validation often encounters data entry inaccuracies (malformed emails, invalid contact numbers) and delayed patient notifications. This automation extracts real-time appointment records from a web portal, validates patient demographics against clinical standards, updates central Google Sheets databases, and dispatches dynamic patient notifications.

---

## 🚀 Key Features
- **Zero-Footprint Cloud Automation:** Built and executed purely via UiPath Studio Web without local software dependencies.
- **Web Scraping & DOM Traversal:** Automated extraction of patient queues directly from a hosted web application.
- **Regex-Driven Data Cleansing:**
  - **Email Validation:** Standard RFC-compliant pattern verification.
  - **Phone Number Validation:** Indian standard 10-digit mobile verification (`^[6-9]\d{9}$`).
- **Dynamic HTML Email Templating:** Sends structured appointment cards with patient metadata and scheduling information via Gmail.
- **Audit Logging & Exception Routing:** Routes successful records to `ProcessedAppointments` and captures failures/exceptions in a dedicated `Logs` sheet.

---

## 🛠️ Tech Stack & Integrations
| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **RPA Platform** | UiPath Studio Web | Workflow design, execution, and cloud orchestration |
| **Web Portal** | HTML5 / JavaScript (GitHub Pages) | Outpatient portal data source |
| **Database / Sheets** | Google Sheets API | Central record repository (`Hospital_Appointment_DB`) |
| **Email Service** | Google Workspace (Gmail) | Automated dispatch of HTML confirmation cards |
| **Expression Engine** | VB.NET | String operations, DataTable manipulation, Regex matching |

---

## 📊 Workflow Architecture & Data Flow
1. **Spreadsheet Initialization:** Connects to `Hospital_Appointment_DB` and reads schema definitions for `Patients`, `Doctors`, `ProcessedAppointments`, and `Logs`.
2. **Browser Extraction:** Navigates to the hosted portal, extracts the live appointment queue into `dtAppointments`.
3. **Iterative Processing (`For Each Row`):**
   - Extracts demographic variables (`patientId`, `patientName`, `email`, `phoneNumber`, `department`, `doctorName`, `appointmentDate`, `appointmentTime`).
   - Runs validation rules (`emailValid And phoneValid`).
4. **Branching Logic:**
   - **Then (Valid Records):**
     - Generates and dispatches responsive HTML email via Gmail.
     - Sets status to `Confirmed`.
     - Appends row to `dtProcessed` (8-column schema).
     - Logs success event to `dtLogs` (5-column schema).
   - **Else (Invalid Records):**
     - Logs validation exception to `dtLogs` with root cause (e.g., `Invalid Email/Phone`).
5. **Data Persistence:** Writes back accumulated data to Google Sheets via `Write Range`.

---

## 🧪 Test Case Matrix
| Test Case ID | Patient ID | Test Scenario | Expected Outcome | Execution Result |
| :--- | :--- | :--- | :--- | :--- |
| **TC-01** | `P1001` | Valid demographics & standard format | Dispatches HTML email, writes to `ProcessedAppointments` and `Logs` | **Pass** (Confirmed) |
| **TC-02** | `P1004` | Malformed email syntax | Skips email dispatch, records failure in `Logs` | **Pass** (Rejected) |
| **TC-03** | `P1005` | Invalid 10-digit phone number format | Skips email dispatch, records failure in `Logs` | **Pass** (Rejected) |

---

## 📂 Google Sheets Schema
- **`ProcessedAppointments` (8 Columns):** `PatientID`, `PatientName`, `Email`, `Department`, `Doctor`, `AppointmentDate`, `Status`, `ProcessedTime`
- **`Logs` (5 Columns):** `Timestamp`, `PatientID`, `Action`, `Result`, `Message`

---

## 👨‍💻 Author
- **Developer:** Shubham Salami Magar
- **Course / Track:** Bachelor of Computer Applications (BCA) - RPA Specialization
