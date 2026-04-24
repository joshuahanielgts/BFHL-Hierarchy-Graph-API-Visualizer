# 🌐 BFHL Neural Grid | Hierarchy Graph API

A high-performance Full-Stack Graph Analysis Dashboard built for the Bajaj Full Stack Engineering Challenge. This system interfaces with a secure Node.js REST API to process complex hierarchical data strings, detect pure cycles, and visualize multi-tree hierarchies.

### 🔗 Live Transmissions
* **Frontend Dashboard (Vercel):** [https://your-frontend-url.vercel.app](https://your-frontend-url.vercel.app)
* **Backend Core API (Render):** [https://your-backend-url.onrender.com](https://your-backend-url.onrender.com)

---

## ⚙️ Core Architecture & Tech Stack

The architecture is split into a robust backend processing engine and a high-fidelity, retro-terminal inspired frontend visualizer.

* **Frontend:** React, Vite / Next.js, Tailwind CSS (Terminal/CRT Aesthetic)
* **Backend:** Node.js, Express.js, CORS
* **Deployment:** Vercel (Client) & Render (Server)

---

## 📡 API Specification

### `POST /bfhl`
Processes an array of directed edge strings (`X->Y`) to construct trees and identify cyclic dependencies.

**Request Headers:**
* `Content-Type: application/json`

**Request Body:**
```json
{
  "data": ["A->B", "A->C", "B->D", "C->E", "E->F"]
}
Response Schema:

JSON
{
  "user_id": "joshuahaniel_DDMMYYYY",
  "email_id": "jj9568@srmist.edu.in",
  "college_roll_number": RA2311003040056,
  "hierarchies": [
    {
      "root": "A",
      "tree": {
        "B": { "D": {} },
        "C": { "E": { "F": {} } }
      },
      "depth": 4
    }
  ],
  "invalid_entries": [],
  "duplicate_edges": [],
  "summary": {
    "total_trees": 1,
    "total_cycles": 0,
    "largest_tree_root": "A"
  }
}
🧠 Algorithmic Execution Rules
Validation & Sanitization: Trims whitespace and strictly enforces the $X->Y$ uppercase schema. Invalid formats (e.g., self-loops, lowercase) are segregated into invalid_entries.

Duplicate Edge Handling: If an edge repeats, the primary occurrence is executed; subsequent occurrences are pushed to duplicate_edges. Multi-parent conflicts are resolved by prioritizing the first-encountered parent.

Cycle Detection (DFS): Utilizes Depth-First Search with recursion stack tracking. Cyclic groups forfeit their depth metric and return has_cycle: true with an empty {} tree structure.

Root Resolution: Roots are mathematically defined as nodes completely absent from the child matrix. Pure cyclic groups default to their lexicographically smallest node as the pseudo-root.

💻 Local Developer Setup
If you wish to initialize the pipeline locally:

1. Clone the Repository

Bash
git clone [https://github.com/yourusername/bfhl-challenge.git](https://github.com/yourusername/bfhl-challenge.git)
cd bfhl-challenge
2. Initialize the Backend Engine

Bash
cd backend
npm install
npm start
The server will boot on port 3000.

3. Initialize the Frontend Visualizer

Bash
# Open a new terminal instance
cd frontend # (or root, depending on your structure)
npm install
npm run dev
Author: J Joshua Haniel
Institution: SRM Institute of Science and Technology (Computer Science and Engineering)


***