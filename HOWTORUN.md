# 🚀 BFHL Hierarchy Graph API Visualizer — How To Run

## Project Structure

```
neon-graph-hub/
├── index.js          ← Express.js REST API (Backend → deploy on Render)
├── src/              ← React + Vite frontend (Frontend → deploy on Vercel)
├── index.html        ← Vite entry point
├── vercel.json       ← Vercel config (SPA rewrites)
└── package.json      ← Frontend deps (Vite/React)
```

---

## 1. 🖥️ Run Locally

### Prerequisites

```bash
node -v    # v18+ recommended
npm -v     # v9+
```

### Step 1 — Install ALL dependencies (frontend + backend in same dir)

```bash
npm install
npm install express cors
```

> `express` and `cors` are runtime deps for `index.js` (backend).

### Step 2 — Run the Backend (Express API)

```bash
node index.js
```

Backend will start at: `http://localhost:3000`

Test it:

```bash
curl -X POST http://localhost:3000/bfhl \
  -H "Content-Type: application/json" \
  -d "{\"data\": [\"A->B\", \"A->C\", \"B->D\", \"A->B\", \"hello\"]}"
```

### Step 3 — Run the Frontend (Vite React)

Open a **second terminal** in the same folder:

```bash
npm run dev
```

Frontend will start at: `http://localhost:5173`

---

## 2. ☁️ Deploy Backend on Render

### Setup

1. Go to [https://render.com](https://render.com) → **New → Web Service**
2. Connect your GitHub repo: `joshuahanielgts/BFHL-Hierarchy-Graph-API-Visualizer`
3. Fill in the settings:

| Setting | Value |
|---|---|
| **Name** | `bfhl-api` (or any name) |
| **Branch** | `main` |
| **Runtime** | `Node` |
| **Build Command** | `npm install express cors` |
| **Start Command** | `node index.js` |
| **Instance Type** | Free |

4. Click **Deploy Web Service**
5. Once live, copy your Render URL → e.g., `https://bfhl-api.onrender.com`

### Verify the live endpoint

```bash
curl -X POST https://bfhl-api.onrender.com/bfhl \
  -H "Content-Type: application/json" \
  -d "{\"data\": [\"A->B\", \"A->C\", \"B->D\"]}"
```

> ✅ CORS is already enabled in `index.js` via `app.use(cors())` — no extra config needed on Render.

---

## 3. 🌐 Deploy Frontend on Vercel

### Setup

1. Go to [https://vercel.com](https://vercel.com) → **Add New → Project**
2. Import GitHub repo: `joshuahanielgts/BFHL-Hierarchy-Graph-API-Visualizer`
3. Vercel auto-detects Vite. Confirm these settings:

| Setting | Value |
|---|---|
| **Framework Preset** | `Vite` |
| **Build Command** | `npm run build` |
| **Output Directory** | `dist` |
| **Install Command** | `npm install` |

4. **Add Environment Variable** (so the frontend knows the backend URL):

| Key | Value |
|---|---|
| `VITE_API_URL` | `https://bfhl-api.onrender.com` |

5. Click **Deploy**

### Push updates to Vercel

```bash
git add .
git commit -m "your message here"
git push origin main
```

Vercel auto-deploys on every push to `main`. ✅

---

## 4. 🔁 Full Git Workflow

```bash
# Stage all changes
git add .

# Commit
git commit -m "feat: describe your change"

# Push to GitHub (triggers Vercel auto-deploy)
git push origin main
```

---

## 5. 📡 API Reference

### `POST /bfhl`

**Request:**

```json
{
  "data": ["A->B", "A->C", "B->D", "A->B", "hello", "1->2"]
}
```

**Response:**

```json
{
  "user_id": "jjoshuahaniel_09012006",
  "email_id": "jj9568@srmist.edu.in",
  "college_roll_number": "RA2311003040056",
  "hierarchies": [
    { "root": "A", "tree": { "B": { "D": {} }, "C": {} }, "depth": 3 }
  ],
  "invalid_entries": ["hello", "1->2"],
  "duplicate_edges": ["A->B"],
  "summary": {
    "total_trees": 1,
    "total_cycles": 0,
    "largest_tree_root": "A"
  }
}
```

### CORS

`cors()` middleware is applied globally — all origins are allowed. ✅

---

## 6. ✅ Quick Checklist Before Submission

- [ ] Backend live on Render → `POST /bfhl` returns correct JSON
- [ ] Frontend live on Vercel → UI loads and calls Render backend
- [ ] `VITE_API_URL` env variable set in Vercel dashboard
- [ ] `user_id`, `email_id`, `college_roll_number` filled in `index.js`
- [ ] CORS enabled (`app.use(cors())` present in `index.js`)
