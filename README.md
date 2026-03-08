# JSON to SQL Converter

A powerful, high-performance Single Page Application that converts complex, nested JSON data into optimized SQL schemas and scripts.

Built as a Single File Component, this application runs entirely in your browser without any backend server.

> This app is developed using AI.

---

## 🌐 Live Demo

Try it out instantly here: [JSON-to-SQL](https://bh00t.github.io/JSON-to-SQL/)

---

## 🚀 Key Features

### Zero Setup
The entire application lives in a single `index.html` file. No build steps, no npm install. Just double-click to run.

### Secure Client-Side Processing
All parsing and schema inference happen locally in your browser. Your data never leaves your machine.

### High Performance
- **Web Workers**: Heavy computation (JSON parsing, flattening) is offloaded to a background thread to keep the UI responsive.
- **Virtualization**: Efficiently renders tables with thousands of rows using virtual scrolling techniques.
- **Smart Sampling**: Analyzes large datasets (up to 50MB) by intelligently sampling rows to infer schemas quickly. Toggle on/off via the toolbar.

### Intelligent Schema Inference
- Automatically detects data types (mapped to AWS Redshift/PostgreSQL standards).
- Handles nested arrays and objects via recursive flattening strategies.

### Flexible SQL Generation
- Generates CREATE VIEW scripts for semi-structured data (using Redshift/Postgres JSON syntax).
- Generates standard CREATE TABLE DDL.
- Handles column name collisions automatically with smart aliasing.

### Interactive UI
- Tree view for exploring raw JSON structure.
- Tabular preview of flattened data with row limiting (100 rows / up to 5000).
- Customizable schema: rename columns, change data types, exclude fields individually or in bulk.
- Global search across keys, values, and fields.
- Dark mode support.

### Responsive Design
- Fully usable on phones and tablets — the sidebar collapses into a horizontal tab bar on smaller screens.
- All layout, spacing, and typography scale gracefully from mobile to wide desktop.

### GitHub Integration
- Quick link to the source repository from the header on every page.

---

## ⌨️ Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+Enter` / `⌘↵` | Analyze / Visualize pasted JSON |
| `Escape` | Clear and exit search |
| `Ctrl+Shift+1` / `⌘⇧1` | Switch to Preview tab |
| `Ctrl+Shift+2` / `⌘⇧2` | Switch to Schema tab |
| `Ctrl+Shift+3` / `⌘⇧3` | Switch to Script tab |
| `Ctrl+Shift+↑` / `⌘⇧↑` | Collapse all (Preview & Schema tabs) |
| `Ctrl+Shift+↓` / `⌘⇧↓` | Expand all (Preview & Schema tabs) |

---

## 📖 How to Use

### 1. Upload Data
- Open the [Live Website](https://bh00t.github.io/JSON-to-SQL/) or open your local `index.html`.
- **Option A**: Drag and drop a `.json` file (up to ~50MB) onto the drop zone.
- **Option B**: Paste raw JSON directly into the text area and press **Visualize** (or `Ctrl+Enter`).

### 2. Explore
- **Preview Tab** (`Ctrl+Shift+1`): See your raw JSON structure in a collapsible tree. Toggle between JSON tree view and a flattened tabular preview.
- **Schema Tab** (`Ctrl+Shift+2`): Review inferred fields. Uncheck fields to exclude them, rename columns via alias inputs, change data types, or bulk-edit multiple fields at once.
- **Script Tab** (`Ctrl+Shift+3`): View the generated SQL. Choose between View Script (querying JSON columns directly) or Table DDL (creating new tables).

### 3. Search
- Use the search bar in the header to filter keys, values, and fields across all views in real time.

### 4. Generate SQL
- Go to the **Script Tab**.
- Choose between **View Script** or **Table DDL**.
- Adjust configuration (Table Name, Diststyle, Sort Keys) in the config panel.

### 5. Export
- Copy the SQL to clipboard or download it as a `.sql` file.
- Download the flattened data preview as a **CSV**.
- Download the raw parsed JSON directly.

---

## 🛠️ Technology Stack

This project uses a modern stack implemented directly within the browser:

- **Core**: HTML5, JavaScript (ES6+)
- **UI Framework**: React 18 (loaded via ESM)
- **Styling**: Tailwind CSS (utility-first framework)
- **Compiler**: Babel Standalone (compiles JSX on the fly)
- **Icons**: Lucide React
- **Architecture**: Web Workers for multi-threading

---

## 🧠 Technical Architecture

### Data Flattening Logic

The application solves the problem of mapping hierarchical JSON (trees) to relational SQL (tables) using a recursive "Explosion" strategy:

- **Discovery**: Traverses the schema to find all array paths (e.g., `orders[]`, `orders[].items[]`).
- **Context Expansion**: Iterates through records, creating a Cartesian product for every array level found. A single JSON object with an array of 5 items becomes 5 table rows.
- **Mapping**: Simple fields are then mapped to these expanded rows based on their depth in the hierarchy.

### The "Single File" Approach

To achieve maximum portability:

- React and ReactDOM are imported via standard ES Modules (`importmap`).
- Babel runs in the browser to transform the `<script type="text/babel">` block into executable JavaScript.
- The Web Worker logic is stored as a template string within the main file, converted to a Blob, and loaded dynamically.
