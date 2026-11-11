
# Sweety - Blood Glucose Tracker

Sweety is a full-stack blood glucose tracking application built with React, Node.js, Express, and PostgreSQL.

Patients can securely record up to four glucose readings per day, monitor their glucose trends, and view daily and monthly reports.

Sweety also includes a demo Doctor Portal where doctors can request access to patient reports. Patients must approve the connection before their reports become accessible.

## Tech Stack

### Frontend
- React
- Vite
- JavaScript
- CSS
- React Testing Library
- Vitest

### Backend
- Node.js
- Express.js
- PostgreSQL
- JWT authentication
- bcryptjs password hashing

## Features

### Patient Portal
- Patient registration and login
- Secure JWT authentication
- Add, edit, and delete glucose readings
- Maximum four readings per day
- Four reading slots:
  - Morning fasting
  - After breakfast
  - After lunch
  - After dinner
- Glucose value validation
- Duplicate reading prevention
- Daily glucose summaries
- Month-to-date reports
- Previous-month reports
- Glucose trend charts
- Reading history
- Patient profile and settings
- Doctor access management

### Doctor Portal
- Doctor demo account registration and login
- Send connection requests using patient email
- View pending and approved patient connections
- View approved patient glucose reports
- Select patient and report date
- Daily and monthly glucose statistics
- Glucose trend visualization
- Read-only access to patient reports

### Patient Privacy
- Patients can approve or reject doctor requests
- Patients can revoke approved doctor access
- Doctors cannot edit or delete patient readings
- Doctor report access requires an approved connection
- Patient reading endpoints are protected by authentication and role checks

## Project Structure

```text
sweety-glucose-tracker/
|-- client/
|   |-- src/
|   |   |-- components/
|   |   |-- context/
|   |   |-- pages/
|   |   |-- services/
|   |   |-- App.jsx
|   |   `-- styles.css
|   `-- package.json
|
|-- server/
|   |-- src/
|   |   |-- middleware/
|   |   |-- routes/
|   |   |-- utils/
|   |   |-- app.js
|   |   |-- db.js
|   |   |-- initDb.js
|   |   |-- schema.sql
|   |   `-- server.js
|   |-- tests/
|   `-- package.json
|
|-- .env.example
|-- .gitignore
|-- package.json
`-- README.md
```

## Prerequisites

- Node.js 20 or later
- PostgreSQL 14 or later
- npm
- Git

## Installation

### 1. Clone the repository

```bash
git clone YOUR_GITHUB_REPOSITORY_URL
cd sweety-glucose-tracker
```

Replace `YOUR_GITHUB_REPOSITORY_URL` with the repository URL.

### 2. Install dependencies

```bash
npm install
npm run install:all
```

### 3. Create the PostgreSQL database

Create a database named `sweety`.

```sql
CREATE DATABASE sweety;
```

### 4. Configure environment variables

Create `server/.env` with:

```env
DATABASE_URL=postgresql://postgres:YOUR_PASSWORD@localhost:5433/sweety
JWT_SECRET=YOUR_LONG_RANDOM_SECRET
PORT=5000
CLIENT_URL=http://localhost:5173
```

The port `5433` is used in the developer's local PostgreSQL setup. Change it if PostgreSQL runs on a different port.

Optionally create `client/.env`:

```env
VITE_API_URL=http://localhost:5000/api
```

Never commit real passwords or JWT secrets.

### 5. Initialize the database

```bash
npm run db:init
```

### 6. Seed demo data (optional)

```bash
npm run db:seed
```

Demo patient account:

- Email: `demo@sweety.app`
- Password: `Demo@1234`

The demo credentials are for local development only.

### 7. Start the application

```bash
npm run dev
```

Frontend:

http://localhost:5173

Backend:

http://localhost:5000

Health endpoint:

http://localhost:5000/api/health

## API Endpoints

### Authentication

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | /api/auth/register | Register a user |
| POST | /api/auth/login | Log in |
| GET | /api/auth/me | Get current user |

### Patient Readings

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | /api/readings | Get patient readings |
| POST | /api/readings | Add a reading |
| PUT | /api/readings/:id | Update a reading |
| DELETE | /api/readings/:id | Delete a reading |

### Patient Reports

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | /api/reports?date=YYYY-MM-DD | Daily and monthly reports |

### Doctor Connections

| Method | Endpoint | Description |
| --- | --- | --- |
| POST | /api/connections/request | Doctor sends patient request |
| GET | /api/connections/doctor | Doctor views connections |
| GET | /api/connections/patient | Patient views connections |
| PATCH | /api/connections/:id/respond | Patient approves or rejects |
| DELETE | /api/connections/:id | Patient revokes access |

### Doctor Reports

| Method | Endpoint | Description |
| --- | --- | --- |
| GET | /api/doctor-reports/:patientId?date=YYYY-MM-DD | Approved patient's read-only report |

Protected endpoints require:

```text
Authorization: Bearer YOUR_JWT_TOKEN
```

## Validation

The application validates glucose readings on the frontend and backend.

- Glucose values must be whole numbers between 40 and 600 mg/dL
- Maximum four readings per day
- Only one reading per time slot per day
- Future dates are not allowed
- Invalid or missing values are rejected
- Patients can access only their own readings

PostgreSQL enforces uniqueness for each user's reading date and time slot.

## Security

- Passwords are hashed with bcryptjs
- JWT authentication protects private endpoints
- Role-based authorization separates patients and doctors
- Database queries use parameterized SQL
- Doctor report access requires patient approval
- Access is checked for each doctor report request
- Patients can revoke doctor access
- Environment secrets are excluded from Git

Doctor registration is intended for demonstration purposes and does not verify professional medical credentials.

## Testing

Run all tests:

```bash
npm test
```

Run the frontend production build:

```bash
npm run build
```

Latest local verification:

- Backend: 8 tests passed
- Frontend: 2 tests passed
- Frontend production build successful
- Backend syntax checks successful

Automated tests cover glucose reading validation, report date calculations, and frontend form validation.

Doctor connection and report workflows were also checked manually.

## Assumptions

- Glucose readings are measured in mg/dL
- Each patient can record up to four readings per calendar day
- Reports use a selected calendar date
- Month-to-date covers the start of the month through the selected date
- Previous-month reports cover the complete preceding month
- Doctors must receive patient approval before viewing reports

## Limitations and Future Improvements

- Doctor accounts are demo accounts without professional credential verification
- No production medical compliance certification is claimed
- Automated doctor authorization integration tests can be expanded
- Add rate limiting and refresh-token rotation
- Add email notifications for connection requests
- Add Docker Compose for easier deployment
- Deploy the application with managed PostgreSQL

## Disclaimer

Sweety is a glucose tracking application for educational and demonstration purposes. It does not provide medical diagnosis or treatment recommendations.
