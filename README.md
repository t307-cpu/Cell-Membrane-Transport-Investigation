<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />

  <meta
    name="viewport"
    content="width=device-width, initial-scale=1.0"
  />

  <title>Interactive Cell Membrane Investigation</title>

  <style>
    :root {
      --page-bg: #eaf5fb;
      --panel-bg: #ffffff;
      --surface: #f7fbff;
      --border: #d5e0eb;
      --text: #172338;
      --muted: #5f6e81;

      --primary: #2563eb;
      --primary-dark: #1d4ed8;
      --primary-soft: #eaf2ff;

      --success: #15803d;
      --success-soft: #ecfdf3;

      --warning: #a65b08;
      --warning-soft: #fff7e8;

      --danger: #b91c1c;
      --danger-soft: #fff1f1;

      --shadow: 0 14px 35px rgba(31, 56, 88, 0.12);
    }

    * {
      box-sizing: border-box;
    }

    html {
      color-scheme: light;
    }

    body {
      min-height: 100vh;
      margin: 0;
      padding: 20px;

      background:
        radial-gradient(circle at 10% 0%, #d8f2ff, transparent 34%),
        radial-gradient(circle at 100% 100%, #e4f8e9, transparent 32%),
        linear-gradient(145deg, #edf8ff, #f5f8fc);

      color: var(--text);

      font-family:
        Inter,
        ui-sans-serif,
        system-ui,
        -apple-system,
        BlinkMacSystemFont,
        "Segoe UI",
        sans-serif;
    }

    button,
    input {
      font: inherit;
    }

    button {
      color: inherit;
    }

    .app {
      width: min(1500px, 100%);
      margin: 0 auto;
    }

    /* ------------------------------------------------------------
       Header
    ------------------------------------------------------------ */

    .page-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 24px;

      margin-bottom: 18px;
      padding: 22px 26px;

      border: 1px solid rgba(213, 224, 235, 0.92);
      border-radius: 22px;

      background: rgba(255, 255, 255, 0.92);
      box-shadow: var(--shadow);
      backdrop-filter: blur(10px);
    }

    .title-area {
      min-width: 0;
    }

    .eyebrow {
      margin-bottom: 6px;
      color: var(--primary);

      font-size: 0.78rem;
      font-weight: 850;
      letter-spacing: 0.12em;
      text-transform: uppercase;
    }

    .page-header h1 {
      margin: 0 0 8px;
      color: #17375f;

      font-size: clamp(1.65rem, 3vw, 2.55rem);
      line-height: 1.12;
    }

    .page-header p {
      max-width: 850px;
      margin: 0;

      color: var(--muted);
      font-size: 0.98rem;
      line-height: 1.55;
    }

    .header-badge {
      flex: 0 0 auto;

      padding: 10px 14px;
      border: 1px solid #a8c8f8;
      border-radius: 999px;

      background: #eef5ff;
      color: #1d4ed8;

      font-size: 0.84rem;
      font-weight: 800;
      white-space: nowrap;
    }

    /* ------------------------------------------------------------
       Main layout
    ------------------------------------------------------------ */

    .main-layout {
      display: grid;
      grid-template-columns: minmax(0, 1fr) 390px;
      gap: 18px;
      align-items: start;
    }

    .simulation-panel,
    .investigation-panel {
      border: 1px solid var(--border);
      border-radius: 22px;
      background: var(--panel-bg);
      box-shadow: var(--shadow);
    }

    .simulation-panel {
      overflow: hidden;
    }

    /* ------------------------------------------------------------
       Toolbar
    ------------------------------------------------------------ */

    .toolbar {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 9px;

      padding: 13px 14px;

      border-bottom: 1px solid var(--border);
      background: var(--surface);
    }

    .toolbar-group {
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      gap: 7px;
    }

    .toolbar-label {
      margin-right: 2px;
      color: #46576c;

      font-size: 0.8rem;
      font-weight: 800;
    }

    .toolbar-divider {
      width: 1px;
      height: 30px;
      margin: 0 3px;
      background: var(--border);
    }

    .toolbar-spacer {
      flex: 1;
    }

    .particle-button,
    .control-button,
    .navigation-button,
    .action-button {
      border: 1px solid var(--border);
      border-radius: 10px;

      background: #ffffff;
      cursor: pointer;

      transition:
        transform 120ms ease,
        background-color 120ms ease,
        border-color 120ms ease,
        box-shadow 120ms ease;
    }

    .particle-button:hover,
    .control-button:hover,
    .navigation-button:hover:not(:disabled),
    .action-button:hover:not(:disabled) {
      transform: translateY(-1px);
      border-color: #8eb5f5;
      box-shadow: 0 5px 14px rgba(37, 99, 235, 0.12);
    }

    .particle-button {
      display: inline-flex;
      align-items: center;
      gap: 7px;

      padding: 8px 10px;

      font-size: 0.82rem;
      font-weight: 800;
    }

    .particle-button.active {
      border-color: var(--primary);
      background: var(--primary-soft);
      color: var(--primary-dark);
      box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.11);
    }

    .control-button {
      padding: 8px 11px;
      font-size: 0.82rem;
      font-weight: 800;
    }

    .control-button.primary {
      border-color: var(--primary);
      background: var(--primary);
      color: #ffffff;
    }

    .control-button.primary:hover {
      background: var(--primary-dark);
    }

    .particle-icon {
      width: 14px;
      height: 14px;

      border: 2px solid rgba(0, 0, 0, 0.18);
      border-radius: 50%;
    }

    .particle-icon.oxygen {
      background: #ef4444;
    }

    .particle-icon.water {
      background: #38bdf8;
    }

    .particle-icon.ion {
      background: #facc15;
    }

    .particle-icon.glucose {
      border-radius: 4px;
      background: #fb923c;
    }

    /* ------------------------------------------------------------
       Speed control
    ------------------------------------------------------------ */

    .speed-control {
      display: flex;
      align-items: center;
      gap: 7px;

      padding: 4px 9px;
      border: 1px solid var(--border);
      border-radius: 10px;
      background: #ffffff;
    }

    .speed-control label {
      color: #46576c;
      font-size: 0.78rem;
      font-weight: 800;
    }

    .speed-control input {
      width: 80px;
      accent-color: var(--primary);
    }

    .speed-value {
      min-width: 28px;
      color: var(--primary-dark);
      font-size: 0.78rem;
      font-weight: 850;
      text-align: right;
    }

    /* ------------------------------------------------------------
       Canvas
    ------------------------------------------------------------ */

    .canvas-container {
      position: relative;
      width: 100%;

      overflow: hidden;
      background: #dff3fc;
      touch-action: none;
    }

    canvas {
      display: block;
      width: 100%;
      height: auto;

      cursor: grab;
      touch-action: none;
      user-select: none;
    }

    canvas.dragging {
      cursor: grabbing;
    }

    .canvas-message {
      position: absolute;
      top: 14px;
      left: 50%;

      max-width: calc(100% - 30px);
      padding: 9px 14px;

      border: 1px solid rgba(81, 113, 147, 0.25);
      border-radius: 999px;

      background: rgba(255, 255, 255, 0.92);
      color: #36516e;

      font-size: 0.8rem;
      font-weight: 750;
      text-align: center;

      pointer-events: none;
      transform: translateX(-50%);
      backdrop-filter: blur(5px);
    }

    /* ------------------------------------------------------------
       Legend
    ------------------------------------------------------------ */

    .legend {
      display: flex;
      flex-wrap: wrap;
      gap: 9px 17px;

      padding: 12px 15px;

      border-top: 1px solid var(--border);
      background: #fbfdff;

      color: #526174;
      font-size: 0.78rem;
    }

    .legend-item {
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }

    .protein-icon {
      width: 17px;
      height: 17px;

      border: 2px solid rgba(0, 0, 0, 0.18);
      border-radius: 5px;
    }

    .protein-icon.aquaporin {
      background: #13a5b8;
    }

    .protein-icon.carrier {
      background: #8b5cf6;
    }

    .protein-icon.pump {
      background: #ef476f;
    }

    /* ------------------------------------------------------------
       Guided investigation panel
    ------------------------------------------------------------ */

    .investigation-panel {
      position: sticky;
      top: 18px;
      padding: 19px;
    }

    .panel-heading {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;

      margin-bottom: 10px;
    }

    .panel-heading strong {
      color: #243a56;
      font-size: 0.96rem;
    }

    .panel-heading span {
      color: var(--muted);
      font-size: 0.8rem;
      font-weight: 800;
    }

    .progress-track {
      height: 10px;
      margin-bottom: 18px;

      overflow: hidden;
      border-radius: 999px;
      background: #e4ecf5;
    }

    .progress-fill {
      width: 20%;
      height: 100%;

      border-radius: inherit;
      background: linear-gradient(90deg, #2563eb, #38bdf8);

      transition: width 250ms ease;
    }

    .lesson-number {
      margin-bottom: 4px;
      color: var(--primary);

      font-size: 0.75rem;
      font-weight: 850;
      letter-spacing: 0.1em;
      text-transform: uppercase;
    }

    .investigation-panel h2 {
      margin: 0 0 10px;
      color: #17375f;

      font-size: 1.43rem;
      line-height: 1.25;
    }

    .lesson-description {
      margin: 0 0 14px;
      color: #526174;

      font-size: 0.93rem;
      line-height: 1.58;
    }

    .information-box {
      margin: 10px 0;
      padding: 12px 13px;

      border-left: 4px solid;
      border-radius: 10px;

      font-size: 0.88rem;
      line-height: 1.48;
    }

    .information-box strong {
      display: block;
      margin-bottom: 3px;

      color: #25364d;
      font-size: 0.76rem;
      letter-spacing: 0.05em;
      text-transform: uppercase;
    }

    .objective-box {
      border-color: var(--primary);
      background: #eef5ff;
    }

    .science-box {
      border-color: #0d9488;
      background: #ecfdf9;
    }

    .hint-box {
      display: none;
      border-color: #8b5cf6;
      background: #f6f1ff;
    }

    .hint-box.visible {
      display: block;
    }

    /* ------------------------------------------------------------
       Feedback
    ------------------------------------------------------------ */

    .feedback {
      min-height: 66px;
      margin: 13px 0;
      padding: 12px 13px;

      border: 1px solid #d7e1eb;
      border-radius: 10px;

      background: #f8fafc;
      color: #4b5b6f;

      font-size: 0.88rem;
      line-height: 1.45;
    }

    .feedback.success {
      border-color: #86d29a;
      background: var(--success-soft);
      color: #166534;
    }

    .feedback.warning {
      border-color: #efc274;
      background: var(--warning-soft);
      color: #8a4906;
    }

    .feedback.error {
      border-color: #efa3a3;
      background: var(--danger-soft);
      color: #991b1b;
    }

    /* ------------------------------------------------------------
       Status cards
    ------------------------------------------------------------ */

    .status-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 8px;

      margin: 12px 0;
    }

    .status-card {
      min-width: 0;
      padding: 9px 7px;

      border: 1px solid var(--border);
      border-radius: 10px;

      background: #fbfdff;
      text-align: center;
    }

    .status-card span {
      display: block;
      margin-bottom: 3px;

      overflow: hidden;
      color: var(--muted);

      font-size: 0.66rem;
      font-weight: 800;
      text-overflow: ellipsis;
      text-transform: uppercase;
      white-space: nowrap;
    }

    .status-card strong {
      display: block;
      overflow: hidden;

      color: #1e416c;
      font-size: 1rem;
      text-overflow: ellipsis;
      white-space: nowrap;
    }

    /* ------------------------------------------------------------
       Panel buttons
    ------------------------------------------------------------ */

    .lesson-actions {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;

      margin-bottom: 10px;
    }

    .action-button {
      padding: 9px 10px;
      font-size: 0.8rem;
      font-weight: 800;
    }

    .lesson-navigation {
      display: flex;
      gap: 9px;
    }

    .navigation-button {
      flex: 1;
      padding: 10px 11px;
      font-weight: 850;
    }

    .navigation-button.next {
      border-color: var(--primary);
      background: var(--primary);
      color: #ffffff;
    }

    .navigation-button.next:hover:not(:disabled) {
      background: var(--primary-dark);
    }

    .navigation-button:disabled,
    .action-button:disabled {
      cursor: not-allowed;
      opacity: 0.45;
    }

    .completion-banner {
      display: none;

      margin-top: 12px;
      padding: 11px;

      border: 1px solid #86d29a;
      border-radius: 10px;

      background: var(--success-soft);
      color: #166534;

      font-size: 0.88rem;
      font-weight: 850;
      text-align: center;
    }

    .completion-banner.visible {
      display: block;
    }

    /* ------------------------------------------------------------
       Responsive layout
    ------------------------------------------------------------ */

    @media (max-width: 1080px) {
      .main-layout {
        grid-template-columns: 1fr;
      }

      .investigation-panel {
        position: static;
      }
    }

    @media (max-width: 720px) {
      body {
        padding: 9px;
      }

      .page-header {
        align-items: flex-start;
        flex-direction: column;
        padding: 18px;
      }

      .header-badge {
        white-space: normal;
      }

      .toolbar {
        gap: 7px;
      }

      .toolbar-spacer,
      .toolbar-divider {
        display: none;
      }

      .toolbar-group {
        width: 100%;
      }

      .particle-button {
        flex: 1;
        justify-content: center;
      }

      .control-button {
        flex: 1;
      }

      .speed-control {
        width: 100%;
      }

      .speed-control input {
        flex: 1;
      }

      .investigation-panel {
        padding: 16px;
      }
    }

    @media (max-width: 460px) {
      .particle-button {
        min-width: calc(50% - 5px);
      }

      .status-grid {
        grid-template-columns: 1fr;
      }

      .status-card {
        display: flex;
        align-items: center;
        justify-content: space-between;
      }

      .status-card span {
        margin: 0;
      }
    }
  </style>
</head>

<body>
  <main class="app">
    <header class="page-header">
      <div class="title-area">
        <div class="eyebrow">Interactive biology laboratory</div>

        <h1>Cell Membrane Transport Investigation</h1>

        <p>
          Observe continuously moving molecules, drag them across the cell
          membrane, and complete a guided investigation of diffusion, osmosis,
          facilitated diffusion, and active transport.
        </p>
      </div>

      <div class="header-badge">
        Molecules move continuously
      </div>
    </header>

    <div class="main-layout">
      <!-- ========================================================
           SIMULATION
      ========================================================= -->

      <section class="simulation-panel" aria-label="Cell membrane simulation">
        <div class="toolbar">
          <div class="toolbar-group">
            <span class="toolbar-label">Molecule:</span>

            <button
              class="particle-button active"
              data-particle="oxygen"
              type="button"
            >
              <span class="particle-icon oxygen"></span>
              Oxygen
            </button>

            <button
              class="particle-button"
              data-particle="water"
              type="button"
            >
              <span class="particle-icon water"></span>
              Water
            </button>

            <button
              class="particle-button"
              data-particle="ion"
              type="button"
            >
              <span class="particle-icon ion"></span>
              Ion
            </button>

            <button
              class="particle-button"
              data-particle="glucose"
              type="button"
            >
              <span class="particle-icon glucose"></span>
              Glucose
            </button>
          </div>

          <div class="toolbar-divider"></div>

          <div class="speed-control">
            <label for="speedSlider">Motion</label>

            <input
              id="speedSlider"
              type="range"
              min="0"
              max="200"
              step="10"
              value="100"
            />

            <span id="speedValue" class="speed-value">1.0×</span>
          </div>

          <div class="toolbar-spacer"></div>

          <button id="addButton" class="control-button" type="button">
            Add molecule
          </button>

          <button id="pauseButton" class="control-button" type="button">
            Pause motion
          </button>

          <button
            id="resetButton"
            class="control-button primary"
            type="button"
          >
            Reset lesson
          </button>
        </div>

        <div class="canvas-container">
          <canvas
            id="simulationCanvas"
            width="1050"
            height="680"
            aria-label="Interactive cell membrane with moving molecules"
          >
            Your browser does not support the HTML canvas element.
          </canvas>

          <div class="canvas-message">
            Drag molecules across the membrane at the highlighted location.
          </div>
        </div>

        <div class="legend">
          <span class="legend-item">
            <span class="particle-icon oxygen"></span>
            Oxygen
          </span>

          <span class="legend-item">
            <span class="particle-icon water"></span>
            Water
          </span>

          <span class="legend-item">
            <span class="particle-icon ion"></span>
            Ion
          </span>

          <span class="legend-item">
            <span class="particle-icon glucose"></span>
            Glucose
          </span>

          <span class="legend-item">
            <span class="protein-icon aquaporin"></span>
            Aquaporin
          </span>

          <span class="legend-item">
            <span class="protein-icon carrier"></span>
            Carrier protein
          </span>

          <span class="legend-item">
            <span class="protein-icon pump"></span>
            ATP pump
          </span>
        </div>
      </section>

      <!-- ========================================================
           GUIDED INVESTIGATION
      ========================================================= -->

      <aside class="investigation-panel">
        <div class="panel-heading">
          <strong>Guided investigation</strong>
          <span id="progressLabel">Activity 1 of 5</span>
        </div>

        <div class="progress-track">
          <div id="progressFill" class="progress-fill"></div>
        </div>

        <div id="lessonNumber" class="lesson-number">
          Activity 1
        </div>

        <h2 id="lessonTitle">Simple diffusion</h2>

        <p id="lessonDescription" class="lesson-description"></p>

        <div class="information-box objective-box">
          <strong>Investigation task</strong>
          <span id="lessonObjective"></span>
        </div>

        <div class="information-box science-box">
          <strong>Scientific principle</strong>
          <span id="sciencePrinciple"></span>
        </div>

        <div id="hintBox" class="information-box hint-box">
          <strong>Hint</strong>
          <span id="lessonHint"></span>
        </div>

        <div
          id="feedback"
          class="feedback"
          role="status"
          aria-live="polite"
        >
          Select and drag a molecule to begin.
        </div>

        <div class="status-grid">
          <div class="status-card">
            <span>Selected</span>
            <strong id="selectedStatus">Oxygen</strong>
          </div>

          <div class="status-card">
            <span>ATP</span>
            <strong id="atpStatus">3</strong>
          </div>

          <div class="status-card">
            <span>Transfers</span>
            <strong id="transferStatus">0</strong>
          </div>
        </div>

        <div class="lesson-actions">
          <button id="hintButton" class="action-button" type="button">
            Show hint
          </button>

          <button id="addRequiredButton" class="action-button" type="button">
            Add required molecule
          </button>
        </div>

        <div class="lesson-navigation">
          <button
            id="previousButton"
            class="navigation-button"
            type="button"
            disabled
          >
            Previous
          </button>

          <button
            id="nextButton"
            class="navigation-button next"
            type="button"
          >
            Next activity
          </button>
        </div>

        <div id="completionBanner" class="completion-banner">
          Investigation task completed.
        </div>
      </aside>
    </div>
  </main>

  <script>
    "use strict";

    /* ============================================================
       DOM REFERENCES
    ============================================================ */

    const canvas = document.getElementById("simulationCanvas");
    const ctx = canvas.getContext("2d");

    const particleButtons =
      document.querySelectorAll(".particle-button");

    const addButton = document.getElementById("addButton");
    const pauseButton = document.getElementById("pauseButton");
    const resetButton = document.getElementById("resetButton");

    const speedSlider = document.getElementById("speedSlider");
    const speedValue = document.getElementById("speedValue");

    const progressLabel = document.getElementById("progressLabel");
    const progressFill = document.getElementById("progressFill");

    const lessonNumber = document.getElementById("lessonNumber");
    const lessonTitle = document.getElementById("lessonTitle");
    const lessonDescription =
      document.getElementById("lessonDescription");

    const lessonObjective =
      document.getElementById("lessonObjective");

    const sciencePrinciple =
      document.getElementById("sciencePrinciple");

    const lessonHint = document.getElementById("lessonHint");
    const hintBox = document.getElementById("hintBox");
    const hintButton = document.getElementById("hintButton");

    const feedback = document.getElementById("feedback");

    const selectedStatus =
      document.getElementById("selectedStatus");

    const atpStatus = document.getElementById("atpStatus");

    const transferStatus =
      document.getElementById("transferStatus");

    const addRequiredButton =
      document.getElementById("addRequiredButton");

    const previousButton =
      document.getElementById("previousButton");

    const nextButton = document.getElementById("nextButton");

    const completionBanner =
      document.getElementById("completionBanner");

    /* ============================================================
       SIMULATION CONSTANTS
    ============================================================ */

    const WIDTH = canvas.width;
    const HEIGHT = canvas.height;

    const membraneTop = 292;
    const membraneBottom = 388;
    const membraneCenter =
      (membraneTop + membraneBottom) / 2;

    const proteinPositions = {
      openBilayer: 145,
      aquaporin: 385,
      carrier: 675,
      pump: 910
    };

    const moleculeTypes = {
      oxygen: {
        name: "Oxygen",
        shortLabel: "O₂",
        color: "#ef4444",
        outline: "#991b1b",
        radius: 12,
        baseSpeed: 52
      },

      water: {
        name: "Water",
        shortLabel: "H₂O",
        color: "#38bdf8",
        outline: "#0369a1",
        radius: 10,
        baseSpeed: 66
      },

      ion: {
        name: "Ion",
        shortLabel: "+",
        color: "#facc15",
        outline: "#a16207",
        radius: 12,
        baseSpeed: 44
      },

      glucose: {
        name: "Glucose",
        shortLabel: "G",
        color: "#fb923c",
        outline: "#9a3412",
        radius: 15,
        baseSpeed: 32
      }
    };

    /* ============================================================
       GUIDED INVESTIGATION DATA
    ============================================================ */

    const lessons = [
      {
        title: "Simple diffusion",

        description:
          "Small nonpolar molecules move continuously and can pass " +
          "directly through the phospholipid bilayer.",

        objective:
          "Move one oxygen molecule from outside the cell to inside " +
          "the cell through the highlighted phospholipid bilayer.",

        principle:
          "Simple diffusion moves particles from high concentration " +
          "to low concentration without a transport protein or ATP.",

        hint:
          "Drag a red oxygen molecule through the highlighted area " +
          "on the left side of the membrane.",

        requiredType: "oxygen",
        requiredStart: "outside",
        requiredTarget: "inside",
        requiredRoute: "bilayer",

        initialCounts: {
          oxygen: [15, 4],
          water: [6, 6],
          ion: [4, 4],
          glucose: [3, 3]
        }
      },

      {
        title: "Osmosis through aquaporin",

        description:
          "Water molecules are always moving. Aquaporin proteins " +
          "provide a hydrophilic passage across the membrane.",

        objective:
          "Move one water molecule from outside the cell to inside " +
          "the cell through the highlighted aquaporin.",

        principle:
          "Osmosis is the movement of water across a selectively " +
          "permeable membrane. Aquaporins increase the rate of water movement.",

        hint:
          "Drag a blue water molecule through the teal channel protein.",

        requiredType: "water",
        requiredStart: "outside",
        requiredTarget: "inside",
        requiredRoute: "aquaporin",

        initialCounts: {
          oxygen: [5, 5],
          water: [16, 5],
          ion: [4, 4],
          glucose: [3, 3]
        }
      },

      {
        title: "Facilitated diffusion",

        description:
          "Charged particles cannot easily cross the nonpolar center " +
          "of the phospholipid bilayer.",

        objective:
          "Move one ion from outside the cell to inside the cell " +
          "through the highlighted carrier protein.",

        principle:
          "Facilitated diffusion moves particles down their " +
          "concentration gradient using a membrane protein. ATP is not required.",

        hint:
          "Drag a yellow ion through the purple carrier protein, " +
          "not through the pink ATP pump.",

        requiredType: "ion",
        requiredStart: "outside",
        requiredTarget: "inside",
        requiredRoute: "carrier",

        initialCounts: {
          oxygen: [5, 5],
          water: [6, 6],
          ion: [15, 4],
          glucose: [3, 3]
        }
      },

      {
        title: "Active transport",

        description:
          "Cells sometimes move substances against their " +
          "concentration gradient using energy from ATP.",

        objective:
          "Move one ion from outside the cell, where its concentration " +
          "is lower, to inside the cell, where its concentration is higher, " +
          "using the highlighted ATP pump.",

        principle:
          "Active transport moves substances from lower concentration " +
          "to higher concentration. A membrane pump and energy are required.",

        hint:
          "Drag a yellow ion through the pink pump. A successful " +
          "transfer consumes one ATP.",

        requiredType: "ion",
        requiredStart: "outside",
        requiredTarget: "inside",
        requiredRoute: "pump",

        initialCounts: {
          oxygen: [5, 5],
          water: [6, 6],
          ion: [4, 15],
          glucose: [3, 3]
        }
      },

      {
        title: "Glucose transport",

        description:
          "Glucose is a relatively large polar molecule and cannot " +
          "move freely through the hydrophobic membrane interior.",

        objective:
          "Move one glucose molecule from outside the cell to inside " +
          "the cell through the highlighted carrier protein.",

        principle:
          "A glucose carrier allows glucose to move down its " +
          "concentration gradient by facilitated diffusion without using ATP.",

        hint:
          "Drag an orange glucose molecule through the purple carrier protein.",

        requiredType: "glucose",
        requiredStart: "outside",
        requiredTarget: "inside",
        requiredRoute: "carrier",

        initialCounts: {
          oxygen: [5, 5],
          water: [6, 6],
          ion: [4, 4],
          glucose: [15, 4]
        }
      }
    ];

    /* ============================================================
       APPLICATION STATE
    ============================================================ */

    let molecules = [];
    let nextMoleculeId = 1;

    let currentLessonIndex = 0;
    let selectedType = "oxygen";

    let motionPaused = false;
    let motionMultiplier = 1;

    let draggedMolecule = null;
    let dragOffsetX = 0;
    let dragOffsetY = 0;

    let dragStartPosition = null;
    let dragStartSide = null;

    let atp = 3;
    let successfulTransfers = 0;
    let lessonCompleted = false;

    let lastAnimationTime = 0;
    let elapsedAnimationTime = 0;

    /* ============================================================
       UTILITY FUNCTIONS
    ============================================================ */

    function randomBetween(min, max) {
      return min + Math.random() * (max - min);
    }

    function clamp(value, minimum, maximum) {
      return Math.max(minimum, Math.min(maximum, value));
    }

    function sideFromY(y) {
      if (y < membraneTop) {
        return "outside";
      }

      if (y > membraneBottom) {
        return "inside";
      }

      return "membrane";
    }

    function oppositeSide(side) {
      return side === "outside" ? "inside" : "outside";
    }

    function getRandomVelocity(type) {
      const data = moleculeTypes[type];
      const angle = randomBetween(0, Math.PI * 2);
      const speed = data.baseSpeed * randomBetween(0.72, 1.15);

      return {
        vx: Math.cos(angle) * speed,
        vy: Math.sin(angle) * speed
      };
    }

    function getRandomPosition(side, radius) {
      const padding = radius + 18;

      const x = randomBetween(
        padding,
        WIDTH - padding
      );

      if (side === "outside") {
        return {
          x,
          y: randomBetween(
            76 + radius,
            membraneTop - radius - 14
          )
        };
      }

      return {
        x,
        y: randomBetween(
          membraneBottom + radius + 14,
          HEIGHT - radius - 22
        )
      };
    }

    function countMolecules(type, side) {
      return molecules.filter((molecule) => {
        return molecule.type === type &&
          molecule.side === side;
      }).length;
    }

    function isDownGradient(type, startSide, targetSide) {
      const startCount = countMolecules(type, startSide);
      const targetCount = countMolecules(type, targetSide);

      return startCount > targetCount;
    }

    /* ============================================================
       MOLECULE CREATION
    ============================================================ */

    function createMolecule(type, side = "outside") {
      const data = moleculeTypes[type];
      const position = getRandomPosition(side, data.radius);
      const velocity = getRandomVelocity(type);

      return {
        id: nextMoleculeId++,
        type,
        side,

        x: position.x,
        y: position.y,

        vx: velocity.vx,
        vy: velocity.vy,

        radius: data.radius,
        dragging: false
      };
    }

    function addMolecule(type, side = "outside") {
      molecules.push(createMolecule(type, side));
      updateStatus();
    }

    function addMoleculeGroup(type, outsideCount, insideCount) {
      for (let i = 0; i < outsideCount; i++) {
        addMolecule(type, "outside");
      }

      for (let i = 0; i < insideCount; i++) {
        addMolecule(type, "inside");
      }
    }

    /* ============================================================
       LESSON SETUP
    ============================================================ */

    function configureLesson(index) {
      currentLessonIndex = index;

      molecules = [];
      draggedMolecule = null;
      nextMoleculeId = 1;

      atp = 3;
      successfulTransfers = 0;
      lessonCompleted = false;

      canvas.classList.remove("dragging");
      completionBanner.classList.remove("visible");
      hintBox.classList.remove("visible");

      hintButton.textContent = "Show hint";

      const lesson = lessons[currentLessonIndex];

      Object.entries(lesson.initialCounts).forEach(
        ([type, counts]) => {
          addMoleculeGroup(type, counts[0], counts[1]);
        }
      );

      selectMoleculeType(lesson.requiredType, false);
      updateLessonPanel();

      setFeedback(
        "Observe the moving molecules, then drag the required molecule " +
        "through the highlighted membrane region.",
        "neutral"
      );
    }

    function updateLessonPanel() {
      const lesson = lessons[currentLessonIndex];
      const activityNumber = currentLessonIndex + 1;

      lessonNumber.textContent =
        `Activity ${activityNumber}`;

      lessonTitle.textContent = lesson.title;
      lessonDescription.textContent = lesson.description;
      lessonObjective.textContent = lesson.objective;
      sciencePrinciple.textContent = lesson.principle;
      lessonHint.textContent = lesson.hint;

      progressLabel.textContent =
        `Activity ${activityNumber} of ${lessons.length}`;

      progressFill.style.width =
        `${(activityNumber / lessons.length) * 100}%`;

      previousButton.disabled =
        currentLessonIndex === 0;

      nextButton.textContent =
        currentLessonIndex === lessons.length - 1
          ? "Restart investigation"
          : "Next activity";

      updateStatus();
    }

    function updateStatus() {
      selectedStatus.textContent =
        moleculeTypes[selectedType].name;

      atpStatus.textContent = String(atp);
      transferStatus.textContent =
        String(successfulTransfers);
    }

    function selectMoleculeType(type, showFeedback = true) {
      selectedType = type;

      particleButtons.forEach((button) => {
        button.classList.toggle(
          "active",
          button.dataset.particle === type
        );
      });

      updateStatus();

      if (showFeedback) {
        setFeedback(
          `${moleculeTypes[type].name} selected. ` +
          "Drag a matching molecule across the membrane.",
          "neutral"
        );
      }
    }

    function setFeedback(message, type = "neutral") {
      feedback.textContent = message;
      feedback.className = "feedback";

      if (type !== "neutral") {
        feedback.classList.add(type);
      }
    }

    function completeCurrentLesson(message) {
      if (lessonCompleted) {
        return;
      }

      lessonCompleted = true;

      completionBanner.textContent = message;
      completionBanner.classList.add("visible");
    }

    /* ============================================================
       MOLECULE MOTION
    ============================================================ */

    function updateMoleculeMotion(deltaTime) {
      if (motionPaused || motionMultiplier === 0) {
        return;
      }

      const adjustedDelta = deltaTime * motionMultiplier;

      for (const molecule of molecules) {
        if (molecule.dragging) {
          continue;
        }

        molecule.x += molecule.vx * adjustedDelta;
        molecule.y += molecule.vy * adjustedDelta;

        applyRandomMotion(molecule, adjustedDelta);
        constrainMolecule(molecule);
      }

      resolveMoleculeCollisions();
    }

    function applyRandomMotion(molecule, deltaTime) {
      const randomStrength = 13;

      molecule.vx +=
        randomBetween(-randomStrength, randomStrength) *
        deltaTime;

      molecule.vy +=
        randomBetween(-randomStrength, randomStrength) *
        deltaTime;

      const data = moleculeTypes[molecule.type];
      const speed = Math.hypot(molecule.vx, molecule.vy);

      const minimumSpeed = data.baseSpeed * 0.55;
      const maximumSpeed = data.baseSpeed * 1.35;

      if (speed < minimumSpeed || speed > maximumSpeed) {
        const targetSpeed = clamp(
          speed,
          minimumSpeed,
          maximumSpeed
        );

        if (speed > 0) {
          molecule.vx =
            (molecule.vx / speed) * targetSpeed;

          molecule.vy =
            (molecule.vy / speed) * targetSpeed;
        }
      }
    }

    function constrainMolecule(molecule) {
      const radius = molecule.radius;

      if (molecule.x - radius < 0) {
        molecule.x = radius;
        molecule.vx = Math.abs(molecule.vx);
      }

      if (molecule.x + radius > WIDTH) {
        molecule.x = WIDTH - radius;
        molecule.vx = -Math.abs(molecule.vx);
      }

      if (molecule.side === "outside") {
        const upperBoundary = 72;

        if (molecule.y - radius < upperBoundary) {
          molecule.y = upperBoundary + radius;
          molecule.vy = Math.abs(molecule.vy);
        }

        if (molecule.y + radius > membraneTop - 5) {
          molecule.y = membraneTop - radius - 5;
          molecule.vy = -Math.abs(molecule.vy);
        }
      }

      if (molecule.side === "inside") {
        const lowerBoundary = HEIGHT - 10;

        if (molecule.y - radius < membraneBottom + 5) {
          molecule.y = membraneBottom + radius + 5;
          molecule.vy = Math.abs(molecule.vy);
        }

        if (molecule.y + radius > lowerBoundary) {
          molecule.y = lowerBoundary - radius;
          molecule.vy = -Math.abs(molecule.vy);
        }
      }
    }

    function resolveMoleculeCollisions() {
      for (let i = 0; i < molecules.length; i++) {
        const first = molecules[i];

        if (first.dragging) {
          continue;
        }

        for (let j = i + 1; j < molecules.length; j++) {
          const second = molecules[j];

          if (
            second.dragging ||
            first.side !== second.side
          ) {
            continue;
          }

          const dx = second.x - first.x;
          const dy = second.y - first.y;

          const minimumDistance =
            first.radius + second.radius;

          const distanceSquared = dx * dx + dy * dy;

          if (
            distanceSquared === 0 ||
            distanceSquared >=
              minimumDistance * minimumDistance
          ) {
            continue;
          }

          const distance = Math.sqrt(distanceSquared);
          const normalX = dx / distance;
          const normalY = dy / distance;

          const overlap =
            minimumDistance - distance;

          first.x -= normalX * overlap * 0.5;
          first.y -= normalY * overlap * 0.5;

          second.x += normalX * overlap * 0.5;
          second.y += normalY * overlap * 0.5;

          const firstNormalVelocity =
            first.vx * normalX +
            first.vy * normalY;

          const secondNormalVelocity =
            second.vx * normalX +
            second.vy * normalY;

          if (
            firstNormalVelocity >
            secondNormalVelocity
          ) {
            const impulse =
              secondNormalVelocity -
              firstNormalVelocity;

            first.vx += impulse * normalX;
            first.vy += impulse * normalY;

            second.vx -= impulse * normalX;
            second.vy -= impulse * normalY;
          }

          constrainMolecule(first);
          constrainMolecule(second);
        }
      }
    }

    /* ============================================================
       TRANSPORT VALIDATION
    ============================================================ */

    function determineRoute(x) {
      if (
        Math.abs(x - proteinPositions.aquaporin) <= 58
      ) {
        return "aquaporin";
      }

      if (
        Math.abs(x - proteinPositions.carrier) <= 68
      ) {
        return "carrier";
      }

      if (
        Math.abs(x - proteinPositions.pump) <= 68
      ) {
        return "pump";
      }

      return "bilayer";
    }

    function routeDisplayName(route) {
      const routeNames = {
        bilayer: "phospholipid bilayer",
        aquaporin: "aquaporin",
        carrier: "carrier protein",
        pump: "ATP pump"
      };

      return routeNames[route];
    }

    function restoreDraggedMolecule(molecule) {
      molecule.x = dragStartPosition.x;
      molecule.y = dragStartPosition.y;
      molecule.side = dragStartSide;

      molecule.vx = dragStartPosition.vx;
      molecule.vy = dragStartPosition.vy;
    }

    function finishSuccessfulTransport(
      molecule,
      targetSide
    ) {
      molecule.side = targetSide;

      const horizontalPadding =
        molecule.radius + 18;

      molecule.x = clamp(
        molecule.x,
        horizontalPadding,
        WIDTH - horizontalPadding
      );

      if (targetSide === "outside") {
        molecule.y =
          membraneTop - molecule.radius - 20;

        molecule.vy =
          -Math.abs(molecule.vy || 35);
      } else {
        molecule.y =
          membraneBottom + molecule.radius + 20;

        molecule.vy =
          Math.abs(molecule.vy || 35);
      }

      const newVelocity =
        getRandomVelocity(molecule.type);

      molecule.vx = newVelocity.vx;
      molecule.vy =
        targetSide === "inside"
          ? Math.abs(newVelocity.vy)
          : -Math.abs(newVelocity.vy);

      successfulTransfers += 1;
      updateStatus();
    }

    function attemptTransport(
      molecule,
      targetSide,
      route
    ) {
      const lesson = lessons[currentLessonIndex];

      const movingDownGradient = isDownGradient(
        molecule.type,
        dragStartSide,
        targetSide
      );

      /*
        Oxygen:
        Can use simple diffusion through the bilayer.
      */

      if (molecule.type === "oxygen") {
        if (route !== "bilayer") {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "Oxygen does not require a membrane protein. " +
            "Move it directly through an open part of the bilayer.",
            "warning"
          );

          return;
        }

        if (!movingDownGradient) {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "Simple diffusion must move oxygen from higher " +
            "concentration to lower concentration.",
            "warning"
          );

          return;
        }

        finishSuccessfulTransport(molecule, targetSide);

        setFeedback(
          "Success: oxygen crossed directly through the " +
          "phospholipid bilayer by simple diffusion.",
          "success"
        );

        checkLessonCompletion(
          molecule,
          targetSide,
          route
        );

        return;
      }

      /*
        Water:
        Uses aquaporin in this educational model.
      */

      if (molecule.type === "water") {
        if (route !== "aquaporin") {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "Use the teal aquaporin channel to transport water " +
            "in this investigation.",
            "warning"
          );

          return;
        }

        if (!movingDownGradient) {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "For osmosis, there is net water movement from the side with" +
            "higher water potential to the side with lower water potential.",
            "warning"
          );

          return;
        }

        finishSuccessfulTransport(molecule, targetSide);

        setFeedback(
          "Success: water moved through aquaporin by osmosis.",
          "success"
        );

        checkLessonCompletion(
          molecule,
          targetSide,
          route
        );

        return;
      }

      /*
        Ions:
        Can use a carrier down the gradient or a pump against it.
      */

      if (molecule.type === "ion") {
        if (route === "carrier") {
          if (!movingDownGradient) {
            restoreDraggedMolecule(molecule);

            setFeedback(
              "The carrier cannot move this ion against its " +
              "concentration gradient. Use the ATP pump.",
              "warning"
            );

            return;
          }

          finishSuccessfulTransport(molecule, targetSide);

          setFeedback(
            "Success: the ion used facilitated diffusion through " +
            "the carrier. No ATP was consumed.",
            "success"
          );

          checkLessonCompletion(
            molecule,
            targetSide,
            route
          );

          return;
        }

        if (route === "pump") {
          if (movingDownGradient) {
            restoreDraggedMolecule(molecule);

            setFeedback(
              "This activity uses the pump for movement against " +
              "the concentration gradient.",
              "warning"
            );

            return;
          }

          if (atp <= 0) {
            restoreDraggedMolecule(molecule);

            setFeedback(
              "No ATP remains. Reset the activity to restore ATP.",
              "error"
            );

            return;
          }

          atp -= 1;
          finishSuccessfulTransport(molecule, targetSide);
          updateStatus();

          setFeedback(
            "Success: the ATP pump moved the ion against its " +
            "concentration gradient and consumed one ATP.",
            "success"
          );

          checkLessonCompletion(
            molecule,
            targetSide,
            route
          );

          return;
        }

        restoreDraggedMolecule(molecule);

        setFeedback(
          "An ion cannot cross the hydrophobic membrane interior " +
          "directly. Use the carrier or ATP pump.",
          "warning"
        );

        return;
      }

      /*
        Glucose:
        Uses facilitated diffusion through the carrier.
      */

      if (molecule.type === "glucose") {
        if (route !== "carrier") {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "Glucose is too large and polar to cross the bilayer " +
            "directly. Use the purple carrier protein.",
            "warning"
          );

          return;
        }

        if (!movingDownGradient) {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "This glucose carrier performs facilitated diffusion, " +
            "so glucose must move down its concentration gradient.",
            "warning"
          );

          return;
        }

        finishSuccessfulTransport(molecule, targetSide);

        setFeedback(
          "Success: glucose crossed through the carrier by " +
          "facilitated diffusion without using ATP.",
          "success"
        );

        checkLessonCompletion(
          molecule,
          targetSide,
          route
        );
      }
    }

    function checkLessonCompletion(
      molecule,
      targetSide,
      route
    ) {
      const lesson = lessons[currentLessonIndex];

      const correctType =
        molecule.type === lesson.requiredType;

      const correctStart =
        dragStartSide === lesson.requiredStart;

      const correctTarget =
        targetSide === lesson.requiredTarget;

      const correctRoute =
        route === lesson.requiredRoute;

      if (
        correctType &&
        correctStart &&
        correctTarget &&
        correctRoute
      ) {
        completeCurrentLesson(
          `${lesson.title} investigation completed successfully.`
        );
      } else {
        setTimeout(() => {
          if (!lessonCompleted) {
            setFeedback(
              "That transport was scientifically valid, but it did " +
              "not complete the current investigation task. " +
              "Check the objective and try again.",
              "warning"
            );
          }
        }, 400);
      }
    }

    /* ============================================================
       DRAWING HELPERS
    ============================================================ */

    function roundedRectangle(
      x,
      y,
      width,
      height,
      radius,
      fill = true,
      stroke = false
    ) {
      const r = Math.min(
        radius,
        width / 2,
        height / 2
      );

      ctx.beginPath();
      ctx.moveTo(x + r, y);
      ctx.lineTo(x + width - r, y);

      ctx.quadraticCurveTo(
        x + width,
        y,
        x + width,
        y + r
      );

      ctx.lineTo(
        x + width,
        y + height - r
      );

      ctx.quadraticCurveTo(
        x + width,
        y + height,
        x + width - r,
        y + height
      );

      ctx.lineTo(x + r, y + height);

      ctx.quadraticCurveTo(
        x,
        y + height,
        x,
        y + height - r
      );

      ctx.lineTo(x, y + r);

      ctx.quadraticCurveTo(
        x,
        y,
        x + r,
        y
      );

      ctx.closePath();

      if (fill) {
        ctx.fill();
      }

      if (stroke) {
        ctx.stroke();
      }
    }

    function drawBackground() {
      const outsideGradient =
        ctx.createLinearGradient(
          0,
          0,
          0,
          membraneTop
        );

      outsideGradient.addColorStop(
        0,
        "#dff5ff"
      );

      outsideGradient.addColorStop(
        1,
        "#bce7fa"
      );

      ctx.fillStyle = outsideGradient;
      ctx.fillRect(
        0,
        0,
        WIDTH,
        membraneTop
      );

      const insideGradient =
        ctx.createLinearGradient(
          0,
          membraneBottom,
          0,
          HEIGHT
        );

      insideGradient.addColorStop(
        0,
        "#e7f8eb"
      );

      insideGradient.addColorStop(
        1,
        "#c8e9d2"
      );

      ctx.fillStyle = insideGradient;

      ctx.fillRect(
        0,
        membraneBottom,
        WIDTH,
        HEIGHT - membraneBottom
      );

      drawRegionLabel(
        22,
        18,
        "Outside the cell"
      );

      drawRegionLabel(
        22,
        HEIGHT - 61,
        "Inside the cell"
      );
    }

    function drawRegionLabel(x, y, text) {
      ctx.font =
        "800 18px system-ui, sans-serif";

      const width =
        ctx.measureText(text).width + 28;

      ctx.fillStyle =
        "rgba(255, 255, 255, 0.88)";

      ctx.strokeStyle =
        "rgba(40, 78, 110, 0.22)";

      ctx.lineWidth = 1;

      roundedRectangle(
        x,
        y,
        width,
        42,
        11,
        true,
        true
      );

      ctx.fillStyle = "#245276";
      ctx.textAlign = "left";
      ctx.textBaseline = "middle";

      ctx.fillText(
        text,
        x + 14,
        y + 21
      );
    }

    /* ============================================================
       MEMBRANE DRAWING
    ============================================================ */

    function drawMembrane() {
      const membraneGradient =
        ctx.createLinearGradient(
          0,
          membraneTop,
          0,
          membraneBottom
        );

      membraneGradient.addColorStop(
        0,
        "#9dd8ef"
      );

      membraneGradient.addColorStop(
        0.5,
        "#80c5e3"
      );

      membraneGradient.addColorStop(
        1,
        "#9dd8ef"
      );

      ctx.fillStyle = membraneGradient;

      ctx.fillRect(
        0,
        membraneTop,
        WIDTH,
        membraneBottom - membraneTop
      );

      const spacing = 24;

      for (
        let x = 8;
        x < WIDTH;
        x += spacing
      ) {
        drawPhospholipid(
          x,
          membraneTop + 15,
          1
        );

        drawPhospholipid(
          x,
          membraneBottom - 15,
          -1
        );
      }

      ctx.strokeStyle =
        "rgba(25, 72, 105, 0.24)";

      ctx.lineWidth = 2;

      ctx.beginPath();
      ctx.moveTo(0, membraneTop);
      ctx.lineTo(WIDTH, membraneTop);

      ctx.moveTo(0, membraneBottom);
      ctx.lineTo(WIDTH, membraneBottom);

      ctx.stroke();
    }

    function drawPhospholipid(x, y, direction) {
      ctx.strokeStyle = "#516d82";
      ctx.lineWidth = 2;

      ctx.beginPath();

      ctx.moveTo(
        x - 3,
        y + 7 * direction
      );

      ctx.lineTo(
        x - 6,
        y + 27 * direction
      );

      ctx.moveTo(
        x + 3,
        y + 7 * direction
      );

      ctx.lineTo(
        x + 6,
        y + 27 * direction
      );

      ctx.stroke();

      ctx.fillStyle = "#f7d55f";
      ctx.strokeStyle = "#8a6814";
      ctx.lineWidth = 1.4;

      ctx.beginPath();
      ctx.arc(
        x,
        y,
        7,
        0,
        Math.PI * 2
      );

      ctx.fill();
      ctx.stroke();
    }

    /* ============================================================
       PROTEIN DRAWING
    ============================================================ */

    function drawProteins() {
      drawAquaporin();
      drawCarrier();
      drawPump();
    }

    function drawAquaporin() {
      const x = proteinPositions.aquaporin;

      ctx.save();
      ctx.translate(x, membraneCenter);

      ctx.fillStyle = "#13a5b8";
      ctx.strokeStyle = "#075f6e";
      ctx.lineWidth = 4;

      ctx.beginPath();

      ctx.moveTo(-42, -61);
      ctx.quadraticCurveTo(-58, -32, -45, 0);
      ctx.quadraticCurveTo(-58, 32, -42, 61);

      ctx.lineTo(-14, 61);
      ctx.quadraticCurveTo(-26, 30, -17, 0);
      ctx.quadraticCurveTo(-26, -30, -14, -61);

      ctx.closePath();
      ctx.fill();
      ctx.stroke();

      ctx.beginPath();

      ctx.moveTo(42, -61);
      ctx.quadraticCurveTo(58, -32, 45, 0);
      ctx.quadraticCurveTo(58, 32, 42, 61);

      ctx.lineTo(14, 61);
      ctx.quadraticCurveTo(26, 30, 17, 0);
      ctx.quadraticCurveTo(26, -30, 14, -61);

      ctx.closePath();
      ctx.fill();
      ctx.stroke();

      ctx.fillStyle = "#e4fbff";
      ctx.fillRect(-12, -62, 24, 124);

      ctx.restore();

      drawProteinLabel(
        x,
        membraneBottom + 37,
        "Aquaporin"
      );
    }

    function drawCarrier() {
      const x = proteinPositions.carrier;

      ctx.save();
      ctx.translate(x, membraneCenter);

      ctx.fillStyle = "#8b5cf6";
      ctx.strokeStyle = "#5b21b6";
      ctx.lineWidth = 4;

      ctx.beginPath();

      ctx.moveTo(-52, -61);
      ctx.quadraticCurveTo(-69, -27, -48, 0);
      ctx.quadraticCurveTo(-66, 27, -48, 61);

      ctx.lineTo(-8, 61);
      ctx.quadraticCurveTo(-31, 27, -16, 0);
      ctx.quadraticCurveTo(-30, -27, -8, -61);

      ctx.closePath();
      ctx.fill();
      ctx.stroke();

      ctx.beginPath();

      ctx.moveTo(52, -61);
      ctx.quadraticCurveTo(69, -27, 48, 0);
      ctx.quadraticCurveTo(66, 27, 48, 61);

      ctx.lineTo(8, 61);
      ctx.quadraticCurveTo(31, 27, 16, 0);
      ctx.quadraticCurveTo(30, -27, 8, -61);

      ctx.closePath();
      ctx.fill();
      ctx.stroke();

      ctx.fillStyle = "#f5efff";

      ctx.beginPath();
      ctx.ellipse(
        0,
        0,
        14,
        24,
        0,
        0,
        Math.PI * 2
      );

      ctx.fill();
      ctx.restore();

      drawProteinLabel(
        x,
        membraneBottom + 37,
        "Carrier"
      );
    }

    function drawPump() {
      const x = proteinPositions.pump;

      ctx.save();
      ctx.translate(x, membraneCenter);

      ctx.fillStyle = "#ef476f";
      ctx.strokeStyle = "#9f1239";
      ctx.lineWidth = 4;

      ctx.beginPath();

      ctx.moveTo(-50, -61);
      ctx.quadraticCurveTo(-68, -30, -49, -5);
      ctx.quadraticCurveTo(-61, 25, -35, 61);

      ctx.lineTo(6, 61);
      ctx.quadraticCurveTo(22, 29, 10, 4);
      ctx.quadraticCurveTo(27, -28, 8, -61);

      ctx.closePath();
      ctx.fill();
      ctx.stroke();

      ctx.beginPath();

      ctx.moveTo(10, -61);
      ctx.quadraticCurveTo(39, -30, 23, -3);
      ctx.quadraticCurveTo(45, 25, 31, 61);

      ctx.lineTo(54, 61);
      ctx.quadraticCurveTo(71, 25, 50, 0);
      ctx.quadraticCurveTo(69, -30, 51, -61);

      ctx.closePath();
      ctx.fill();
      ctx.stroke();

      ctx.fillStyle = "#fff0f4";

      ctx.beginPath();
      ctx.arc(
        8,
        0,
        13,
        0,
        Math.PI * 2
      );

      ctx.fill();

      ctx.fillStyle = "#9f1239";
      ctx.font =
        "800 11px system-ui, sans-serif";

      ctx.textAlign = "center";
      ctx.textBaseline = "middle";
      ctx.fillText("ATP", 8, 0);

      ctx.restore();

      drawProteinLabel(
        x,
        membraneBottom + 37,
        "ATP pump"
      );
    }

    function drawProteinLabel(x, y, text) {
      ctx.font =
        "800 13px system-ui, sans-serif";

      const width =
        ctx.measureText(text).width + 18;

      ctx.fillStyle =
        "rgba(255, 255, 255, 0.91)";

      ctx.strokeStyle =
        "rgba(31, 72, 110, 0.25)";

      ctx.lineWidth = 1;

      roundedRectangle(
        x - width / 2,
        y - 13,
        width,
        26,
        8,
        true,
        true
      );

      ctx.fillStyle = "#30465f";
      ctx.textAlign = "center";
      ctx.textBaseline = "middle";

      ctx.fillText(text, x, y);
    }

    /* ============================================================
       HIGHLIGHT CURRENT LESSON TARGET
    ============================================================ */

    function drawTargetHighlight() {
      const lesson = lessons[currentLessonIndex];

      let x = proteinPositions.openBilayer;
      let horizontalRadius = 56;

      if (lesson.requiredRoute === "aquaporin") {
        x = proteinPositions.aquaporin;
        horizontalRadius = 62;
      }

      if (lesson.requiredRoute === "carrier") {
        x = proteinPositions.carrier;
        horizontalRadius = 72;
      }

      if (lesson.requiredRoute === "pump") {
        x = proteinPositions.pump;
        horizontalRadius = 72;
      }

      const pulse =
        7 +
        Math.sin(elapsedAnimationTime * 0.004) * 5;

      ctx.save();

      ctx.fillStyle =
        "rgba(37, 99, 235, 0.09)";

      ctx.strokeStyle =
        "rgba(37, 99, 235, 0.85)";

      ctx.lineWidth = 5;
      ctx.setLineDash([11, 8]);

      ctx.beginPath();

      ctx.ellipse(
        x,
        membraneCenter,
        horizontalRadius + pulse,
        80 + pulse,
        0,
        0,
        Math.PI * 2
      );

      ctx.fill();
      ctx.stroke();

      ctx.restore();
    }

    /* ============================================================
       MOLECULE DRAWING
    ============================================================ */

    function drawMolecule(molecule) {
      const data = moleculeTypes[molecule.type];

      ctx.save();
      ctx.translate(molecule.x, molecule.y);

      if (molecule.dragging) {
        ctx.shadowColor =
          "rgba(0, 0, 0, 0.38)";

        ctx.shadowBlur = 15;
        ctx.shadowOffsetY = 5;
      }

      if (molecule.type === "glucose") {
        drawGlucoseMolecule(
          molecule.radius,
          data
        );
      } else {
        ctx.fillStyle = data.color;
        ctx.strokeStyle = data.outline;
        ctx.lineWidth = 2.4;

        ctx.beginPath();

        ctx.arc(
          0,
          0,
          molecule.radius,
          0,
          Math.PI * 2
        );

        ctx.fill();
        ctx.stroke();

        if (molecule.type === "oxygen") {
          ctx.fillStyle =
            "rgba(255,255,255,0.78)";

          ctx.beginPath();
          ctx.arc(-4, -4, 3.4, 0, Math.PI * 2);
          ctx.fill();
        }

        if (molecule.type === "water") {
          ctx.fillStyle =
            "rgba(255,255,255,0.85)";

          ctx.beginPath();
          ctx.arc(-3, -3, 2.5, 0, Math.PI * 2);
          ctx.fill();
        }

        if (molecule.type === "ion") {
          ctx.fillStyle = "#684d00";

          ctx.font =
            "900 16px system-ui, sans-serif";

          ctx.textAlign = "center";
          ctx.textBaseline = "middle";

          ctx.fillText("+", 0, -1);
        }
      }

      ctx.restore();
    }

    function drawGlucoseMolecule(radius, data) {
      ctx.fillStyle = data.color;
      ctx.strokeStyle = data.outline;
      ctx.lineWidth = 2.5;

      ctx.beginPath();

      for (let i = 0; i < 6; i++) {
        const angle =
          (Math.PI / 3) * i -
          Math.PI / 6;

        const x =
          Math.cos(angle) * radius;

        const y =
          Math.sin(angle) * radius;

        if (i === 0) {
          ctx.moveTo(x, y);
        } else {
          ctx.lineTo(x, y);
        }
      }

      ctx.closePath();
      ctx.fill();
      ctx.stroke();

      ctx.fillStyle = "#7c2d12";
      ctx.font =
        "900 10px system-ui, sans-serif";

      ctx.textAlign = "center";
      ctx.textBaseline = "middle";
      ctx.fillText("G", 0, 0);
    }

    /* ============================================================
       CONCENTRATION COUNTERS
    ============================================================ */

    function drawConcentrationCounters() {
      const lesson = lessons[currentLessonIndex];
      const type = lesson.requiredType;
      const name = moleculeTypes[type].name;

      const outsideCount =
        countMolecules(type, "outside");

      const insideCount =
        countMolecules(type, "inside");

      drawCounter(
        WIDTH - 250,
        18,
        `${name} outside: ${outsideCount}`
      );

      drawCounter(
        WIDTH - 250,
        HEIGHT - 61,
        `${name} inside: ${insideCount}`
      );
    }

    function drawCounter(x, y, text) {
      ctx.font =
        "800 14px system-ui, sans-serif";

      const width =
        ctx.measureText(text).width + 24;

      ctx.fillStyle =
        "rgba(255,255,255,0.9)";

      ctx.strokeStyle =
        "rgba(31,72,110,0.22)";

      ctx.lineWidth = 1;

      roundedRectangle(
        x,
        y,
        width,
        42,
        11,
        true,
        true
      );

      ctx.fillStyle = "#245276";
      ctx.textAlign = "left";
      ctx.textBaseline = "middle";

      ctx.fillText(
        text,
        x + 12,
        y + 21
      );
    }

    /* ============================================================
       MAIN RENDER LOOP
    ============================================================ */

    function render(timestamp) {
      if (!lastAnimationTime) {
        lastAnimationTime = timestamp;
      }

      let deltaTime =
        (timestamp - lastAnimationTime) / 1000;

      deltaTime = Math.min(deltaTime, 0.035);

      lastAnimationTime = timestamp;
      elapsedAnimationTime = timestamp;

      updateMoleculeMotion(deltaTime);

      ctx.clearRect(0, 0, WIDTH, HEIGHT);

      drawBackground();
      drawMembrane();
      drawTargetHighlight();
      drawProteins();

      for (const molecule of molecules) {
        if (molecule !== draggedMolecule) {
          drawMolecule(molecule);
        }
      }

      if (draggedMolecule) {
        drawMolecule(draggedMolecule);
      }

      drawConcentrationCounters();

      requestAnimationFrame(render);
    }

    /* ============================================================
       POINTER AND DRAG INTERACTION
    ============================================================ */

    function getCanvasCoordinates(event) {
      const rectangle =
        canvas.getBoundingClientRect();

      const scaleX =
        canvas.width / rectangle.width;

      const scaleY =
        canvas.height / rectangle.height;

      return {
        x:
          (event.clientX - rectangle.left) *
          scaleX,

        y:
          (event.clientY - rectangle.top) *
          scaleY
      };
    }

    function findMoleculeAt(x, y) {
      for (
        let index = molecules.length - 1;
        index >= 0;
        index--
      ) {
        const molecule = molecules[index];

        const dx = x - molecule.x;
        const dy = y - molecule.y;

        const hitRadius =
          molecule.radius + 9;

        if (
          dx * dx + dy * dy <=
          hitRadius * hitRadius
        ) {
          return molecule;
        }
      }

      return null;
    }

    canvas.addEventListener(
      "pointerdown",
      (event) => {
        event.preventDefault();

        const position =
          getCanvasCoordinates(event);

        const molecule =
          findMoleculeAt(
            position.x,
            position.y
          );

        if (!molecule) {
          return;
        }

        draggedMolecule = molecule;
        molecule.dragging = true;

        dragStartPosition = {
          x: molecule.x,
          y: molecule.y,
          vx: molecule.vx,
          vy: molecule.vy
        };

        dragStartSide = molecule.side;

        dragOffsetX =
          position.x - molecule.x;

        dragOffsetY =
          position.y - molecule.y;

        canvas.classList.add("dragging");

        canvas.setPointerCapture(
          event.pointerId
        );

        selectMoleculeType(
          molecule.type,
          false
        );
      }
    );

    canvas.addEventListener(
      "pointermove",
      (event) => {
        if (!draggedMolecule) {
          return;
        }

        event.preventDefault();

        const position =
          getCanvasCoordinates(event);

        draggedMolecule.x = clamp(
          position.x - dragOffsetX,
          draggedMolecule.radius,
          WIDTH - draggedMolecule.radius
        );

        draggedMolecule.y = clamp(
          position.y - dragOffsetY,
          draggedMolecule.radius,
          HEIGHT - draggedMolecule.radius
        );
      }
    );

    function finishDrag(event) {
      if (!draggedMolecule) {
        return;
      }

      event.preventDefault();

      const molecule = draggedMolecule;

      molecule.dragging = false;
      draggedMolecule = null;

      canvas.classList.remove("dragging");

      const targetSide =
        sideFromY(molecule.y);

      if (targetSide === "membrane") {
        restoreDraggedMolecule(molecule);

        setFeedback(
          "Release the molecule completely on the opposite " +
          "side of the membrane.",
          "warning"
        );

        return;
      }

      if (targetSide === dragStartSide) {
        molecule.side = dragStartSide;

        setFeedback(
          "The molecule remained on the same side of the membrane.",
          "neutral"
        );

        return;
      }

      const route =
        determineRoute(molecule.x);

      attemptTransport(
        molecule,
        targetSide,
        route
      );
    }

    canvas.addEventListener(
      "pointerup",
      finishDrag
    );

    canvas.addEventListener(
      "pointercancel",
      finishDrag
    );

    /* ============================================================
       TOOLBAR EVENTS
    ============================================================ */

    particleButtons.forEach((button) => {
      button.addEventListener("click", () => {
        selectMoleculeType(
          button.dataset.particle
        );
      });
    });

    addButton.addEventListener("click", () => {
      addMolecule(selectedType, "outside");

      setFeedback(
        `Added one ${moleculeTypes[
          selectedType
        ].name.toLowerCase()} molecule outside the cell.`,
        "neutral"
      );
    });

    addRequiredButton.addEventListener(
      "click",
      () => {
        const lesson =
          lessons[currentLessonIndex];

        addMolecule(
          lesson.requiredType,
          lesson.requiredStart
        );

        selectMoleculeType(
          lesson.requiredType,
          false
        );

        setFeedback(
          `Added one ${moleculeTypes[
            lesson.requiredType
          ].name.toLowerCase()} molecule to the required starting area.`,
          "neutral"
        );
      }
    );

    pauseButton.addEventListener("click", () => {
      motionPaused = !motionPaused;

      pauseButton.textContent =
        motionPaused
          ? "Resume motion"
          : "Pause motion";

      setFeedback(
        motionPaused
          ? "Molecular motion is paused. You can still drag molecules."
          : "Molecular motion resumed.",
        "neutral"
      );
    });

    speedSlider.addEventListener("input", () => {
      motionMultiplier =
        Number(speedSlider.value) / 100;

      speedValue.textContent =
        `${motionMultiplier.toFixed(1)}×`;
    });

    resetButton.addEventListener("click", () => {
      configureLesson(currentLessonIndex);
    });

    /* ============================================================
       GUIDED INVESTIGATION EVENTS
    ============================================================ */

    hintButton.addEventListener("click", () => {
      const isVisible =
        hintBox.classList.toggle("visible");

      hintButton.textContent =
        isVisible
          ? "Hide hint"
          : "Show hint";
    });

    previousButton.addEventListener(
      "click",
      () => {
        if (currentLessonIndex > 0) {
          configureLesson(
            currentLessonIndex - 1
          );
        }
      }
    );

    nextButton.addEventListener(
      "click",
      () => {
        if (
          currentLessonIndex <
          lessons.length - 1
        ) {
          configureLesson(
            currentLessonIndex + 1
          );
        } else {
          configureLesson(0);
        }
      }
    );

    /* ============================================================
       INITIALIZATION
    ============================================================ */

    configureLesson(0);
    requestAnimationFrame(render);
  </script>
</body>
</html>
