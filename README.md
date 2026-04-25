# CityPulse

CityPulse is a citizen-government infrastructure reporting platform.

People can report civic issues (potholes, cracks, flooding, broken lights, sidewalks), track status updates, and view transparency actions. Authorities get prioritization, escalation workflows, and analytics.

## Highlights

- Citizen issue reporting with photos and map location
- AI-assisted damage classification and severity support
- Priority scoring with weather/location/population weighting
- Public transparency wall and report action timeline
- In-app notification flows for reporters/admins
- Admin dashboard for report operations and analytics
- Firebase-backed real-time data (Firestore/Auth/Storage/Functions)

## Monorepo Structure

- `web-app/` — React frontend (citizen + admin portals)
- `backend/` — Firebase config, Firestore/Storage rules, functions source
- `backend/functions/` — Cloud Functions (classification, scoring, scheduling, notifications)
- `logo/` — branding assets

## Tech Stack

- Frontend: React, React Router, Tailwind CSS, Framer Motion
- Maps/Charts: Google Maps JS API, Recharts, Chart.js
- Backend: Firebase Auth, Firestore, Storage, Cloud Functions (Node 22)
- AI: Google Gemini via server-side functions/API routes

## Prerequisites

- Node.js 20+ (project uses modern tooling; Functions runtime is Node 22)
- npm
- Firebase CLI (`npx firebase ...` works without global install)
- A Firebase project

## Quick Start (Local Development)

1. Install root dependencies:

```bash
npm install
```

2. Install web app dependencies:

```bash
cd web-app
npm install
cd ..
```

3. Install backend API dependencies:

```bash
cd backend
npm install
cd ..
```

4. Install Cloud Functions dependencies:

```bash
cd backend/functions
npm install
cd ../..
```

5. Configure environment:
   - Copy `web-app/.env.example` to `web-app/.env`
   - Fill `REACT_APP_FIREBASE_*` values from your Firebase Web app config
   - Add `REACT_APP_GOOGLE_MAPS_API_KEY` if using map features

6. Run full stack locally from repo root:

```bash
npm run dev
```

This starts:
- Backend API server from `backend/api/server.js`
- React app from `web-app` (default `http://localhost:3000`)

## Key Scripts

### Root

- `npm run dev` — run API + web in parallel
- `npm run dev:lan` — LAN-friendly dev mode
- `npm run deploy:hosting` — build web and deploy Firebase Hosting
- `npm run deploy:backend` — deploy Firestore/Storage/Functions from `backend`
- `npm run deploy:all` — deploy hosting + backend

### Backend (`backend/`)

- `npm run deploy:firestore`
- `npm run deploy:storage`
- `npm run deploy:functions`
- `npm run deploy:stack`

## Firebase Setup

1. Create Firebase project.
2. Enable:
   - Authentication (Email/Password, Google)
   - Firestore
   - Storage
3. Set project id in:
   - `backend/.firebaserc` (copy from `backend/.firebaserc.example`)
4. Deploy rules/indexes/functions from `backend/`:

```bash
firebase deploy --only firestore,storage,functions
```

## Cloud Functions Overview

Functions implement report lifecycle automation:

- Report enrichment and profile denormalization
- AI classification hooks for report images
- Priority score calculation and weather-risk updates
- Escalation scheduler for stale pending reports
- Notification fan-out (reporters/admins/nearby users)
- Weekly aggregate report generation
- Gemini API endpoints for classify/chat/voice and translation endpoint

## Translation Notes

- UI language selector exists in the web app.
- Browser-native translation (Edge/Chrome popup) is recommended for full-page generic UI translation experience.
- Report-specific multilingual blocks are generated server-side for report content fields.

## Data Collections (High-Level)

- `users` — user profiles and roles
- `publicProfiles` — public display profile fields
- `reports` — issue reports
- `reportActions` — transparency timeline entries
- `notifications` — in-app inbox notifications
- `reportComments` — report discussion/comments
- `badges` — user gamification metadata

## Deployment

### Hosting only

```bash
npm run deploy:hosting
```

### Backend only

```bash
npm run deploy:backend
```

### Full deploy

```bash
npm run deploy:all
```

## Security

- Firestore access is controlled by `backend/firestore.rules`
- Storage access is controlled by `backend/storage.rules`
- Secrets are managed with Firebase Functions Secret Manager

## Troubleshooting

- If Firebase features are in demo mode, verify `web-app/.env` values and restart dev server.
- If functions deployment prompts for secrets, set them first using:

```bash
firebase functions:secrets:set SECRET_NAME
```

- If maps are blank, verify Google Maps API key and key restrictions.

## License

Private/internal project unless you explicitly add an OSS license file.
