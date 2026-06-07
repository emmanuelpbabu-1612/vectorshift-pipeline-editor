# VectorShift — Pipeline Editor

A visual AI pipeline builder built as part of the VectorShift Frontend Technical Assessment. Drag, connect, and analyse nodes in a fully interactive canvas — backed by a FastAPI server that validates pipeline structure in real time.

---

## Demo

> Drop nodes from the toolbar → connect them → click **Run Pipeline** to get an instant DAG analysis.

![Pipeline Editor Preview](./preview.png)

---

## Features

### Part 1 — Node Abstraction
- Built a `BaseNode` component that all node types extend — eliminates repeated code across nodes
- Every node is defined by three props: `title`, `handles` config array, and `children`
- Added **5 new custom nodes** on top of the original 4: Filter, API Call, Merge, Transform, Notes
- Each node demonstrates a different handle configuration (0 handles → multiple inputs/outputs)

### Part 2 — Styling
- Dark toolbar with colour-coded node chips per type
- White node cards with unique accent colour headers (blue, purple, emerald, amber, red, cyan, pink, indigo, lime)
- Dot-grid canvas background, dark minimap with colour-matched node indicators
- Shared design token system via `nodeStyles.js` — change any node's colour in one line

### Part 3 — Text Node Logic
- **Auto-resize:** node width and height expand dynamically as the user types — driven by character count and line breaks
- **Variable handles:** typing `{{ variableName }}` inside the textarea instantly creates a new target Handle on the left side of the node
- Regex validates proper JavaScript variable names — duplicates are ignored
- Each variable gets a labelled amber badge aligned to its handle position

### Part 4 — Backend Integration
- Submit button reads nodes + edges from Zustand global store and POSTs to `/pipelines/parse`
- FastAPI backend counts nodes and edges, then runs **Kahn's Algorithm** to detect cycles
- Returns `{ num_nodes, num_edges, is_dag }` — displayed in a styled modal
- Green header = valid DAG. Red header = cycle detected.
- Inline error banner if backend is unreachable

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, React Flow, Zustand |
| Backend | Python, FastAPI, Uvicorn |
| Styling | Inline styles + CSS variables, Figtree (Google Fonts) |
| State | Zustand (global node/edge store) |
| Algorithm | Kahn's Algorithm (topological sort for DAG detection) |

---

## Project Structure

```
├── frontend/
│   └── src/
│       ├── App.js              # Root layout
│       ├── ui.js               # React Flow canvas
│       ├── toolbar.js          # Node palette toolbar
│       ├── draggableNode.js    # Draggable node chips
│       ├── submit.js           # Submit button + result modal
│       ├── store.js            # Zustand global state
│       └── nodes/
│           ├── baseNode.js     # Core node abstraction
│           ├── nodeStyles.js   # Shared colour constants
│           ├── inputNode.js
│           ├── outputNode.js
│           ├── llmNode.js
│           ├── textNode.js     # Dynamic resize + variable handles
│           ├── filterNode.js
│           ├── apiNode.js
│           ├── mergeNode.js
│           ├── transformNode.js
│           └── notesNode.js
└── backend/
    └── main.py                 # FastAPI server + DAG check
```

---

## Getting Started

### Prerequisites
- Node.js ≥ 16
- Python ≥ 3.9
- pip

### 1. Clone the repo

```bash
git clone https://github.com/emmanuelpbabu-1612/vectorshift-pipeline-editor.git
cd vectorshift-pipeline-editor
```

### 2. Run the backend

```bash
cd backend
pip install fastapi uvicorn
uvicorn main:app --reload
```

Backend runs at `http://localhost:8000`

### 3. Run the frontend

```bash
cd frontend
npm install
npm start
```

Frontend runs at `http://localhost:3000`

---

## How to Use

1. **Drag** any node from the top toolbar onto the canvas
2. **Connect** nodes by dragging from an output handle (right dot) to an input handle (left dot)
3. **Try the Text node** — type `{{ variableName }}` to see a new input handle appear live
4. **Click Run Pipeline** — the backend analyses your pipeline and returns node count, edge count, and whether it's a valid DAG

---

## API

### `POST /pipelines/parse`

**Request body:**
```json
{
  "nodes": [...],
  "edges": [...]
}
```

**Response:**
```json
{
  "num_nodes": 4,
  "num_edges": 3,
  "is_dag": true
}
```

---

## Author

**Emmanuel P Babu**
Co-Founder & CEO, VENPORT AI
[LinkedIn](https://www.linkedin.com/in/emmanuel-p-babu-2603b236b) · [GitHub](https://github.com/emmanuelpbabu-1612)
