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
- **Smart Sampling**: Analyzes large datasets (up to 20MB+) by intelligently sampling rows to infer schemas quickly.

### Intelligent Schema Inference
- Automatically detects data types (mapped to AWS Redshift/PostgreSQL standards).
- Handles nested arrays and objects via recursive flattening strategies.

### Flexible SQL Generation
- Generates CREATE VIEW scripts for semi-structured data (using Redshift/Postgres JSON syntax).
- Generates standard CREATE TABLE DDL.
- Handles column name collisions automatically with smart aliasing.

### Interactive UI
- Tree view for exploring raw JSON structure.
- Tabular preview of flattened data.
- Customizable schema (rename columns, change data types, exclude fields).
- Dark Mode support.

---

## 📖 How to Use

### 1. Upload Data
- Open the [Live Website](https://bh00t.github.io/JSON-to-SQL/) or open your local `index.html`.
- Drag and drop a `.json` file (up to ~20MB) onto the drop zone.

### 2. Explore
- **Preview Tab**: See your raw JSON structure.
- **Schema Tab**: Review inferred fields. You can uncheck fields to exclude them, rename columns using the alias inputs, or change data types.
- **Data Preview**: Toggle between "JSON View" and "Data Preview" (Table) to see how the nested data flattens into rows.

### 3. Generate SQL
- Go to the **Script Tab**.
- Choose between **View Script** (for querying JSON columns directly) or **Table DDL** (for creating new tables).
- Adjust configuration (Table Name, Diststyle, Sort Keys) in the config panel.

### 4. Export
- Click the copy button or download the SQL file directly.
- You can also download the flattened data as a CSV.

---

## 🛠️ Technology Stack

This project uses a modern stack implemented directly within the browser:

- **Core**: HTML5, JavaScript (ES6+)
- **UI Framework**: React 18 (loaded via ESM)
- **Styling**: Tailwind CSS (Utility-first framework)
- **Compiler**: Babel Standalone (Compiles JSX on the fly)
- **Icons**: Lucide React
- **Architecture**: Web Workers for multi-threading

---

## 📦 How to Run Locally

1. Download the `index.html` file.
2. Open the file in any modern web browser (Chrome, Firefox, Edge, Safari).

That's it! You are ready to convert data offline.

---

## 🧠 Technical Architecture

### Data Flattening Logic

The application solves the problem of mapping hierarchical JSON (trees) to relational SQL (tables) using a recursive "Explosion" strategy:

- **Discovery**: It traverses the schema to find all array paths (e.g., `orders[]`, `orders[].items[]`).
- **Context Expansion**: It iterates through records, creating a Cartesian product for every array level found. A single JSON object with an array of 5 items becomes 5 table rows.
- **Mapping**: Simple fields are then mapped to these expanded rows based on their depth in the hierarchy.

### The "Single File" Approach

To achieve maximum portability:

- React and ReactDOM are imported via standard ES Modules (`importmap`).
- Babel runs in the browser to transform the `<script type="text/babel">` block into executable JavaScript.
- The Web Worker logic is stored as a template string within the main file, converted to a Blob, and loaded dynamically.