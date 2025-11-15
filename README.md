# pesu-dash

## Overview:
A clean, fast, modern replacement for PESU Academy’s cluttered UI.

## Tech stack

### Frontend
- ReactJS
- Tailwind CSS
- Shadcn UI - https://ui.shadcn.com/
- SWR / React Query for data fetching + caching
- Vercel deployment

### Backend
- FastAPI (Python)
- pesuacademy Python wrapper
- In-memory session manager
- Render deployment

### Security
> [!WARNING]
> No passwords or user credentials are to be stored under any circumstance.
> They are NOT to be logged in production.
> They are NOT to be stored in memory for longer than required.
> It is important to write a proper Terms and Conditions + Privacy policy for this application

- Credentials are used once to authenticate via the wrapper
- Then immediately removed from memory
- Only a temporary Python object (PESUAcademy instance) is kept
- Backend sessions automatically expire on inactivity or server restart

### Dev tools
- Docker to isolate services
- UV package manager
- GH Actions for linting & CI
- .env for local credentials only (never in production)

### API Specifications

#### Base url
```
/api
```

#### Auth
`POST /auth/login`
Authenticate with PESU Academy using the wrapper.
Creates a temporary in-memory wrapper session.
Request:
```json
{
  "username": "PESU_ID",
  "password": "PASSWORD"
}
```

Sessions in memory:
```py
SESSIONS = {
    "b2a3d...": <PESUAcademy instance>,
    "9e12f...": <PESUAcademy instance>,
}
SESSIONS[session_id] = session
```


#### Fetching data
`POST /profile`
```json
{ "session_id": "uuid" }
```
`POST /attendance`
```json
{ "session_id": "uuid", "semester": 4 }
```
`POST /courses`
```json
{ "session_id": "uuid", "semester": 4 }
```
`POST /results`
```json
{ "session_id": "uuid", "semester": 3 }
```
`POST /announcements`
```json
{ "session_id": "uuid" }
```
`POST /timetable`
```json
{ "session_id": "uuid" }
```

#### Materials Workflow
- `/materials/units`
- `/materials/topics`
- `/materials/links`
