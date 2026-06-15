# Wildcat Eats

**A real-time dining hall status app for Northwestern University students.**

Live Site → [dining-hall-website.github.io](https://anayamuzaffar.github.io/Dining-Hall-Website.github.io/)

---

## Overview

Northwestern's current dining app lacks real-time visibility into what's actually happening on the ground — long lines, broken equipment, or stations running out of food. Wildcat Eats fills that gap by giving students a fast, crowd-sourced way to see which dining hall has the shortest wait, report issues, and leave private menu feedback for dining staff.

---

## Features

### For Students
- **Real-time dining traffic** — Rate how busy a hall is (Empty → Packed) with estimated wait times. Traffic data auto-resets every 30 minutes to stay current.
- **Issue reporting** — Report problems like broken dispensers, closed stations, or food running out. Active reports surface for 4 hours before expiring. Limited to 3 submissions per day to ensure data quality.
- **Menu feedback** — Leave private, anonymous ratings and comments on specific menu items, sent directly to dining staff.
- **Points system** — Earn +10 points for every report or feedback submission (up to 3/day), rewarding students who contribute.
- **Shortest wait sorting** — The home screen ranks dining halls by current traffic so students can immediately see where to go.
- **Open/closed status** — Each hall displays live open or closed status with exact hours, including Late Night service for Elder and Plex East.

### For Dining Staff (Admin Dashboard)
- Password-protected admin view
- Filter and search across all menu feedback, issue reports, and traffic data
- Insights panel with top-rated menu items, most common issues, and per-hall ratings
- Sort by newest or oldest submissions

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 (via CDN, no build step) |
| Styling | Vanilla CSS with custom design system |
| Database | Firebase Realtime Database |
| Hosting | GitHub Pages |
| Auth | Anonymous device ID (localStorage) |

---

## Architecture

The app is intentionally built as a **single HTML file** with no build toolchain — making it trivially deployable to GitHub Pages and easy to maintain. React is loaded via CDN and JSX is transpiled client-side with Babel Standalone. Firebase is initialized as an ES module and its methods are exposed globally for the React layer to consume.

All shared data (reports, traffic, feedback) is stored in Firebase Realtime Database and synced live across all connected clients via `onValue` listeners. Per-user state (points, daily submission count) is stored in `localStorage` to avoid unnecessary database reads.

```
Firebase Realtime Database
├── reports/
│   └── {hallId}/
│       └── {pushId} → { category, detail, time, user }
├── traffic/
│   └── {hallId}/
│       └── {userId} → { rating, time }
└── feedback/
    └── {pushId} → { dhId, menuItem, rating, comment, time, user }
```

---

## Running Locally

No installation required. Just clone the repo and open the file:

```bash
git clone https://github.com/your-username/Dining-Hall-Website.github.io.git
cd Dining-Hall-Website.github.io
open index.html
```

Or visit the live site directly at [dining-hall-website.github.io](https://dining-hall-website.github.io).

---

## Firebase Security

The Firebase API key is intentionally included in the client-side code — this is standard practice for Firebase web apps. The key is a project identifier, not an authentication credential. Data access is controlled entirely through **Firebase Security Rules**:

```json
{
  "rules": {
    "reports":  { ".read": true,  ".write": true },
    "traffic":  { ".read": true,  ".write": true },
    "feedback": { ".read": false, ".write": true }
  }
}
```

Menu feedback is **write-only** for students — only staff with direct Firebase Console access can read it, keeping responses private.

---

## Dining Halls Supported

| Hall | Location | Late Night |
|---|---|---|
| Elder Dining Hall | North Campus | Mon–Thu until 10 PM |
| Sargent Dining Hall | North Campus | — |
| Plex East | Mid Campus (Foster-Walker) | Mon–Thu until 10 PM |
| Plex West | Mid Campus (Foster-Walker) | — |
| Allison Dining Hall | South Campus | — |

---

## Motivation

This project was built to address a real gap in the Northwestern dining experience. The official dining app shows menus but gives no insight into real-time conditions — how busy a hall is, whether a station is down, or what students actually think of the food. Wildcat Eats is a student-built alternative that puts that information front and center.

---

## Future Development

Wildcat Eats was built as a standalone prototype, but the ideal end state would be full integration into the existing Dine on Campus platform that Northwestern already uses. Having real-time traffic, issue reporting, and menu feedback living inside the app students already open every day would significantly lower the barrier to participation and put all dining information in one place. A deeper integration could also allow features like push notifications for resolved issues, authenticated submissions tied to student accounts, and direct two-way communication between students and dining staff.

---

## Author

Built by **Anaya Muzaffar** · Northwestern University  
Industrial Engineering/Computer Science · Class of 2029

---

*This is an independent student project and is not affiliated with Northwestern University Dining Services.*
