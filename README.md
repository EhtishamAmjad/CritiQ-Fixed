# CritiQ
A decentralised web application for handling customer reviews and feedback for products and services.

## Bugs Fixed (Technical Assessment)

### Bug 1 — MongoDB Connection
- **Root cause:** `process.env.USER` is a reserved OS variable (always = system username like `"root"`), not the MongoDB username. Connection string was always wrong, causing silent auth failure.
- **Fix:** Replaced with `process.env.MONGODB_URI` (single connection string variable).
- **Files changed:** `server/conn/db.js`, `server/controller/conn/db.js`, added `server/.env.example`

### Bug 2 — Connect Wallet Button Positioning
- **Root cause 1:** `flex-col-reverse` in Navbar caused the button to visually jump above the search bar on 640–768px screens and shift on resize.
- **Root cause 2:** Mobile dropdown had `z-index: 10`, too low, letting page content bleed through.
- **Root cause 3:** `userData` was initialised to `{}` instead of `null`, making every auth guard always true and causing a runtime crash on `walletAddress.substring()`.
- **Fix:** `flex-col-reverse` → `flex-col`, `z-10` → `z-50`, `userData` init to `null`, added optional chaining.
- **Files changed:** `client/src/components/Navbar.jsx`, `client/src/context/StateProvider.jsx`

---

## Setup & Running

### Prerequisites
- Node.js v18+
- A MongoDB Atlas account (free tier works)
- MetaMask browser extension

### Step 1 — Clone
```bash
git clone <your-repo-url>
cd CritiQ
```

### Step 2 — Install all dependencies
```bash
npm install npm-run-all --save-dev
npm run install:all
```

### Step 3 — Set up environment variables
```bash
cp server/.env.example server/.env
```
Open `server/.env` and fill in your MongoDB URI:
```
MONGODB_URI=mongodb+srv://youruser:yourpass@cluster0.abc.mongodb.net/critiq?retryWrites=true&w=majority
```
Get this string from: **MongoDB Atlas → Your Cluster → Connect → Drivers → copy the connection string**

### Step 4 — Run the project
```bash
npm start
```
- Frontend runs at: `http://localhost:5173`
- Backend runs at: `http://localhost:5000`

---

## Project Structure
```
CritiQ/
├── client/          # React + Vite frontend
│   └── src/
│       ├── components/Navbar.jsx     ← Bug 2 fixed here
│       ├── context/StateProvider.jsx ← Bug 2 fixed here
│       └── pages/
├── server/          # Express + MongoDB backend
│   ├── conn/db.js                   ← Bug 1 fixed here
│   ├── .env.example                 ← New: copy to .env
│   ├── routes/
│   ├── controller/
│   └── models/
└── package.json     # Root: runs both client and server
```

## License
MIT
