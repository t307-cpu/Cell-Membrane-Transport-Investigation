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

      --secondary: #0d9488;
      --secondary-dark: #0f766e;

      --success: #15803d;
      --success-soft: #ecfdf3;

      --warning: #a65b08;
      --warning-soft: #fff7e8;

      --danger: #b91c1c;
      --danger-soft: #fff1f1;

      --gold: #d49b17;
      --gold-soft: #fff8df;

      --shadow: 0 14px 35px rgba(31, 56, 88, 0.12);
    }

    * {
      box-sizing: border-box;
    }

    html {
      scroll-behavior: smooth;
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

    fieldset {
      min-width: 0;
    }

    .app {
      width: min(1500px, 100%);
      margin: 0 auto;
    }

    /* ============================================================
       APPLICATION PAGES
    ============================================================ */

    .app-page {
      display: none;
      animation: pageFade 300ms ease;
    }

    .app-page.active {
      display: block;
    }

    @keyframes pageFade {
      from {
        opacity: 0;
        transform: translateY(8px);
      }

      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    /* ============================================================
       GLOBAL HEADER
    ============================================================ */

    .page-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 24px;

      margin-bottom: 18px;
      padding: 22px 26px;

      border: 1px solid rgba(213, 224, 235, 0.92);
      border-radius: 22px;

      background: rgba(255, 255, 255, 0.94);
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

    /* ============================================================
       OVERALL STEP INDICATOR
    ============================================================ */

    .course-progress {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;

      margin-bottom: 18px;
      padding: 12px;

      border: 1px solid var(--border);
      border-radius: 18px;

      background: rgba(255, 255, 255, 0.92);
      box-shadow: var(--shadow);
    }

    .course-step {
      display: flex;
      align-items: center;
      gap: 10px;

      min-width: 0;
      padding: 10px 12px;

      border: 1px solid transparent;
      border-radius: 12px;

      color: #68778a;
      background: #f7fafc;
    }

    .course-step-number {
      display: grid;
      flex: 0 0 auto;
      place-items: center;

      width: 29px;
      height: 29px;

      border: 2px solid #b9c7d6;
      border-radius: 50%;

      background: #ffffff;
      font-size: 0.78rem;
      font-weight: 900;
    }

    .course-step-label {
      overflow: hidden;
      font-size: 0.8rem;
      font-weight: 850;
      text-overflow: ellipsis;
      white-space: nowrap;
    }

    .course-step.active {
      border-color: #9abcf5;
      background: var(--primary-soft);
      color: var(--primary-dark);
    }

    .course-step.active .course-step-number {
      border-color: var(--primary);
      background: var(--primary);
      color: #ffffff;
    }

    .course-step.completed {
      border-color: #a7d7b4;
      background: var(--success-soft);
      color: var(--success);
    }

    .course-step.completed .course-step-number {
      border-color: var(--success);
      background: var(--success);
      color: #ffffff;
    }

    /* ============================================================
       COMMON CARDS AND BUTTONS
    ============================================================ */

    .content-card {
      width: min(900px, 100%);
      margin: 0 auto;
      padding: 30px;

      border: 1px solid var(--border);
      border-radius: 24px;

      background: rgba(255, 255, 255, 0.96);
      box-shadow: var(--shadow);
    }

    .content-card h2 {
      margin: 0 0 9px;
      color: #17375f;

      font-size: clamp(1.55rem, 3vw, 2.1rem);
    }

    .card-introduction {
      margin: 0 0 24px;
      color: var(--muted);

      font-size: 0.98rem;
      line-height: 1.65;
    }

    .primary-button,
    .secondary-button,
    .outline-button,
    .navigation-button,
    .action-button,
    .control-button,
    .particle-button {
      border: 1px solid var(--border);
      border-radius: 10px;

      background: #ffffff;
      cursor: pointer;

      font-weight: 800;

      transition:
        transform 120ms ease,
        background-color 120ms ease,
        border-color 120ms ease,
        box-shadow 120ms ease;
    }

    .primary-button,
    .secondary-button,
    .outline-button {
      padding: 11px 18px;
    }

    .primary-button {
      border-color: var(--primary);
      background: var(--primary);
      color: #ffffff;
    }

    .primary-button:hover:not(:disabled) {
      transform: translateY(-1px);
      background: var(--primary-dark);
      box-shadow: 0 7px 18px rgba(37, 99, 235, 0.2);
    }

    .secondary-button {
      border-color: var(--secondary);
      background: var(--secondary);
      color: #ffffff;
    }

    .secondary-button:hover:not(:disabled) {
      transform: translateY(-1px);
      background: var(--secondary-dark);
    }

    .outline-button:hover:not(:disabled),
    .control-button:hover:not(:disabled),
    .particle-button:hover:not(:disabled),
    .navigation-button:hover:not(:disabled),
    .action-button:hover:not(:disabled) {
      transform: translateY(-1px);
      border-color: #8eb5f5;
      box-shadow: 0 5px 14px rgba(37, 99, 235, 0.12);
    }

    button:disabled {
      cursor: not-allowed;
      opacity: 0.48;
    }

    .page-button-row {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      gap: 10px;

      margin-top: 24px;
    }

    .page-button-row.end {
      justify-content: flex-end;
    }

    /* ============================================================
       STUDENT INFORMATION PAGE
    ============================================================ */

    .welcome-layout {
      display: grid;
      grid-template-columns: 0.9fr 1.1fr;
      gap: 24px;
      align-items: stretch;
    }

    .welcome-visual {
      display: flex;
      flex-direction: column;
      justify-content: center;

      min-height: 400px;
      padding: 28px;

      overflow: hidden;
      position: relative;

      border-radius: 20px;

      background:
        radial-gradient(circle at 20% 20%, rgba(255, 255, 255, 0.32), transparent 22%),
        linear-gradient(145deg, #2563eb, #0d9488);

      color: #ffffff;
    }

    .welcome-visual::after {
      content: "";

      position: absolute;
      right: -70px;
      bottom: -85px;

      width: 250px;
      height: 250px;

      border: 30px solid rgba(255, 255, 255, 0.09);
      border-radius: 50%;
    }

    .welcome-icon {
      display: grid;
      place-items: center;

      width: 84px;
      height: 84px;
      margin-bottom: 22px;

      border: 3px solid rgba(255, 255, 255, 0.65);
      border-radius: 50%;

      background: rgba(255, 255, 255, 0.14);
      font-size: 2.5rem;
    }

    .welcome-visual h3 {
      position: relative;
      z-index: 1;

      margin: 0 0 12px;
      font-size: 1.65rem;
    }

    .welcome-visual p {
      position: relative;
      z-index: 1;

      margin: 0 0 20px;
      line-height: 1.6;
    }

    .welcome-list {
      position: relative;
      z-index: 1;

      display: grid;
      gap: 10px;

      margin: 0;
      padding: 0;

      list-style: none;
    }

    .welcome-list li {
      display: flex;
      align-items: center;
      gap: 10px;

      font-size: 0.92rem;
      font-weight: 700;
    }

    .welcome-list li::before {
      content: "✓";

      display: grid;
      flex: 0 0 auto;
      place-items: center;

      width: 22px;
      height: 22px;

      border-radius: 50%;
      background: rgba(255, 255, 255, 0.2);

      font-size: 0.75rem;
    }

    .student-form {
      display: flex;
      flex-direction: column;
      justify-content: center;
    }

    .student-form h2 {
      margin-bottom: 9px;
    }

    .form-grid {
      display: grid;
      gap: 17px;
    }

    .form-group {
      display: grid;
      gap: 7px;
    }

    .form-group label {
      color: #33465d;
      font-size: 0.87rem;
      font-weight: 850;
    }

    .required-mark {
      color: var(--danger);
    }

    .form-group input {
      width: 100%;
      padding: 12px 13px;

      border: 1px solid #bbc9d8;
      border-radius: 10px;

      background: #ffffff;
      color: var(--text);
      outline: none;

      transition:
        border-color 120ms ease,
        box-shadow 120ms ease;
    }

    .form-group input:focus {
      border-color: var(--primary);
      box-shadow: 0 0 0 4px rgba(37, 99, 235, 0.11);
    }

    .form-group input.invalid {
      border-color: var(--danger);
      background: var(--danger-soft);
    }

    .form-help {
      color: var(--muted);
      font-size: 0.76rem;
      line-height: 1.4;
    }

    .form-error {
      display: none;

      padding: 11px 13px;
      border: 1px solid #efa3a3;
      border-radius: 10px;

      background: var(--danger-soft);
      color: #991b1b;

      font-size: 0.86rem;
      font-weight: 700;
    }

    .form-error.visible {
      display: block;
    }

    /* ============================================================
       SIMULATION LAYOUT
    ============================================================ */

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

    /* ============================================================
       SIMULATION TOOLBAR
    ============================================================ */

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

    .particle-button {
      display: inline-flex;
      align-items: center;
      gap: 7px;

      padding: 8px 10px;

      font-size: 0.82rem;
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

    /* ============================================================
       CANVAS
    ============================================================ */

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

    /* ============================================================
       GUIDED INVESTIGATION PANEL
    ============================================================ */

    .investigation-panel {
      position: sticky;
      top: 18px;
      padding: 19px;
    }

    .student-mini-card {
      margin-bottom: 15px;
      padding: 10px 12px;

      border: 1px solid #bad0ef;
      border-radius: 10px;

      background: #f1f6ff;
      color: #294c78;

      font-size: 0.8rem;
      line-height: 1.45;
    }

    .student-mini-card strong {
      color: #17375f;
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
      border-color: var(--secondary);
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

    .lesson-actions {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;

      margin-bottom: 10px;
    }

    .action-button {
      padding: 9px 10px;
      font-size: 0.8rem;
    }

    .lesson-navigation {
      display: flex;
      gap: 9px;
    }

    .navigation-button {
      flex: 1;
      padding: 10px 11px;
    }

    .navigation-button.next {
      border-color: var(--primary);
      background: var(--primary);
      color: #ffffff;
    }

    .navigation-button.next:hover:not(:disabled) {
      background: var(--primary-dark);
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

    .simulation-page-footer {
      margin-top: 18px;
      text-align: center;
    }

    /* ============================================================
       QUIZ PAGE
    ============================================================ */

    .quiz-card {
      width: min(1000px, 100%);
    }

    .quiz-information {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;

      margin-bottom: 22px;
    }

    .quiz-information-item {
      padding: 12px;

      border: 1px solid var(--border);
      border-radius: 12px;

      background: #f8fbff;
      text-align: center;
    }

    .quiz-information-item span {
      display: block;
      margin-bottom: 4px;

      color: var(--muted);
      font-size: 0.72rem;
      font-weight: 800;
      text-transform: uppercase;
    }

    .quiz-information-item strong {
      color: #1e416c;
      font-size: 1rem;
    }

    .quiz-form {
      display: grid;
      gap: 18px;
    }

    .question-card {
      margin: 0;
      padding: 18px;

      border: 1px solid var(--border);
      border-radius: 15px;

      background: #ffffff;
    }

    .question-card.unanswered {
      border-color: #e2a34a;
      background: #fffaf0;
    }

    .question-card legend {
      width: 100%;
      padding: 0 0 12px;

      color: #263d5b;
      font-size: 0.96rem;
      font-weight: 850;
      line-height: 1.5;
    }

    .question-number {
      display: inline-grid;
      place-items: center;

      width: 27px;
      height: 27px;
      margin-right: 7px;

      border-radius: 50%;

      background: var(--primary);
      color: #ffffff;

      font-size: 0.76rem;
      vertical-align: middle;
    }

    .answer-list {
      display: grid;
      gap: 8px;
    }

    .answer-option {
      display: flex;
      align-items: flex-start;
      gap: 10px;

      padding: 11px 12px;

      border: 1px solid #d8e1ea;
      border-radius: 10px;

      background: #fafcff;
      cursor: pointer;

      transition:
        border-color 120ms ease,
        background-color 120ms ease,
        transform 120ms ease;
    }

    .answer-option:hover {
      transform: translateX(2px);
      border-color: #93b7f2;
      background: #f2f7ff;
    }

    .answer-option:has(input:checked) {
      border-color: var(--primary);
      background: var(--primary-soft);
      color: #173e7a;
    }

    .answer-option input {
      flex: 0 0 auto;
      margin-top: 3px;
      accent-color: var(--primary);
    }

    .quiz-error {
      display: none;

      margin-top: 18px;
      padding: 12px 14px;

      border: 1px solid #e3a451;
      border-radius: 10px;

      background: var(--warning-soft);
      color: #8a4906;

      font-size: 0.88rem;
      font-weight: 750;
    }

    .quiz-error.visible {
      display: block;
    }

    /* ============================================================
       CERTIFICATE PAGE
    ============================================================ */

    .certificate-wrapper {
      width: min(1050px, 100%);
      margin: 0 auto;
    }

    .certificate {
      position: relative;

      min-height: 680px;
      padding: 22px;

      overflow: hidden;

      border: 2px solid #c89a2d;
      border-radius: 8px;

      background:
        radial-gradient(circle at 10% 10%, rgba(212, 155, 23, 0.08), transparent 20%),
        radial-gradient(circle at 90% 90%, rgba(37, 99, 235, 0.08), transparent 22%),
        #fffef8;

      box-shadow: var(--shadow);
    }

    .certificate-inner {
      display: flex;
      align-items: center;
      flex-direction: column;
      justify-content: center;

      min-height: 632px;
      padding: 36px;

      border: 5px double #d6ab43;

      text-align: center;
    }

    .certificate-seal {
      display: grid;
      place-items: center;

      width: 90px;
      height: 90px;
      margin-bottom: 16px;

      border: 5px double #c9951c;
      border-radius: 50%;

      background: var(--gold-soft);
      color: #a66e00;

      font-size: 2.25rem;
      box-shadow: 0 6px 20px rgba(154, 103, 0, 0.16);
    }

    .certificate-small-title {
      margin-bottom: 8px;
      color: #93670d;

      font-size: 0.82rem;
      font-weight: 900;
      letter-spacing: 0.2em;
      text-transform: uppercase;
    }

    .certificate h2 {
      margin: 0 0 8px;
      color: #17375f;

      font-family: Georgia, "Times New Roman", serif;
      font-size: clamp(2.2rem, 6vw, 4.5rem);
      font-weight: 700;
      line-height: 1.05;
    }

    .certificate-intro {
      margin: 13px 0 6px;
      color: #536274;

      font-family: Georgia, "Times New Roman", serif;
      font-size: 1.05rem;
    }

    .certificate-name {
      min-width: min(600px, 90%);
      margin: 8px 0 5px;
      padding: 5px 20px 9px;

      border-bottom: 2px solid #b98a25;

      color: #1d4ed8;

      font-family: Georgia, "Times New Roman", serif;
      font-size: clamp(1.8rem, 5vw, 3.3rem);
      font-weight: 700;
      line-height: 1.2;
    }

    .certificate-statement {
      max-width: 760px;
      margin: 11px auto 17px;

      color: #47566a;
      font-family: Georgia, "Times New Roman", serif;
      font-size: 1rem;
      line-height: 1.65;
    }

    .certificate-score {
      margin: 5px 0 13px;
      color: #17375f;

      font-family: Georgia, "Times New Roman", serif;
      font-size: clamp(1.5rem, 4vw, 2.3rem);
      font-weight: 700;
    }

    .certificate-score strong {
      color: var(--success);
    }

    .certificate-grade {
      display: inline-block;

      margin-bottom: 20px;
      padding: 8px 18px;

      border: 1px solid #d4a845;
      border-radius: 999px;

      background: var(--gold-soft);
      color: #805800;

      font-size: 0.9rem;
      font-weight: 900;
    }

    .certificate-details {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 15px;

      width: min(780px, 100%);
      margin-top: 8px;
    }

    .certificate-detail {
      padding: 9px;

      border-top: 1px solid #b99a59;

      color: #33465d;
      font-size: 0.86rem;
    }

    .certificate-detail span {
      display: block;

      margin-bottom: 3px;
      color: #7a6843;

      font-size: 0.68rem;
      font-weight: 850;
      letter-spacing: 0.08em;
      text-transform: uppercase;
    }

    .results-panel {
      margin-top: 20px;
      padding: 22px;

      border: 1px solid var(--border);
      border-radius: 18px;

      background: #ffffff;
      box-shadow: var(--shadow);
    }

    .results-panel h3 {
      margin: 0 0 14px;
      color: #17375f;
    }

    .results-summary {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;

      margin-bottom: 17px;
    }

    .result-stat {
      padding: 14px;

      border: 1px solid var(--border);
      border-radius: 12px;

      background: #f8fbff;
      text-align: center;
    }

    .result-stat span {
      display: block;
      color: var(--muted);

      font-size: 0.72rem;
      font-weight: 850;
      text-transform: uppercase;
    }

    .result-stat strong {
      display: block;
      margin-top: 5px;

      color: #1e416c;
      font-size: 1.35rem;
    }

    .answer-review {
      display: grid;
      gap: 10px;
    }

    .review-item {
      padding: 12px 14px;

      border: 1px solid var(--border);
      border-left: 5px solid;
      border-radius: 10px;

      font-size: 0.87rem;
      line-height: 1.5;
    }

    .review-item.correct {
      border-left-color: var(--success);
      background: var(--success-soft);
    }

    .review-item.incorrect {
      border-left-color: var(--danger);
      background: var(--danger-soft);
    }

    .review-item strong {
      display: block;
      margin-bottom: 3px;
    }

    .certificate-actions {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;

      margin-top: 20px;
    }

    /* ============================================================
       RESPONSIVE DESIGN
    ============================================================ */

    @media (max-width: 1080px) {
      .main-layout {
        grid-template-columns: 1fr;
      }

      .investigation-panel {
        position: static;
      }
    }

    @media (max-width: 800px) {
      .welcome-layout {
        grid-template-columns: 1fr;
      }

      .welcome-visual {
        min-height: 300px;
      }

      .course-progress {
        grid-template-columns: repeat(2, 1fr);
      }

      .certificate-details,
      .results-summary,
      .quiz-information {
        grid-template-columns: 1fr;
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

      .content-card {
        padding: 20px;
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

      .certificate {
        padding: 10px;
      }

      .certificate-inner {
        min-height: 600px;
        padding: 24px 14px;
      }
    }

    @media (max-width: 500px) {
      .course-progress {
        grid-template-columns: 1fr;
      }

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

      .page-button-row,
      .certificate-actions {
        flex-direction: column;
      }

      .page-button-row button,
      .certificate-actions button {
        width: 100%;
      }

      .certificate-name {
        min-width: 100%;
      }
    }

    /* ============================================================
       PRINTED CERTIFICATE
    ============================================================ */

    @media print {
      @page {
        size: landscape;
        margin: 8mm;
      }

      body {
        padding: 0;
        background: #ffffff;
      }

      .page-header,
      .course-progress,
      .results-panel,
      .certificate-actions,
      #studentPage,
      #simulationPage,
      #quizPage {
        display: none !important;
      }

      #certificatePage {
        display: block !important;
      }

      .certificate-wrapper {
        width: 100%;
        margin: 0;
      }

      .certificate {
        min-height: 180mm;
        box-shadow: none;
        print-color-adjust: exact;
        -webkit-print-color-adjust: exact;
      }

      .certificate-inner {
        min-height: 168mm;
      }
    }
  </style>
</head>

<body>
  <main class="app">
    <!-- ==========================================================
         GLOBAL HEADER
    =========================================================== -->

    <header class="page-header">
      <div class="title-area">
        <div class="eyebrow">Interactive biology laboratory</div>

        <h1>Cell Membrane Transport Investigation</h1>

        <p>
          Complete the guided membrane simulation, answer the assessment
          questions, and receive a personalized certificate showing your
          results.
        </p>
      </div>

      <div id="headerBadge" class="header-badge">
        Step 1: Student information
      </div>
    </header>

    <!-- ==========================================================
         COURSE PROGRESS
    =========================================================== -->

    <nav class="course-progress" aria-label="Investigation progress">
      <div class="course-step active" data-course-step="0">
        <span class="course-step-number">1</span>
        <span class="course-step-label">Student details</span>
      </div>

      <div class="course-step" data-course-step="1">
        <span class="course-step-number">2</span>
        <span class="course-step-label">Simulation</span>
      </div>

      <div class="course-step" data-course-step="2">
        <span class="course-step-number">3</span>
        <span class="course-step-label">Questions</span>
      </div>

      <div class="course-step" data-course-step="3">
        <span class="course-step-number">4</span>
        <span class="course-step-label">Certificate</span>
      </div>
    </nav>

    <!-- ==========================================================
         PAGE 1: STUDENT INFORMATION
    =========================================================== -->

    <section
      id="studentPage"
      class="app-page active"
      aria-labelledby="studentPageTitle"
    >
      <div class="content-card">
        <div class="welcome-layout">
          <div class="welcome-visual">
            <div class="welcome-icon" aria-hidden="true">🔬</div>

            <h3>Welcome to the investigation</h3>

            <p>
              You will investigate how substances move across a cell
              membrane and then complete a short assessment.
            </p>

            <ul class="welcome-list">
              <li>Complete five interactive transport activities</li>
              <li>Investigate diffusion, osmosis, and active transport</li>
              <li>Answer eight assessment questions</li>
              <li>Receive a personalized certificate</li>
            </ul>
          </div>

          <form id="studentForm" class="student-form" novalidate>
            <div class="eyebrow">Before you begin</div>

            <h2 id="studentPageTitle">Enter your information</h2>

            <p class="card-introduction">
              This information will appear on your certificate. Check that
              your name and class information are correct before continuing.
            </p>

            <div class="form-grid">
              <div class="form-group">
                <label for="studentName">
                  Student name
                  <span class="required-mark">*</span>
                </label>

                <input
                  id="studentName"
                  name="studentName"
                  type="text"
                  maxlength="80"
                  autocomplete="name"
                  placeholder="Enter your full name"
                  required
                />

                <span class="form-help">
                  Enter the name that should appear on the certificate.
                </span>
              </div>

              <div class="form-group">
                <label for="studentClass">
                  Class
                  <span class="required-mark">*</span>
                </label>

                <input
                  id="studentClass"
                  name="studentClass"
                  type="text"
                  maxlength="40"
                  placeholder="For example: Biology 8A"
                  required
                />
              </div>

              <div class="form-group">
                <label for="studentNumber">
                  Class number
                  <span class="required-mark">*</span>
                </label>

                <input
                  id="studentNumber"
                  name="studentNumber"
                  type="text"
                  maxlength="20"
                  placeholder="For example: 17"
                  required
                />
              </div>

              <div
                id="studentFormError"
                class="form-error"
                role="alert"
              >
                Please complete all three fields before beginning.
              </div>
            </div>

            <div class="page-button-row end">
              <button class="primary-button" type="submit">
                Begin investigation
              </button>
            </div>
          </form>
        </div>
      </div>
    </section>

    <!-- ==========================================================
         PAGE 2: SIMULATION
    =========================================================== -->

    <section
      id="simulationPage"
      class="app-page"
      aria-label="Interactive membrane simulation"
    >
      <div class="main-layout">
        <!-- Simulation panel -->

        <section
          class="simulation-panel"
          aria-label="Cell membrane simulation"
        >
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
              Reset activity
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

        <!-- Guided investigation panel -->

        <aside class="investigation-panel">
          <div id="simulationStudent" class="student-mini-card"></div>

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

          <p
            id="lessonDescription"
            class="lesson-description"
          ></p>

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

            <button
              id="addRequiredButton"
              class="action-button"
              type="button"
            >
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
              disabled
            >
              Complete activity first
            </button>
          </div>

          <div id="completionBanner" class="completion-banner">
            Investigation task completed.
          </div>
        </aside>
      </div>

      <div class="simulation-page-footer">
        <button
          id="simulationBackButton"
          class="outline-button"
          type="button"
        >
          Edit student information
        </button>
      </div>
    </section>

    <!-- ==========================================================
         PAGE 3: QUESTIONS
    =========================================================== -->

    <section
      id="quizPage"
      class="app-page"
      aria-labelledby="quizPageTitle"
    >
      <div class="content-card quiz-card">
        <div class="eyebrow">Knowledge assessment</div>

        <h2 id="quizPageTitle">Cell membrane transport questions</h2>

        <p class="card-introduction">
          Answer every question. Your score and answer review will appear
          with your certificate after submission.
        </p>

        <div class="quiz-information">
          <div class="quiz-information-item">
            <span>Student</span>
            <strong id="quizStudentName">—</strong>
          </div>

          <div class="quiz-information-item">
            <span>Class</span>
            <strong id="quizStudentClass">—</strong>
          </div>

          <div class="quiz-information-item">
            <span>Questions</span>
            <strong>8</strong>
          </div>
        </div>

        <form id="quizForm" class="quiz-form" novalidate>
          <!-- Question 1 -->

          <fieldset class="question-card" data-question-card="q1">
            <legend>
              <span class="question-number">1</span>
              Which substance can move directly through the phospholipid
              bilayer by simple diffusion?
            </legend>

            <div class="answer-list">
              <label class="answer-option">
                <input type="radio" name="q1" value="a" />
                <span>A charged ion</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q1" value="b" />
                <span>Oxygen</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q1" value="c" />
                <span>Glucose</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q1" value="d" />
                <span>A large protein</span>
              </label>
            </div>
          </fieldset>

          <!-- Question 2 -->

          <fieldset class="question-card" data-question-card="q2">
            <legend>
              <span class="question-number">2</span>
              What is osmosis?
            </legend>

            <div class="answer-list">
              <label class="answer-option">
                <input type="radio" name="q2" value="a" />
                <span>
                  The movement of water across a selectively permeable
                  membrane
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q2" value="b" />
                <span>
                  The movement of glucose using ATP
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q2" value="c" />
                <span>
                  The production of ATP inside a cell
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q2" value="d" />
                <span>
                  The movement of proteins through the nucleus
                </span>
              </label>
            </div>
          </fieldset>

          <!-- Question 3 -->

          <fieldset class="question-card" data-question-card="q3">
            <legend>
              <span class="question-number">3</span>
              Which membrane protein provides a channel for water?
            </legend>

            <div class="answer-list">
              <label class="answer-option">
                <input type="radio" name="q3" value="a" />
                <span>ATP synthase</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q3" value="b" />
                <span>Glucose carrier</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q3" value="c" />
                <span>Aquaporin</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q3" value="d" />
                <span>DNA polymerase</span>
              </label>
            </div>
          </fieldset>

          <!-- Question 4 -->

          <fieldset class="question-card" data-question-card="q4">
            <legend>
              <span class="question-number">4</span>
              Facilitated diffusion moves substances:
            </legend>

            <div class="answer-list">
              <label class="answer-option">
                <input type="radio" name="q4" value="a" />
                <span>
                  Against the concentration gradient using ATP
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q4" value="b" />
                <span>
                  Down the concentration gradient using a membrane protein
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q4" value="c" />
                <span>
                  Only through the phospholipid bilayer
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q4" value="d" />
                <span>
                  From low concentration to high concentration without energy
                </span>
              </label>
            </div>
          </fieldset>

          <!-- Question 5 -->

          <fieldset class="question-card" data-question-card="q5">
            <legend>
              <span class="question-number">5</span>
              Why do charged ions usually require a membrane protein?
            </legend>

            <div class="answer-list">
              <label class="answer-option">
                <input type="radio" name="q5" value="a" />
                <span>
                  The hydrophobic interior of the membrane restricts charged
                  particles
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q5" value="b" />
                <span>
                  Ions are always larger than cells
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q5" value="c" />
                <span>
                  Ions do not move
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q5" value="d" />
                <span>
                  The membrane is made entirely of water
                </span>
              </label>
            </div>
          </fieldset>

          <!-- Question 6 -->

          <fieldset class="question-card" data-question-card="q6">
            <legend>
              <span class="question-number">6</span>
              Which transport process requires ATP?
            </legend>

            <div class="answer-list">
              <label class="answer-option">
                <input type="radio" name="q6" value="a" />
                <span>Simple diffusion</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q6" value="b" />
                <span>Osmosis</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q6" value="c" />
                <span>Facilitated diffusion</span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q6" value="d" />
                <span>Active transport</span>
              </label>
            </div>
          </fieldset>

          <!-- Question 7 -->

          <fieldset class="question-card" data-question-card="q7">
            <legend>
              <span class="question-number">7</span>
              Active transport can move a substance:
            </legend>

            <div class="answer-list">
              <label class="answer-option">
                <input type="radio" name="q7" value="a" />
                <span>
                  From high concentration to low concentration only
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q7" value="b" />
                <span>
                  From low concentration to high concentration
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q7" value="c" />
                <span>
                  Without a membrane
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q7" value="d" />
                <span>
                  Only if the substance is oxygen
                </span>
              </label>
            </div>
          </fieldset>

          <!-- Question 8 -->

          <fieldset class="question-card" data-question-card="q8">
            <legend>
              <span class="question-number">8</span>
              Why does glucose generally use a carrier protein to cross the
              membrane?
            </legend>

            <div class="answer-list">
              <label class="answer-option">
                <input type="radio" name="q8" value="a" />
                <span>
                  Glucose is a relatively large polar molecule
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q8" value="b" />
                <span>
                  Glucose is a small nonpolar gas
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q8" value="c" />
                <span>
                  Glucose is made of ATP
                </span>
              </label>

              <label class="answer-option">
                <input type="radio" name="q8" value="d" />
                <span>
                  Glucose cannot move down a concentration gradient
                </span>
              </label>
            </div>
          </fieldset>

          <div id="quizError" class="quiz-error" role="alert">
            Please answer every question before submitting your assessment.
          </div>

          <div class="page-button-row">
            <button
              id="quizBackButton"
              class="outline-button"
              type="button"
            >
              Return to simulation
            </button>

            <button class="primary-button" type="submit">
              Submit answers and create certificate
            </button>
          </div>
        </form>
      </div>
    </section>

    <!-- ==========================================================
         PAGE 4: CERTIFICATE AND RESULTS
    =========================================================== -->

    <section
      id="certificatePage"
      class="app-page"
      aria-label="Certificate and assessment results"
    >
      <div class="certificate-wrapper">
        <article id="certificate" class="certificate">
          <div class="certificate-inner">
            <div class="certificate-seal" aria-hidden="true">★</div>

            <div class="certificate-small-title">
              Certificate of completion
            </div>

            <h2>Cell Membrane Investigation</h2>

            <p class="certificate-intro">
              This certificate is presented to
            </p>

            <div id="certificateName" class="certificate-name">
              Student Name
            </div>

            <p class="certificate-statement">
              for successfully completing five interactive investigations
              of simple diffusion, osmosis, facilitated diffusion, active
              transport, and glucose transport, followed by the cell
              membrane knowledge assessment.
            </p>

            <div class="certificate-score">
              Assessment score:
              <strong id="certificateScore">0%</strong>
            </div>

            <div id="certificateGrade" class="certificate-grade">
              Investigation completed
            </div>

            <div class="certificate-details">
              <div class="certificate-detail">
                <span>Class</span>
                <strong id="certificateClass">—</strong>
              </div>

              <div class="certificate-detail">
                <span>Class number</span>
                <strong id="certificateNumber">—</strong>
              </div>

              <div class="certificate-detail">
                <span>Date completed</span>
                <strong id="certificateDate">—</strong>
              </div>
            </div>
          </div>
        </article>

        <section class="results-panel">
          <h3>Assessment results</h3>

          <div class="results-summary">
            <div class="result-stat">
              <span>Correct answers</span>
              <strong id="correctAnswerResult">0 / 8</strong>
            </div>

            <div class="result-stat">
              <span>Percentage</span>
              <strong id="percentageResult">0%</strong>
            </div>

            <div class="result-stat">
              <span>Performance</span>
              <strong id="performanceResult">—</strong>
            </div>
          </div>

          <div id="answerReview" class="answer-review"></div>
        </section>

        <div class="certificate-actions">
          <button
            id="printCertificateButton"
            class="primary-button"
            type="button"
          >
            Print certificate
          </button>

          <button
            id="retakeQuizButton"
            class="secondary-button"
            type="button"
          >
            Retake questions
          </button>

          <button
            id="restartButton"
            class="outline-button"
            type="button"
          >
            Start again
          </button>
        </div>
      </div>
    </section>
  </main>

  <script>
    "use strict";

    /* ============================================================
       PAGE NAVIGATION AND STUDENT DATA
    ============================================================ */

    const appPages = [
      document.getElementById("studentPage"),
      document.getElementById("simulationPage"),
      document.getElementById("quizPage"),
      document.getElementById("certificatePage")
    ];

    const courseSteps = document.querySelectorAll(".course-step");
    const headerBadge = document.getElementById("headerBadge");

    const pageTitles = [
      "Step 1: Student information",
      "Step 2: Interactive simulation",
      "Step 3: Assessment questions",
      "Step 4: Results and certificate"
    ];

    let currentPageIndex = 0;
    let highestPageReached = 0;

    const studentData = {
      name: "",
      className: "",
      classNumber: ""
    };

    function showPage(pageIndex) {
      currentPageIndex = pageIndex;
      highestPageReached = Math.max(highestPageReached, pageIndex);

      appPages.forEach((page, index) => {
        page.classList.toggle("active", index === pageIndex);
      });

      courseSteps.forEach((step, index) => {
        step.classList.toggle("active", index === pageIndex);
        step.classList.toggle("completed", index < pageIndex);
      });

      headerBadge.textContent = pageTitles[pageIndex];

      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });

      if (pageIndex === 1) {
        updateSimulationStudentCard();
      }

      if (pageIndex === 2) {
        updateQuizStudentInformation();
      }
    }

    /* ============================================================
       STUDENT INFORMATION
    ============================================================ */

    const studentForm = document.getElementById("studentForm");
    const studentNameInput = document.getElementById("studentName");
    const studentClassInput = document.getElementById("studentClass");
    const studentNumberInput = document.getElementById("studentNumber");
    const studentFormError = document.getElementById("studentFormError");
    const simulationStudent = document.getElementById("simulationStudent");

    function cleanInputValue(value) {
      return value.trim().replace(/\s+/g, " ");
    }

    function validateStudentForm() {
      const inputs = [
        studentNameInput,
        studentClassInput,
        studentNumberInput
      ];

      let valid = true;

      inputs.forEach((input) => {
        const hasValue = cleanInputValue(input.value).length > 0;
        input.classList.toggle("invalid", !hasValue);

        if (!hasValue) {
          valid = false;
        }
      });

      studentFormError.classList.toggle("visible", !valid);
      return valid;
    }

    studentForm.addEventListener("submit", (event) => {
      event.preventDefault();

      if (!validateStudentForm()) {
        return;
      }

      studentData.name = cleanInputValue(studentNameInput.value);
      studentData.className = cleanInputValue(studentClassInput.value);
      studentData.classNumber = cleanInputValue(studentNumberInput.value);

      configureLesson(0);
      showPage(1);
    });

    [
      studentNameInput,
      studentClassInput,
      studentNumberInput
    ].forEach((input) => {
      input.addEventListener("input", () => {
        input.classList.remove("invalid");

        if (
          cleanInputValue(studentNameInput.value) &&
          cleanInputValue(studentClassInput.value) &&
          cleanInputValue(studentNumberInput.value)
        ) {
          studentFormError.classList.remove("visible");
        }
      });
    });

    function updateSimulationStudentCard() {
      simulationStudent.innerHTML = "";

      const nameLine = document.createElement("div");
      const nameLabel = document.createElement("strong");

      nameLabel.textContent = "Student: ";
      nameLine.append(nameLabel, studentData.name);

      const classLine = document.createElement("div");
      classLine.textContent =
        `${studentData.className} • Class number ${studentData.classNumber}`;

      simulationStudent.append(nameLine, classLine);
    }

    /* ============================================================
       SIMULATION DOM REFERENCES
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

    const simulationBackButton =
      document.getElementById("simulationBackButton");

    /* ============================================================
       SIMULATION CONSTANTS
    ============================================================ */

    const WIDTH = canvas.width;
    const HEIGHT = canvas.height;

    const membraneTop = 292;
    const membraneBottom = 388;
    const membraneCenter = (membraneTop + membraneBottom) / 2;

    const proteinPositions = {
      openBilayer: 145,
      aquaporin: 385,
      carrier: 675,
      pump: 910
    };

    const moleculeTypes = {
      oxygen: {
        name: "Oxygen",
        color: "#ef4444",
        outline: "#991b1b",
        radius: 12,
        baseSpeed: 52
      },

      water: {
        name: "Water",
        color: "#38bdf8",
        outline: "#0369a1",
        radius: 10,
        baseSpeed: 66
      },

      ion: {
        name: "Ion",
        color: "#facc15",
        outline: "#a16207",
        radius: 12,
        baseSpeed: 44
      },

      glucose: {
        name: "Glucose",
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
       SIMULATION STATE
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

    const completedLessons = new Set();

    /* ============================================================
       SIMULATION UTILITIES
    ============================================================ */

    function randomBetween(minimum, maximum) {
      return minimum + Math.random() * (maximum - minimum);
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
      const x = randomBetween(padding, WIDTH - padding);

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
        return molecule.type === type && molecule.side === side;
      }).length;
    }

    function isDownGradient(type, startSide, targetSide) {
      return (
        countMolecules(type, startSide) >
        countMolecules(type, targetSide)
      );
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

      lessonCompleted = completedLessons.has(currentLessonIndex);

      canvas.classList.remove("dragging");
      hintBox.classList.remove("visible");
      hintButton.textContent = "Show hint";

      const lesson = lessons[currentLessonIndex];

      Object.entries(lesson.initialCounts).forEach(([type, counts]) => {
        addMoleculeGroup(type, counts[0], counts[1]);
      });

      selectMoleculeType(lesson.requiredType, false);
      updateLessonPanel();

      if (lessonCompleted) {
        completionBanner.textContent =
          "You previously completed this activity.";

        completionBanner.classList.add("visible");

        setFeedback(
          "This activity is already complete. You may repeat it or continue.",
          "success"
        );
      } else {
        completionBanner.classList.remove("visible");

        setFeedback(
          "Observe the moving molecules, then drag the required molecule " +
          "through the highlighted membrane region.",
          "neutral"
        );
      }
    }

    function updateLessonPanel() {
      const lesson = lessons[currentLessonIndex];
      const activityNumber = currentLessonIndex + 1;

      lessonNumber.textContent = `Activity ${activityNumber}`;
      lessonTitle.textContent = lesson.title;
      lessonDescription.textContent = lesson.description;
      lessonObjective.textContent = lesson.objective;
      sciencePrinciple.textContent = lesson.principle;
      lessonHint.textContent = lesson.hint;

      progressLabel.textContent =
        `Activity ${activityNumber} of ${lessons.length}`;

      progressFill.style.width =
        `${(activityNumber / lessons.length) * 100}%`;

      previousButton.disabled = currentLessonIndex === 0;

      updateNextButton();
      updateStatus();
    }

    function updateNextButton() {
      const isComplete =
        completedLessons.has(currentLessonIndex) || lessonCompleted;

      nextButton.disabled = !isComplete;

      if (!isComplete) {
        nextButton.textContent = "Complete activity first";
      } else if (currentLessonIndex === lessons.length - 1) {
        nextButton.textContent = "Continue to questions";
      } else {
        nextButton.textContent = "Next activity";
      }
    }

    function updateStatus() {
      selectedStatus.textContent = moleculeTypes[selectedType].name;
      atpStatus.textContent = String(atp);
      transferStatus.textContent = String(successfulTransfers);
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
      completedLessons.add(currentLessonIndex);

      completionBanner.textContent = message;
      completionBanner.classList.add("visible");

      updateNextButton();
    }

    /* ============================================================
       MOLECULE MOTION
    ============================================================ */

    function updateMoleculeMotion(deltaTime) {
      if (
        motionPaused ||
        motionMultiplier === 0 ||
        currentPageIndex !== 1
      ) {
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
        randomBetween(-randomStrength, randomStrength) * deltaTime;

      molecule.vy +=
        randomBetween(-randomStrength, randomStrength) * deltaTime;

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
          molecule.vx = (molecule.vx / speed) * targetSpeed;
          molecule.vy = (molecule.vy / speed) * targetSpeed;
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

          if (second.dragging || first.side !== second.side) {
            continue;
          }

          const dx = second.x - first.x;
          const dy = second.y - first.y;

          const minimumDistance = first.radius + second.radius;
          const distanceSquared = dx * dx + dy * dy;

          if (
            distanceSquared === 0 ||
            distanceSquared >= minimumDistance * minimumDistance
          ) {
            continue;
          }

          const distance = Math.sqrt(distanceSquared);
          const normalX = dx / distance;
          const normalY = dy / distance;
          const overlap = minimumDistance - distance;

          first.x -= normalX * overlap * 0.5;
          first.y -= normalY * overlap * 0.5;

          second.x += normalX * overlap * 0.5;
          second.y += normalY * overlap * 0.5;

          const firstNormalVelocity =
            first.vx * normalX + first.vy * normalY;

          const secondNormalVelocity =
            second.vx * normalX + second.vy * normalY;

          if (firstNormalVelocity > secondNormalVelocity) {
            const impulse =
              secondNormalVelocity - firstNormalVelocity;

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
      if (Math.abs(x - proteinPositions.aquaporin) <= 58) {
        return "aquaporin";
      }

      if (Math.abs(x - proteinPositions.carrier) <= 68) {
        return "carrier";
      }

      if (Math.abs(x - proteinPositions.pump) <= 68) {
        return "pump";
      }

      return "bilayer";
    }

    function restoreDraggedMolecule(molecule) {
      molecule.x = dragStartPosition.x;
      molecule.y = dragStartPosition.y;
      molecule.side = dragStartSide;
      molecule.vx = dragStartPosition.vx;
      molecule.vy = dragStartPosition.vy;
    }

    function finishSuccessfulTransport(molecule, targetSide) {
      molecule.side = targetSide;

      const horizontalPadding = molecule.radius + 18;

      molecule.x = clamp(
        molecule.x,
        horizontalPadding,
        WIDTH - horizontalPadding
      );

      if (targetSide === "outside") {
        molecule.y = membraneTop - molecule.radius - 20;
      } else {
        molecule.y = membraneBottom + molecule.radius + 20;
      }

      const newVelocity = getRandomVelocity(molecule.type);

      molecule.vx = newVelocity.vx;

      molecule.vy =
        targetSide === "inside"
          ? Math.abs(newVelocity.vy)
          : -Math.abs(newVelocity.vy);

      successfulTransfers += 1;
      updateStatus();
    }

    function attemptTransport(molecule, targetSide, route) {
      const movingDownGradient = isDownGradient(
        molecule.type,
        dragStartSide,
        targetSide
      );

      /* Oxygen */

      if (molecule.type === "oxygen") {
        if (route !== "bilayer") {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "Oxygen does not require a membrane protein. Move it through " +
            "an open part of the phospholipid bilayer.",
            "warning"
          );

          return;
        }

        if (!movingDownGradient) {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "Simple diffusion moves oxygen from higher concentration " +
            "to lower concentration.",
            "warning"
          );

          return;
        }

        finishSuccessfulTransport(molecule, targetSide);

        setFeedback(
          "Success: oxygen crossed the phospholipid bilayer by " +
          "simple diffusion.",
          "success"
        );

        checkLessonCompletion(molecule, targetSide, route);
        return;
      }

      /* Water */

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
            "Move water from the side with more water molecules " +
            "to the side with fewer water molecules.",
            "warning"
          );

          return;
        }

        finishSuccessfulTransport(molecule, targetSide);

        setFeedback(
          "Success: water moved through aquaporin by osmosis.",
          "success"
        );

        checkLessonCompletion(molecule, targetSide, route);
        return;
      }

      /* Ions */

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
            "Success: the ion used facilitated diffusion. " +
            "No ATP was consumed.",
            "success"
          );

          checkLessonCompletion(molecule, targetSide, route);
          return;
        }

        if (route === "pump") {
          if (movingDownGradient) {
            restoreDraggedMolecule(molecule);

            setFeedback(
              "Use the ATP pump for movement against the " +
              "concentration gradient.",
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

          checkLessonCompletion(molecule, targetSide, route);
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

      /* Glucose */

      if (molecule.type === "glucose") {
        if (route !== "carrier") {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "Glucose is too large and polar to cross directly. " +
            "Use the purple carrier protein.",
            "warning"
          );

          return;
        }

        if (!movingDownGradient) {
          restoreDraggedMolecule(molecule);

          setFeedback(
            "This glucose carrier performs facilitated diffusion, " +
            "so there is net glucose movemenmt down its concentration gradient.",
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

        checkLessonCompletion(molecule, targetSide, route);
      }
    }

    function checkLessonCompletion(molecule, targetSide, route) {
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
              "That transport was scientifically valid, but it did not " +
              "complete the current task. Check the objective and try again.",
              "warning"
            );
          }
        }, 400);
      }
    }

    /* ============================================================
       CANVAS DRAWING HELPERS
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
      const r = Math.min(radius, width / 2, height / 2);

      ctx.beginPath();
      ctx.moveTo(x + r, y);
      ctx.lineTo(x + width - r, y);

      ctx.quadraticCurveTo(
        x + width,
        y,
        x + width,
        y + r
      );

      ctx.lineTo(x + width, y + height - r);

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
      ctx.quadraticCurveTo(x, y, x + r, y);
      ctx.closePath();

      if (fill) {
        ctx.fill();
      }

      if (stroke) {
        ctx.stroke();
      }
    }

    function drawBackground() {
      const outsideGradient = ctx.createLinearGradient(
        0,
        0,
        0,
        membraneTop
      );

      outsideGradient.addColorStop(0, "#dff5ff");
      outsideGradient.addColorStop(1, "#bce7fa");

      ctx.fillStyle = outsideGradient;
      ctx.fillRect(0, 0, WIDTH, membraneTop);

      const insideGradient = ctx.createLinearGradient(
        0,
        membraneBottom,
        0,
        HEIGHT
      );

      insideGradient.addColorStop(0, "#e7f8eb");
      insideGradient.addColorStop(1, "#c8e9d2");

      ctx.fillStyle = insideGradient;

      ctx.fillRect(
        0,
        membraneBottom,
        WIDTH,
        HEIGHT - membraneBottom
      );

      drawRegionLabel(22, 18, "Outside the cell");
      drawRegionLabel(22, HEIGHT - 61, "Inside the cell");
    }

    function drawRegionLabel(x, y, text) {
      ctx.font = "800 18px system-ui, sans-serif";

      const width = ctx.measureText(text).width + 28;

      ctx.fillStyle = "rgba(255, 255, 255, 0.88)";
      ctx.strokeStyle = "rgba(40, 78, 110, 0.22)";
      ctx.lineWidth = 1;

      roundedRectangle(x, y, width, 42, 11, true, true);

      ctx.fillStyle = "#245276";
      ctx.textAlign = "left";
      ctx.textBaseline = "middle";
      ctx.fillText(text, x + 14, y + 21);
    }

    /* ============================================================
       MEMBRANE DRAWING
    ============================================================ */

    function drawMembrane() {
      const membraneGradient = ctx.createLinearGradient(
        0,
        membraneTop,
        0,
        membraneBottom
      );

      membraneGradient.addColorStop(0, "#9dd8ef");
      membraneGradient.addColorStop(0.5, "#80c5e3");
      membraneGradient.addColorStop(1, "#9dd8ef");

      ctx.fillStyle = membraneGradient;

      ctx.fillRect(
        0,
        membraneTop,
        WIDTH,
        membraneBottom - membraneTop
      );

      const spacing = 24;

      for (let x = 8; x < WIDTH; x += spacing) {
        drawPhospholipid(x, membraneTop + 15, 1);
        drawPhospholipid(x, membraneBottom - 15, -1);
      }

      ctx.strokeStyle = "rgba(25, 72, 105, 0.24)";
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

      ctx.moveTo(x - 3, y + 7 * direction);
      ctx.lineTo(x - 6, y + 27 * direction);

      ctx.moveTo(x + 3, y + 7 * direction);
      ctx.lineTo(x + 6, y + 27 * direction);

      ctx.stroke();

      ctx.fillStyle = "#f7d55f";
      ctx.strokeStyle = "#8a6814";
      ctx.lineWidth = 1.4;

      ctx.beginPath();
      ctx.arc(x, y, 7, 0, Math.PI * 2);
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

      drawProteinLabel(x, membraneBottom + 37, "Aquaporin");
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
      ctx.ellipse(0, 0, 14, 24, 0, 0, Math.PI * 2);
      ctx.fill();

      ctx.restore();

      drawProteinLabel(x, membraneBottom + 37, "Carrier");
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
      ctx.arc(8, 0, 13, 0, Math.PI * 2);
      ctx.fill();

      ctx.fillStyle = "#9f1239";
      ctx.font = "800 11px system-ui, sans-serif";
      ctx.textAlign = "center";
      ctx.textBaseline = "middle";
      ctx.fillText("ATP", 8, 0);

      ctx.restore();

      drawProteinLabel(x, membraneBottom + 37, "ATP pump");
    }

    function drawProteinLabel(x, y, text) {
      ctx.font = "800 13px system-ui, sans-serif";

      const width = ctx.measureText(text).width + 18;

      ctx.fillStyle = "rgba(255, 255, 255, 0.91)";
      ctx.strokeStyle = "rgba(31, 72, 110, 0.25)";
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
       TARGET HIGHLIGHT
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
        7 + Math.sin(elapsedAnimationTime * 0.004) * 5;

      ctx.save();

      ctx.fillStyle = "rgba(37, 99, 235, 0.09)";
      ctx.strokeStyle = "rgba(37, 99, 235, 0.85)";
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
        ctx.shadowColor = "rgba(0, 0, 0, 0.38)";
        ctx.shadowBlur = 15;
        ctx.shadowOffsetY = 5;
      }

      if (molecule.type === "glucose") {
        drawGlucoseMolecule(molecule.radius, data);
      } else {
        ctx.fillStyle = data.color;
        ctx.strokeStyle = data.outline;
        ctx.lineWidth = 2.4;

        ctx.beginPath();
        ctx.arc(0, 0, molecule.radius, 0, Math.PI * 2);
        ctx.fill();
        ctx.stroke();

        if (molecule.type === "oxygen") {
          ctx.fillStyle = "rgba(255,255,255,0.78)";
          ctx.beginPath();
          ctx.arc(-4, -4, 3.4, 0, Math.PI * 2);
          ctx.fill();
        }

        if (molecule.type === "water") {
          ctx.fillStyle = "rgba(255,255,255,0.85)";
          ctx.beginPath();
          ctx.arc(-3, -3, 2.5, 0, Math.PI * 2);
          ctx.fill();
        }

        if (molecule.type === "ion") {
          ctx.fillStyle = "#684d00";
          ctx.font = "900 16px system-ui, sans-serif";
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
        const angle = (Math.PI / 3) * i - Math.PI / 6;
        const x = Math.cos(angle) * radius;
        const y = Math.sin(angle) * radius;

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
      ctx.font = "900 10px system-ui, sans-serif";
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

      const outsideCount = countMolecules(type, "outside");
      const insideCount = countMolecules(type, "inside");

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
      ctx.font = "800 14px system-ui, sans-serif";

      const width = ctx.measureText(text).width + 24;

      ctx.fillStyle = "rgba(255,255,255,0.9)";
      ctx.strokeStyle = "rgba(31,72,110,0.22)";
      ctx.lineWidth = 1;

      roundedRectangle(x, y, width, 42, 11, true, true);

      ctx.fillStyle = "#245276";
      ctx.textAlign = "left";
      ctx.textBaseline = "middle";
      ctx.fillText(text, x + 12, y + 21);
    }

    /* ============================================================
       RENDER LOOP
    ============================================================ */

    function render(timestamp) {
      if (!lastAnimationTime) {
        lastAnimationTime = timestamp;
      }

      let deltaTime = (timestamp - lastAnimationTime) / 1000;
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
      const rectangle = canvas.getBoundingClientRect();

      const scaleX = canvas.width / rectangle.width;
      const scaleY = canvas.height / rectangle.height;

      return {
        x: (event.clientX - rectangle.left) * scaleX,
        y: (event.clientY - rectangle.top) * scaleY
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
        const hitRadius = molecule.radius + 9;

        if (dx * dx + dy * dy <= hitRadius * hitRadius) {
          return molecule;
        }
      }

      return null;
    }

    canvas.addEventListener("pointerdown", (event) => {
      if (currentPageIndex !== 1) {
        return;
      }

      event.preventDefault();

      const position = getCanvasCoordinates(event);

      const molecule = findMoleculeAt(
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

      dragOffsetX = position.x - molecule.x;
      dragOffsetY = position.y - molecule.y;

      canvas.classList.add("dragging");
      canvas.setPointerCapture(event.pointerId);

      selectMoleculeType(molecule.type, false);
    });

    canvas.addEventListener("pointermove", (event) => {
      if (!draggedMolecule) {
        return;
      }

      event.preventDefault();

      const position = getCanvasCoordinates(event);

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
    });

    function finishDrag(event) {
      if (!draggedMolecule) {
        return;
      }

      event.preventDefault();

      const molecule = draggedMolecule;

      molecule.dragging = false;
      draggedMolecule = null;

      canvas.classList.remove("dragging");

      const targetSide = sideFromY(molecule.y);

      if (targetSide === "membrane") {
        restoreDraggedMolecule(molecule);

        setFeedback(
          "Release the molecule completely on the opposite side " +
          "of the membrane.",
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

      const route = determineRoute(molecule.x);

      attemptTransport(molecule, targetSide, route);
    }

    canvas.addEventListener("pointerup", finishDrag);
    canvas.addEventListener("pointercancel", finishDrag);

    /* ============================================================
       SIMULATION CONTROL EVENTS
    ============================================================ */

    particleButtons.forEach((button) => {
      button.addEventListener("click", () => {
        selectMoleculeType(button.dataset.particle);
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

    addRequiredButton.addEventListener("click", () => {
      const lesson = lessons[currentLessonIndex];

      addMolecule(
        lesson.requiredType,
        lesson.requiredStart
      );

      selectMoleculeType(lesson.requiredType, false);

      setFeedback(
        `Added one ${moleculeTypes[
          lesson.requiredType
        ].name.toLowerCase()} molecule to the required starting area.`,
        "neutral"
      );
    });

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
      motionMultiplier = Number(speedSlider.value) / 100;
      speedValue.textContent = `${motionMultiplier.toFixed(1)}×`;
    });

    resetButton.addEventListener("click", () => {
      configureLesson(currentLessonIndex);
    });

    hintButton.addEventListener("click", () => {
      const isVisible = hintBox.classList.toggle("visible");

      hintButton.textContent =
        isVisible ? "Hide hint" : "Show hint";
    });

    previousButton.addEventListener("click", () => {
      if (currentLessonIndex > 0) {
        configureLesson(currentLessonIndex - 1);
      }
    });

    nextButton.addEventListener("click", () => {
      if (!completedLessons.has(currentLessonIndex)) {
        setFeedback(
          "Complete the current investigation task before continuing.",
          "warning"
        );

        return;
      }

      if (currentLessonIndex < lessons.length - 1) {
        configureLesson(currentLessonIndex + 1);
      } else if (completedLessons.size === lessons.length) {
        showPage(2);
      }
    });

    simulationBackButton.addEventListener("click", () => {
      showPage(0);
    });

    /* ============================================================
       QUIZ DATA AND SUBMISSION
    ============================================================ */

    const quizForm = document.getElementById("quizForm");
    const quizError = document.getElementById("quizError");
    const quizStudentName = document.getElementById("quizStudentName");
    const quizStudentClass = document.getElementById("quizStudentClass");
    const quizBackButton = document.getElementById("quizBackButton");

    const quizQuestions = [
      {
        name: "q1",
        prompt:
          "Which substance can move directly through the phospholipid bilayer?",
        correct: "b",
        correctText: "Oxygen"
      },

      {
        name: "q2",
        prompt: "What is osmosis?",
        correct: "a",
        correctText:
          "The movement of water across a selectively permeable membrane"
      },

      {
        name: "q3",
        prompt:
          "Which membrane protein provides a channel for water?",
        correct: "c",
        correctText: "Aquaporin"
      },

      {
        name: "q4",
        prompt: "How does facilitated diffusion move substances?",
        correct: "b",
        correctText:
          "Down the concentration gradient using a membrane protein"
      },

      {
        name: "q5",
        prompt:
          "Why do charged ions usually require a membrane protein?",
        correct: "a",
        correctText:
          "The hydrophobic membrane interior restricts charged particles"
      },

      {
        name: "q6",
        prompt: "Which transport process requires ATP?",
        correct: "d",
        correctText: "Active transport"
      },

      {
        name: "q7",
        prompt: "How can active transport move a substance?",
        correct: "b",
        correctText:
          "From low concentration to high concentration"
      },

      {
        name: "q8",
        prompt:
          "Why does glucose generally use a carrier protein?",
        correct: "a",
        correctText:
          "Glucose is a relatively large polar molecule"
      }
    ];

    let quizResults = {
      score: 0,
      total: quizQuestions.length,
      percentage: 0,
      answers: []
    };

    function updateQuizStudentInformation() {
      quizStudentName.textContent = studentData.name;

      quizStudentClass.textContent =
        `${studentData.className} / No. ${studentData.classNumber}`;
    }

    function getSelectedAnswerText(input) {
      if (!input) {
        return "No answer";
      }

      const option = input.closest(".answer-option");
      const textElement = option.querySelector("span");

      return textElement.textContent.trim();
    }

    quizForm.addEventListener("submit", (event) => {
      event.preventDefault();

      const unansweredQuestions = [];

      quizQuestions.forEach((question) => {
        const selected = quizForm.querySelector(
          `input[name="${question.name}"]:checked`
        );

        const questionCard = quizForm.querySelector(
          `[data-question-card="${question.name}"]`
        );

        questionCard.classList.toggle("unanswered", !selected);

        if (!selected) {
          unansweredQuestions.push(question.name);
        }
      });

      if (unansweredQuestions.length > 0) {
        quizError.textContent =
          `Please answer all questions. ` +
          `${unansweredQuestions.length} question` +
          `${unansweredQuestions.length === 1 ? " is" : "s are"} unanswered.`;

        quizError.classList.add("visible");

        const firstUnanswered = quizForm.querySelector(
          `[data-question-card="${unansweredQuestions[0]}"]`
        );

        firstUnanswered.scrollIntoView({
          behavior: "smooth",
          block: "center"
        });

        return;
      }

      quizError.classList.remove("visible");

      let score = 0;
      const answers = [];

      quizQuestions.forEach((question, index) => {
        const selected = quizForm.querySelector(
          `input[name="${question.name}"]:checked`
        );

        const correct = selected.value === question.correct;

        if (correct) {
          score += 1;
        }

        answers.push({
          number: index + 1,
          prompt: question.prompt,
          selectedValue: selected.value,
          selectedText: getSelectedAnswerText(selected),
          correct,
          correctText: question.correctText
        });
      });

      const percentage = Math.round(
        (score / quizQuestions.length) * 100
      );

      quizResults = {
        score,
        total: quizQuestions.length,
        percentage,
        answers
      };

      buildCertificate();
      showPage(3);
    });

    quizForm.addEventListener("change", (event) => {
      if (event.target.matches('input[type="radio"]')) {
        const questionCard = event.target.closest(".question-card");
        questionCard.classList.remove("unanswered");
      }
    });

    quizBackButton.addEventListener("click", () => {
      configureLesson(lessons.length - 1);
      showPage(1);
    });

    /* ============================================================
       CERTIFICATE AND RESULT GENERATION
    ============================================================ */

    const certificateName =
      document.getElementById("certificateName");

    const certificateClass =
      document.getElementById("certificateClass");

    const certificateNumber =
      document.getElementById("certificateNumber");

    const certificateDate =
      document.getElementById("certificateDate");

    const certificateScore =
      document.getElementById("certificateScore");

    const certificateGrade =
      document.getElementById("certificateGrade");

    const correctAnswerResult =
      document.getElementById("correctAnswerResult");

    const percentageResult =
      document.getElementById("percentageResult");

    const performanceResult =
      document.getElementById("performanceResult");

    const answerReview =
      document.getElementById("answerReview");

    const printCertificateButton =
      document.getElementById("printCertificateButton");

    const retakeQuizButton =
      document.getElementById("retakeQuizButton");

    const restartButton =
      document.getElementById("restartButton");

    function getPerformanceLabel(percentage) {
      if (percentage === 100) {
        return "Outstanding";
      }

      if (percentage >= 88) {
        return "Excellent";
      }

      if (percentage >= 75) {
        return "Very good";
      }

      if (percentage >= 63) {
        return "Good";
      }

      if (percentage >= 50) {
        return "Developing";
      }

      return "Further review recommended";
    }

    function formatCertificateDate() {
      return new Intl.DateTimeFormat(undefined, {
        year: "numeric",
        month: "long",
        day: "numeric"
      }).format(new Date());
    }

    function buildCertificate() {
      const performance =
        getPerformanceLabel(quizResults.percentage);

      certificateName.textContent = studentData.name;
      certificateClass.textContent = studentData.className;
      certificateNumber.textContent = studentData.classNumber;
      certificateDate.textContent = formatCertificateDate();

      certificateScore.textContent =
        `${quizResults.percentage}%`;

      certificateGrade.textContent = performance;

      correctAnswerResult.textContent =
        `${quizResults.score} / ${quizResults.total}`;

      percentageResult.textContent =
        `${quizResults.percentage}%`;

      performanceResult.textContent = performance;

      answerReview.innerHTML = "";

      quizResults.answers.forEach((answer) => {
        const reviewItem = document.createElement("div");

        reviewItem.className =
          `review-item ${answer.correct ? "correct" : "incorrect"}`;

        const heading = document.createElement("strong");

        heading.textContent =
          `Question ${answer.number}: ` +
          `${answer.correct ? "Correct" : "Incorrect"}`;

        const questionText = document.createElement("div");
        questionText.textContent = answer.prompt;

        const selectedText = document.createElement("div");
        selectedText.textContent =
          `Your answer: ${answer.selectedText}`;

        reviewItem.append(
          heading,
          questionText,
          selectedText
        );

        if (!answer.correct) {
          const correctText = document.createElement("div");
          correctText.textContent =
            `Correct answer: ${answer.correctText}`;

          reviewItem.append(correctText);
        }

        answerReview.append(reviewItem);
      });
    }

    printCertificateButton.addEventListener("click", () => {
      window.print();
    });

    retakeQuizButton.addEventListener("click", () => {
      showPage(2);
    });

    restartButton.addEventListener("click", () => {
      const confirmed = window.confirm(
        "Start again? This will clear the student information, " +
        "simulation progress, and assessment answers."
      );

      if (!confirmed) {
        return;
      }

      studentData.name = "";
      studentData.className = "";
      studentData.classNumber = "";

      studentForm.reset();
      quizForm.reset();

      completedLessons.clear();

      document.querySelectorAll(".question-card").forEach((card) => {
        card.classList.remove("unanswered");
      });

      studentFormError.classList.remove("visible");
      quizError.classList.remove("visible");

      motionPaused = false;
      motionMultiplier = 1;

      speedSlider.value = "100";
      speedValue.textContent = "1.0×";
      pauseButton.textContent = "Pause motion";

      configureLesson(0);
      showPage(0);

      studentNameInput.focus();
    });

    /* ============================================================
       INITIALIZATION
    ============================================================ */

    configureLesson(0);
    showPage(0);
    requestAnimationFrame(render);
  </script>
</body>
</html>
