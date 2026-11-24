<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>SPACE SAVER // TACTICAL BOSS-RUSH</title>
  <!-- High-tech geometric typography with safe monospace fallbacks -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;800&family=Orbitron:wght@500;700;900&display=swap" rel="stylesheet">
  <style>
    /* ==========================================================================
       STRICT PALETTE TOKENS: ONLY #000000, #ff0000, AND #ffffff
       ========================================================================== */
    :root {
      --bg-black: #000000;
      --threat-red: #ff0000;
      --hud-white: #ffffff;
      --font-display: 'Orbitron', 'Courier New', monospace;
      --font-mono: 'JetBrains Mono', 'Courier New', monospace;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      user-select: none;
      -webkit-user-select: none;
      touch-action: none;
    }

    html, body {
      width: 100%;
      height: 100%;
      background-color: var(--bg-black);
      color: var(--hud-white);
      font-family: var(--font-mono);
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    /* Main Outer Frame */
    #app-shell {
      position: relative;
      width: 100vw;
      height: 100vh;
      background: var(--bg-black);
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }

    /* High-Tech Canvas Viewport */
    #gameCanvas {
      display: block;
      background: var(--bg-black);
      border: 1px solid rgba(255, 255, 255, 0.25);
      box-shadow: 0 0 30px rgba(255, 0, 0, 0.14);
    }

    /* Overlay Screens (Start, Victory, Game Over) */
    .tactical-overlay {
      position: absolute;
      inset: 0;
      background: rgba(0, 0, 0, 0.88);
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 1.25rem;
      z-index: 20;
      transition: opacity 0.25s ease;
    }

    .tactical-overlay.hidden {
      opacity: 0;
      pointer-events: none;
    }

    /* Open-Viewport Galaxy Startup HUD Layer */
    .tactical-overlay.galaxy-hud-layer {
      background: transparent;
      pointer-events: none;
      padding: 0;
      display: block;
    }

    .galaxy-top-left {
      position: absolute;
      top: 10px;
      left: 12px;
      pointer-events: auto;
      max-width: 340px;
      background: rgba(0, 0, 0, 0.82);
      border: 1px solid rgba(255, 255, 255, 0.28);
      border-left: 2px solid var(--threat-red);
      padding: 0.5rem 0.7rem;
    }

    .galaxy-top-left .game-title {
      font-size: clamp(1.1rem, 2.2vw, 1.6rem);
      margin-bottom: 0.15rem;
    }

    .galaxy-top-left .subtitle {
      font-size: 0.62rem;
      margin-bottom: 0.45rem;
    }

    .galaxy-bottom-dock {
      position: absolute;
      left: 50%;
      bottom: 10px;
      transform: translateX(-50%);
      width: calc(100% - 24px);
      max-width: 820px;
      pointer-events: auto;
      background: rgba(0, 0, 0, 0.88);
      border: 1px solid var(--hud-white);
      padding: 0.5rem 0.85rem;
      box-shadow: 0 0 20px rgba(255, 0, 0, 0.18);
      clip-path: polygon(
        10px 0%, calc(100% - 10px) 0%,
        100% 10px, 100% calc(100% - 10px),
        calc(100% - 10px) 100%, 10px 100%,
        0% calc(100% - 10px), 0% 10px
      );
    }

    /* Chamfered High-Tech Modal Panel (Victory & Game Over) */
    .hud-modal {
      position: relative;
      width: 100%;
      max-width: 780px;
      max-height: 95vh;
      overflow-y: auto;
      background: var(--bg-black);
      border: 2px solid var(--hud-white);
      padding: 1.65rem 2rem;
      box-shadow: 0 0 0 1px var(--bg-black), 0 0 32px rgba(255, 0, 0, 0.28);
      clip-path: polygon(
        18px 0%, calc(100% - 18px) 0%,
        100% 18px, 100% calc(100% - 18px),
        calc(100% - 18px) 100%, 18px 100%,
        0% calc(100% - 18px), 0% 14px
      );
    }

    .hud-modal.danger-border {
      border-color: var(--threat-red);
    }

    .hud-tag {
      display: inline-block;
      font-size: 0.72rem;
      font-weight: 800;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--bg-black);
      background: var(--threat-red);
      padding: 0.25rem 0.65rem;
      margin-bottom: 0.85rem;
    }

    .hud-tag.white-tag {
      background: var(--hud-white);
      color: var(--bg-black);
    }

    .game-title {
      font-family: var(--font-display);
      font-size: clamp(2rem, 5vw, 3.4rem);
      font-weight: 900;
      letter-spacing: 0.14em;
      line-height: 1.05;
      color: var(--hud-white);
      text-transform: uppercase;
      margin-bottom: 0.4rem;
    }

    .game-title span {
      color: var(--threat-red);
    }

    .subtitle {
      font-size: 0.85rem;
      letter-spacing: 0.12em;
      color: rgba(255, 255, 255, 0.78);
      margin-bottom: 1.25rem;
      text-transform: uppercase;
    }

    .key-badge {
      display: inline-block;
      border: 1px solid var(--hud-white);
      padding: 0.08rem 0.4rem;
      font-size: 0.76rem;
      font-weight: 800;
      margin: 0 0.1rem;
      background: var(--bg-black);
      color: var(--hud-white);
    }

    .key-badge.red {
      border-color: var(--threat-red);
      color: var(--threat-red);
    }

    /* ==========================================================================
       WORLD & TARGET SELECTOR UI (STRICT #000000, #ff0000, #ffffff)
       ========================================================================== */
    .world-selector-bar {
      display: grid;
      grid-template-columns: 1fr;
      gap: 0.35rem;
      margin-bottom: 0.25rem;
    }

    .world-tab {
      background: var(--bg-black);
      border: 1px solid rgba(255, 255, 255, 0.35);
      padding: 0.3rem 0.5rem;
      color: var(--hud-white);
      text-align: left;
      cursor: pointer;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 0.4rem;
      transition: border-color 0.14s ease, box-shadow 0.14s ease;
    }

    .world-tab.active {
      border: 1.5px solid var(--threat-red);
      box-shadow: inset 0 0 10px rgba(255, 0, 0, 0.2), 0 0 8px rgba(255, 0, 0, 0.25);
    }

    .world-tab-code {
      display: block;
      font-size: 0.56rem;
      font-weight: 800;
      letter-spacing: 0.14em;
      color: var(--threat-red);
      margin-bottom: 0.08rem;
    }

    .world-tab-name {
      display: block;
      font-family: var(--font-display);
      font-size: 0.68rem;
      font-weight: 900;
      letter-spacing: 0.08em;
      color: var(--hud-white);
    }

    .world-tab-badge {
      font-size: 0.56rem;
      font-weight: 800;
      letter-spacing: 0.1em;
      padding: 0.12rem 0.35rem;
      border: 1px solid var(--hud-white);
      color: var(--hud-white);
      white-space: nowrap;
    }

    .world-tab.active .world-tab-badge {
      background: var(--threat-red);
      border-color: var(--threat-red);
      color: var(--hud-white);
    }

    .world-status-note {
      font-size: 0.58rem;
      letter-spacing: 0.08em;
      color: var(--threat-red);
      margin-bottom: 0;
      min-height: 0.75rem;
      text-transform: uppercase;
    }

    /* Selected Sector Telemetry Readout Box */
    .sector-readout {
      border: 1px solid rgba(255, 255, 255, 0.2);
      background: rgba(255, 255, 255, 0.02);
      padding: 0.35rem 0.6rem;
      margin-bottom: 0.4rem;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      gap: 0.5rem;
    }

    .readout-main {
      flex: 1 1 280px;
    }

    .readout-title {
      font-family: var(--font-display);
      font-size: 0.74rem;
      font-weight: 800;
      letter-spacing: 0.08em;
      color: var(--hud-white);
      margin-bottom: 0.1rem;
    }

    .readout-title span {
      color: var(--threat-red);
    }

    .readout-desc {
      font-size: 0.64rem;
      color: rgba(255, 255, 255, 0.78);
      line-height: 1.25;
    }

    .readout-metrics {
      display: flex;
      gap: 0.5rem;
      font-size: 0.62rem;
      letter-spacing: 0.08em;
    }

    .metric-pill {
      border: 1px solid rgba(255, 0, 0, 0.6);
      padding: 0.2rem 0.4rem;
      text-align: center;
    }

    .metric-pill strong {
      display: block;
      font-family: var(--font-display);
      font-size: 0.72rem;
      color: var(--hud-white);
    }

    .controls-strip {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      align-items: center;
      gap: 0.3rem 0.8rem;
      font-size: 0.62rem;
      color: rgba(255, 255, 255, 0.8);
      border-bottom: 1px solid rgba(255, 255, 255, 0.14);
      padding-bottom: 0.35rem;
      margin-bottom: 0.45rem;
    }

    @media (max-width: 680px) {
      .galaxy-top-left {
        top: 42px;
        left: 8px;
        right: 8px;
        max-width: none;
        padding: 0.4rem 0.5rem;
      }
      .galaxy-bottom-dock {
        bottom: 8px;
        width: calc(100% - 16px);
        padding: 0.4rem 0.5rem;
      }
      .sector-readout .readout-desc {
        display: none;
      }
    }

    /* Action Buttons */
    .btn-row {
      display: flex;
      flex-wrap: wrap;
      gap: 0.85rem;
      align-items: center;
    }

    .tactical-btn {
      font-family: var(--font-display);
      font-size: 0.95rem;
      font-weight: 800;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      padding: 0.95rem 2rem;
      border: 2px solid var(--threat-red);
      background: var(--threat-red);
      color: var(--hud-white);
      cursor: pointer;
      transition: transform 0.12s ease, background 0.12s ease, color 0.12s ease;
      clip-path: polygon(10px 0%, 100% 0%, 100% calc(100% - 10px), calc(100% - 10px) 100%, 0% 100%, 0% 10px);
    }

    .tactical-btn:hover, .tactical-btn:focus {
      background: var(--hud-white);
      border-color: var(--hud-white);
      color: var(--bg-black);
      transform: translateY(-1px);
      outline: none;
    }

    .tactical-btn:active {
      transform: translateY(1px);
    }

    .tactical-btn.secondary {
      background: var(--bg-black);
      border-color: var(--hud-white);
      color: var(--hud-white);
      padding: 0.95rem 1.35rem;
      font-size: 0.8rem;
    }

    .tactical-btn.secondary:hover {
      background: var(--hud-white);
      color: var(--bg-black);
    }

    .tactical-btn.hard-active {
      background: var(--bg-black);
      border-color: var(--threat-red);
      color: var(--threat-red);
      box-shadow: 0 0 14px rgba(255, 0, 0, 0.55);
    }

    .tactical-btn.hard-active:hover {
      background: var(--threat-red);
      border-color: var(--threat-red);
      color: var(--hud-white);
    }

    .tactical-btn.practice-active {
      background: var(--hud-white);
      border-color: var(--hud-white);
      color: var(--bg-black);
      box-shadow: 0 0 14px rgba(255, 255, 255, 0.55);
    }

    .tactical-btn.practice-active:hover {
      background: var(--bg-black);
      border-color: var(--hud-white);
      color: var(--hud-white);
    }

    /* Telemetry Stats on End Screens */
    .stats-row {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 0.75rem;
      margin: 1.35rem 0 1.65rem 0;
    }

    .stat-box {
      border: 1px solid rgba(255, 255, 255, 0.35);
      padding: 0.75rem;
      text-align: center;
    }

    .stat-box .stat-num {
      font-family: var(--font-display);
      font-size: 1.35rem;
      font-weight: 900;
      color: var(--hud-white);
    }

    .stat-box .stat-num.red {
      color: var(--threat-red);
    }

    .stat-box .stat-lbl {
      font-size: 0.66rem;
      letter-spacing: 0.14em;
      color: rgba(255, 255, 255, 0.65);
      margin-top: 0.25rem;
      text-transform: uppercase;
    }

    /* Top-right Quick Audio & Sector Buttons during active play */
    .top-hud-actions {
      position: absolute;
      top: 14px;
      right: 16px;
      z-index: 15;
      display: flex;
      gap: 8px;
    }

    .top-hud-btn {
      font-family: var(--font-mono);
      font-size: 0.72rem;
      font-weight: 800;
      letter-spacing: 0.12em;
      padding: 0.42rem 0.75rem;
      background: var(--bg-black);
      color: var(--hud-white);
      border: 1px solid var(--hud-white);
      cursor: pointer;
    }

    .top-hud-btn:hover {
      border-color: var(--threat-red);
      color: var(--threat-red);
    }

    /* Touch Controls for Mobile / Tablet */
    #touch-controls {
      display: none;
      position: absolute;
      inset: 0;
      pointer-events: none;
      z-index: 12;
    }

    @media (pointer: coarse), (max-width: 768px) {
      #touch-controls.active {
        display: block;
      }
      .hud-modal {
        padding: 1.35rem 1.25rem;
      }
    }

    .touch-zone {
      position: absolute;
      bottom: 24px;
      width: 124px;
      height: 124px;
      border: 1px solid rgba(255, 255, 255, 0.35);
      border-radius: 50%;
      background: rgba(0, 0, 0, 0.45);
      pointer-events: auto;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.65rem;
      letter-spacing: 0.12em;
      color: rgba(255, 255, 255, 0.6);
    }

    #touch-move-zone { left: 22px; }
    #touch-aim-zone { right: 22px; border-color: rgba(255, 0, 0, 0.55); }

    #touch-dash-btn {
      position: absolute;
      bottom: 160px;
      right: 36px;
      pointer-events: auto;
      padding: 0.65rem 1rem;
      background: var(--bg-black);
      border: 1px solid var(--hud-white);
      color: var(--hud-white);
      font-family: var(--font-display);
      font-size: 0.72rem;
      font-weight: 800;
      letter-spacing: 0.14em;
    }
  </style>
</head>
<body>
  <div id="app-shell">
    <!-- Main HTML5 Canvas -->
    <canvas id="gameCanvas"></canvas>

    <!-- Quick Top-Right HUD Controls -->
    <div class="top-hud-actions">
      <button id="sector-select-hud-btn" class="top-hud-btn" type="button" aria-label="Open Space Sector Selector">SECTORS</button>
      <button id="audio-toggle-btn" class="top-hud-btn" type="button" aria-label="Toggle Sound">SFX: ON</button>
    </div>

    <!-- Touch Twin-Stick Overlay (Auto-activates on touch devices) -->
    <div id="touch-controls">
      <div id="touch-move-zone" class="touch-zone">MOVE</div>
      <div id="touch-aim-zone" class="touch-zone">AIM / FIRE</div>
      <button id="touch-dash-btn" type="button">DASH</button>
    </div>

    <!-- ======================================================================
         START / ANIMATED TOP-DOWN GALAXY OVERLAY
         ====================================================================== -->
    <div id="start-overlay" class="tactical-overlay galaxy-hud-layer">
      <!-- Top-Left Compact Title & Target Selector -->
      <div class="galaxy-top-left">
        <div class="hud-tag" style="margin-bottom: 0.45rem;">GALACTIC STAR-MAP // DEEP SPACE SCAN</div>
        <h1 class="game-title">SPACE <span>SAVER</span></h1>
        <p class="subtitle">TOP-DOWN GALAXY VIEW // SELECT TARGET &amp; CLICK DEPLOY</p>

        <div class="world-selector-bar" id="world-selector-bar">
          <button type="button" class="world-tab active" data-target="guardians">
            <div>
              <span class="world-tab-code">ORBITING STAR SYSTEM // GAUNTLET</span>
              <span class="world-tab-name">THE GUARDIANS</span>
            </div>
            <span class="world-tab-badge">5 BOSSES</span>
          </button>
          <button type="button" class="world-tab" data-target="void">
            <div>
              <span class="world-tab-code">GALACTIC CORE // SINGULARITY</span>
              <span class="world-tab-name">THE TESSERACTS VOID</span>
            </div>
            <span class="world-tab-badge">1 BOSS</span>
          </button>
        </div>
        <div class="world-status-note" id="world-status-note">CLICK THE STAR OR BLACK HOLE TO LOCK TARGET, THEN CLICK DEPLOY</div>
      </div>

      <!-- Bottom Floating Tactical Telemetry & Launch Dock -->
      <div class="galaxy-bottom-dock">
        <div class="sector-readout">
          <div class="readout-main">
            <div class="readout-title" id="selected-sector-title">THE GUARDIANS // ENTRY: SEC 01 — <span>SENTINEL</span></div>
            <div class="readout-desc" id="selected-sector-desc">
              5-Boss Sequential Gauntlet: 01 SENTINEL &rarr; 02 VIPER-X &rarr; 03 HELIX-CORE &rarr; 04 OBELISK-IV &rarr; 05 OMEGA-ZERO.
            </div>
          </div>
          <div class="readout-metrics">
            <div class="metric-pill">
              COORDS
              <strong id="selected-sector-coords">X-104.8</strong>
            </div>
            <div class="metric-pill">
              ENTRY HULL
              <strong id="selected-sector-hp">650 HP</strong>
            </div>
          </div>
        </div>

        <div class="controls-strip">
          <div><span class="key-badge">W</span><span class="key-badge">A</span><span class="key-badge">S</span><span class="key-badge">D</span> Thrusters</div>
          <div><span class="key-badge red">&uarr;</span><span class="key-badge red">&darr;</span><span class="key-badge red">&larr;</span><span class="key-badge red">&rarr;</span> Lead-Assisted Fire</div>
          <div><span class="key-badge">SHIFT</span> Phase Dash</div>
          <div><span class="key-badge red">CLICK NODE</span> Lock Target</div>
        </div>

        <div class="btn-row">
          <button id="start-btn" class="tactical-btn" type="button" style="padding: 0.55rem 1.25rem; font-size: 0.78rem;">DEPLOY // THE GUARDIANS (01)</button>
          <button id="hard-mode-btn" class="tactical-btn secondary" type="button" style="padding: 0.55rem 0.9rem; font-size: 0.68rem;">HARD MODE: OFF</button>
          <button id="practice-mode-btn" class="tactical-btn secondary" type="button" style="padding: 0.55rem 0.9rem; font-size: 0.68rem;">PRACTICE MODE: OFF</button>
          <button id="modal-audio-btn" class="tactical-btn secondary" type="button" style="padding: 0.55rem 0.9rem; font-size: 0.68rem;">AUDIO: ENABLED</button>
        </div>
      </div>
    </div>

    <!-- ======================================================================
         GAME OVER OVERLAY
         ====================================================================== -->
    <div id="gameover-overlay" class="tactical-overlay hidden">
      <div class="hud-modal danger-border">
        <div class="hud-tag">HULL INTEGRITY CRITICAL // 0%</div>
        <h2 class="game-title">SIGNAL <span>LOST</span></h2>
        <p class="subtitle" id="gameover-reason">INTERCEPTOR DESTROYED IN THE GUARDIANS</p>

        <div class="stats-row">
          <div class="stat-box">
            <div class="stat-num red" id="go-level">SEC 01</div>
            <div class="stat-lbl" id="go-world-lbl">THE GUARDIANS</div>
          </div>
          <div class="stat-box">
            <div class="stat-num" id="go-time">00:00</div>
            <div class="stat-lbl">SURVIVAL TIME</div>
          </div>
          <div class="stat-box">
            <div class="stat-num" id="go-score">00000</div>
            <div class="stat-lbl">COMBAT SCORE</div>
          </div>
        </div>

        <div class="btn-row">
          <button id="retry-btn" class="tactical-btn" type="button">REBOOT &amp; RETRY</button>
          <button id="go-sector-btn" class="tactical-btn secondary" type="button">SECTOR SELECT</button>
          <button id="go-hard-mode-btn" class="tactical-btn secondary" type="button">HARD MODE: OFF</button>
          <button id="go-practice-mode-btn" class="tactical-btn secondary" type="button">PRACTICE MODE: OFF</button>
        </div>
      </div>
    </div>

    <!-- ======================================================================
         VICTORY OVERLAY
         ====================================================================== -->
    <div id="victory-overlay" class="tactical-overlay hidden">
      <div class="hud-modal">
        <div class="hud-tag white-tag" id="win-tag">THE GUARDIANS NEUTRALIZED // WORLD 01 CLEARED</div>
        <h2 class="game-title" id="win-title">WORLD <span>SECURED</span></h2>
        <p class="subtitle" id="win-subtitle">ALL GUARDIAN COMMAND CORES TERMINATED</p>

        <div class="stats-row">
          <div class="stat-box">
            <div class="stat-num" id="win-cleared">GUARDIANS 01-05</div>
            <div class="stat-lbl">TARGET CLEARED</div>
          </div>
          <div class="stat-box">
            <div class="stat-num" id="win-time">00:00</div>
            <div class="stat-lbl">CLEAR TIME</div>
          </div>
          <div class="stat-box">
            <div class="stat-num red" id="win-score">00000</div>
            <div class="stat-lbl">FINAL SCORE</div>
          </div>
        </div>

        <div class="btn-row">
          <button id="play-again-btn" class="tactical-btn" type="button">NEW MISSION</button>
          <button id="win-sector-btn" class="tactical-btn secondary" type="button">SECTOR SELECT</button>
          <button id="win-hard-mode-btn" class="tactical-btn secondary" type="button">HARD MODE: OFF</button>
          <button id="win-practice-mode-btn" class="tactical-btn secondary" type="button">PRACTICE MODE: OFF</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    /* ============================================================================
       1. TWEAKABLE GAME CONSTANTS (EDIT THESE NUMBERS TO CUSTOMIZE GAMEPLAY)
       ============================================================================ */
    const GAME_CONFIG = {
      // Logical Arena Dimensions (auto-scaled to fit any screen cleanly)
      ARENA_WIDTH: 1200,
      ARENA_HEIGHT: 760,

      // Player Movement, Thruster Physics & Zero-G Damping ("Jump/Gravity" feel)
      PLAYER_MOVE_SPEED: 510,          // Max thruster velocity (px/sec)
      PLAYER_ACCELERATION: 4400,       // Thruster acceleration responsiveness
      PLAYER_ZERO_G_GRAVITY: 0.90,     // Inertial drag/damping per frame (0.80 = snappy, 0.96 = floaty zero-G)
      PLAYER_SHIP_TURN_SPEED: 16,      // Ship hull rotation speed toward movement vector
      PLAYER_DASH_SPEED: 980,          // Burst speed during Shift Phase-Dash
      PLAYER_DASH_DURATION: 0.16,      // Seconds of dash burst & invulnerability
      PLAYER_DASH_COOLDOWN: 1.25,      // Seconds before next dash is ready

      // Player Weapons & Predictive Ballistic Aim Assist
      PLAYER_FIRE_RATE: 0.11,          // Seconds between plasma shots (lower = faster)
      PLAYER_BULLET_SPEED: 880,        // Velocity of player plasma bolts (px/sec)
      PLAYER_BULLET_DAMAGE: 25,        // Damage per bolt
      PLAYER_AIM_ASSIST_CONE: 0.58,    // Magnetic lock-on cone in radians (~33 deg)
      PLAYER_AIM_ASSIST_RANGE: 950,    // Max target lock-on range (px)
      PLAYER_MAX_HP: 100,              // Starting hull integrity
      PLAYER_INVULN_TIME: 0.95,        // Invulnerability seconds after taking a hit
      SECTOR_CLEAR_HEAL: 25,           // HP restored on sector clear (disabled in Hard Mode)

      // Combat Health Pack Drops (15-30s cooldown, heals 25%-50% HP)
      HEALTH_PACK_COOLDOWN_MIN: 15,    // Minimum cooldown (seconds) between health pack spawns
      HEALTH_PACK_COOLDOWN_MAX: 30,    // Maximum cooldown (seconds) between health pack spawns
      HEALTH_PACK_HEAL_MIN: 0.25,      // Minimum heal percentage (25% of max HP)
      HEALTH_PACK_HEAL_MAX: 0.50,      // Maximum heal percentage (50% of max HP)
      HEALTH_PACK_RADIUS: 18,          // Pickup collision radius (px)

      // Combat Upgrade Pack Drops (30s or 60s cooldown: +10-25% Fire Rate, +10-15% DMG, or +25% Max HP)
      UPGRADE_PACK_COOLDOWNS: [30, 60], // Cooldown options (seconds) between upgrade pack spawns
      UPGRADE_FIRE_RATE_MIN: 0.10,      // Minimum fire rate boost (+10%)
      UPGRADE_FIRE_RATE_MAX: 0.25,      // Maximum fire rate boost (+25%)
      upgrade_DMG_MIN: 0.10,            // Minimum weapon damage boost (+10%)
      UPGRADE_DMG_MAX: 0.15,            // Maximum weapon damage boost (+15%)
      UPGRADE_MAX_HP_BONUS: 0.25,       // Max HP boost (+25% of current max HP)
      UPGRADE_PACK_RADIUS: 20,          // Pickup collision radius (px)

      // Boss Health, Speed & Attack Spawn Rates
      BOSS_BASE_HP: 1000,              // Sector 1 Sentinel HP
      BOSS_HP_PER_LEVEL: 150,          // Additional HP per sector (overridden explicitly by Guardian gauntlet array)
      BOSS_BULLET_SPEED: 340,          // Base enemy projectile speed (px/sec)
      BOSS_ATTACK_INTERVAL: 1.05,      // Base seconds between boss attack patterns
      VIPER_DASH_SPEED: 620,           // Level 2 Viper-X burst dash velocity
      VIPER_TURN_SPEED: 14,            // Level 2 Viper-X directional nose rotation speed
      FINAL_BOSS_SPIN_SPEED: 0.42,     // Slow rotation speed (rad/sec) of Level 5 RGB Prism Triangle
      FINAL_BOSS_RGB_SPEED: 165,       // Hue cycle speed (deg/sec) of Level 5 RGB Prism
      VOID_BOSS_HP: 50000,             // Standalone HP for The Tesseracts Void singularity boss
      VOID_PULL_FORCE: 135,            // Gravitational pull strength (px/sec^2) of The Tesseracts Void
      VOID_ATTACK_DELAY_MIN: 2.0,      // Minimum seconds Tesseract waits before attacking again
      VOID_ATTACK_DELAY_MAX: 5.0,      // Maximum seconds Tesseract waits before attacking again
      VOID_PHASE_THRESHOLDS: [1.0, 0.75, 0.50, 0.25], // Phase 1 (100%), Phase 2 (75%), Phase 3 (50%), Phase 4 (25%)
      VOID_PHASE_DMG_MULTS: [1.0, 1.25, 1.55, 1.90],  // Damage multiplier per Tesseract phase
      VOID_ULT_THRESHOLD: 0.10,        // Ult Form triggers at 10% HP (5,000 HP)
      VOID_ULT_HEAL: 10000,            // HP recovered upon entering Ult Form (+10,000 HP)
      VOID_ULT_DMG_MULT: 2.45          // Damage multiplier in Ult Form
    };

    /* ============================================================================
       2. PROCEDURAL WEB AUDIO SYNTHESIZER (NO EXTERNAL SOUND FILES)
       ============================================================================ */
    class SoundEngine {
      constructor() {
        this.ctx = null;
        this.enabled = true;
      }

      init() {
        if (!this.ctx) {
          const AudioCtx = window.AudioContext || window.webkitAudioContext;
          if (AudioCtx) this.ctx = new AudioCtx();
        }
        if (this.ctx && this.ctx.state === 'suspended') {
          this.ctx.resume();
        }
      }

      toggle() {
        this.enabled = !this.enabled;
        return this.enabled;
      }

      playTone(freqStart, freqEnd, duration, type = 'sawtooth', gainVal = 0.08) {
        if (!this.enabled) return;
        this.init();
        if (!this.ctx) return;

        const now = this.ctx.currentTime;
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = type;
        osc.frequency.setValueAtTime(freqStart, now);
        osc.frequency.exponentialRampToValueAtTime(Math.max(20, freqEnd), now + duration);

        gain.gain.setValueAtTime(gainVal, now);
        gain.gain.exponentialRampToValueAtTime(0.0001, now + duration);

        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start(now);
        osc.stop(now + duration);
      }

      playNoise(duration = 0.22, gainVal = 0.14) {
        if (!this.enabled) return;
        this.init();
        if (!this.ctx) return;

        const now = this.ctx.currentTime;
        const bufferSize = Math.floor(this.ctx.sampleRate * duration);
        const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
        const data = buffer.getChannelData(0);
        for (let i = 0; i < bufferSize; i++) {
          data[i] = (Math.random() * 2 - 1) * Math.exp(-i / (bufferSize * 0.35));
        }

        const noise = this.ctx.createBufferSource();
        noise.buffer = buffer;

        const filter = this.ctx.createBiquadFilter();
        filter.type = 'bandpass';
        filter.frequency.setValueAtTime(420, now);

        const gain = this.ctx.createGain();
        gain.gain.setValueAtTime(gainVal, now);
        gain.gain.exponentialRampToValueAtTime(0.0001, now + duration);

        noise.connect(filter);
        filter.connect(gain);
        gain.connect(this.ctx.destination);
        noise.start(now);
      }

      playShoot() {
        this.playTone(920, 240, 0.065, 'square', 0.045);
      }

      playBossFire() {
        this.playTone(230, 85, 0.11, 'sawtooth', 0.055);
      }

      playHit() {
        this.playTone(170, 52, 0.16, 'sawtooth', 0.14);
        this.playNoise(0.14, 0.12);
      }

      playDash() {
        this.playTone(300, 980, 0.12, 'triangle', 0.08);
      }

      playExplosion(large = false) {
        this.playNoise(large ? 0.55 : 0.26, large ? 0.22 : 0.13);
        this.playTone(large ? 115 : 150, 28, large ? 0.5 : 0.22, 'sawtooth', 0.14);
      }

      playPhaseShift() {
        this.playTone(180, 640, 0.28, 'sawtooth', 0.11);
      }

      playLaser() {
        this.playTone(540, 120, 0.35, 'sawtooth', 0.1);
      }

      playOmegaBeam() {
        this.playNoise(0.55, 0.22);
        this.playTone(140, 36, 0.65, 'sawtooth', 0.18);
        this.playTone(880, 190, 0.42, 'square', 0.09);
      }

      playSelect() {
        this.playTone(620, 920, 0.07, 'square', 0.05);
      }

      playWin() {
        this.playTone(440, 880, 0.22, 'triangle', 0.1);
        setTimeout(() => this.playTone(554, 1108, 0.32, 'triangle', 0.11), 140);
      }

      playLose() {
        this.playTone(280, 70, 0.45, 'sawtooth', 0.13);
      }

      playHeal() {
        if (!this.enabled) return;
        this.playTone(520, 780, 0.12, 'triangle', 0.08);
        setTimeout(() => this.playTone(780, 1040, 0.18, 'triangle', 0.09), 80);
      }

      playUpgrade() {
        if (!this.enabled) return;
        this.playTone(440, 660, 0.11, 'square', 0.07);
        setTimeout(() => this.playTone(660, 880, 0.12, 'triangle', 0.08), 75);
        setTimeout(() => this.playTone(880, 1320, 0.22, 'triangle', 0.09), 150);
      }
    }

    const sfx = new SoundEngine();

    /* ============================================================================
       3. BOSS DEFINITIONS & WORLD TARGET METADATA
       ============================================================================ */
    const BOSS_DEFINITIONS = [
      {
        level: 1,
        code: 'SEC-01',
        name: 'SENTINEL',
        subtitle: 'ORBITAL DEFENSE WARDEN // FULL-MAP ROAMER',
        coords: 'X-104.8',
        radius: 38,
        sides: 6,
        hp: 1000
      },
      {
        level: 2,
        code: 'SEC-02',
        name: 'VIPER-X',
        subtitle: 'INTERCEPTOR DREADNOUGHT // PROXIMITY MINES',
        coords: 'Y-219.4',
        radius: 36,
        sides: 3,
        hp: 2500
      },
      {
        level: 3,
        code: 'SEC-03',
        name: 'HELIX-CORE',
        subtitle: 'AUTONOMOUS SWARM REACTOR // DRONE CARRIER',
        coords: 'Z-388.1',
        radius: 42,
        sides: 8,
        hp: 5000
      },
      {
        level: 4,
        code: 'SEC-04',
        name: 'OBELISK-IV',
        subtitle: 'HIGH-ENERGY RAIL CITADEL // LASER ARRAY',
        coords: 'K-512.0',
        radius: 44,
        sides: 4,
        hp: 7500
      },
      {
        level: 5,
        code: 'SEC-05',
        name: 'OMEGA-ZERO',
        subtitle: 'PRISM SINGULARITY // RGB VERTEX CORE',
        coords: 'W-999.9',
        radius: 52,
        sides: 3,
        hp: 10000
      }
    ];

    const WORLD_TARGETS = {
      guardians: {
        id: 'guardians',
        titleHtml: 'THE GUARDIANS // ENTRY: SEC 01 — <span>SENTINEL</span>',
        desc: '5-Boss Sequential Gauntlet: 01 SENTINEL \u2192 02 VIPER-X \u2192 03 HELIX-CORE \u2192 04 OBELISK-IV \u2192 05 OMEGA-ZERO.',
        coords: 'X-104.8',
        hpText: '1,000 HP'
      },
      void: {
        id: 'void',
        titleHtml: 'THE TESSERACTS VOID // CORE — <span>SINGULARITY</span>',
        desc: 'Standalone Galactic Core Boss: 4D Hypercube Singularity with 50,000 HP, 4 Escalation Phases (75%/50%/25%), and 6 Dimensional Attacks.',
        coords: 'CORE-0.0',
        hpText: `${GAME_CONFIG.VOID_BOSS_HP.toLocaleString()} HP`
      }
    };

    /* ============================================================================
       4. CENTRALIZED GAME STATE & GALAXY STAR-MAP DATA
       ============================================================================ */
    const state = {
      screen: 'START', // 'START' | 'PLAYING' | 'GAMEOVER' | 'VICTORY'
      selectedTarget: 'guardians', // 'guardians' | 'void'
      hardMode: false,
      practiceMode: false,
      level: 1,
      score: 0,
      elapsedTime: 0,
      shakeTime: 0,
      shakeMagnitude: 0,
      flashAlpha: 0,
      bannerTimer: 0,
      bannerTag: '',
      bannerTitle: '',
      bannerSubtitle: '',
      galaxyAngle: 0.72,
      galaxyHovered: null, // 'guardians' | 'void' | null
      galaxyStars: Array.from({ length: 190 }, () => ({
        x: Math.random() * GAME_CONFIG.ARENA_WIDTH,
        y: Math.random() * GAME_CONFIG.ARENA_HEIGHT,
        size: Math.random() < 0.82 ? 1.2 : 2.2,
        alpha: 0.25 + Math.random() * 0.7,
        twinkle: 1.5 + Math.random() * 3.5,
        isRed: Math.random() < 0.16
      })),
      player: {
        x: 600,
        y: 620,
        vx: 0,
        vy: 0,
        angle: -Math.PI / 2,
        radius: 14,
        hp: GAME_CONFIG.PLAYER_MAX_HP,
        maxHp: GAME_CONFIG.PLAYER_MAX_HP,
        fireCooldown: 0,
        dashTimer: 0,
        dashCooldown: 0,
        dashVx: 0,
        dashVy: 0,
        invulnTimer: 0,
        muzzleFlash: 0,
        aimPoint: null,
        afterimages: [],
        rateMult: 1.0,
        dmgMult: 1.0
      },
      boss: null,
      playerBullets: [],
      enemyBullets: [],
      mines: [],
      drones: [],
      lasers: [],
      cracks: [],
      lightnings: [],
      teleports: [],
      healthPack: null,
      healthPackTimer: 18,
      upgradePack: null,
      upgradePackTimer: 30,
      floatingTexts: [],
      particles: [],
      shockwaves: []
    };

    /* ============================================================================
       5. RESPONSIVE CANVAS & DPR SCALING
       ============================================================================ */
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    let canvasScale = 1;

    function resizeCanvas() {
      const containerW = window.innerWidth;
      const containerH = window.innerHeight;
      const targetAspect = GAME_CONFIG.ARENA_WIDTH / GAME_CONFIG.ARENA_HEIGHT;
      let displayW = containerW;
      let displayH = containerW / targetAspect;

      if (displayH > containerH) {
        displayH = containerH;
        displayW = containerH * targetAspect;
      }

      const dpr = Math.min(window.devicePixelRatio || 1, 2);
      canvas.style.width = `${Math.floor(displayW)}px`;
      canvas.style.height = `${Math.floor(displayH)}px`;
      canvas.width = Math.floor(GAME_CONFIG.ARENA_WIDTH * dpr);
      canvas.height = Math.floor(GAME_CONFIG.ARENA_HEIGHT * dpr);
      canvasScale = dpr;
    }

    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    /* ============================================================================
       6. GALAXY STAR-MAP SELECTOR & UI CONTROLS
       ============================================================================ */
    const startOverlay = document.getElementById('start-overlay');
    const gameoverOverlay = document.getElementById('gameover-overlay');
    const victoryOverlay = document.getElementById('victory-overlay');
    const startBtn = document.getElementById('start-btn');
    const worldStatusNote = document.getElementById('world-status-note');

    function getGalaxyGeometry() {
      const cx = GAME_CONFIG.ARENA_WIDTH * 0.54;
      const cy = GAME_CONFIG.ARENA_HEIGHT * 0.45;
      const orbitRx = 265;
      const orbitRy = 165;
      const starX = cx + Math.cos(state.galaxyAngle) * orbitRx;
      const starY = cy + Math.sin(state.galaxyAngle) * orbitRy;
      return { cx, cy, orbitRx, orbitRy, starX, starY };
    }

    function clientToArenaCoords(clientX, clientY) {
      const rect = canvas.getBoundingClientRect();
      if (!rect.width || !rect.height) return { x: 0, y: 0 };
      return {
        x: ((clientX - rect.left) / rect.width) * GAME_CONFIG.ARENA_WIDTH,
        y: ((clientY - rect.top) / rect.height) * GAME_CONFIG.ARENA_HEIGHT
      };
    }

    function selectTarget(targetId) {
      state.selectedTarget = targetId === 'void' ? 'void' : 'guardians';
      const info = WORLD_TARGETS[state.selectedTarget];

      document.querySelectorAll('.world-tab').forEach((tab) => {
        tab.classList.toggle('active', tab.dataset.target === state.selectedTarget);
      });

      document.getElementById('selected-sector-title').innerHTML = info.titleHtml;
      document.getElementById('selected-sector-desc').textContent = info.desc;
      document.getElementById('selected-sector-coords').textContent = info.coords;
      document.getElementById('selected-sector-hp').textContent = info.hpText;

      if (state.selectedTarget === 'void') {
        startBtn.textContent = 'DEPLOY // THE TESSERACTS VOID';
        worldStatusNote.textContent = 'TARGET LOCKED: GALACTIC CORE SINGULARITY (50,000 HP) — CLICK DEPLOY TO LAUNCH';
      } else {
        startBtn.textContent = 'DEPLOY // THE GUARDIANS (01)';
        worldStatusNote.textContent = 'TARGET LOCKED: THE GUARDIANS (5-BOSS GAUNTLET) — CLICK DEPLOY TO LAUNCH';
      }
      sfx.playSelect();
    }

    document.querySelectorAll('.world-tab').forEach((tab) => {
      tab.addEventListener('click', () => {
        selectTarget(tab.dataset.target);
      });
    });

    canvas.addEventListener('mousemove', (e) => {
      if (state.screen !== 'START') {
        state.galaxyHovered = null;
        canvas.style.cursor = 'default';
        return;
      }
      const pt = clientToArenaCoords(e.clientX, e.clientY);
      const g = getGalaxyGeometry();
      const distVoid = Math.hypot(pt.x - g.cx, pt.y - g.cy);
      const distStar = Math.hypot(pt.x - g.starX, pt.y - g.starY);

      if (distVoid <= 62) {
        state.galaxyHovered = 'void';
        canvas.style.cursor = 'pointer';
      } else if (distStar <= 48) {
        state.galaxyHovered = 'guardians';
        canvas.style.cursor = 'pointer';
      } else {
        state.galaxyHovered = null;
        canvas.style.cursor = 'default';
      }
    });

    canvas.addEventListener('click', (e) => {
      if (state.screen !== 'START') return;
      const pt = clientToArenaCoords(e.clientX, e.clientY);
      const g = getGalaxyGeometry();
      const distVoid = Math.hypot(pt.x - g.cx, pt.y - g.cy);
      const distStar = Math.hypot(pt.x - g.starX, pt.y - g.starY);

      if (distVoid <= 62) {
        selectTarget('void');
      } else if (distStar <= 48) {
        selectTarget('guardians');
      }
    });

    function openSectorSelector() {
      state.screen = 'START';
      state.boss = null;
      state.playerBullets = [];
      state.enemyBullets = [];
      state.mines = [];
      state.drones = [];
      state.lasers = [];
      state.cracks = [];
      state.lightnings = [];
      state.teleports = [];
      state.healthPack = null;
      state.upgradePack = null;
      state.floatingTexts = [];
      state.particles = [];
      state.shockwaves = [];
      gameoverOverlay.classList.add('hidden');
      victoryOverlay.classList.add('hidden');
      startOverlay.classList.remove('hidden');
      selectTarget(state.selectedTarget);
    }

    function syncHardModeButtons() {
      const label = state.hardMode ? 'HARD MODE: ON' : 'HARD MODE: OFF';
      ['hard-mode-btn', 'go-hard-mode-btn', 'win-hard-mode-btn'].forEach((id) => {
        const el = document.getElementById(id);
        if (!el) return;
        el.textContent = label;
        el.classList.toggle('hard-active', state.hardMode);
      });
    }

    function syncPracticeModeButtons() {
      const label = state.practiceMode ? 'PRACTICE MODE: ON' : 'PRACTICE MODE: OFF';
      ['practice-mode-btn', 'go-practice-mode-btn', 'win-practice-mode-btn'].forEach((id) => {
        const el = document.getElementById(id);
        if (!el) return;
        el.textContent = label;
        el.classList.toggle('practice-active', state.practiceMode);
      });
    }

    function toggleHardMode() {
      state.hardMode = !state.hardMode;
      if (state.hardMode && state.practiceMode) {
        state.practiceMode = false;
        syncPracticeModeButtons();
      }
      syncHardModeButtons();
      sfx.playSelect();
    }

    function togglePracticeMode() {
      state.practiceMode = !state.practiceMode;
      if (state.practiceMode && state.hardMode) {
        state.hardMode = false;
        syncHardModeButtons();
      }
      syncPracticeModeButtons();
      sfx.playSelect();
    }

    function toggleAudioState() {
      const isOn = sfx.toggle();
      document.getElementById('audio-toggle-btn').textContent = isOn ? 'SFX: ON' : 'SFX: OFF';
      document.getElementById('modal-audio-btn').textContent = isOn ? 'AUDIO: ENABLED' : 'AUDIO: MUTED';
    }

    document.getElementById('start-btn').addEventListener('click', () => startMission(state.selectedTarget));
    document.getElementById('retry-btn').addEventListener('click', () => startMission(state.selectedTarget));
    document.getElementById('play-again-btn').addEventListener('click', () => {
      if (state.practiceMode) {
        state.practiceMode = false;
        syncPracticeModeButtons();
      }
      startMission(state.selectedTarget);
    });
    document.getElementById('sector-select-hud-btn').addEventListener('click', openSectorSelector);
    document.getElementById('go-sector-btn').addEventListener('click', openSectorSelector);
    document.getElementById('win-sector-btn').addEventListener('click', openSectorSelector);
    document.getElementById('hard-mode-btn').addEventListener('click', toggleHardMode);
    document.getElementById('go-hard-mode-btn').addEventListener('click', toggleHardMode);
    document.getElementById('win-hard-mode-btn').addEventListener('click', toggleHardMode);
    document.getElementById('practice-mode-btn').addEventListener('click', togglePracticeMode);
    document.getElementById('go-practice-mode-btn').addEventListener('click', togglePracticeMode);
    document.getElementById('win-practice-mode-btn').addEventListener('click', togglePracticeMode);
    document.getElementById('audio-toggle-btn').addEventListener('click', toggleAudioState);
    document.getElementById('modal-audio-btn').addEventListener('click', toggleAudioState);

    /* ============================================================================
       7. INPUT HANDLING (KEYBOARD WASD + ARROWS + SHIFT, PLUS MOBILE TOUCH)
       ============================================================================ */
    const keys = {
      KeyW: false,
      KeyA: false,
      KeyS: false,
      KeyD: false,
      ArrowUp: false,
      ArrowDown: false,
      ArrowLeft: false,
      ArrowRight: false,
      ShiftLeft: false,
      ShiftRight: false
    };

    const touchInput = {
      moveX: 0,
      moveY: 0,
      aimX: 0,
      aimY: 0,
      firing: false,
      dashRequested: false
    };

    window.addEventListener('keydown', (e) => {
      if (e.code in keys) {
        e.preventDefault();
        keys[e.code] = true;
      }
      if ((e.code === 'ShiftLeft' || e.code === 'ShiftRight') && state.screen === 'PLAYING') {
        tryPlayerDash();
      }
      if (e.code === 'Enter' && state.screen === 'START') {
        startMission(state.selectedTarget);
      }
    });

    window.addEventListener('keyup', (e) => {
      if (e.code in keys) {
        keys[e.code] = false;
      }
    });

    // Touch controls setup for touchscreens
    const touchControls = document.getElementById('touch-controls');
    if ('ontouchstart' in window || navigator.maxTouchPoints > 0) {
      touchControls.classList.add('active');
    }

    function bindStickZone(zoneId, isMove) {
      const zone = document.getElementById(zoneId);
      let activeId = null;

      zone.addEventListener('touchstart', (e) => {
        e.preventDefault();
        const t = e.changedTouches[0];
        activeId = t.identifier;
        updateStick(t);
      }, { passive: false });

      zone.addEventListener('touchmove', (e) => {
        e.preventDefault();
        for (const t of e.changedTouches) {
          if (t.identifier === activeId) updateStick(t);
        }
      }, { passive: false });

      const endTouch = (e) => {
        for (const t of e.changedTouches) {
          if (t.identifier === activeId) {
            activeId = null;
            if (isMove) {
              touchInput.moveX = 0;
              touchInput.moveY = 0;
            } else {
              touchInput.aimX = 0;
              touchInput.aimY = 0;
              touchInput.firing = false;
            }
          }
        }
      };

      zone.addEventListener('touchend', endTouch);
      zone.addEventListener('touchcancel', endTouch);

      function updateStick(touch) {
        const rect = zone.getBoundingClientRect();
        const cx = rect.left + rect.width / 2;
        const cy = rect.top + rect.height / 2;
        const dx = touch.clientX - cx;
        const dy = touch.clientY - cy;
        const dist = Math.hypot(dx, dy);
        const maxR = rect.width * 0.42;
        const nx = dist > 6 ? (dx / Math.max(dist, maxR)) : 0;
        const ny = dist > 6 ? (dy / Math.max(dist, maxR)) : 0;

        if (isMove) {
          touchInput.moveX = Math.max(-1, Math.min(1, nx));
          touchInput.moveY = Math.max(-1, Math.min(1, ny));
        } else {
          touchInput.aimX = Math.max(-1, Math.min(1, nx));
          touchInput.aimY = Math.max(-1, Math.min(1, ny));
          touchInput.firing = dist > 10;
        }
      }
    }

    bindStickZone('touch-move-zone', true);
    bindStickZone('touch-aim-zone', false);

    document.getElementById('touch-dash-btn').addEventListener('touchstart', (e) => {
      e.preventDefault();
      if (state.screen === 'PLAYING') tryPlayerDash();
    }, { passive: false });

    /* ============================================================================
       8. MISSION LIFECYCLE & BOSS SPAWNING
       ============================================================================ */
    function startMission(targetId = state.selectedTarget) {
      sfx.init();
      sfx.playSelect();

      state.selectedTarget = targetId === 'void' ? 'void' : 'guardians';
      state.screen = 'PLAYING';
      state.level = 1;
      state.score = 0;
      state.elapsedTime = 0;
      state.shakeTime = 0;
      state.flashAlpha = 0;
      state.playerBullets = [];
      state.enemyBullets = [];
      state.mines = [];
      state.drones = [];
      state.lasers = [];
      state.cracks = [];
      state.lightnings = [];
      state.teleports = [];
      state.healthPack = null;
      state.healthPackTimer =
        GAME_CONFIG.HEALTH_PACK_COOLDOWN_MIN +
        Math.random() * (GAME_CONFIG.HEALTH_PACK_COOLDOWN_MAX - GAME_CONFIG.HEALTH_PACK_COOLDOWN_MIN);
      state.upgradePack = null;
      state.upgradePackTimer =
        GAME_CONFIG.UPGRADE_PACK_COOLDOWNS[Math.floor(Math.random() * GAME_CONFIG.UPGRADE_PACK_COOLDOWNS.length)];
      state.floatingTexts = [];
      state.particles = [];
      state.shockwaves = [];

      // Reset Player
      const p = state.player;
      p.x = GAME_CONFIG.ARENA_WIDTH / 2;
      p.y = GAME_CONFIG.ARENA_HEIGHT - 125;
      p.vx = 0;
      p.vy = 0;
      p.angle = -Math.PI / 2;
      p.maxHp = GAME_CONFIG.PLAYER_MAX_HP;
      p.hp = GAME_CONFIG.PLAYER_MAX_HP;
      p.fireCooldown = 0;
      p.dashTimer = 0;
      p.dashCooldown = 0;
      p.invulnTimer = 1.0;
      p.aimPoint = null;
      p.afterimages = [];
      p.rateMult = 1.0;
      p.dmgMult = 1.0;

      startOverlay.classList.add('hidden');
      gameoverOverlay.classList.add('hidden');
      victoryOverlay.classList.add('hidden');

      if (state.selectedTarget === 'void') {
        spawnVoidBoss();
      } else {
        spawnBossForLevel(1);
      }
    }

    function spawnVoidBoss() {
      const startX = GAME_CONFIG.ARENA_WIDTH * 0.5;
      const startY = GAME_CONFIG.ARENA_HEIGHT * 0.42;

      state.boss = {
        level: 6,
        isVoid: true,
        code: 'CORE-00',
        name: 'TESSERACT-VOID',
        subtitle: '4D HYPERCUBE SINGULARITY // EVENT HORIZON',
        x: startX,
        y: startY,
        vx: 0,
        vy: 0,
        radius: 58,
        sides: 4,
        hp: GAME_CONFIG.VOID_BOSS_HP,
        maxHp: GAME_CONFIG.VOID_BOSS_HP,
        phase: 1,
        angle: 0,
        rotSpeed: 0.89,
        hitFlash: 0,
        attackTimer: 1.35,
        attackStep: 0,
        moveTimer: 0,

        // Tesseract Void 6-Attack Sequencer & 4-Phase Escalation
        voidAttackIndex: 0,
        voidAttackTimer: 1.35,
        voidAttackLabel: '4D SINGULARITY ACTIVE',
        voidDmgMult: 1.0,
        voidBgShards: Array.from({ length: 18 }, () => ({
          x: Math.random() * GAME_CONFIG.ARENA_WIDTH,
          y: 70 + Math.random() * (GAME_CONFIG.ARENA_HEIGHT - 80),
          size: 8 + Math.random() * 18,
          angle: Math.random() * Math.PI * 2,
          spin: (Math.random() - 0.5) * 2.2,
          orbitSpeed: 18 + Math.random() * 36
        })),
        omegaBeamState: 'NONE', // 'NONE' | 'TELEGRAPH' | 'FIRING'
        omegaBeamTimer: 0,
        omegaBeamAngle: Math.PI / 2,
        arcChargeTimer: 0,
        arcChargeMax: 0,
        arcChargeAngle: 0,
        implosionTimer: 0,
        voidTeleportLeft: 0,
        voidTeleportTimer: 0,
        warpAnimTimer: 0,
        isUlt: false,
        ultTriggered: false,
        ultAnimTimer: 0
      };

      state.bannerTag = 'SINGULARITY BREACH // GALACTIC CORE';
      state.bannerTitle = 'TESSERACT-VOID';
      state.bannerSubtitle = '4D HYPERCUBE SINGULARITY // GRAVITY WELL ACTIVE';
      state.bannerTimer = 2.6;

      spawnShockwave(state.boss.x, state.boss.y, 260, '#ff0000', 0.65);
      spawnShockwave(state.boss.x, state.boss.y, 180, '#ffffff', 0.5);
      spawnParticles(state.boss.x, state.boss.y, 38, '#ff0000', 90, 340, 0.75);
      spawnParticles(state.boss.x, state.boss.y, 32, '#ffffff', 80, 310, 0.7);
      sfx.playPhaseShift();
    }

    function spawnBossForLevel(lvl) {
      const def = BOSS_DEFINITIONS[lvl - 1];
      const maxHp = def.hp;

      const isRoamer = lvl === 1 || lvl === 2;
      const startX = isRoamer
        ? GAME_CONFIG.ARENA_WIDTH * (lvl === 1 ? 0.25 : 0.75)
        : GAME_CONFIG.ARENA_WIDTH / 2;
      const startY = isRoamer ? 190 : 175;
      const initialWp = isRoamer
        ? {
            x: GAME_CONFIG.ARENA_WIDTH * (lvl === 1 ? 0.75 : 0.25),
            y: GAME_CONFIG.ARENA_HEIGHT * 0.55
          }
        : { x: startX, y: startY };

      state.boss = {
        level: lvl,
        isVoid: false,
        code: def.code,
        name: def.name,
        subtitle: def.subtitle,
        x: startX,
        y: startY,
        vx: 0,
        vy: 0,
        radius: def.radius,
        sides: def.sides,
        hp: maxHp,
        maxHp: maxHp,
        phase: 1,
        angle: lvl === 2 ? Math.PI / 2 : 0,
        rotSpeed: lvl === 5 ? GAME_CONFIG.FINAL_BOSS_SPIN_SPEED : (0.55 + lvl * 0.14),
        rgbHue: 0,
        hitFlash: 0,
        attackTimer: 1.1,
        attackStep: 0,
        moveTimer: 0,
        dashTimer: 0,
        dashCooldown: 1.6,
        roamTarget: initialWp
      };

      // Non-blocking HUD alert banner so gameplay never freezes on boss spawn
      state.bannerTag = `SECTOR 0${lvl} // THREAT DETECTED`;
      state.bannerTitle = `${def.code} — ${def.name}`;
      state.bannerSubtitle = def.subtitle;
      state.bannerTimer = 2.4;

      spawnShockwave(state.boss.x, state.boss.y, 210, '#ff0000', 0.55);
      spawnParticles(state.boss.x, state.boss.y, 34, '#ffffff', 80, 320, 0.7);
      sfx.playPhaseShift();
    }

    function triggerGameOver() {
      state.screen = 'GAMEOVER';
      sfx.playLose();
      const isVoid = state.selectedTarget === 'void';
      document.getElementById('gameover-reason').textContent = isVoid
        ? 'INTERCEPTOR CRUSHED BY THE TESSERACTS VOID'
        : `INTERCEPTOR DESTROYED IN SECTOR 0${state.level} (${state.boss ? state.boss.name : 'HOSTILE'})`;
      document.getElementById('go-world-lbl').textContent = isVoid ? 'TESSERACT-VOID' : 'THE GUARDIANS';
      document.getElementById('go-level').textContent = isVoid ? 'CORE-00' : `SEC 0${state.level}`;
      document.getElementById('go-time').textContent = formatTime(state.elapsedTime);
      document.getElementById('go-score').textContent = String(state.score).padStart(5, '0');
      gameoverOverlay.classList.remove('hidden');
    }

    function triggerVictory() {
      state.screen = 'VICTORY';
      sfx.playWin();
      const isVoid = state.selectedTarget === 'void';
      const playAgainBtn = document.getElementById('play-again-btn');

      if (state.practiceMode) {
        document.getElementById('win-tag').textContent = 'PRACTICE SIMULATION COMPLETE // ZERO HULL LOSS';
        document.getElementById('win-title').innerHTML = 'NOW TRY IN <span>NORMAL MODE</span>';
        document.getElementById('win-subtitle').textContent = isVoid
          ? 'THE TESSERACTS VOID CLEARED IN PRACTICE — NOW TRY IN NORMAL MODE'
          : 'ALL 5 GUARDIANS CLEARED IN PRACTICE — NOW TRY IN NORMAL MODE';
        if (playAgainBtn) playAgainBtn.textContent = 'TRY NORMAL MODE';
      } else {
        document.getElementById('win-tag').textContent = isVoid
          ? 'THE TESSERACTS VOID COLLAPSED // GALACTIC CORE STABILIZED'
          : 'THE GUARDIANS NEUTRALIZED // ALL 5 SECTORS CLEARED';
        document.getElementById('win-title').innerHTML = isVoid
          ? 'VOID <span>COLLAPSED</span>'
          : 'WORLD <span>SECURED</span>';
        document.getElementById('win-subtitle').textContent = isVoid
          ? '4D HYPERCUBE SINGULARITY TERMINATED'
          : 'ALL GUARDIAN COMMAND CORES TERMINATED';
        if (playAgainBtn) playAgainBtn.textContent = 'NEW MISSION';
      }

      document.getElementById('win-cleared').textContent = isVoid ? 'TESSERACT-VOID' : 'GUARDIANS 01-05';
      document.getElementById('win-time').textContent = formatTime(state.elapsedTime);
      document.getElementById('win-score').textContent = String(state.score).padStart(5, '0');
      victoryOverlay.classList.remove('hidden');
    }

    function formatTime(sec) {
      const mins = Math.floor(sec / 60);
      const rem = Math.floor(sec % 60);
      return `${String(mins).padStart(2, '0')}:${String(rem).padStart(2, '0')}`;
    }

    /* ============================================================================
       9. PARTICLE & SHOCKWAVE FX SYSTEM
       ============================================================================ */
    function spawnParticles(x, y, count, color, speedMin, speedMax, lifeMax) {
      for (let i = 0; i < count; i++) {
        const ang = Math.random() * Math.PI * 2;
        const spd = speedMin + Math.random() * (speedMax - speedMin);
        state.particles.push({
          x,
          y,
          vx: Math.cos(ang) * spd,
          vy: Math.sin(ang) * spd,
          life: lifeMax * (0.5 + Math.random() * 0.5),
          maxLife: lifeMax,
          size: 1.8 + Math.random() * 2.8,
          color
        });
      }
    }

    function spawnShockwave(x, y, maxRadius, color, duration) {
      state.shockwaves.push({
        x,
        y,
        radius: 8,
        maxRadius,
        color,
        life: duration,
        maxLife: duration
      });
    }

    function spawnTeleportAnimation(fromX, fromY, toX, toY, radius = 14, isBoss = false) {
      state.teleports.push({
        fromX,
        fromY,
        toX,
        toY,
        radius,
        isBoss,
        life: isBoss ? 0.52 : 0.42,
        maxLife: isBoss ? 0.52 : 0.42,
        seed: Math.random() * 100
      });
    }

    function updateTeleports(dt) {
      for (let i = state.teleports.length - 1; i >= 0; i--) {
        state.teleports[i].life -= dt;
        if (state.teleports[i].life <= 0) {
          state.teleports.splice(i, 1);
        }
      }
    }

    function spawnFloatingText(x, y, text, color = '#ffffff') {
      state.floatingTexts.push({
        x,
        y,
        vy: -42,
        text,
        color,
        life: 1.25,
        maxLife: 1.25
      });
    }

    function updateHealthPacks(dt) {
      if (state.screen !== 'PLAYING' || !state.boss) return;

      const p = state.player;
      if (!state.healthPack) {
        state.healthPackTimer -= dt;
        if (state.healthPackTimer <= 0) {
          let wp = pickFullArenaWaypoint(110);
          for (let tries = 0; tries < 8; tries++) {
            const dPlayer = Math.hypot(wp.x - p.x, wp.y - p.y);
            const dBoss = Math.hypot(wp.x - state.boss.x, wp.y - state.boss.y);
            if (dPlayer >= 130 && dBoss >= state.boss.radius + 95) break;
            wp = pickFullArenaWaypoint(110);
          }

          state.healthPack = {
            x: wp.x,
            y: wp.y,
            radius: GAME_CONFIG.HEALTH_PACK_RADIUS,
            pulse: 0
          };
          spawnShockwave(wp.x, wp.y, 64, '#ffffff', 0.38);
          spawnParticles(wp.x, wp.y, 12, '#ffffff', 35, 140, 0.4);
          sfx.playSelect();
        }
      } else {
        const hpPack = state.healthPack;
        hpPack.pulse += dt * 4.5;

        if (Math.hypot(p.x - hpPack.x, p.y - hpPack.y) <= p.radius + hpPack.radius) {
          const healPct =
            GAME_CONFIG.HEALTH_PACK_HEAL_MIN +
            Math.random() * (GAME_CONFIG.HEALTH_PACK_HEAL_MAX - GAME_CONFIG.HEALTH_PACK_HEAL_MIN);
          const healAmount = Math.max(1, Math.round(p.maxHp * healPct));
          p.hp = Math.min(p.maxHp, p.hp + healAmount);

          sfx.playHeal();
          spawnShockwave(hpPack.x, hpPack.y, 75, '#ffffff', 0.35);
          spawnShockwave(hpPack.x, hpPack.y, 50, '#ff0000', 0.25);
          spawnParticles(hpPack.x, hpPack.y, 18, '#ffffff', 45, 190, 0.45);
          spawnFloatingText(hpPack.x, hpPack.y - 16, `+${healAmount} HP (${Math.round(healPct * 100)}%)`, '#ffffff');

          state.healthPack = null;
          state.healthPackTimer =
            GAME_CONFIG.HEALTH_PACK_COOLDOWN_MIN +
            Math.random() * (GAME_CONFIG.HEALTH_PACK_COOLDOWN_MAX - GAME_CONFIG.HEALTH_PACK_COOLDOWN_MIN);
        }
      }
    }

    function updateUpgradePacks(dt) {
      if (state.screen !== 'PLAYING' || !state.boss) return;

      const p = state.player;
      if (!state.upgradePack) {
        state.upgradePackTimer -= dt;
        if (state.upgradePackTimer <= 0) {
          let wp = pickFullArenaWaypoint(110);
          for (let tries = 0; tries < 8; tries++) {
            const dPlayer = Math.hypot(wp.x - p.x, wp.y - p.y);
            const dBoss = Math.hypot(wp.x - state.boss.x, wp.y - state.boss.y);
            const dHealth = state.healthPack ? Math.hypot(wp.x - state.healthPack.x, wp.y - state.healthPack.y) : 999;
            if (dPlayer >= 130 && dBoss >= state.boss.radius + 95 && dHealth >= 80) break;
            wp = pickFullArenaWaypoint(110);
          }

          state.upgradePack = {
            x: wp.x,
            y: wp.y,
            radius: GAME_CONFIG.UPGRADE_PACK_RADIUS,
            pulse: 0
          };
          spawnShockwave(wp.x, wp.y, 72, '#ff0000', 0.42);
          spawnShockwave(wp.x, wp.y, 52, '#ffffff', 0.32);
          spawnParticles(wp.x, wp.y, 14, '#ff0000', 40, 160, 0.45);
          sfx.playSelect();
        }
      } else {
        const upg = state.upgradePack;
        upg.pulse += dt * 4.2;

        if (Math.hypot(p.x - upg.x, p.y - upg.y) <= p.radius + upg.radius) {
          const roll = Math.floor(Math.random() * 3);
          let label = '';

          if (roll === 0) {
            const boost =
              GAME_CONFIG.UPGRADE_FIRE_RATE_MIN +
              Math.random() * (GAME_CONFIG.UPGRADE_FIRE_RATE_MAX - GAME_CONFIG.UPGRADE_FIRE_RATE_MIN);
            p.rateMult = +(p.rateMult + boost).toFixed(3);
            label = `+${Math.round(boost * 100)}% FIRE RATE`;
          } else if (roll === 1) {
            const boost =
              GAME_CONFIG.UPGRADE_DMG_MIN +
              Math.random() * (GAME_CONFIG.UPGRADE_DMG_MAX - GAME_CONFIG.UPGRADE_DMG_MIN);
            p.dmgMult = +(p.dmgMult + boost).toFixed(3);
            label = `+${Math.round(boost * 100)}% WEAPON DMG`;
          } else {
            const bonusHp = Math.max(10, Math.round(p.maxHp * GAME_CONFIG.UPGRADE_MAX_HP_BONUS));
            p.maxHp += bonusHp;
            p.hp = Math.min(p.maxHp, p.hp + bonusHp);
            label = `+25% MAX HP (+${bonusHp})`;
          }

          sfx.playUpgrade();
          spawnShockwave(upg.x, upg.y, 88, '#ff0000', 0.42);
          spawnShockwave(upg.x, upg.y, 62, '#ffffff', 0.32);
          spawnParticles(upg.x, upg.y, 20, '#ff0000', 55, 220, 0.5);
          spawnParticles(upg.x, upg.y, 14, '#ffffff', 50, 190, 0.45);
          spawnFloatingText(upg.x, upg.y - 18, `UPGRADE // ${label}`, '#ff0000');

          state.upgradePack = null;
          state.upgradePackTimer =
            GAME_CONFIG.UPGRADE_PACK_COOLDOWNS[Math.floor(Math.random() * GAME_CONFIG.UPGRADE_PACK_COOLDOWNS.length)];
        }
      }
    }

    function updateFX(dt) {
      for (let i = state.particles.length - 1; i >= 0; i--) {
        const pt = state.particles[i];
        pt.x += pt.vx * dt;
        pt.y += pt.vy * dt;
        pt.vx *= 0.94;
        pt.vy *= 0.94;
        pt.life -= dt;
        if (pt.life <= 0) state.particles.splice(i, 1);
      }

      for (let i = state.shockwaves.length - 1; i >= 0; i--) {
        const sw = state.shockwaves[i];
        sw.life -= dt;
        const progress = 1 - Math.max(0, sw.life / sw.maxLife);
        sw.radius = 8 + (sw.maxRadius - 8) * (1 - Math.pow(1 - progress, 3));
        if (sw.life <= 0) state.shockwaves.splice(i, 1);
      }

      for (let i = state.floatingTexts.length - 1; i >= 0; i--) {
        const ft = state.floatingTexts[i];
        ft.y += ft.vy * dt;
        ft.life -= dt;
        if (ft.life <= 0) state.floatingTexts.splice(i, 1);
      }
    }

    /* ============================================================================
       9B. PROJECTILES, MINES & DRONE / 4D FLEET SIMULATION
       ============================================================================ */
    function updateEntities(dt) {
      const p = state.player;
      const b = state.boss;
      const w = GAME_CONFIG.ARENA_WIDTH;
      const h = GAME_CONFIG.ARENA_HEIGHT;

      // 1. Update Player Plasma Bolts & Collision Checks
      for (let i = state.playerBullets.length - 1; i >= 0; i--) {
        const pb = state.playerBullets[i];
        pb.x += pb.vx * dt;
        pb.y += pb.vy * dt;
        pb.life -= dt;

        if (pb.life <= 0 || pb.x < -25 || pb.x > w + 25 || pb.y < 52 || pb.y > h + 25) {
          state.playerBullets.splice(i, 1);
          continue;
        }

        let consumed = false;

        // Check Hit on Proximity Mines
        for (let mIdx = state.mines.length - 1; mIdx >= 0; mIdx--) {
          const m = state.mines[mIdx];
          if (Math.hypot(pb.x - m.x, pb.y - m.y) <= pb.radius + m.radius) {
            spawnExplosion(m.x, m.y, false);
            state.mines.splice(mIdx, 1);
            state.score += 50;
            consumed = true;
            break;
          }
        }
        if (consumed) {
          state.playerBullets.splice(i, 1);
          continue;
        }

        // Check Hit on Drones & Tesseract 4D Fleet Ships
        for (let dIdx = state.drones.length - 1; dIdx >= 0; dIdx--) {
          const dr = state.drones[dIdx];
          if (Math.hypot(pb.x - dr.x, pb.y - dr.y) <= pb.radius + dr.radius) {
            dr.hp -= pb.damage;
            spawnParticles(pb.x, pb.y, 4, '#ffffff', 40, 150, 0.2);
            if (dr.hp <= 0) {
              spawnExplosion(dr.x, dr.y, false);
              state.drones.splice(dIdx, 1);
              state.score += 100;
            }
            consumed = true;
            break;
          }
        }
        if (consumed) {
          state.playerBullets.splice(i, 1);
          continue;
        }

        // Check Hit on Active Boss Hull
        if (b && b.hp > 0 && !b.isDying) {
          if (Math.hypot(pb.x - b.x, pb.y - b.y) <= pb.radius + b.radius) {
            b.hp = Math.max(0, b.hp - pb.damage);
            b.hitFlash = 0.05;
            state.score += 10;
            spawnParticles(pb.x, pb.y, 4, '#ffffff', 45, 165, 0.22);
            state.playerBullets.splice(i, 1);

            if (b.hp <= 0) {
              handleBossDefeated();
              return;
            }
            continue;
          }
        }
      }

      // 2. Update Enemy Projectiles (Standard, Homing & RGB Prism Bolts)
      for (let i = state.enemyBullets.length - 1; i >= 0; i--) {
        const eb = state.enemyBullets[i];
        if (eb.homing && state.screen === 'PLAYING') {
          const targetAng = Math.atan2(p.y - eb.y, p.x - eb.x);
          const curAng = Math.atan2(eb.vy, eb.vx);
          const spd = Math.hypot(eb.vx, eb.vy);
          const nextAng = rotateAngleTowards(curAng, targetAng, 1.85 * dt);
          eb.vx = Math.cos(nextAng) * spd;
          eb.vy = Math.sin(nextAng) * spd;
        }

        eb.x += eb.vx * dt;
        eb.y += eb.vy * dt;
        eb.life -= dt;

        if (eb.life <= 0 || eb.x < -30 || eb.x > w + 30 || eb.y < 52 || eb.y > h + 30) {
          state.enemyBullets.splice(i, 1);
          continue;
        }

        if (state.screen === 'PLAYING' && Math.hypot(eb.x - p.x, eb.y - p.y) < eb.radius + p.radius - 2) {
          state.enemyBullets.splice(i, 1);
          damagePlayer(eb.damage || 15);
          if (state.screen !== 'PLAYING') return;
        }
      }

      // 3. Update Level 2 Proximity Mines
      for (let i = state.mines.length - 1; i >= 0; i--) {
        const m = state.mines[i];
        m.x += m.vx * dt;
        m.y += m.vy * dt;
        m.vx *= 0.96;
        m.vy *= 0.96;
        m.x = Math.max(28, Math.min(w - 28, m.x));
        m.y = Math.max(82, Math.min(h - 28, m.y));
        m.pulse += dt * 6;
        m.timer -= dt;

        const distToPlayer = Math.hypot(p.x - m.x, p.y - m.y);
        if (distToPlayer <= m.triggerRadius || m.timer <= 0) {
          spawnExplosion(m.x, m.y, false);
          state.mines.splice(i, 1);
          if (state.screen === 'PLAYING' && distToPlayer <= m.triggerRadius + p.radius) {
            damagePlayer(18);
            if (state.screen !== 'PLAYING') return;
          }
        }
      }

      // 4. Update Sector Drones & Tesseract Void 4-Class Fleet
      for (let i = state.drones.length - 1; i >= 0; i--) {
        const dr = state.drones[i];
        const shipPhase = dr.phase || 1;
        const dmgMult = dr.dmgMult || 1.0;
        const angToPlayer = Math.atan2(p.y - dr.y, p.x - dr.x);
        const distToPlayer = Math.hypot(p.x - dr.x, p.y - dr.y);

        if (dr.type === 'teleporter') {
          if (dr.warpAnimTimer > 0) dr.warpAnimTimer = Math.max(0, dr.warpAnimTimer - dt);
          dr.angle = angToPlayer;
          dr.stateTimer -= dt;

          if (dr.stateTimer <= 0) {
            if (dr.teleportsLeft > 0) {
              const oldX = dr.x;
              const oldY = dr.y;
              const flankAng = angToPlayer + (Math.random() - 0.5) * 2.2;
              const flankDist = 175 + Math.random() * 90;
              dr.x = Math.max(40, Math.min(w - 40, p.x + Math.cos(flankAng) * flankDist));
              dr.y = Math.max(90, Math.min(h - 40, p.y + Math.sin(flankAng) * flankDist));
              dr.angle = Math.atan2(p.y - dr.y, p.x - dr.x);
              dr.warpAnimTimer = 0.3;
              dr.teleportsLeft -= 1;
              dr.stateTimer = Math.max(0.65, 1.15 - (shipPhase - 1) * 0.12);

              spawnTeleportAnimation(oldX, oldY, dr.x, dr.y, dr.radius, false);
              sfx.playBossFire();

              const shotDmg = Math.round(12 * dmgMult);
              const spreads = shipPhase >= 3 ? [-0.16, 0, 0.16] : [0];
              spreads.forEach((off) => {
                spawnEnemyBullet(
                  dr.x + Math.cos(dr.angle) * 14,
                  dr.y + Math.sin(dr.angle) * 14,
                  dr.angle + off,
                  GAME_CONFIG.BOSS_BULLET_SPEED * 1.05,
                  5,
                  '#ff0000',
                  false,
                  null,
                  shotDmg
                );
              });
            } else {
              spawnParticles(dr.x, dr.y, 10, '#ffffff', 40, 150, 0.35);
              state.drones.splice(i, 1);
              continue;
            }
          }
        } else if (dr.type === 'speeder') {
          if (dr.afterimages) {
            for (let aIdx = dr.afterimages.length - 1; aIdx >= 0; aIdx--) {
              dr.afterimages[aIdx].life -= dt;
              if (dr.afterimages[aIdx].life <= 0) dr.afterimages.splice(aIdx, 1);
            }
          }

          if (dr.subState === 'ORBIT_DASH') {
            dr.orbitAngle += dr.orbitDir * (2.5 + shipPhase * 0.3) * dt;
            const orbitR = 235;
            const tx = Math.max(40, Math.min(w - 40, p.x + Math.cos(dr.orbitAngle) * orbitR));
            const ty = Math.max(90, Math.min(h - 40, p.y + Math.sin(dr.orbitAngle) * orbitR));
            dr.vx = (tx - dr.x) * 4.5;
            dr.vy = (ty - dr.y) * 4.5;
            dr.x += dr.vx * dt;
            dr.y += dr.vy * dt;
            dr.angle = angToPlayer;

            dr.stateTimer -= dt;
            if (dr.stateTimer <= 0) {
              dr.subState = 'TELEGRAPH';
              dr.angle = angToPlayer;
              dr.stateTimer = 0.55;
            }
          } else if (dr.subState === 'TELEGRAPH') {
            if (dr.stateTimer > 0.2) {
              dr.angle = rotateAngleTowards(dr.angle, angToPlayer, dt * 4.2);
            }
            dr.stateTimer -= dt;
            if (dr.stateTimer <= 0) {
              dr.subState = 'CHARGE';
              const chargeSpeed = 680 + shipPhase * 55;
              dr.vx = Math.cos(dr.angle) * chargeSpeed;
              dr.vy = Math.sin(dr.angle) * chargeSpeed;
              dr.stateTimer = 0.65;
              sfx.playDash();
            }
          } else if (dr.subState === 'CHARGE') {
            dr.x += dr.vx * dt;
            dr.y += dr.vy * dt;
            if (dr.afterimages) {
              dr.afterimages.push({ x: dr.x, y: dr.y, angle: dr.angle, life: 0.18 });
            }
            dr.stateTimer -= dt;
            if (dr.stateTimer <= 0 || dr.x < 30 || dr.x > w - 30 || dr.y < 80 || dr.y > h - 30) {
              dr.x = Math.max(32, Math.min(w - 32, dr.x));
              dr.y = Math.max(82, Math.min(h - 32, dr.y));
              dr.subState = 'ORBIT_DASH';
              dr.orbitAngle = Math.atan2(dr.y - p.y, dr.x - p.x);
              dr.stateTimer = 1.75;
            }
          }
        } else if (dr.type === 'bomber') {
          const chaseSpeed = 225 + (shipPhase - 1) * 24;
          dr.angle = angToPlayer;
          dr.x += Math.cos(angToPlayer) * chaseSpeed * dt;
          dr.y += Math.sin(angToPlayer) * chaseSpeed * dt;
          dr.fuseTimer -= dt;

          const blastR = (dr.blastRadius || 105) + (shipPhase - 1) * 14;
          if (distToPlayer <= dr.radius + p.radius + 10 || dr.fuseTimer <= 0) {
            spawnExplosion(dr.x, dr.y, true);
            spawnShockwave(dr.x, dr.y, blastR, '#ff0000', 0.38);
            state.drones.splice(i, 1);
            if (state.screen === 'PLAYING' && distToPlayer <= blastR + p.radius) {
              damagePlayer(Math.round(20 * dmgMult));
              if (state.screen !== 'PLAYING') return;
            }
            continue;
          }
        } else if (dr.type === 'shooter') {
          dr.angle = angToPlayer;
          const desiredDist = 285;
          const radialPush = distToPlayer > desiredDist + 25 ? 1 : (distToPlayer < desiredDist - 25 ? -0.8 : 0);
          const strafeAng = angToPlayer + (Math.PI / 2) * dr.strafeDir;
          const moveSpeed = 165 + (shipPhase - 1) * 18;
          dr.x += (Math.cos(angToPlayer) * radialPush + Math.cos(strafeAng) * 0.85) * moveSpeed * dt;
          dr.y += (Math.sin(angToPlayer) * radialPush + Math.sin(strafeAng) * 0.85) * moveSpeed * dt;
          dr.x = Math.max(36, Math.min(w - 36, dr.x));
          dr.y = Math.max(86, Math.min(h - 36, dr.y));

          dr.fireTimer -= dt;
          if (dr.fireTimer <= 0) {
            dr.fireTimer = Math.max(0.85, 1.35 - (shipPhase - 1) * 0.12);
            sfx.playBossFire();
            const perpX = -Math.sin(dr.angle);
            const perpY = Math.cos(dr.angle);
            const shotDmg = Math.round(11 * dmgMult);
            [-5, 5].forEach((offset) => {
              spawnEnemyBullet(
                dr.x + Math.cos(dr.angle) * 15 + perpX * offset,
                dr.y + Math.sin(dr.angle) * 15 + perpY * offset,
                dr.angle,
                GAME_CONFIG.BOSS_BULLET_SPEED * 1.08,
                4.8,
                '#ff0000',
                false,
                null,
                shotDmg
              );
            });
          }

          dr.lifeTimer -= dt;
          if (dr.lifeTimer <= 0) {
            spawnParticles(dr.x, dr.y, 10, '#ff0000', 40, 150, 0.35);
            state.drones.splice(i, 1);
            continue;
          }
        } else {
          // Standard Level 3 Helix-Core Autonomous Drone
          const moveSpeed = 175;
          if (distToPlayer > 170) {
            dr.vx = Math.cos(angToPlayer) * moveSpeed;
            dr.vy = Math.sin(angToPlayer) * moveSpeed;
          } else {
            const orbitAng = angToPlayer + Math.PI / 2;
            dr.vx = Math.cos(orbitAng) * moveSpeed * 0.85;
            dr.vy = Math.sin(orbitAng) * moveSpeed * 0.85;
          }
          dr.x = Math.max(32, Math.min(w - 32, dr.x + dr.vx * dt));
          dr.y = Math.max(82, Math.min(h - 32, dr.y + dr.vy * dt));

          dr.fireTimer -= dt;
          if (dr.fireTimer <= 0) {
            dr.fireTimer = 1.65;
            sfx.playBossFire();
            spawnEnemyBullet(dr.x, dr.y, angToPlayer, GAME_CONFIG.BOSS_BULLET_SPEED * 0.9, 5, '#ff0000', false, null, 12);
          }
        }

        // Contact damage if player collides with drone hull
        if (state.screen === 'PLAYING' && Math.hypot(p.x - dr.x, p.y - dr.y) < p.radius + dr.radius - 2) {
          damagePlayer(Math.round(14 * dmgMult));
          if (state.screen !== 'PLAYING') return;
        }
      }
    }

    /* ============================================================================
       10. PLAYER PHYSICS, DIRECTIONAL FACING, DASH & AIM-ASSISTED WEAPONS
       ============================================================================ */
    function rotateAngleTowards(current, target, maxStep) {
      let diff = target - current;
      while (diff > Math.PI) diff -= Math.PI * 2;
      while (diff < -Math.PI) diff += Math.PI * 2;
      if (Math.abs(diff) <= maxStep) return target;
      return current + Math.sign(diff) * maxStep;
    }

    function computeLeadPoint(sx, sy, tx, ty, tvx, tvy, projSpeed) {
      const rx = tx - sx;
      const ry = ty - sy;
      const a = tvx * tvx + tvy * tvy - projSpeed * projSpeed;
      const b = 2 * (rx * tvx + ry * tvy);
      const c = rx * rx + ry * ry;

      let t = 0;
      if (Math.abs(a) < 1e-4) {
        if (Math.abs(b) > 1e-4) t = Math.max(0, -c / b);
      } else {
        const disc = b * b - 4 * a * c;
        if (disc >= 0) {
          const sqrtD = Math.sqrt(disc);
          const t1 = (-b - sqrtD) / (2 * a);
          const t2 = (-b + sqrtD) / (2 * a);
          const valid = [t1, t2].filter((val) => val > 0);
          if (valid.length > 0) t = Math.min(...valid);
        }
      }
      t = Math.min(t, 1.35);
      return { x: tx + tvx * t, y: ty + tvy * t };
    }

    function computeAimAssist(rawAngle) {
      const p = state.player;
      const candidates = [];

      if (state.boss && state.boss.hp > 0) {
        candidates.push({
          x: state.boss.x,
          y: state.boss.y,
          vx: state.boss.vx || 0,
          vy: state.boss.vy || 0,
          priority: 1.0
        });
      }

      for (const dr of state.drones) {
        candidates.push({
          x: dr.x,
          y: dr.y,
          vx: dr.vx || 0,
          vy: dr.vy || 0,
          priority: 1.15
        });
      }

      for (const m of state.mines) {
        candidates.push({
          x: m.x,
          y: m.y,
          vx: m.vx || 0,
          vy: m.vy || 0,
          priority: 1.25
        });
      }

      let bestTarget = null;
      let bestScore = Infinity;

      for (const cand of candidates) {
        const dx = cand.x - p.x;
        const dy = cand.y - p.y;
        const dist = Math.hypot(dx, dy);
        if (dist > GAME_CONFIG.PLAYER_AIM_ASSIST_RANGE || dist < 10) continue;

        const directAngle = Math.atan2(dy, dx);
        let diff = directAngle - rawAngle;
        while (diff > Math.PI) diff -= Math.PI * 2;
        while (diff < -Math.PI) diff += Math.PI * 2;

        if (Math.abs(diff) <= GAME_CONFIG.PLAYER_AIM_ASSIST_CONE) {
          const score = Math.abs(diff) * cand.priority + (dist / GAME_CONFIG.PLAYER_AIM_ASSIST_RANGE) * 0.16;
          if (score < bestScore) {
            bestScore = score;
            bestTarget = cand;
          }
        }
      }

      if (!bestTarget) {
        p.aimPoint = null;
        return rawAngle;
      }

      const lead = computeLeadPoint(
        p.x,
        p.y,
        bestTarget.x,
        bestTarget.y,
        bestTarget.vx,
        bestTarget.vy,
        GAME_CONFIG.PLAYER_BULLET_SPEED
      );
      p.aimPoint = lead;
      return Math.atan2(lead.y - p.y, lead.x - p.x);
    }

    function tryPlayerDash() {
      const p = state.player;
      if (p.dashCooldown > 0 || p.dashTimer > 0) return;

      let ix = (keys.KeyD ? 1 : 0) - (keys.KeyA ? 1 : 0) + touchInput.moveX;
      let iy = (keys.KeyS ? 1 : 0) - (keys.KeyW ? 1 : 0) + touchInput.moveY;
      const mag = Math.hypot(ix, iy);

      if (mag > 0.05) {
        ix /= mag;
        iy /= mag;
      } else {
        ix = Math.cos(p.angle);
        iy = Math.sin(p.angle);
      }

      p.angle = Math.atan2(iy, ix);
      p.dashTimer = GAME_CONFIG.PLAYER_DASH_DURATION;
      p.dashCooldown = GAME_CONFIG.PLAYER_DASH_COOLDOWN;
      p.invulnTimer = Math.max(p.invulnTimer, GAME_CONFIG.PLAYER_DASH_DURATION + 0.08);
      p.dashVx = ix * GAME_CONFIG.PLAYER_DASH_SPEED;
      p.dashVy = iy * GAME_CONFIG.PLAYER_DASH_SPEED;

      sfx.playDash();
      spawnShockwave(p.x, p.y, 65, '#ffffff', 0.25);
      spawnParticles(p.x, p.y, 12, '#ffffff', 60, 220, 0.3);
    }

    function updatePlayer(dt) {
      const p = state.player;

      if (p.fireCooldown > 0) p.fireCooldown -= dt;
      if (p.dashCooldown > 0) p.dashCooldown -= dt;
      if (p.invulnTimer > 0) p.invulnTimer -= dt;
      if (p.muzzleFlash > 0) p.muzzleFlash -= dt;

      for (let i = p.afterimages.length - 1; i >= 0; i--) {
        p.afterimages[i].life -= dt;
        if (p.afterimages[i].life <= 0) p.afterimages.splice(i, 1);
      }

      let ix = (keys.KeyD ? 1 : 0) - (keys.KeyA ? 1 : 0) + touchInput.moveX;
      let iy = (keys.KeyS ? 1 : 0) - (keys.KeyW ? 1 : 0) + touchInput.moveY;
      const inputMag = Math.hypot(ix, iy);
      if (inputMag > 1) {
        ix /= inputMag;
        iy /= inputMag;
      }

      if (inputMag > 0.05) {
        const moveTargetAngle = Math.atan2(iy, ix);
        p.angle = rotateAngleTowards(p.angle, moveTargetAngle, GAME_CONFIG.PLAYER_SHIP_TURN_SPEED * dt);
      }

      if (p.dashTimer > 0) {
        p.dashTimer -= dt;
        p.vx = p.dashVx;
        p.vy = p.dashVy;
        p.afterimages.push({ x: p.x, y: p.y, angle: p.angle, life: 0.18 });
      } else {
        p.vx += ix * GAME_CONFIG.PLAYER_ACCELERATION * dt;
        p.vy += iy * GAME_CONFIG.PLAYER_ACCELERATION * dt;

        const damping = Math.pow(GAME_CONFIG.PLAYER_ZERO_G_GRAVITY, dt * 60);
        p.vx *= damping;
        p.vy *= damping;

        // Gravitational Pull from THE TESSERACTS VOID Singularity (escalates with phase)
        if (state.boss && state.boss.isVoid && !state.boss.isDying) {
          const dxVoid = state.boss.x - p.x;
          const dyVoid = state.boss.y - p.y;
          const distVoid = Math.max(60, Math.hypot(dxVoid, dyVoid));
          const phasePullBoost = 1 + (state.boss.phase - 1) * 0.24;
          const pullMult = (state.boss.implosionTimer > 0 ? 2.55 : 1.0) * phasePullBoost;
          const pull = GAME_CONFIG.VOID_PULL_FORCE * pullMult;
          p.vx += (dxVoid / distVoid) * pull * dt;
          p.vy += (dyVoid / distVoid) * pull * dt;
        }

        const currentSpeed = Math.hypot(p.vx, p.vy);
        if (currentSpeed > GAME_CONFIG.PLAYER_MOVE_SPEED) {
          p.vx = (p.vx / currentSpeed) * GAME_CONFIG.PLAYER_MOVE_SPEED;
          p.vy = (p.vy / currentSpeed) * GAME_CONFIG.PLAYER_MOVE_SPEED;
        }
      }

      p.x += p.vx * dt;
      p.y += p.vy * dt;

      const pad = p.radius + 14;
      const topPad = 68;
      p.x = Math.max(pad, Math.min(GAME_CONFIG.ARENA_WIDTH - pad, p.x));
      p.y = Math.max(topPad, Math.min(GAME_CONFIG.ARENA_HEIGHT - pad, p.y));

      let ax = (keys.ArrowRight ? 1 : 0) - (keys.ArrowLeft ? 1 : 0) + touchInput.aimX;
      let ay = (keys.ArrowDown ? 1 : 0) - (keys.ArrowUp ? 1 : 0) + touchInput.aimY;
      const aimMag = Math.hypot(ax, ay);

      if (aimMag > 0.1 || touchInput.firing) {
        const rawFireAngle = Math.atan2(ay, ax);
        const assistedFireAngle = computeAimAssist(rawFireAngle);

        if (p.fireCooldown <= 0) {
          p.fireCooldown = GAME_CONFIG.PLAYER_FIRE_RATE / (p.rateMult || 1.0);
          p.muzzleFlash = 0.045;
          sfx.playShoot();

          const perpX = -Math.sin(assistedFireAngle);
          const perpY = Math.cos(assistedFireAngle);
          const noseX = p.x + Math.cos(assistedFireAngle) * 18;
          const noseY = p.y + Math.sin(assistedFireAngle) * 18;
          const boltDamage = Math.round(GAME_CONFIG.PLAYER_BULLET_DAMAGE * (p.dmgMult || 1.0));

          [-5.5, 5.5].forEach((offset) => {
            state.playerBullets.push({
              x: noseX + perpX * offset,
              y: noseY + perpY * offset,
              vx: Math.cos(assistedFireAngle) * GAME_CONFIG.PLAYER_BULLET_SPEED,
              vy: Math.sin(assistedFireAngle) * GAME_CONFIG.PLAYER_BULLET_SPEED,
              radius: 4,
              damage: boltDamage,
              life: 1.5
            });
          });
        }
      } else {
        p.aimPoint = null;
      }
    }

    function damagePlayer(amount) {
      const p = state.player;
      if (state.practiceMode || p.invulnTimer > 0 || state.screen !== 'PLAYING') return;

      const mult = state.hardMode ? 1.35 : 1.0;
      p.hp = Math.max(0, p.hp - Math.round(amount * mult));
      p.invulnTimer = GAME_CONFIG.PLAYER_INVULN_TIME;
      state.shakeTime = 0.25;
      state.shakeMagnitude = 9;
      sfx.playHit();
      spawnParticles(p.x, p.y, 18, '#ff0000', 60, 260, 0.45);
      spawnShockwave(p.x, p.y, 70, '#ff0000', 0.3);

      if (p.hp <= 0) {
        spawnExplosion(p.x, p.y, true);
        triggerGameOver();
      }
    }

    function spawnExplosion(x, y, large = false) {
      sfx.playExplosion(large);
      spawnShockwave(x, y, large ? 220 : 80, '#ff0000', large ? 0.6 : 0.3);
      spawnShockwave(x, y, large ? 150 : 50, '#ffffff', large ? 0.45 : 0.22);
      spawnParticles(x, y, large ? 45 : 16, '#ff0000', 50, large ? 360 : 210, large ? 0.8 : 0.45);
      spawnParticles(x, y, large ? 30 : 10, '#ffffff', 40, large ? 300 : 180, large ? 0.7 : 0.4);
    }

    /* ============================================================================
       11. BOSS AI & ATTACK PATTERNS (THE GUARDIANS 1-5 + THE TESSERACTS VOID)
       ============================================================================ */
    function spawnEnemyBullet(x, y, angle, speed, radius = 5.5, color = '#ff0000', homing = false, rgbHue = null, damage = 15) {
      state.enemyBullets.push({
        x,
        y,
        vx: Math.cos(angle) * speed,
        vy: Math.sin(angle) * speed,
        radius,
        color,
        rgbHue,
        homing,
        damage,
        life: homing ? 4.2 : 6.0
      });
    }

    function pickFullArenaWaypoint(margin = 95) {
      const minX = margin;
      const maxX = GAME_CONFIG.ARENA_WIDTH - margin;
      const minY = 95;
      const maxY = GAME_CONFIG.ARENA_HEIGHT - margin;
      return {
        x: minX + Math.random() * (maxX - minX),
        y: minY + Math.random() * (maxY - minY)
      };
    }

    function clampBossToArena(b) {
      const minX = b.radius + 26;
      const maxX = GAME_CONFIG.ARENA_WIDTH - b.radius - 26;
      const minY = b.radius + 76;
      const maxY = GAME_CONFIG.ARENA_HEIGHT - b.radius - 26;
      b.x = Math.max(minX, Math.min(maxX, b.x));
      b.y = Math.max(minY, Math.min(maxY, b.y));
    }

    function updateBoss(dt) {
      const b = state.boss;
      const p = state.player;
      if (!b) return;

      // Run Cinematic Center-Pull, Cracking & Detonation Sequence for TESSERACT-VOID
      if (b.isVoid && b.isDying) {
        updateTesseractDeath(b, dt);
        return;
      }

      b.angle += b.rotSpeed * dt;
      if (b.level === 5 && !b.isVoid) {
        b.rgbHue = (b.rgbHue + GAME_CONFIG.FINAL_BOSS_RGB_SPEED * dt) % 360;
      }
      if (b.hitFlash > 0) b.hitFlash -= dt;
      if (b.warpAnimTimer > 0) b.warpAnimTimer = Math.max(0, b.warpAnimTimer - dt);
      if (b.ultAnimTimer > 0) b.ultAnimTimer = Math.max(0, b.ultAnimTimer - dt);

      // Phase Escalation Logic
      if (b.isVoid) {
        // THE TESSERACTS VOID escalates at 75% (Phase 2), 50% (Phase 3), and 25% HP (Phase 4)
        const thresholds = GAME_CONFIG.VOID_PHASE_THRESHOLDS;
        while (b.phase < 4 && b.hp <= b.maxHp * thresholds[b.phase]) {
          b.phase += 1;
          b.voidDmgMult = GAME_CONFIG.VOID_PHASE_DMG_MULTS[b.phase - 1] || 1.0;
          b.rotSpeed = 0.89 * (1 + (b.phase - 1) * 0.34);

          sfx.playPhaseShift();
          sfx.playExplosion(false);
          state.shakeTime = 0.42 + b.phase * 0.08;
          state.shakeMagnitude = 10 + b.phase * 2.5;

          spawnShockwave(b.x, b.y, 180 + b.phase * 45, '#ff0000', 0.55);
          spawnShockwave(b.x, b.y, 130 + b.phase * 35, '#ffffff', 0.42);
          spawnParticles(b.x, b.y, 24 + b.phase * 8, '#ff0000', 80, 340, 0.7);
          spawnParticles(b.x, b.y, 20 + b.phase * 6, '#ffffff', 75, 310, 0.65);

          const phaseSubtitles = {
            2: '75% HULL // GRAVITATIONAL LENSING & DMG +25%',
            3: '50% HULL // REALITY FRACTURING & DMG +55%',
            4: '25% HULL // SINGULARITY OVERDRIVE & DMG +90%'
          };
          state.bannerTag = `SINGULARITY ESCALATION // PHASE 0${b.phase}`;
          state.bannerTitle = `TESSERACT-VOID — PHASE 0${b.phase}`;
          state.bannerSubtitle = phaseSubtitles[b.phase] || 'DIMENSIONAL OUTPUT SURGING';
          state.bannerTimer = 2.4;
        }

        // ULT FORM AWAKENING at 10% HP: Recovers +10,000 HP & enters Phase 5 Overdrive
        if (!b.ultTriggered && b.hp > 0 && b.hp <= b.maxHp * GAME_CONFIG.VOID_ULT_THRESHOLD) {
          b.ultTriggered = true;
          b.isUlt = true;
          b.phase = 5;
          b.ultAnimTimer = 1.4;
          b.voidDmgMult = GAME_CONFIG.VOID_ULT_DMG_MULT;
          b.rotSpeed = 0.89 * 2.55;

          const prevHp = b.hp;
          b.hp = Math.min(b.maxHp, b.hp + GAME_CONFIG.VOID_ULT_HEAL);
          const healedAmount = Math.round(b.hp - prevHp);

          sfx.playOmegaBeam();
          sfx.playPhaseShift();
          sfx.playHeal();
          state.shakeTime = 0.85;
          state.shakeMagnitude = 20;

          spawnShockwave(b.x, b.y, 380, '#ff0000', 0.75);
          spawnShockwave(b.x, b.y, 280, '#ffffff', 0.6);
          spawnShockwave(b.x, b.y, 190, '#ff0000', 0.45);
          spawnParticles(b.x, b.y, 48, '#ff0000', 95, 420, 0.85);
          spawnParticles(b.x, b.y, 40, '#ffffff', 85, 380, 0.8);
          spawnFloatingText(b.x, b.y - b.radius - 34, `ULT FORM // +${healedAmount.toLocaleString()} HP`, '#ffffff');

          state.bannerTag = 'CRITICAL SINGULARITY OVERRIDE // 10% HULL';
          state.bannerTitle = 'TESSERACT-VOID // ULT FORM';
          state.bannerSubtitle = `HYPERCUBE UNBOUND // +${healedAmount.toLocaleString()} HP RECOVERED`;
          state.bannerTimer = 3.1;
        }
      } else if (b.phase === 1 && b.hp <= b.maxHp * 0.5) {
        // Standard Guardians Boss Phase 2 Escalation at 50% HP
        b.phase = 2;
        if (b.level !== 2 && b.level !== 5) {
          b.rotSpeed *= 1.45;
        }
        sfx.playPhaseShift();
        state.shakeTime = 0.4;
        state.shakeMagnitude = 12;
        spawnShockwave(b.x, b.y, 200, '#ff0000', 0.55);
        spawnParticles(b.x, b.y, 30, '#ffffff', 70, 320, 0.65);
      }

      const angleToPlayer = Math.atan2(p.y - b.y, p.x - b.x);
      const speedScale = b.phase === 2 ? 0.78 : 1.0;

      // ------------------------------------------------------------------------
      // STANDALONE SINGULARITY BOSS: THE TESSERACTS VOID (4 PHASES, 6 ATTACKS)
      // ------------------------------------------------------------------------
      if (b.isVoid) {
        const speedMult = 1 + (b.phase - 1) * 0.18;
        const dmgMult = b.voidDmgMult || 1.0;

        // Smooth dimensional figure-8 drift across the arena when not locked in a warp-laser sequence
        if (b.voidTeleportLeft <= 0 && state.lasers.length === 0 && b.omegaBeamState === 'NONE') {
          b.moveTimer += dt;
          const targetX = GAME_CONFIG.ARENA_WIDTH * 0.5 + Math.sin(b.moveTimer * 0.52 * speedMult) * 340;
          const targetY = GAME_CONFIG.ARENA_HEIGHT * 0.44 + Math.sin(b.moveTimer * 1.04 * speedMult) * 185;
          b.x += (targetX - b.x) * Math.min(1, dt * 2.1);
          b.y += (targetY - b.y) * Math.min(1, dt * 2.1);
          clampBossToArena(b);
        }

        if (b.implosionTimer > 0) {
          b.implosionTimer -= dt;
        }

        // Update ATK 03: Arc Lightning Core Charge-Up before Detonation
        if (b.arcChargeTimer > 0) {
          b.arcChargeTimer -= dt;
          if (b.arcChargeTimer > 0.25) {
            b.arcChargeAngle = rotateAngleTowards(b.arcChargeAngle, angleToPlayer, dt * 1.8);
          }
          if (Math.random() < 0.35) {
            const sparkAng = Math.random() * Math.PI * 2;
            const sparkDist = b.radius + 26 + Math.random() * 24;
            spawnParticles(
              b.x + Math.cos(sparkAng) * sparkDist,
              b.y + Math.sin(sparkAng) * sparkDist,
              1,
              Math.random() < 0.5 ? '#ff0000' : '#ffffff',
              20,
              70,
              0.22
            );
          }
          if (b.arcChargeTimer <= 0) {
            b.arcChargeTimer = 0;
            spawnArcLightningBurst(b, b.arcChargeAngle);
          }
        }

        // Update ATK 06: Multi-Jump Chrono-Warp Laser Sequence
        if (b.voidTeleportLeft > 0) {
          b.voidTeleportTimer -= dt;
          if (b.voidTeleportTimer <= 0) {
            executeTesseractWarpLaser(b, p);
            b.voidTeleportLeft -= 1;
            if (b.voidTeleportLeft > 0) {
              // Slower warp-to-warp charge interval in Form 4 and Ult Form
              b.voidTeleportTimer = b.isUlt
                ? 1.25
                : (b.phase === 4 ? 1.12 : Math.max(0.55, 0.92 - (b.phase - 1) * 0.1));
            }
          }
        }

        // Update Active Omega Beam (Attack 01 — Colossal Straight-Line Annihilation Beam)
        if (b.omegaBeamState !== 'NONE') {
          b.omegaBeamTimer -= dt;
          if (b.omegaBeamState === 'TELEGRAPH') {
            const lockCutoff = (b.phase >= 4 || b.isUlt) ? 0.42 : 0.22;
            if (b.omegaBeamTimer > lockCutoff) {
              b.omegaBeamAngle = rotateAngleTowards(b.omegaBeamAngle, angleToPlayer, dt * (2.6 + b.phase * 0.4));
            }
            if (b.omegaBeamTimer <= 0) {
              b.omegaBeamState = 'FIRING';
              b.omegaBeamTimer = 1.35 + (b.phase - 1) * 0.18;
              sfx.playOmegaBeam();
              state.shakeTime = 0.65 + b.phase * 0.08;
              state.shakeMagnitude = 16 + b.phase * 3;

              // Recoil knockback away from the colossal straight beam
              const recoilDist = 28 + b.phase * 6;
              b.x -= Math.cos(b.omegaBeamAngle) * recoilDist;
              b.y -= Math.sin(b.omegaBeamAngle) * recoilDist;
              clampBossToArena(b);

              spawnShockwave(b.x, b.y, 260 + b.phase * 40, '#ff0000', 0.52);
              spawnShockwave(b.x, b.y, 190 + b.phase * 30, '#ffffff', 0.4);
              spawnParticles(b.x, b.y, 34 + b.phase * 8, '#ff0000', 90, 380, 0.6);
              spawnParticles(b.x, b.y, 26 + b.phase * 6, '#ffffff', 80, 340, 0.52);
            }
          } else if (b.omegaBeamState === 'FIRING') {
            const omegaDmg = Math.round(28 * dmgMult);
            const segs = getOmegaBeamSegments(b);
            for (const seg of segs) {
              if (distPointToSegment(p.x, p.y, seg.x1, seg.y1, seg.x2, seg.y2) <= seg.width * 0.46 + p.radius - 2) {
                damagePlayer(omegaDmg);
                break;
              }
            }
            // Spawn continuous plasma sparks at the arena wall impact point
            if (segs.length > 0 && Math.random() < 0.7) {
              for (const imp of segs) {
                spawnParticles(imp.x2, imp.y2, 2, Math.random() < 0.5 ? '#ff0000' : '#ffffff', 60, 260, 0.3);
              }
            }
            if (b.omegaBeamTimer <= 0) {
              b.omegaBeamState = 'NONE';
            }
          }
        }

        // Cycle Through 6 Crazy Dimensional Attacks (waits 2-5 seconds after an attack completes)
        const isVoidAttackActive =
          b.omegaBeamState !== 'NONE' ||
          state.cracks.length > 0 ||
          b.arcChargeTimer > 0 ||
          state.lasers.length > 0 ||
          b.voidTeleportLeft > 0;
        if (!isVoidAttackActive) {
          b.voidAttackTimer -= dt;
        }
        if (b.voidAttackTimer <= 0) {
          const atk = b.voidAttackIndex % 6;
          b.voidAttackIndex += 1;
          const baseVoidWait =
            GAME_CONFIG.VOID_ATTACK_DELAY_MIN +
            Math.random() * (GAME_CONFIG.VOID_ATTACK_DELAY_MAX - GAME_CONFIG.VOID_ATTACK_DELAY_MIN);
          const ultTempo = b.isUlt ? 0.72 : 1.0;
          const nextVoidWait = baseVoidWait * ultTempo;
          // Slower recharge cooldown for ATK 01, ATK 03, and ATK 06 in Form 4 and Ult Form
          const slowVoidWait = (b.phase >= 4 || b.isUlt)
            ? baseVoidWait + (b.isUlt ? 1.4 : 1.1)
            : nextVoidWait;
          const rankTag = b.isUlt ? 'ULT' : `MK-${b.phase}`;

          // Ult Form passive hypercube vertex pulse on every attack cycle
          if (b.isUlt) {
            const pulseDmg = Math.round(14 * dmgMult);
            for (let v = 0; v < 4; v++) {
              const vAng = b.angle + (v * Math.PI) / 2;
              spawnEnemyBullet(
                b.x + Math.cos(vAng) * b.radius,
                b.y + Math.sin(vAng) * b.radius,
                vAng,
                GAME_CONFIG.BOSS_BULLET_SPEED * 1.15,
                6,
                v % 2 === 0 ? '#ff0000' : '#ffffff',
                false,
                null,
                pulseDmg
              );
            }
          }

          if (atk === 0) {
            // ATTACK 01: COLOSSAL STRAIGHT-LINE OMEGA BEAM (Charges slower in Form 4 & Ult Form)
            b.voidAttackLabel = `ATK 01 // ${b.isUlt ? 'TRI-OMEGA LINE' : 'OMEGA LINE'} [${rankTag}]`;
            b.omegaBeamState = 'TELEGRAPH';
            b.omegaBeamTimer = b.isUlt
              ? 1.80
              : (b.phase === 4 ? 1.55 : Math.max(0.75, 1.05 - (b.phase - 1) * 0.08));
            b.omegaBeamAngle = angleToPlayer;
            sfx.playPhaseShift();
            b.voidAttackTimer = slowVoidWait;
          } else if (atk === 1) {
            // ATTACK 02: DIMENSIONAL CRACKS (More & Wider 5-Second Fractures per Phase)
            b.voidAttackLabel = `ATK 02 // DIMENSIONAL CRACKS [${rankTag}]`;
            spawnDimensionalCracks(b, angleToPlayer);
            b.voidAttackTimer = nextVoidWait;
          } else if (atk === 2) {
            // ATTACK 03: CORE EXPLOSION + WALL-SHATTERING HOMING LIGHTNING (Charges slower in Form 4 & Ult Form)
            b.voidAttackLabel = `ATK 03 // ARC LIGHTNING BURST [${rankTag}]`;
            const chargeDur = b.isUlt ? 1.70 : (b.phase === 4 ? 1.45 : 0.55);
            b.arcChargeMax = chargeDur;
            b.arcChargeTimer = chargeDur;
            b.arcChargeAngle = angleToPlayer;
            sfx.playPhaseShift();
            b.voidAttackTimer = slowVoidWait;
          } else if (atk === 3) {
            // ATTACK 04: DIMENSIONAL RIFT BEAMS (4 -> 6 -> 8 -> 10 -> 12 Sweeping Lasers)
            const beamCount = 4 + (b.phase - 1) * 2;
            b.voidAttackLabel = `ATK 04 // ${beamCount}-BEAM RIFT ARRAY`;
            const baseSpin = angleToPlayer + Math.PI / beamCount;
            const spinDir = Math.random() < 0.5 ? 1 : -1;
            sfx.playLaser();
            spawnShockwave(b.x, b.y, 160 + b.phase * 25, '#ff0000', 0.4);
            for (let l = 0; l < beamCount; l++) {
              state.lasers.push({
                angle: baseSpin + (l * Math.PI * 2) / beamCount,
                spinRate: spinDir * (0.38 + (b.phase - 1) * 0.11),
                telegraph: Math.max(0.68, 0.92 - (b.phase - 1) * 0.07),
                duration: 1.55 + (b.phase - 1) * 0.15,
                width: 14 + (b.phase - 1) * 3,
                damage: Math.round(18 * dmgMult),
                color: l % 2 === 0 ? '#ff0000' : '#ffffff',
                voidPhase: b.phase
              });
            }
            b.voidAttackTimer = nextVoidWait;
          } else if (atk === 4) {
            // ATTACK 05: 4D FLEET DEPLOYMENT (5 -> 6 -> 7 -> 8 -> 9 Upgraded Ships)
            const shipCount = 5 + (b.phase - 1);
            b.voidAttackLabel = `ATK 05 // ${shipCount}-SHIP 4D ARMADA`;
            spawnVoidFleet(b, angleToPlayer);
            b.voidAttackTimer = nextVoidWait;
          } else {
            // ATTACK 06: CHRONO-WARP LASER (Charges slower in Form 4 & Ult Form)
            const jumpCount = 5 + (b.phase - 1);
            b.voidAttackLabel = `ATK 06 // ${jumpCount}X CHRONO-WARP LASER`;
            executeTesseractWarpLaser(b, p);
            b.voidTeleportLeft = jumpCount - 1;
            b.voidTeleportTimer = b.isUlt
              ? 1.25
              : (b.phase === 4 ? 1.12 : Math.max(0.55, 0.92 - (b.phase - 1) * 0.1));
            b.voidAttackTimer = slowVoidWait;
          }
        }
      }

      // ------------------------------------------------------------------------
      // LEVEL 1: SENTINEL — Full-Map Roaming + Radial Rings & Aimed Bursts
      // ------------------------------------------------------------------------
      else if (b.level === 1) {
        if (!b.roamTarget) b.roamTarget = pickFullArenaWaypoint(110);
        const dxWp = b.roamTarget.x - b.x;
        const dyWp = b.roamTarget.y - b.y;
        const distWp = Math.hypot(dxWp, dyWp);

        if (distWp < 38) {
          b.roamTarget = pickFullArenaWaypoint(110);
        } else {
          const roamSpeed = b.phase === 2 ? 265 : 195;
          b.vx = (dxWp / distWp) * roamSpeed;
          b.vy = (dyWp / distWp) * roamSpeed;
          b.x += b.vx * dt;
          b.y += b.vy * dt;
          clampBossToArena(b);
        }

        b.attackTimer -= dt;
        if (b.attackTimer <= 0) {
          b.attackStep++;
          sfx.playBossFire();
          const count = b.phase === 2 ? 12 : 8;
          for (let i = 0; i < count; i++) {
            const a = b.angle + (i * Math.PI * 2) / count;
            spawnEnemyBullet(b.x, b.y, a, GAME_CONFIG.BOSS_BULLET_SPEED * 0.85, 5.5, '#ff0000');
          }
          if (b.attackStep % 2 === 0) {
            [-0.14, 0, 0.14].forEach((offset) => {
              spawnEnemyBullet(b.x, b.y, angleToPlayer + offset, GAME_CONFIG.BOSS_BULLET_SPEED * 1.15, 6, '#ffffff');
            });
          }
          b.attackTimer = 1.2 * speedScale;
        }
      }

      // ------------------------------------------------------------------------
      // LEVEL 2: VIPER-X — Full-Map Roaming, Directional Dash & Proximity Mines
      // ------------------------------------------------------------------------
      else if (b.level === 2) {
        if (b.dashTimer > 0) {
          b.dashTimer -= dt;
          b.x += b.vx * dt;
          b.y += b.vy * dt;
          clampBossToArena(b);
          if (Math.hypot(b.vx, b.vy) > 15) {
            b.angle = rotateAngleTowards(b.angle, Math.atan2(b.vy, b.vx), GAME_CONFIG.VIPER_TURN_SPEED * dt);
          }
        } else {
          if (!b.roamTarget) b.roamTarget = pickFullArenaWaypoint(100);
          const dxWp = b.roamTarget.x - b.x;
          const dyWp = b.roamTarget.y - b.y;
          const distWp = Math.hypot(dxWp, dyWp);

          if (distWp < 36) {
            b.roamTarget = pickFullArenaWaypoint(100);
          } else {
            const patrolSpeed = b.phase === 2 ? 310 : 230;
            b.vx = (dxWp / distWp) * patrolSpeed;
            b.vy = (dyWp / distWp) * patrolSpeed;
            b.x += b.vx * dt;
            b.y += b.vy * dt;
            clampBossToArena(b);
            b.angle = rotateAngleTowards(b.angle, Math.atan2(b.vy, b.vx), GAME_CONFIG.VIPER_TURN_SPEED * dt);
          }

          b.dashCooldown -= dt;
          if (b.dashCooldown <= 0) {
            b.dashTimer = 0.45;
            b.dashCooldown = b.phase === 2 ? 1.45 : 2.15;
            const dashTarget = Math.random() < 0.55 ? { x: p.x, y: p.y } : pickFullArenaWaypoint(95);
            b.roamTarget = pickFullArenaWaypoint(100);
            const dashAng = Math.atan2(dashTarget.y - b.y, dashTarget.x - b.x);
            b.angle = dashAng;
            b.vx = Math.cos(dashAng) * GAME_CONFIG.VIPER_DASH_SPEED;
            b.vy = Math.sin(dashAng) * GAME_CONFIG.VIPER_DASH_SPEED;
            sfx.playDash();
            spawnShockwave(b.x, b.y, 90, '#ff0000', 0.35);

            state.mines.push({
              x: b.x,
              y: b.y,
              vx: (Math.random() - 0.5) * 45,
              vy: (Math.random() - 0.5) * 45,
              radius: 11,
              triggerRadius: 85,
              timer: 6.5,
              pulse: 0
            });
          }
        }

        b.attackTimer -= dt;
        if (b.attackTimer <= 0) {
          sfx.playBossFire();
          const spread = b.phase === 2 ? [-0.26, -0.13, 0, 0.13, 0.26] : [-0.18, 0, 0.18];
          spread.forEach((off) => {
            spawnEnemyBullet(b.x, b.y, angleToPlayer + off, GAME_CONFIG.BOSS_BULLET_SPEED * 1.1, 5, '#ff0000');
          });
          b.attackTimer = 1.05 * speedScale;
        }
      }

      // ------------------------------------------------------------------------
      // LEVEL 3: HELIX-CORE — Autonomous Drone Carrier + Spiral Fire
      // ------------------------------------------------------------------------
      else if (b.level === 3) {
        b.moveTimer += dt;
        b.x = GAME_CONFIG.ARENA_WIDTH / 2 + Math.cos(b.moveTimer * 0.9) * 260;
        b.y = 185 + Math.sin(b.moveTimer * 1.8) * 55;

        b.attackTimer -= dt;
        if (b.attackTimer <= 0) {
          b.attackStep++;
          sfx.playBossFire();
          for (let arm = 0; arm < 4; arm++) {
            const a = b.angle + (arm * Math.PI) / 2;
            spawnEnemyBullet(b.x, b.y, a, GAME_CONFIG.BOSS_BULLET_SPEED * 0.95, 5.5, '#ff0000');
          }

          const maxDrones = b.phase === 2 ? 4 : 2;
          if (b.attackStep % 3 === 0 && state.drones.length < maxDrones) {
            state.drones.push({
              x: b.x + (Math.random() - 0.5) * 60,
              y: b.y + 30,
              vx: 0,
              vy: 0,
              radius: 12,
              hp: 45,
              maxHp: 45,
              fireTimer: 1.2
            });
          }
          b.attackTimer = 0.68 * speedScale;
        }
      }

      // ------------------------------------------------------------------------
      // LEVEL 4: OBELISK-IV — High-Energy Sweeping Laser Citadel
      // ------------------------------------------------------------------------
      else if (b.level === 4) {
        b.moveTimer += dt;
        b.x = GAME_CONFIG.ARENA_WIDTH / 2 + Math.sin(b.moveTimer * 0.85) * 240;

        b.attackTimer -= dt;
        if (b.attackTimer <= 0 && state.lasers.length === 0) {
          b.attackStep++;
          sfx.playLaser();
          const beamCount = b.phase === 2 ? 3 : 2;
          const baseSpin = angleToPlayer - 0.45;
          for (let l = 0; l < beamCount; l++) {
            state.lasers.push({
              angle: baseSpin + (l * Math.PI * 2) / beamCount,
              spinRate: (b.attackStep % 2 === 0 ? 1 : -1) * (b.phase === 2 ? 0.62 : 0.48),
              telegraph: 0.95,
              duration: 1.65,
              width: 14
            });
          }
          b.attackTimer = 2.9 * speedScale;
        } else if (b.attackTimer <= 0) {
          sfx.playBossFire();
          [-0.22, 0, 0.22].forEach((off) => {
            spawnEnemyBullet(b.x, b.y, angleToPlayer + off, GAME_CONFIG.BOSS_BULLET_SPEED, 5.5, '#ffffff');
          });
          b.attackTimer = 1.25;
        }
      }

      // ------------------------------------------------------------------------
      // LEVEL 5: OMEGA-ZERO — RGB Prism Triangle with Vertex-Locked Prism Beams
      // ------------------------------------------------------------------------
      else if (b.level === 5) {
        b.moveTimer += dt;
        b.x = GAME_CONFIG.ARENA_WIDTH / 2 + Math.sin(b.moveTimer * 0.95) * 240;
        b.y = 210 + Math.cos(b.moveTimer * 1.45) * 58;

        b.attackTimer -= dt;
        if (b.attackTimer <= 0) {
          b.attackStep++;
          const mode = b.attackStep % 3;

          if (mode === 1) {
            sfx.playBossFire();
            const ringCount = b.phase === 2 ? 15 : 12;
            for (let i = 0; i < ringCount; i++) {
              const a = b.angle + (i * Math.PI * 2) / ringCount;
              const vertexHue = (b.rgbHue + (i % 3) * 120) % 360;
              spawnEnemyBullet(b.x, b.y, a, GAME_CONFIG.BOSS_BULLET_SPEED * 0.95, 6, null, false, vertexHue);
            }
            b.attackTimer = 1.05 * speedScale;
          } else if (mode === 2) {
            sfx.playBossFire();
            for (let v = 0; v < 3; v++) {
              const vAng = b.angle + (v * Math.PI * 2) / 3;
              const vx = b.x + Math.cos(vAng) * b.radius;
              const vy = b.y + Math.sin(vAng) * b.radius;
              const aimFromVertex = Math.atan2(p.y - vy, p.x - vx);
              const vertexHue = (b.rgbHue + v * 120) % 360;
              spawnEnemyBullet(vx, vy, aimFromVertex, GAME_CONFIG.BOSS_BULLET_SPEED * 0.85, 6.5, null, true, vertexHue);
            }
            b.attackTimer = 1.15 * speedScale;
          } else {
            if (state.lasers.length === 0) {
              sfx.playLaser();
              for (let v = 0; v < 3; v++) {
                const vertexOffset = (v * Math.PI * 2) / 3;
                state.lasers.push({
                  angle: b.angle + vertexOffset,
                  vertexOffset,
                  vertexIndex: v,
                  spinRate: 0,
                  telegraph: 0.9,
                  duration: 1.75,
                  width: b.phase === 2 ? 15 : 12
                });
              }
            }
            b.attackTimer = 1.35 * speedScale;
          }
        }
      }

      // Contact Damage if Player touches Boss Hull
      if (!b.isDying) {
        const distToBoss = Math.hypot(p.x - b.x, p.y - b.y);
        if (distToBoss < p.radius + b.radius - 4) {
          const contactDmg = b.isVoid ? Math.round(20 * (b.voidDmgMult || 1.0)) : 20;
          damagePlayer(contactDmg);
        }
      }
    }

    // Initialize Cinematic Tesseract Collapse: Glides to Center -> Cracks -> Explodes
    function startTesseractDeath(b) {
      b.isDying = true;
      b.exploded = false;
      b.deathTimer = 0;
      b.deathStartX = b.x;
      b.deathStartY = b.y;
      b.deathTargetX = GAME_CONFIG.ARENA_WIDTH * 0.5;
      b.deathTargetY = GAME_CONFIG.ARENA_HEIGHT * 0.5;
      b.deathMoveDuration = 0.85;
      b.deathCrackDuration = 2.45; // Cracking phase runs from 0.85s to 3.30s
      b.deathTotalDuration = 3.85; // Final 0.55s lets the massive explosion play out
      b.deathCrackSoundStep = 0;
      b.omegaBeamState = 'NONE';
      b.arcChargeTimer = 0;
      b.voidTeleportLeft = 0;
      b.implosionTimer = 0;

      // Pre-build 14 jagged dimensional fault lines radiating out from the Tesseract core
      b.deathCracks = [];
      const crackCount = 14;
      for (let i = 0; i < crackCount; i++) {
        const baseAng = (i * Math.PI * 2) / crackCount + (Math.random() - 0.5) * 0.22;
        const pts = [{ x: 0, y: 0 }];
        let curR = 10;
        let curAng = baseAng;
        const segs = 6;
        for (let s = 0; s < segs; s++) {
          curR += 28 + Math.random() * 24;
          curAng = baseAng + (s % 2 === 0 ? 1 : -1) * (0.14 + Math.random() * 0.22);
          pts.push({
            x: Math.cos(curAng) * curR,
            y: Math.sin(curAng) * curR
          });
        }
        b.deathCracks.push({
          points: pts,
          appearAt: (i / crackCount) * 0.62,
          isRed: i % 2 === 0
        });
      }

      state.bannerTag = 'SINGULARITY CONTAINMENT FAILURE';
      state.bannerTitle = 'TESSERACT-VOID // DESTABILIZING';
      state.bannerSubtitle = 'DIMENSIONAL MATRIX COLLAPSING AT GALACTIC CORE';
      state.bannerTimer = 3.1;

      sfx.playPhaseShift();
      spawnShockwave(b.x, b.y, 170, '#ffffff', 0.45);
      spawnParticles(b.x, b.y, 24, '#ffffff', 60, 240, 0.55);
    }

    function updateTesseractDeath(b, dt) {
      b.deathTimer += dt;
      const t = b.deathTimer;
      const moveEnd = b.deathMoveDuration;
      const explodeAt = moveEnd + b.deathCrackDuration;

      // Stage 1 (0.0s -> 0.85s): Pull into dead center of the arena
      if (t <= moveEnd) {
        const u = Math.min(1, t / moveEnd);
        const eased = 1 - Math.pow(1 - u, 3);
        b.x = b.deathStartX + (b.deathTargetX - b.deathStartX) * eased;
        b.y = b.deathStartY + (b.deathTargetY - b.deathStartY) * eased;
        b.angle += (2.4 + u * 4.5) * dt;
        b.hitFlash = Math.sin(t * 45) > 0 ? 0.05 : 0;

        if (Math.random() < 0.55) {
          spawnParticles(b.x, b.y, 2, Math.random() < 0.5 ? '#ff0000' : '#ffffff', 30, 140, 0.35);
        }

        // Snap into center lock at the end of Stage 1
        if (t + dt > moveEnd) {
          b.x = b.deathTargetX;
          b.y = b.deathTargetY;
          sfx.playLaser();
          state.shakeTime = 0.25;
          state.shakeMagnitude = 8;
          spawnShockwave(b.x, b.y, 140, '#ff0000', 0.35);
          spawnShockwave(b.x, b.y, 95, '#ffffff', 0.28);
        }
        return;
      }

      // Stage 2 (0.85s -> 3.30s): Locked in center, fracturing & cracking open
      if (t < explodeAt) {
        b.x = b.deathTargetX;
        b.y = b.deathTargetY;
        const crackProgress = (t - moveEnd) / b.deathCrackDuration;

        // Spin slows down erratically as structure fractures
        b.angle += Math.max(0.25, 2.2 * (1 - crackProgress)) * dt;
        b.hitFlash = crackProgress > 0.78 && Math.sin(t * 70) > 0 ? 0.05 : 0;

        state.shakeTime = 0.12;
        state.shakeMagnitude = 3 + crackProgress * 15;

        // Trigger rhythmic fracture snaps and sparks as new cracks spread
        const step = Math.floor(crackProgress * 9);
        if (step > b.deathCrackSoundStep) {
          b.deathCrackSoundStep = step;
          sfx.playHit();
          if (step % 2 === 0) sfx.playLaser();
          spawnShockwave(b.x, b.y, 75 + crackProgress * 140, step % 2 === 0 ? '#ffffff' : '#ff0000', 0.3);
          spawnParticles(b.x, b.y, 14, '#ffffff', 70, 290, 0.45);
          spawnParticles(b.x, b.y, 14, '#ff0000', 65, 260, 0.45);
        }

        // Continuous energy venting from widening cracks
        if (Math.random() < 0.45 + crackProgress * 0.45) {
          const ventAng = Math.random() * Math.PI * 2;
          const ventDist = Math.random() * (b.radius + crackProgress * 90);
          spawnParticles(
            b.x + Math.cos(ventAng) * ventDist,
            b.y + Math.sin(ventAng) * ventDist,
            1,
            Math.random() < 0.55 ? '#ffffff' : '#ff0000',
            40,
            210,
            0.35
          );
        }
        return;
      }

      // Stage 3 (3.30s): Cataclysmic Singularity Detonation
      if (!b.exploded) {
        b.exploded = true;
        state.flashAlpha = 0.85;
        state.shakeTime = 0.95;
        state.shakeMagnitude = 24;

        sfx.playOmegaBeam();
        sfx.playExplosion(true);

        spawnShockwave(b.x, b.y, 560, '#ffffff', 0.85);
        spawnShockwave(b.x, b.y, 430, '#ff0000', 0.72);
        spawnShockwave(b.x, b.y, 290, '#ffffff', 0.55);
        spawnShockwave(b.x, b.y, 170, '#ff0000', 0.42);

        spawnParticles(b.x, b.y, 85, '#ffffff', 90, 520, 1.05);
        spawnParticles(b.x, b.y, 95, '#ff0000', 80, 480, 1.1);
      }

      // Final aftermath pause (3.30s + 5.0s = 8.30s total) so the player experiences the 5-second post-explosion pause before the Victory screen
      if (t >= 8.30) {
        triggerVictory();
      }
    }

    function handleBossDefeated() {
      const b = state.boss;
      if (b.isVoid && b.isDying) return;

      state.score += b.isVoid ? 10000 : state.level * 1000;

      state.enemyBullets = [];
      state.mines = [];
      state.drones = [];
      state.lasers = [];
      state.cracks = [];
      state.lightnings = [];
      state.teleports = [];
      state.healthPack = null;
      state.upgradePack = null;
      state.floatingTexts = [];

      if (b.isVoid) {
        startTesseractDeath(b);
      } else {
        state.shakeTime = 0.55;
        state.shakeMagnitude = 16;
        spawnExplosion(b.x, b.y, true);

        if (state.level >= 5) {
          triggerVictory();
        } else {
          state.level += 1;
          if (!state.hardMode) {
            state.player.hp = Math.min(state.player.maxHp, state.player.hp + GAME_CONFIG.SECTOR_CLEAR_HEAL);
          }
          spawnBossForLevel(state.level);
        }
      }
    }

    // Teleport the Tesseract to a new arena position and fire a laser at the player (ATK 06)
    function executeTesseractWarpLaser(b, p) {
      const oldX = b.x;
      const oldY = b.y;
      const dmgMult = b.voidDmgMult || 1.0;

      let wp = pickFullArenaWaypoint(125);
      for (let tries = 0; tries < 8; tries++) {
        const distFromOld = Math.hypot(wp.x - b.x, wp.y - b.y);
        const distFromPlayer = Math.hypot(wp.x - p.x, wp.y - p.y);
        if (distFromOld >= 210 && distFromPlayer >= 190) break;
        wp = pickFullArenaWaypoint(125);
      }

      b.x = wp.x;
      b.y = wp.y;
      clampBossToArena(b);
      b.warpAnimTimer = 0.38;

      spawnTeleportAnimation(oldX, oldY, b.x, b.y, b.radius, true);
      spawnShockwave(b.x, b.y, 155 + b.phase * 25, '#ff0000', 0.36);
      if (b.phase >= 3) {
        spawnShockwave(b.x, b.y, 115 + b.phase * 20, '#ffffff', 0.28);
      }
      sfx.playDash();
      sfx.playLaser();

      const aimAtPlayer = Math.atan2(p.y - b.y, p.x - b.x);
      const laserAngles = b.phase >= 4 ? [aimAtPlayer - 0.16, aimAtPlayer, aimAtPlayer + 0.16] : [aimAtPlayer];
      // Slower laser charge-up telegraph in Form 4 and Ult Form
      const warpTelegraph = b.isUlt
        ? 0.90
        : (b.phase === 4 ? 0.78 : Math.max(0.32, 0.46 - (b.phase - 1) * 0.04));
      laserAngles.forEach((lAng, idx) => {
        state.lasers.push({
          angle: lAng,
          spinRate: 0,
          telegraph: warpTelegraph,
          duration: 0.34 + (b.phase - 1) * 0.04,
          width: 16 + (b.phase - 1) * 4,
          damage: Math.round(18 * dmgMult),
          color: idx % 2 === 0 ? '#ff0000' : '#ffffff',
          voidPhase: b.phase
        });
      });

      if (b.phase >= 3) {
        const volleyCount = b.phase === 4 ? 10 : 6;
        const bDmg = Math.round(14 * dmgMult);
        for (let k = 0; k < volleyCount; k++) {
          const va = aimAtPlayer + (k * Math.PI * 2) / volleyCount;
          spawnEnemyBullet(
            b.x + Math.cos(va) * b.radius,
            b.y + Math.sin(va) * b.radius,
            va,
            GAME_CONFIG.BOSS_BULLET_SPEED * (0.95 + (b.phase - 3) * 0.15),
            6,
            k % 2 === 0 ? '#ff0000' : '#ffffff',
            false,
            null,
            bDmg
          );
        }
      }
    }

    // Spawn dimensional ships randomly selected from 4 distinct classes (ATK 05)
    function spawnVoidFleet(b, angleToPlayer) {
      sfx.playPhaseShift();
      spawnShockwave(b.x, b.y, 145 + b.phase * 25, '#ff0000', 0.35);
      spawnParticles(b.x, b.y, 22 + b.phase * 6, '#ffffff', 60, 240, 0.4);

      const shipTypes = ['teleporter', 'speeder', 'bomber', 'shooter'];
      const fleetCount = 5 + (b.phase - 1);
      const dmgMult = b.voidDmgMult || 1.0;

      for (let i = 0; i < fleetCount; i++) {
        const type = shipTypes[Math.floor(Math.random() * shipTypes.length)];
        const spawnAng = angleToPlayer + (i - (fleetCount - 1) / 2) * 0.46;
        const sx = Math.max(40, Math.min(GAME_CONFIG.ARENA_WIDTH - 40, b.x + Math.cos(spawnAng) * (b.radius + 26)));
        const sy = Math.max(90, Math.min(GAME_CONFIG.ARENA_HEIGHT - 40, b.y + Math.sin(spawnAng) * (b.radius + 26)));

        if (type === 'teleporter') {
          state.drones.push({
            type: 'teleporter',
            phase: b.phase,
            dmgMult,
            x: sx,
            y: sy,
            vx: 0,
            vy: 0,
            angle: spawnAng,
            radius: 13,
            hp: 42 + (b.phase - 1) * 10,
            maxHp: 42 + (b.phase - 1) * 10,
            teleportsLeft: 5 + (b.phase >= 3 ? 1 : 0),
            stateTimer: 0.35 + i * 0.14
          });
        } else if (type === 'speeder') {
          state.drones.push({
            type: 'speeder',
            subState: 'ORBIT_DASH',
            phase: b.phase,
            dmgMult,
            x: sx,
            y: sy,
            vx: 0,
            vy: 0,
            angle: spawnAng,
            radius: 12,
            hp: 36 + (b.phase - 1) * 8,
            maxHp: 36 + (b.phase - 1) * 8,
            orbitAngle: spawnAng + Math.PI,
            orbitDir: i % 2 === 0 ? 1 : -1,
            dashBurstTimer: 0.25,
            stateTimer: 2.1 + (i % 3) * 0.28,
            afterimages: []
          });
        } else if (type === 'bomber') {
          state.drones.push({
            type: 'bomber',
            phase: b.phase,
            dmgMult,
            x: sx,
            y: sy,
            vx: 0,
            vy: 0,
            angle: spawnAng,
            radius: 15,
            hp: 52 + (b.phase - 1) * 12,
            maxHp: 52 + (b.phase - 1) * 12,
            fuseTimer: 3.3 + (i % 2) * 0.35,
            maxFuse: 3.3 + (i % 2) * 0.35,
            blastRadius: 105
          });
        } else {
          state.drones.push({
            type: 'shooter',
            phase: b.phase,
            dmgMult,
            x: sx,
            y: sy,
            vx: 0,
            vy: 0,
            angle: spawnAng,
            radius: 13,
            hp: 46 + (b.phase - 1) * 10,
            maxHp: 46 + (b.phase - 1) * 10,
            strafeDir: i % 2 === 0 ? 1 : -1,
            fireTimer: 0.75 + i * 0.2,
            lifeTimer: 9.5
          });
        }
      }
    }

    // Build Colossal Straight-Line Omega Beam Segment(s) for THE TESSERACTS VOID (ATK 01)
    function getOmegaBeamSegments(b) {
      const angles = b.isUlt
        ? [b.omegaBeamAngle - 0.34, b.omegaBeamAngle, b.omegaBeamAngle + 0.34]
        : [b.omegaBeamAngle];
      const startDist = b.radius + 10;
      const beamWidth = b.isUlt ? 54 : (44 + (b.phase - 1) * 11);

      return angles.map((ang, idx) => {
        const dirX = Math.cos(ang);
        const dirY = Math.sin(ang);
        const x1 = b.x + dirX * startDist;
        const y1 = b.y + dirY * startDist;

        let tMax = 1550;
        if (dirX > 0.0001) tMax = Math.min(tMax, (GAME_CONFIG.ARENA_WIDTH - 12 - b.x) / dirX);
        else if (dirX < -0.0001) tMax = Math.min(tMax, (12 - b.x) / dirX);

        if (dirY > 0.0001) tMax = Math.min(tMax, (GAME_CONFIG.ARENA_HEIGHT - 12 - b.y) / dirY);
        else if (dirY < -0.0001) tMax = Math.min(tMax, (68 - b.y) / dirY);

        tMax = Math.max(startDist + 40, tMax);

        return {
          beamIndex: idx,
          angle: ang,
          x1,
          y1,
          x2: b.x + dirX * tMax,
          y2: b.y + dirY * tMax,
          width: beamWidth,
          length: tMax - startDist
        };
      });
    }

    function distPointToSegment(px, py, x1, y1, x2, y2) {
      const l2 = (x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1);
      if (l2 === 0) return Math.hypot(px - x1, py - y1);
      let t = ((px - x1) * (x2 - x1) + (py - y1) * (y2 - y1)) / l2;
      t = Math.max(0, Math.min(1, t));
      const projX = x1 + t * (x2 - x1);
      const projY = y1 + t * (y2 - y1);
      return Math.hypot(px - projX, py - projY);
    }

    // Spawn jagged dimensional fractures across the arena that persist for 5 seconds
    function spawnDimensionalCracks(b, aimAngle) {
      sfx.playLaser();
      state.shakeTime = 0.32 + b.phase * 0.05;
      state.shakeMagnitude = 9 + b.phase * 2;
      spawnShockwave(b.x, b.y, 190 + b.phase * 30, '#ff0000', 0.42);
      spawnShockwave(b.x, b.y, 130 + b.phase * 25, '#ffffff', 0.34);

      const crackCount = 5 + (b.phase - 1) * 2;
      const crackWidth = 8 + (b.phase - 1) * 2;
      const crackDmg = Math.round(14 * (b.voidDmgMult || 1.0));
      const growTime = 0.35;
      const activeDuration = 5.0;

      for (let c = 0; c < crackCount; c++) {
        const baseAng =
          aimAngle +
          (c * Math.PI * 2) / crackCount +
          (Math.random() - 0.5) * 0.28;

        const pts = [];
        let cx = b.x + Math.cos(baseAng) * (b.radius * 0.75);
        let cy = b.y + Math.sin(baseAng) * (b.radius * 0.75);
        pts.push({ x: cx, y: cy });

        let curAng = baseAng;
        const segments = 7;
        for (let s = 0; s < segments; s++) {
          const zig = (s % 2 === 0 ? 1 : -1) * (0.28 + Math.random() * 0.34);
          curAng = baseAng + zig;
          const stepLen = 82 + Math.random() * 44;
          const nx = cx + Math.cos(curAng) * stepLen;
          const ny = cy + Math.sin(curAng) * stepLen;

          const clampedX = Math.max(28, Math.min(GAME_CONFIG.ARENA_WIDTH - 28, nx));
          const clampedY = Math.max(80, Math.min(GAME_CONFIG.ARENA_HEIGHT - 28, ny));
          pts.push({ x: clampedX, y: clampedY });

          cx = clampedX;
          cy = clampedY;

          if (nx !== clampedX || ny !== clampedY) break;
        }

        if (pts.length >= 2) {
          state.cracks.push({
            points: pts,
            life: growTime + activeDuration,
            maxLife: growTime + activeDuration,
            growTime,
            width: crackWidth,
            damage: crackDmg,
            phase: b.phase
          });

          const branchFreq = b.phase >= 3 ? 1 : 2;
          if (c % branchFreq === 0 && pts.length >= 4) {
            const branchStart = pts[2];
            const branchAng = baseAng + (c % 4 === 0 ? 0.75 : -0.75);
            const b1 = {
              x: Math.max(28, Math.min(GAME_CONFIG.ARENA_WIDTH - 28, branchStart.x + Math.cos(branchAng) * 90)),
              y: Math.max(80, Math.min(GAME_CONFIG.ARENA_HEIGHT - 28, branchStart.y + Math.sin(branchAng) * 90))
            };
            const b2 = {
              x: Math.max(28, Math.min(GAME_CONFIG.ARENA_WIDTH - 28, b1.x + Math.cos(branchAng - 0.35) * 85)),
              y: Math.max(80, Math.min(GAME_CONFIG.ARENA_HEIGHT - 28, b1.y + Math.sin(branchAng - 0.35) * 85))
            };
            state.cracks.push({
              points: [branchStart, b1, b2],
              life: growTime + activeDuration,
              maxLife: growTime + activeDuration,
              growTime,
              width: Math.max(7, crackWidth - 1),
              damage: crackDmg,
              phase: b.phase
            });
          }
        }
      }
    }

    function updateCracks(dt) {
      const p = state.player;
      for (let i = state.cracks.length - 1; i >= 0; i--) {
        const cr = state.cracks[i];
        cr.life -= dt;
        if (cr.life <= 0) {
          state.cracks.splice(i, 1);
          continue;
        }

        const elapsed = cr.maxLife - cr.life;
        if (elapsed >= cr.growTime && state.screen === 'PLAYING') {
          if (Math.random() < 0.14 + (cr.phase || 1) * 0.04) {
            const v = cr.points[Math.floor(Math.random() * cr.points.length)];
            spawnParticles(v.x, v.y, 1, Math.random() < 0.5 ? '#ff0000' : '#ffffff', 25, 115, 0.25);
          }

          for (let s = 0; s < cr.points.length - 1; s++) {
            const p1 = cr.points[s];
            const p2 = cr.points[s + 1];
            if (distPointToSegment(p.x, p.y, p1.x, p1.y, p2.x, p2.y) <= cr.width * 0.5 + p.radius - 2) {
              damagePlayer(cr.damage || 14);
              if (state.screen !== 'PLAYING') return;
              break;
            }
          }
        }
      }
    }

    // Trigger small core explosion that launches powerful wall-breaking lightning bolts
    function spawnArcLightningBurst(b, aimAngle) {
      sfx.playExplosion(false);
      sfx.playLaser();
      state.shakeTime = 0.26 + b.phase * 0.05;
      state.shakeMagnitude = 8 + b.phase * 2;
      const dmgMult = b.voidDmgMult || 1.0;

      spawnShockwave(b.x, b.y, 130 + b.phase * 25, '#ffffff', 0.36);
      spawnShockwave(b.x, b.y, 95 + b.phase * 20, '#ff0000', 0.3);
      spawnParticles(b.x, b.y, 22 + b.phase * 6, '#ffffff', 75, 310, 0.44);
      spawnParticles(b.x, b.y, 24 + b.phase * 6, '#ff0000', 65, 280, 0.48);

      // Fire fewer primary lightning bolts in Form 4 (6) and Ult Form (8)
      const boltCount = b.isUlt
        ? 8
        : (b.phase === 4 ? 6 : 6 + (b.phase - 1) * 2);
      const boltSpeed = 495 + (b.phase - 1) * 42;
      const boltDmg = Math.round(16 * dmgMult);

      for (let i = 0; i < boltCount; i++) {
        const ang = aimAngle + (i * Math.PI * 2) / boltCount;
        const sx = b.x + Math.cos(ang) * (b.radius + 6);
        const sy = b.y + Math.sin(ang) * (b.radius + 6);
        state.lightnings.push({
          x: sx,
          y: sy,
          vx: Math.cos(ang) * boltSpeed,
          vy: Math.sin(ang) * boltSpeed,
          isPrimary: true,
          phase: b.phase,
          dmgMult,
          radius: 7 + (b.phase - 1) * 0.8,
          damage: boltDmg,
          homingRate: 0,
          life: 4.5,
          trail: [{ x: sx, y: sy }],
          seed: Math.random() * 100
        });
      }
    }

    function updateLightnings(dt) {
      const p = state.player;
      const minX = 16;
      const maxX = GAME_CONFIG.ARENA_WIDTH - 16;
      const minY = 72;
      const maxY = GAME_CONFIG.ARENA_HEIGHT - 16;

      for (let i = state.lightnings.length - 1; i >= 0; i--) {
        const lt = state.lightnings[i];
        lt.life -= dt;
        if (lt.life <= 0) {
          state.lightnings.splice(i, 1);
          continue;
        }

        if (!lt.isPrimary && lt.homingRate > 0) {
          const targetAng = Math.atan2(p.y - lt.y, p.x - lt.x);
          const curAng = Math.atan2(lt.vy, lt.vx);
          const spd = Math.hypot(lt.vx, lt.vy);
          const nextAng = rotateAngleTowards(curAng, targetAng, lt.homingRate * dt);
          lt.vx = Math.cos(nextAng) * spd;
          lt.vy = Math.sin(nextAng) * spd;
        }

        lt.x += lt.vx * dt;
        lt.y += lt.vy * dt;

        lt.trail.push({ x: lt.x, y: lt.y });
        if (lt.trail.length > (lt.isPrimary ? 7 : 5)) {
          lt.trail.shift();
        }

        if (state.screen === 'PLAYING' && Math.hypot(lt.x - p.x, lt.y - p.y) < lt.radius + p.radius - 2) {
          spawnParticles(lt.x, lt.y, 12, '#ffffff', 40, 180, 0.28);
          state.lightnings.splice(i, 1);
          damagePlayer(lt.damage);
          if (state.screen !== 'PLAYING') return;
          continue;
        }

        let hitWall = false;
        let nx = 0;
        let ny = 0;
        if (lt.x <= minX) {
          lt.x = minX;
          nx += 1;
          hitWall = true;
        } else if (lt.x >= maxX) {
          lt.x = maxX;
          nx -= 1;
          hitWall = true;
        }
        if (lt.y <= minY) {
          lt.y = minY;
          ny += 1;
          hitWall = true;
        } else if (lt.y >= maxY) {
          lt.y = maxY;
          ny -= 1;
          hitWall = true;
        }

        if (hitWall) {
          if (lt.isPrimary) {
            const ltPhase = lt.phase || 1;
            const ltDmgMult = lt.dmgMult || 1.0;
            sfx.playHit();
            spawnShockwave(lt.x, lt.y, 52 + ltPhase * 10, '#ff0000', 0.24);
            spawnParticles(lt.x, lt.y, 10 + ltPhase * 2, '#ffffff', 45, 200, 0.3);
            spawnParticles(lt.x, lt.y, 8 + ltPhase * 2, '#ff0000', 40, 180, 0.32);

            const nLen = Math.hypot(nx, ny) || 1;
            nx /= nLen;
            ny /= nLen;

            const dot = lt.vx * nx + lt.vy * ny;
            const rvx = lt.vx - 2 * dot * nx;
            const rvy = lt.vy - 2 * dot * ny;
            const reflectAng = Math.atan2(rvy, rvx);
            const playerAng = Math.atan2(p.y - lt.y, p.x - lt.x);
            let baseCenterAng = rotateAngleTowards(reflectAng, playerAng, 0.45);

            if (Math.cos(baseCenterAng) * nx + Math.sin(baseCenterAng) * ny < 0.28) {
              baseCenterAng = Math.atan2(ny, nx);
            }

            const childSpeed = 370 + (ltPhase - 1) * 30;
            const childDmg = Math.round(11 * ltDmgMult);
            // Fire fewer wall-shatter split lightning bolts in Form 4 (2) and Ult Form (3)
            const splitSpreads = ltPhase >= 5
              ? [-0.34, 0, 0.34]
              : (ltPhase === 4
                  ? [-0.26, 0.26]
                  : (ltPhase === 3 ? [-0.45, -0.15, 0.15, 0.45] : [-0.36, 0, 0.36]));
            splitSpreads.forEach((spread) => {
              let childAng = baseCenterAng + spread;
              if (Math.cos(childAng) * nx + Math.sin(childAng) * ny < 0.15) {
                childAng = Math.atan2(ny, nx) + spread * 0.4;
              }
              state.lightnings.push({
                x: lt.x + nx * 6,
                y: lt.y + ny * 6,
                vx: Math.cos(childAng) * childSpeed,
                vy: Math.sin(childAng) * childSpeed,
                isPrimary: false,
                phase: ltPhase,
                dmgMult: ltDmgMult,
                radius: 4.5 + (ltPhase - 1) * 0.4,
                damage: childDmg,
                homingRate: 1.55 + (ltPhase - 1) * 0.22,
                life: 3.3,
                trail: [{ x: lt.x, y: lt.y }],
                seed: Math.random() * 100
              });
            });
          } else {
            spawnParticles(lt.x, lt.y, 5, '#ffffff', 30, 120, 0.2);
          }

          state.lightnings.splice(i, 1);
        }
      }
    }

    function updateLasers(dt) {
      const b = state.boss;
      const p = state.player;
      if (!b) return;

      for (let i = state.lasers.length - 1; i >= 0; i--) {
        const l = state.lasers[i];
        if (typeof l.vertexOffset === 'number') {
          l.angle = b.angle + l.vertexOffset;
        } else {
          l.angle += l.spinRate * dt;
        }

        if (l.telegraph > 0) {
          l.telegraph -= dt;
        } else {
          l.duration -= dt;
          const startDist = typeof l.vertexOffset === 'number' ? b.radius : 0;
          const sx = b.x + Math.cos(l.angle) * startDist;
          const sy = b.y + Math.sin(l.angle) * startDist;
          const ex = b.x + Math.cos(l.angle) * 1500;
          const ey = b.y + Math.sin(l.angle) * 1500;
          const dist = distPointToSegment(p.x, p.y, sx, sy, ex, ey);
          if (dist < l.width * 0.5 + p.radius - 2) {
            damagePlayer(l.damage || 20);
          }
          if (l.duration <= 0) {
            state.lasers.splice(i, 1);
          }
        }
      }
    }

    /* ============================================================================
       13. HIGH-TECH VECTOR RENDERING ENGINE (#000000, #ff0000, #ffffff + RGB L5)
       ============================================================================ */
    function drawPolygon(context, sides, radius) {
      context.beginPath();
      for (let i = 0; i < sides; i++) {
        const a = (i * Math.PI * 2) / sides - Math.PI / 2;
        const px = Math.cos(a) * radius;
        const py = Math.sin(a) * radius;
        if (i === 0) context.moveTo(px, py);
        else context.lineTo(px, py);
      }
      context.closePath();
      context.stroke();
    }

    function drawArenaGrid() {
      const w = GAME_CONFIG.ARENA_WIDTH;
      const h = GAME_CONFIG.ARENA_HEIGHT;

      ctx.fillStyle = '#000000';
      ctx.fillRect(0, 0, w, h);

      ctx.strokeStyle = 'rgba(255, 255, 255, 0.05)';
      ctx.lineWidth = 1;
      const step = 60;
      ctx.beginPath();
      for (let x = step; x < w; x += step) {
        ctx.moveTo(x, 60);
        ctx.lineTo(x, h);
      }
      for (let y = 60; y < h; y += step) {
        ctx.moveTo(0, y);
        ctx.lineTo(w, y);
      }
      ctx.stroke();

      ctx.strokeStyle = 'rgba(255, 0, 0, 0.09)';
      ctx.beginPath();
      ctx.arc(w / 2, h / 2, 160, 0, Math.PI * 2);
      ctx.arc(w / 2, h / 2, 310, 0, Math.PI * 2);
      ctx.stroke();

      ctx.strokeStyle = 'rgba(255, 255, 255, 0.35)';
      ctx.lineWidth = 1.5;
      ctx.strokeRect(10, 66, w - 20, h - 76);

      const bLen = 18;
      ctx.strokeStyle = '#ff0000';
      ctx.lineWidth = 2.5;
      ctx.beginPath();
      ctx.moveTo(10, 66 + bLen); ctx.lineTo(10, 66); ctx.lineTo(10 + bLen, 66);
      ctx.moveTo(w - 10 - bLen, 66); ctx.lineTo(w - 10, 66); ctx.lineTo(w - 10, 66 + bLen);
      ctx.moveTo(10, h - 10 - bLen); ctx.lineTo(10, h - 10); ctx.lineTo(10 + bLen, h - 10);
      ctx.moveTo(w - 10 - bLen, h - 10); ctx.lineTo(w - 10, h - 10); ctx.lineTo(w - 10, h - 10 - bLen);
      ctx.stroke();
    }

    // Progressive Background Reality-Distortion Layer for THE TESSERACTS VOID (Phases 1 -> 4)
    function drawVoidRealityBG() {
      const b = state.boss;
      if (!b || !b.isVoid) return;

      const w = GAME_CONFIG.ARENA_WIDTH;
      const h = GAME_CONFIG.ARENA_HEIGHT;
      const t = state.elapsedTime;
      const ph = b.phase;

      ctx.save();

      // 1. Expanding Event Horizon Pulse Waves radiating across the arena background
      const waveCount = ph + 1;
      for (let i = 0; i < waveCount; i++) {
        const cycle = (t * (0.28 + ph * 0.08) + i / waveCount) % 1;
        const rad = b.radius + cycle * (520 + ph * 110);
        ctx.strokeStyle = i % 2 === 0
          ? `rgba(255, 0, 0, ${(1 - cycle) * (0.14 + ph * 0.05)})`
          : `rgba(255, 255, 255, ${(1 - cycle) * (0.06 + ph * 0.025)})`;
        ctx.lineWidth = 1 + ph * 0.35;
        ctx.beginPath();
        ctx.arc(b.x, b.y, rad, 0, Math.PI * 2);
        ctx.stroke();
      }

      // 2. Phase 2+ (<= 75% HP): Gravitational Lensing Warp Lines curving into the Tesseract
      if (ph >= 2) {
        const rayCount = 8 + ph * 4;
        ctx.strokeStyle = `rgba(255, 0, 0, ${0.08 + ph * 0.04})`;
        ctx.lineWidth = 1.1;
        for (let r = 0; r < rayCount; r++) {
          const baseAng = (r * Math.PI * 2) / rayCount + t * (0.12 * ph);
          const outerR = 760;
          const midR = 320;
          const bend = Math.sin(t * 2 + r) * (0.12 * ph);
          ctx.beginPath();
          ctx.moveTo(b.x + Math.cos(baseAng) * outerR, b.y + Math.sin(baseAng) * outerR);
          ctx.quadraticCurveTo(
            b.x + Math.cos(baseAng + bend) * midR,
            b.y + Math.sin(baseAng + bend) * midR,
            b.x + Math.cos(baseAng) * (b.radius + 12),
            b.y + Math.sin(baseAng) * (b.radius + 12)
          );
          ctx.stroke();
        }
      }

      // 3. Phase 2+ Floating Dimensional Wireframe Shards drifting in deep space
      if (ph >= 2 && b.voidBgShards) {
        const activeShards = Math.min(b.voidBgShards.length, ph * 6);
        for (let s = 0; s < activeShards; s++) {
          const sh = b.voidBgShards[s];
          const dx = b.x - sh.x;
          const dy = b.y - sh.y;
          const dist = Math.max(80, Math.hypot(dx, dy));
          sh.x += (dx / dist) * sh.orbitSpeed * (0.4 + ph * 0.25) * 0.016;
          sh.y += (dy / dist) * sh.orbitSpeed * (0.4 + ph * 0.25) * 0.016;
          sh.angle += sh.spin * 0.016;
          if (dist < b.radius + 35) {
            const edgeAng = Math.random() * Math.PI * 2;
            sh.x = b.x + Math.cos(edgeAng) * 640;
            sh.y = b.y + Math.sin(edgeAng) * 440;
          }

          ctx.save();
          ctx.translate(sh.x, sh.y);
          ctx.rotate(sh.angle);
          ctx.strokeStyle = s % 2 === 0
            ? `rgba(255, 0, 0, ${0.14 + ph * 0.05})`
            : `rgba(255, 255, 255, ${0.10 + ph * 0.04})`;
          ctx.lineWidth = 1.2;
          ctx.strokeRect(-sh.size * 0.5, -sh.size * 0.5, sh.size, sh.size);
          if (ph >= 3) {
            ctx.rotate(Math.PI / 4);
            ctx.strokeRect(-sh.size * 0.3, -sh.size * 0.3, sh.size * 0.6, sh.size * 0.6);
          }
          ctx.restore();
        }
      }

      // 4. Phase 3+ (<= 50% HP): Giant Rotating 4D Geometric Singularity Mandala in Background
      if (ph >= 3) {
        ctx.save();
        ctx.translate(b.x, b.y);
        ctx.rotate(-t * 0.22);
        ctx.strokeStyle = ph === 4 ? 'rgba(255, 0, 0, 0.24)' : 'rgba(255, 0, 0, 0.14)';
        ctx.lineWidth = ph === 4 ? 1.8 : 1.2;
        const mandalaR = ph === 4 ? 340 : 280;
        drawPolygon(ctx, 4, mandalaR);
        ctx.rotate(Math.PI / 4);
        ctx.strokeStyle = ph === 4 ? 'rgba(255, 255, 255, 0.16)' : 'rgba(255, 255, 255, 0.09)';
        drawPolygon(ctx, 4, mandalaR);
        ctx.restore();

        const borderPulse = 0.25 + Math.abs(Math.sin(t * (2.5 + ph))) * (ph === 4 ? 0.55 : 0.3);
        ctx.strokeStyle = `rgba(255, 0, 0, ${borderPulse})`;
        ctx.lineWidth = ph === 4 ? 3 : 2;
        ctx.strokeRect(10, 66, w - 20, h - 76);
      }

      // 5. Phase 4 (<= 25% HP): High-Speed Inward Accretion Streaks across the Entire Background
      if (ph >= 4) {
        const streamCount = 28;
        for (let k = 0; k < streamCount; k++) {
          const prog = (t * 0.95 + k / streamCount) % 1;
          const sAng = (k * 2.39996) + t * 0.45;
          const rStart = 720 * (1 - prog) + b.radius;
          const rEnd = Math.max(b.radius, rStart - 55);
          ctx.strokeStyle = k % 3 === 0 ? 'rgba(255, 255, 255, 0.26)' : 'rgba(255, 0, 0, 0.34)';
          ctx.lineWidth = 1.6;
          ctx.beginPath();
          ctx.moveTo(b.x + Math.cos(sAng) * rStart, b.y + Math.sin(sAng) * rStart);
          ctx.lineTo(b.x + Math.cos(sAng) * rEnd, b.y + Math.sin(sAng) * rEnd);
          ctx.stroke();
        }
      }

      // 6. Ult Form (<= 10% HP awakened): Colossal 4D Hypercube Reality Cage in Background
      if (b.isUlt) {
        ctx.save();
        ctx.translate(b.x, b.y);
        const outerHyper = 410 + Math.sin(t * 3.2) * 18;
        const innerHyper = 230 + Math.cos(t * 4.1) * 14;
        const aOut = t * 0.35;
        const aIn = -t * 0.55;

        const oPts = [];
        const iPts = [];
        for (let i = 0; i < 4; i++) {
          const ao = aOut + (i * Math.PI) / 2;
          const ai = aIn + (i * Math.PI) / 2;
          oPts.push({ x: Math.cos(ao) * outerHyper, y: Math.sin(ao) * outerHyper });
          iPts.push({ x: Math.cos(ai) * innerHyper, y: Math.sin(ai) * innerHyper });
        }

        ctx.strokeStyle = 'rgba(255, 255, 255, 0.22)';
        ctx.lineWidth = 1.4;
        ctx.beginPath();
        for (let i = 0; i < 4; i++) {
          ctx.moveTo(oPts[i].x, oPts[i].y);
          ctx.lineTo(iPts[i].x, iPts[i].y);
        }
        ctx.stroke();

        ctx.strokeStyle = 'rgba(255, 0, 0, 0.32)';
        ctx.lineWidth = 2.2;
        ctx.beginPath();
        for (let i = 0; i < 4; i++) {
          const nxt = (i + 1) % 4;
          ctx.moveTo(oPts[i].x, oPts[i].y);
          ctx.lineTo(oPts[nxt].x, oPts[nxt].y);
          ctx.moveTo(iPts[i].x, iPts[i].y);
          ctx.lineTo(iPts[nxt].x, iPts[nxt].y);
        }
        ctx.stroke();
        ctx.restore();
      }

      ctx.restore();
    }

    function drawLasers() {
      const b = state.boss;
      if (!b) return;

      // 1) Draw Colossal Straight-Line Omega Beam(s) (THE TESSERACTS VOID Attack 01 — Phase Scaled 1->5/ULT)
      if (b.isVoid && b.omegaBeamState !== 'NONE') {
        const segs = getOmegaBeamSegments(b);
        for (const s of segs) {
          const ang = typeof s.angle === 'number' ? s.angle : b.omegaBeamAngle;
          const dirX = Math.cos(ang);
          const dirY = Math.sin(ang);
          const perpX = -dirY;
          const perpY = dirX;
          const ph = b.phase;
          const halfW = s.width * 0.5;
          const beamLen = s.length || Math.hypot(s.x2 - s.x1, s.y2 - s.y1);
          const t = state.elapsedTime;

          ctx.save();
          if (b.omegaBeamState === 'TELEGRAPH') {
            const isLocked = b.omegaBeamTimer <= 0.22;
            const pulse = 0.5 + 0.5 * Math.sin(t * 28);

            ctx.fillStyle = isLocked ? 'rgba(255, 0, 0, 0.24)' : 'rgba(255, 0, 0, 0.10)';
            ctx.beginPath();
            ctx.moveTo(s.x1 + perpX * halfW, s.y1 + perpY * halfW);
            ctx.lineTo(s.x2 + perpX * halfW, s.y2 + perpY * halfW);
            ctx.lineTo(s.x2 - perpX * halfW, s.y2 - perpY * halfW);
            ctx.lineTo(s.x1 - perpX * halfW, s.y1 - perpY * halfW);
            ctx.closePath();
            ctx.fill();

            ctx.strokeStyle = isLocked ? '#ffffff' : '#ff0000';
            ctx.lineWidth = isLocked ? 2.5 : 1.5;
            ctx.setLineDash([14, 10]);
            ctx.lineDashOffset = -t * 140;
            [-1, 1].forEach((side) => {
              ctx.beginPath();
              ctx.moveTo(s.x1 + perpX * halfW * side, s.y1 + perpY * halfW * side);
              ctx.lineTo(s.x2 + perpX * halfW * side, s.y2 + perpY * halfW * side);
              ctx.stroke();
            });
            ctx.setLineDash([]);

            ctx.strokeStyle = isLocked ? '#ffffff' : `rgba(255, 255, 255, ${0.4 + pulse * 0.5})`;
            ctx.lineWidth = isLocked ? 3.5 : 1.5;
            ctx.beginPath();
            ctx.moveTo(s.x1, s.y1);
            ctx.lineTo(s.x2, s.y2);
            ctx.stroke();

            if (ph >= 2) {
              const gateCount = ph + 1;
              for (let g = 1; g <= gateCount; g++) {
                const dist = (beamLen * g) / (gateCount + 1);
                const gx = s.x1 + dirX * dist;
                const gy = s.y1 + dirY * dist;
                ctx.save();
                ctx.translate(gx, gy);
                ctx.rotate(ang);
                ctx.strokeStyle = g % 2 === 0 ? '#ffffff' : '#ff0000';
                ctx.lineWidth = 1.6;
                ctx.strokeRect(-6, -halfW * 0.85, 12, halfW * 1.7);
                ctx.restore();
              }
            }
          } else if (b.omegaBeamState === 'FIRING') {
            const flicker = Math.sin(t * 58) * (3 + ph);
            const outerW = s.width + 18 + ph * 4 + flicker;
            const redW = s.width + flicker * 0.6;
            const blackSheathW = s.width * 0.52;
            const whiteCoreW = 14 + ph * 3.5 + Math.cos(t * 64) * 2.5;

            ctx.lineCap = 'butt';
            ctx.strokeStyle = `rgba(255, 0, 0, ${0.26 + ph * 0.04})`;
            ctx.lineWidth = outerW + 18 + ph * 6;
            ctx.beginPath();
            ctx.moveTo(s.x1, s.y1);
            ctx.lineTo(s.x2, s.y2);
            ctx.stroke();

            ctx.shadowColor = '#ff0000';
            ctx.shadowBlur = 30 + ph * 6;
            ctx.strokeStyle = '#ff0000';
            ctx.lineWidth = redW;
            ctx.beginPath();
            ctx.moveTo(s.x1, s.y1);
            ctx.lineTo(s.x2, s.y2);
            ctx.stroke();
            ctx.shadowBlur = 0;

            ctx.strokeStyle = '#000000';
            ctx.lineWidth = blackSheathW;
            ctx.beginPath();
            ctx.moveTo(s.x1, s.y1);
            ctx.lineTo(s.x2, s.y2);
            ctx.stroke();

            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = whiteCoreW;
            ctx.beginPath();
            ctx.moveTo(s.x1, s.y1);
            ctx.lineTo(s.x2, s.y2);
            ctx.stroke();

            const railOffset = redW * 0.5 + 8 + ph * 2;
            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = 1.8 + (ph >= 3 ? 0.8 : 0);
            [-1, 1].forEach((side) => {
              ctx.beginPath();
              ctx.moveTo(s.x1 + perpX * railOffset * side, s.y1 + perpY * railOffset * side);
              ctx.lineTo(s.x2 + perpX * railOffset * side, s.y2 + perpY * railOffset * side);
              ctx.stroke();
            });

            if (ph >= 2) {
              const ringSpacing = 140;
              const offset = (t * (380 + ph * 70)) % ringSpacing;
              for (let d = offset; d < beamLen; d += ringSpacing) {
                const rx = s.x1 + dirX * d;
                const ry = s.y1 + dirY * d;
                ctx.save();
                ctx.translate(rx, ry);
                ctx.rotate(ang);
                ctx.strokeStyle = '#ffffff';
                ctx.lineWidth = 2 + (ph >= 4 ? 0.8 : 0);
                ctx.beginPath();
                ctx.ellipse(0, 0, 8 + ph * 2, railOffset + 5, 0, 0, Math.PI * 2);
                ctx.stroke();
                ctx.restore();
              }
            }

            const steps = Math.max(8, Math.floor(beamLen / 46));
            const coilCount = ph >= 4 ? 4 : (ph >= 2 ? 2 : 1);
            for (let c = 0; c < coilCount; c++) {
              const phaseShift = (c * Math.PI * 2) / coilCount;
              ctx.strokeStyle = c % 2 === 0 ? '#ffffff' : '#ff0000';
              ctx.lineWidth = c === 0 ? 2.4 : 1.8;
              ctx.beginPath();
              for (let k = 0; k <= steps; k++) {
                const dist = (beamLen * k) / steps;
                const wave =
                  Math.sin(dist * (0.045 + c * 0.012) - t * (32 + c * 8) + phaseShift) *
                  (redW * (0.58 + ph * 0.05));
                const lx = s.x1 + dirX * dist + perpX * wave;
                const ly = s.y1 + dirY * dist + perpY * wave;
                if (k === 0) ctx.moveTo(lx, ly);
                else ctx.lineTo(lx, ly);
              }
              ctx.stroke();
            }

            if (ph >= 3) {
              const ventSpacing = ph === 4 ? 95 : 135;
              const vOffset = (t * 440) % ventSpacing;
              for (let d = vOffset; d < beamLen; d += ventSpacing) {
                const vx = s.x1 + dirX * d;
                const vy = s.y1 + dirY * d;
                [-1, 1].forEach((side) => {
                  const spurLen = 22 + ph * 7 + Math.sin(t * 30 + d) * 6;
                  const sAng = ang + side * (Math.PI * 0.34);
                  ctx.strokeStyle = side === 1 ? '#ff0000' : '#ffffff';
                  ctx.lineWidth = 2;
                  ctx.beginPath();
                  ctx.moveTo(vx + perpX * railOffset * side, vy + perpY * railOffset * side);
                  ctx.lineTo(
                    vx + perpX * railOffset * side + Math.cos(sAng) * spurLen,
                    vy + perpY * railOffset * side + Math.sin(sAng) * spurLen
                  );
                  ctx.stroke();
                });
              }
            }

            const chevSpacing = 120;
            const scroll = (t * (540 + ph * 80)) % chevSpacing;
            ctx.strokeStyle = '#000000';
            ctx.lineWidth = 3;
            for (let d = scroll; d < beamLen; d += chevSpacing) {
              const cx = s.x1 + dirX * d;
              const cy = s.y1 + dirY * d;
              const cSize = whiteCoreW * 0.65;
              ctx.beginPath();
              ctx.moveTo(cx - dirX * cSize + perpX * cSize, cy - dirY * cSize + perpY * cSize);
              ctx.lineTo(cx + dirX * cSize, cy + dirY * cSize);
              ctx.lineTo(cx - dirX * cSize - perpX * cSize, cy - dirY * cSize - perpY * cSize);
              ctx.stroke();
            }

            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(s.x1, s.y1, redW * 0.65, 0, Math.PI * 2);
            ctx.fill();
            ctx.strokeStyle = '#ff0000';
            ctx.lineWidth = 3.5;
            ctx.beginPath();
            ctx.arc(s.x1, s.y1, redW * 0.85 + Math.sin(t * 45) * 4, 0, Math.PI * 2);
            ctx.stroke();

            if (ph >= 2) {
              ctx.save();
              ctx.translate(s.x1, s.y1);
              ctx.rotate(t * 6);
              ctx.strokeStyle = '#ffffff';
              ctx.lineWidth = 2;
              drawPolygon(ctx, 4, redW * 1.15);
              ctx.restore();
            }

            ctx.fillStyle = '#ff0000';
            ctx.beginPath();
            ctx.arc(s.x2, s.y2, redW * 0.9, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(s.x2, s.y2, whiteCoreW * 0.95, 0, Math.PI * 2);
            ctx.fill();

            if (ph >= 3) {
              ctx.strokeStyle = '#ffffff';
              ctx.lineWidth = 2;
              ctx.beginPath();
              ctx.arc(s.x2, s.y2, redW * 1.3 + ((t * 160) % 24), 0, Math.PI * 2);
              ctx.stroke();
            }
          }
          ctx.restore();
        }
      }

      // 2) Draw Standard, Phase-Scaled Void, & RGB Sweeping Beams
      for (const l of state.lasers) {
        const startDist = typeof l.vertexOffset === 'number' ? b.radius : 0;
        const sx = b.x + Math.cos(l.angle) * startDist;
        const sy = b.y + Math.sin(l.angle) * startDist;
        const ex = b.x + Math.cos(l.angle) * 1500;
        const ey = b.y + Math.sin(l.angle) * 1500;
        const dirX = Math.cos(l.angle);
        const dirY = Math.sin(l.angle);
        const perpX = -dirY;
        const perpY = dirX;
        const vPhase = l.voidPhase || 1;
        const isRgbBeam = typeof l.vertexIndex === 'number';
        const beamHue = isRgbBeam ? (b.rgbHue + l.vertexIndex * 120) % 360 : 0;
        const beamColor = isRgbBeam ? `hsl(${beamHue}, 100%, 54%)` : (l.color || '#ff0000');

        ctx.save();
        if (l.telegraph > 0) {
          ctx.strokeStyle = beamColor;
          ctx.lineWidth = 2 + (vPhase - 1) * 0.4;
          ctx.setLineDash([12, 8]);
          ctx.beginPath();
          ctx.moveTo(sx, sy);
          ctx.lineTo(ex, ey);
          ctx.stroke();

          if (b.isVoid && vPhase >= 2) {
            const halfW = l.width * 0.5;
            ctx.strokeStyle = 'rgba(255, 255, 255, 0.35)';
            ctx.lineWidth = 1;
            [-1, 1].forEach((side) => {
              ctx.beginPath();
              ctx.moveTo(sx + perpX * halfW * side, sy + perpY * halfW * side);
              ctx.lineTo(ex + perpX * halfW * side, ey + perpY * halfW * side);
              ctx.stroke();
            });
          }
        } else {
          ctx.shadowColor = beamColor;
          ctx.shadowBlur = 18 + vPhase * 4;
          ctx.strokeStyle = beamColor;
          ctx.lineWidth = l.width;
          ctx.beginPath();
          ctx.moveTo(sx, sy);
          ctx.lineTo(ex, ey);
          ctx.stroke();

          ctx.shadowBlur = 0;
          ctx.strokeStyle = '#ffffff';
          ctx.lineWidth = 4 + (vPhase - 1) * 1.2;
          ctx.beginPath();
          ctx.moveTo(sx, sy);
          ctx.lineTo(ex, ey);
          ctx.stroke();

          if (b.isVoid && vPhase >= 2) {
            const railOff = l.width * 0.5 + 5;
            ctx.strokeStyle = beamColor === '#ff0000' ? '#ffffff' : '#ff0000';
            ctx.lineWidth = 1.3;
            [-1, 1].forEach((side) => {
              ctx.beginPath();
              ctx.moveTo(sx + perpX * railOff * side, sy + perpY * railOff * side);
              ctx.lineTo(ex + perpX * railOff * side, ey + perpY * railOff * side);
              ctx.stroke();
            });
          }

          if (b.isVoid && vPhase >= 3) {
            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = 1.6;
            ctx.beginPath();
            const segs = 26;
            for (let k = 0; k <= segs; k++) {
              const dist = (1400 * k) / segs;
              const wave = Math.sin(dist * 0.05 - state.elapsedTime * 34) * (l.width * 0.65);
              const px = sx + dirX * dist + perpX * wave;
              const py = sy + dirY * dist + perpY * wave;
              if (k === 0) ctx.moveTo(px, py);
              else ctx.lineTo(px, py);
            }
            ctx.stroke();
          }

          if (b.isVoid && vPhase >= 4) {
            const step = 130;
            const scroll = (state.elapsedTime * 520) % step;
            ctx.strokeStyle = '#000000';
            ctx.lineWidth = 2.2;
            for (let d = scroll; d < 1400; d += step) {
              const cx = sx + dirX * d;
              const cy = sy + dirY * d;
              const sz = 6;
              ctx.beginPath();
              ctx.moveTo(cx - dirX * sz + perpX * sz, cy - dirY * sz + perpY * sz);
              ctx.lineTo(cx + dirX * sz, cy + dirY * sz);
              ctx.lineTo(cx - dirX * sz - perpX * sz, cy - dirY * sz - perpY * sz);
              ctx.stroke();
            }
          }

          if (isRgbBeam) {
            ctx.fillStyle = beamColor;
            ctx.beginPath();
            ctx.arc(sx, sy, 6, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(sx, sy, 2.8, 0, Math.PI * 2);
            ctx.fill();
          }
        }
        ctx.restore();
      }
    }

    function drawCracks() {
      for (const cr of state.cracks) {
        const elapsed = cr.maxLife - cr.life;
        const growRatio = Math.min(1, elapsed / cr.growTime);
        const fadeRatio = cr.life < 0.7 ? Math.max(0, cr.life / 0.7) : 1;
        const cPhase = cr.phase || 1;
        const tremor = cPhase >= 4 && growRatio >= 1 ? (Math.random() - 0.5) * 3.5 : 0;

        const totalSegs = cr.points.length - 1;
        const exactSegs = totalSegs * growRatio;
        const fullSegs = Math.floor(exactSegs);
        const partial = exactSegs - fullSegs;

        const drawPts = [];
        for (let i = 0; i <= Math.min(fullSegs, totalSegs); i++) {
          drawPts.push({
            x: cr.points[i].x + (i > 0 ? tremor : 0),
            y: cr.points[i].y + (i > 0 ? tremor : 0)
          });
        }
        if (fullSegs < totalSegs && partial > 0) {
          const pA = cr.points[fullSegs];
          const pB = cr.points[fullSegs + 1];
          drawPts.push({
            x: pA.x + (pB.x - pA.x) * partial + tremor,
            y: pA.y + (pB.y - pA.y) * partial + tremor
          });
        }

        if (drawPts.length < 2) continue;

        ctx.save();
        ctx.globalAlpha = fadeRatio;
        ctx.lineCap = 'round';
        ctx.lineJoin = 'miter';

        if (cPhase >= 2) {
          const echoOffset = 8 + cPhase * 2;
          ctx.strokeStyle = 'rgba(255, 0, 0, 0.42)';
          ctx.lineWidth = 1.6;
          [-1, 1].forEach((side) => {
            ctx.beginPath();
            drawPts.forEach((pt, idx) => {
              const ex = pt.x + side * echoOffset;
              const ey = pt.y - side * echoOffset * 0.6;
              if (idx === 0) ctx.moveTo(ex, ey);
              else ctx.lineTo(ex, ey);
            });
            ctx.stroke();
          });
        }

        ctx.shadowColor = '#ff0000';
        ctx.shadowBlur = (growRatio < 1 ? 10 : 20) + cPhase * 4;
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = growRatio < 1 ? cr.width * 0.55 : cr.width + (cPhase - 1) * 1.5;
        ctx.beginPath();
        drawPts.forEach((pt, idx) => {
          if (idx === 0) ctx.moveTo(pt.x, pt.y);
          else ctx.lineTo(pt.x, pt.y);
        });
        ctx.stroke();

        ctx.shadowBlur = 0;
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = growRatio < 1 ? 1.8 : 2.8 + (cPhase - 1) * 0.6;
        ctx.beginPath();
        drawPts.forEach((pt, idx) => {
          if (idx === 0) ctx.moveTo(pt.x, pt.y);
          else ctx.lineTo(pt.x, pt.y);
        });
        ctx.stroke();

        for (let idx = 1; idx < drawPts.length - 1; idx++) {
          const pt = drawPts[idx];
          if (cPhase >= 3) {
            const pPrev = drawPts[idx - 1];
            const segAng = Math.atan2(pt.y - pPrev.y, pt.x - pPrev.x) + Math.PI / 2;
            const tearLen = 12 + cPhase * 4;
            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = 1.6;
            ctx.beginPath();
            ctx.moveTo(pt.x - Math.cos(segAng) * tearLen, pt.y - Math.sin(segAng) * tearLen);
            ctx.lineTo(pt.x + Math.cos(segAng) * tearLen, pt.y + Math.sin(segAng) * tearLen);
            ctx.stroke();
          }

          ctx.save();
          ctx.translate(pt.x, pt.y);
          ctx.rotate(state.elapsedTime * 3 + idx);
          ctx.fillStyle = '#000000';
          ctx.strokeStyle = '#ffffff';
          ctx.lineWidth = 1.5;
          drawPolygon(ctx, 4, 4 + cPhase * 1.2);
          ctx.restore();
        }

        const tip = drawPts[drawPts.length - 1];
        ctx.fillStyle = '#ffffff';
        ctx.beginPath();
        ctx.arc(tip.x, tip.y, growRatio < 1 ? 5.5 : 3.5 + cPhase * 0.7, 0, Math.PI * 2);
        ctx.fill();

        if (cPhase >= 4) {
          ctx.save();
          ctx.translate(tip.x, tip.y);
          ctx.rotate(-state.elapsedTime * 6);
          ctx.strokeStyle = '#ff0000';
          ctx.lineWidth = 1.8;
          drawPolygon(ctx, 4, 10);
          ctx.restore();
        }

        ctx.restore();
      }
    }

    function drawLightnings() {
      for (const lt of state.lightnings) {
        if (!lt.trail || lt.trail.length === 0) continue;
        const pts = lt.trail;
        const ltPhase = lt.phase || 1;
        const strandCount = lt.isPrimary
          ? (ltPhase >= 4 ? 3 : (ltPhase >= 2 ? 2 : 1))
          : (ltPhase >= 3 ? 2 : 1);

        ctx.save();
        ctx.lineCap = 'round';
        ctx.lineJoin = 'round';

        for (let st = 0; st < strandCount; st++) {
          const jitterAmp = (lt.isPrimary ? 10 : 6) + ltPhase * 2.2;
          const jagPts = [pts[0]];
          for (let i = 1; i < pts.length; i++) {
            const pA = pts[i - 1];
            const pB = pts[i];
            const midX = (pA.x + pB.x) * 0.5;
            const midY = (pA.y + pB.y) * 0.5;
            const dx = pB.x - pA.x;
            const dy = pB.y - pA.y;
            const len = Math.hypot(dx, dy) || 1;
            const nx = -dy / len;
            const ny = dx / len;
            const jitter =
              Math.sin(state.elapsedTime * (42 + st * 11) + i * 2.7 + lt.seed + st * 1.9) * jitterAmp;
            jagPts.push({ x: midX + nx * jitter, y: midY + ny * jitter });
            jagPts.push(pB);
          }

          if (st === 0) {
            ctx.shadowColor = '#ff0000';
            ctx.shadowBlur = (lt.isPrimary ? 16 : 11) + ltPhase * 3;
            ctx.strokeStyle = '#ff0000';
            ctx.lineWidth = (lt.isPrimary ? 6.5 : 4.2) + (ltPhase - 1) * 1.1;
            ctx.beginPath();
            jagPts.forEach((pt, idx) => {
              if (idx === 0) ctx.moveTo(pt.x, pt.y);
              else ctx.lineTo(pt.x, pt.y);
            });
            ctx.stroke();
          }

          ctx.shadowBlur = 0;
          ctx.strokeStyle = st % 2 === 0 ? '#ffffff' : '#ff0000';
          ctx.lineWidth = st === 0 ? (lt.isPrimary ? 2.6 : 1.7) : 1.4;
          ctx.beginPath();
          jagPts.forEach((pt, idx) => {
            if (idx === 0) ctx.moveTo(pt.x, pt.y);
            else ctx.lineTo(pt.x, pt.y);
          });
          ctx.stroke();
        }

        if (ltPhase >= 3 && pts.length >= 2) {
          const sparkPt = pts[Math.floor(pts.length * 0.5)];
          const sAng = state.elapsedTime * 19 + lt.seed;
          const sLen = 10 + ltPhase * 3;
          ctx.strokeStyle = '#ffffff';
          ctx.lineWidth = 1.4;
          ctx.beginPath();
          ctx.moveTo(sparkPt.x, sparkPt.y);
          ctx.lineTo(sparkPt.x + Math.cos(sAng) * sLen, sparkPt.y + Math.sin(sAng) * sLen);
          ctx.stroke();
        }

        ctx.fillStyle = '#ff0000';
        ctx.beginPath();
        ctx.arc(lt.x, lt.y, lt.radius + 2.5, 0, Math.PI * 2);
        ctx.fill();

        if (ltPhase >= 2) {
          ctx.save();
          ctx.translate(lt.x, lt.y);
          ctx.rotate(state.elapsedTime * 9 + lt.seed);
          ctx.strokeStyle = '#ffffff';
          ctx.lineWidth = 1.6;
          drawPolygon(ctx, 4, lt.radius + 4 + ltPhase * 0.8);
          ctx.restore();
        }

        ctx.fillStyle = '#ffffff';
        ctx.beginPath();
        ctx.arc(lt.x, lt.y, lt.radius * 0.55, 0, Math.PI * 2);
        ctx.fill();

        ctx.restore();
      }
    }

    function drawTeleports() {
      for (const tp of state.teleports) {
        const progress = 1 - Math.max(0, tp.life / tp.maxLife);
        const fade = Math.max(0, 1 - progress);
        const baseR = tp.isBoss ? tp.radius * 1.15 : tp.radius * 1.45;
        const tpPhase = tp.isBoss && state.boss && state.boss.isVoid ? state.boss.phase : 1;

        ctx.save();

        // 1) Quantum Fold Tether Line connecting old position -> new position
        if (progress < 0.78) {
          const lineAlpha = (1 - progress / 0.78) * (0.7 + tpPhase * 0.08);
          ctx.strokeStyle = `rgba(255, 0, 0, ${lineAlpha})`;
          ctx.lineWidth = (tp.isBoss ? 2.2 : 1.3) + (tpPhase - 1) * 0.6;
          ctx.setLineDash([8, 6]);
          ctx.beginPath();
          ctx.moveTo(tp.fromX, tp.fromY);
          ctx.lineTo(tp.toX, tp.toY);
          ctx.stroke();
          ctx.setLineDash([]);

          if (tp.isBoss && tpPhase >= 2) {
            const dx = tp.toX - tp.fromX;
            const dy = tp.toY - tp.fromY;
            const len = Math.hypot(dx, dy) || 1;
            const perpX = -dy / len;
            const perpY = dx / len;
            const railOff = 12 + tpPhase * 3;
            ctx.strokeStyle = `rgba(255, 255, 255, ${lineAlpha * 0.65})`;
            ctx.lineWidth = 1.2;
            [-1, 1].forEach((side) => {
              ctx.beginPath();
              ctx.moveTo(tp.fromX + perpX * railOff * side, tp.fromY + perpY * railOff * side);
              ctx.lineTo(tp.toX + perpX * railOff * side, tp.toY + perpY * railOff * side);
              ctx.stroke();
            });
          }

          if (tp.isBoss && tpPhase >= 3) {
            const dx = tp.toX - tp.fromX;
            const dy = tp.toY - tp.fromY;
            const len = Math.hypot(dx, dy) || 1;
            const perpX = -dy / len;
            const perpY = dx / len;
            ctx.strokeStyle = `rgba(255, 255, 255, ${lineAlpha})`;
            ctx.lineWidth = 1.8;
            ctx.beginPath();
            for (let k = 0; k <= 10; k++) {
              const f = k / 10;
              const jag = k === 0 || k === 10 ? 0 : Math.sin(k * 3.1 + tp.seed + progress * 20) * (14 + tpPhase * 3);
              const lx = tp.fromX + dx * f + perpX * jag;
              const ly = tp.fromY + dy * f + perpY * jag;
              if (k === 0) ctx.moveTo(lx, ly);
              else ctx.lineTo(lx, ly);
            }
            ctx.stroke();
          }

          const pulseT = Math.min(1, progress / 0.55);
          const px = tp.fromX + (tp.toX - tp.fromX) * pulseT;
          const py = tp.fromY + (tp.toY - tp.fromY) * pulseT;
          ctx.fillStyle = '#ffffff';
          ctx.beginPath();
          ctx.arc(px, py, (tp.isBoss ? 5.5 : 3.5) + (tpPhase - 1) * 1.2, 0, Math.PI * 2);
          ctx.fill();
        }

        // 2) Departure Implosion Gate (at old coordinates)
        ctx.save();
        ctx.translate(tp.fromX, tp.fromY);
        ctx.globalAlpha = fade;
        const depR = Math.max(2, baseR * (1 - progress * 0.92));

        ctx.save();
        ctx.rotate(-progress * 3.2);
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = 2;
        drawPolygon(ctx, 4, depR);
        ctx.restore();

        const slitH = Math.max(2, baseR * 1.35 * (1 - progress));
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 2.4 * fade;
        ctx.beginPath();
        ctx.moveTo(0, -slitH);
        ctx.lineTo(0, slitH);
        ctx.stroke();

        for (let s = -2; s <= 2; s++) {
          const sy = s * (baseR * 0.28);
          const sw = depR * (0.55 + Math.sin(tp.seed + s * 2.1) * 0.35);
          ctx.strokeStyle = s % 2 === 0 ? '#ffffff' : '#ff0000';
          ctx.lineWidth = 1.4;
          ctx.beginPath();
          ctx.moveTo(-sw, sy);
          ctx.lineTo(sw, sy);
          ctx.stroke();
        }
        ctx.restore();

        // 3) Arrival Dimensional Rift Gate (at new coordinates)
        ctx.save();
        ctx.translate(tp.toX, tp.toY);
        ctx.globalAlpha = fade;
        const arrR = baseR * (0.35 + Math.sin(Math.min(1, progress * 1.4) * (Math.PI / 2)) * 0.85);

        ctx.save();
        ctx.rotate(progress * 2.8);
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 2.2;
        drawPolygon(ctx, 4, arrR);
        ctx.rotate(Math.PI / 4);
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = 1.6;
        drawPolygon(ctx, 4, arrR * 0.72);
        ctx.restore();

        const arrSlit = baseR * 1.5 * (1 - progress * 0.65);
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 3 * fade;
        ctx.beginPath();
        ctx.moveTo(0, -arrSlit);
        ctx.lineTo(0, arrSlit);
        ctx.stroke();

        if (tp.isBoss && tpPhase >= 4) {
          ctx.strokeStyle = '#ff0000';
          ctx.lineWidth = 2.8 * fade;
          ctx.beginPath();
          ctx.moveTo(-arrSlit * 1.15, 0);
          ctx.lineTo(arrSlit * 1.15, 0);
          ctx.stroke();
        }

        const br = arrR * 1.15;
        const bLen = br * 0.36;
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = 2;
        ctx.beginPath();
        ctx.moveTo(-br, -br + bLen); ctx.lineTo(-br, -br); ctx.lineTo(-br + bLen, -br);
        ctx.moveTo(br - bLen, -br); ctx.lineTo(br, -br); ctx.lineTo(br, -br + bLen);
        ctx.moveTo(-br, br - bLen); ctx.lineTo(-br, br); ctx.lineTo(-br + bLen, br);
        ctx.moveTo(br - bLen, br); ctx.lineTo(br, br); ctx.lineTo(br, br - bLen);
        ctx.stroke();

        ctx.restore();
        ctx.restore();
      }
    }

    function drawBoss() {
      const b = state.boss;
      if (!b || b.exploded) return;

      // ------------------------------------------------------------------------
      // SPECIAL RENDERER: STANDALONE SINGULARITY BOSS — THE TESSERACTS VOID
      // ------------------------------------------------------------------------
      if (b.isVoid) {
        const primaryStroke = b.hitFlash > 0 ? '#ffffff' : '#ff0000';
        const secondaryStroke = b.hitFlash > 0 ? '#ff0000' : '#ffffff';
        const isDying = !!b.isDying;
        const crackProgress = isDying
          ? Math.max(0, Math.min(1, (b.deathTimer - b.deathMoveDuration) / b.deathCrackDuration))
          : 0;

        ctx.save();
        ctx.translate(b.x, b.y);

        // Violent structural jitter & final implosion squeeze during death cracking
        if (isDying && crackProgress > 0) {
          const jitter = crackProgress * 7.5;
          ctx.translate((Math.random() - 0.5) * jitter * 2, (Math.random() - 0.5) * jitter * 2);
          if (crackProgress > 0.84) {
            const squeeze = 1 - ((crackProgress - 0.84) / 0.16) * 0.38;
            ctx.scale(squeeze, squeeze);
          }
        }

        if (b.warpAnimTimer > 0) {
          const wt = b.warpAnimTimer / 0.38;
          ctx.scale(1 - wt * 0.55, 1 + wt * 0.65);
        }

        if (b.ultAnimTimer > 0) {
          const ut = b.ultAnimTimer / 1.4;
          const pulseScale = 1 + Math.sin(ut * Math.PI) * 0.24;
          ctx.scale(pulseScale, pulseScale);
        }

        const auraRadius = b.isUlt ? b.radius + 78 : b.radius + 54;
        const eventGrad = ctx.createRadialGradient(0, 0, b.radius * 0.25, 0, 0, auraRadius);
        eventGrad.addColorStop(0, 'rgba(0, 0, 0, 1)');
        eventGrad.addColorStop(0.55, b.isUlt ? 'rgba(255, 0, 0, 0.45)' : 'rgba(255, 0, 0, 0.28)');
        eventGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');
        ctx.fillStyle = eventGrad;
        ctx.beginPath();
        ctx.arc(0, 0, auraRadius, 0, Math.PI * 2);
        ctx.fill();

        ctx.save();
        ctx.rotate(-b.angle * 0.8);
        ctx.strokeStyle = b.isUlt ? '#ffffff' : 'rgba(255, 0, 0, 0.65)';
        ctx.lineWidth = b.isUlt ? 2.5 : 2;
        ctx.setLineDash([14, 10]);
        ctx.beginPath();
        ctx.arc(0, 0, b.radius + 14, 0, Math.PI * 2);
        ctx.stroke();
        ctx.restore();

        ctx.fillStyle = '#000000';
        ctx.beginPath();
        ctx.arc(0, 0, b.radius * 0.88, 0, Math.PI * 2);
        ctx.fill();

        const outerR = b.radius * 0.74;
        const innerR = b.radius * 0.36;
        const aOuter = b.angle;
        const aInner = -b.angle * 1.65;

        const outerVerts = [];
        const innerVerts = [];
        for (let i = 0; i < 4; i++) {
          const ao = aOuter + (i * Math.PI) / 2;
          const ai = aInner + (i * Math.PI) / 2;
          outerVerts.push({ x: Math.cos(ao) * outerR, y: Math.sin(ao) * outerR });
          innerVerts.push({ x: Math.cos(ai) * innerR, y: Math.sin(ai) * innerR });
        }

        // Phase 2+ (<= 75% HP): Additional 3rd Nested 4D Diamond Lattice & Secondary Event Horizon Ring
        if (b.phase >= 2) {
          ctx.save();
          ctx.rotate(b.angle * 2.1);
          ctx.strokeStyle = secondaryStroke;
          ctx.lineWidth = 1.4;
          ctx.setLineDash([8, 6]);
          ctx.beginPath();
          ctx.arc(0, 0, b.radius + 26, 0, Math.PI * 2);
          ctx.stroke();
          ctx.setLineDash([]);

          ctx.strokeStyle = primaryStroke;
          ctx.lineWidth = 1.6;
          drawPolygon(ctx, 4, b.radius * 0.56);
          ctx.restore();
        }

        // Phase 3+ (<= 50% HP): 4 Orbiting Quantum Containment Pylons tethered to Outer Vertices
        if (b.phase >= 3) {
          const pylonDist = b.radius + 36;
          for (let py = 0; py < 4; py++) {
            const pAng = -b.angle * 1.25 + (py * Math.PI) / 2;
            const px = Math.cos(pAng) * pylonDist;
            const pyPos = Math.sin(pAng) * pylonDist;

            ctx.strokeStyle = 'rgba(255, 0, 0, 0.7)';
            ctx.lineWidth = 1.4;
            ctx.beginPath();
            ctx.moveTo(outerVerts[py].x, outerVerts[py].y);
            ctx.lineTo(px, pyPos);
            ctx.stroke();

            ctx.save();
            ctx.translate(px, pyPos);
            ctx.rotate(pAng);
            ctx.fillStyle = '#000000';
            ctx.strokeStyle = secondaryStroke;
            ctx.lineWidth = 1.8;
            drawPolygon(ctx, 4, 7.5);
            ctx.fillStyle = primaryStroke;
            ctx.beginPath();
            ctx.arc(0, 0, 2.8, 0, Math.PI * 2);
            ctx.fill();
            ctx.restore();
          }
        }

        // Phase 4 (<= 25% HP): 8 Jagged Singularity Corona Spikes & Intense White/Red Vortex Core
        if (b.phase >= 4) {
          const spikeCount = b.isUlt ? 12 : 8;
          for (let sp = 0; sp < spikeCount; sp++) {
            const sAng = b.angle * 1.8 + (sp * Math.PI * 2) / spikeCount;
            const pulseLen = (b.isUlt ? 24 : 18) + Math.sin(state.elapsedTime * 18 + sp) * 8;
            ctx.strokeStyle = sp % 2 === 0 ? '#ffffff' : '#ff0000';
            ctx.lineWidth = b.isUlt ? 2.6 : 2.2;
            ctx.beginPath();
            ctx.moveTo(Math.cos(sAng) * b.radius, Math.sin(sAng) * b.radius);
            ctx.lineTo(Math.cos(sAng) * (b.radius + pulseLen), Math.sin(sAng) * (b.radius + pulseLen));
            ctx.stroke();
          }
        }

        // Ult Form (<= 10% HP awakened): Outer 4D Hyper-Cage & 4 Extra Outer Quantum Pylons
        if (b.isUlt) {
          ctx.save();
          ctx.rotate(-b.angle * 1.15);
          ctx.strokeStyle = '#ffffff';
          ctx.lineWidth = 2;
          drawPolygon(ctx, 4, b.radius + 46);
          ctx.rotate(Math.PI / 4);
          ctx.strokeStyle = '#ff0000';
          ctx.lineWidth = 2.2;
          drawPolygon(ctx, 4, b.radius + 46);
          ctx.restore();

          const outerPylonDist = b.radius + 58;
          for (let op = 0; op < 4; op++) {
            const opAng = b.angle * 1.45 + (op * Math.PI) / 2 + Math.PI / 4;
            const opx = Math.cos(opAng) * outerPylonDist;
            const opy = Math.sin(opAng) * outerPylonDist;

            ctx.save();
            ctx.translate(opx, opy);
            ctx.rotate(-opAng * 2);
            ctx.fillStyle = '#000000';
            ctx.strokeStyle = '#ff0000';
            ctx.lineWidth = 2;
            drawPolygon(ctx, 4, 8.5);
            ctx.fillStyle = '#ffffff';
            ctx.beginPath();
            ctx.arc(0, 0, 3.2, 0, Math.PI * 2);
            ctx.fill();
            ctx.restore();
          }
        }

        ctx.strokeStyle = secondaryStroke;
        ctx.lineWidth = 1.6 + (b.phase - 1) * 0.3;
        ctx.beginPath();
        for (let i = 0; i < 4; i++) {
          ctx.moveTo(outerVerts[i].x, outerVerts[i].y);
          ctx.lineTo(innerVerts[i].x, innerVerts[i].y);
        }
        ctx.stroke();

        // Outer Hypercube Frame (splits & separates outward as death cracks widen)
        const splitOffset = crackProgress * 18;
        ctx.strokeStyle = primaryStroke;
        ctx.lineWidth = 2.6 + (b.phase - 1) * 0.4;
        for (let i = 0; i < 4; i++) {
          const next = (i + 1) % 4;
          const midAng = Math.atan2(
            (outerVerts[i].y + outerVerts[next].y) * 0.5,
            (outerVerts[i].x + outerVerts[next].x) * 0.5
          );
          const ox = Math.cos(midAng) * splitOffset;
          const oy = Math.sin(midAng) * splitOffset;
          ctx.beginPath();
          ctx.moveTo(outerVerts[i].x + ox, outerVerts[i].y + oy);
          ctx.lineTo(outerVerts[next].x + ox, outerVerts[next].y + oy);
          ctx.stroke();
        }

        ctx.strokeStyle = secondaryStroke;
        ctx.lineWidth = 2.2 + (b.phase - 1) * 0.3;
        ctx.beginPath();
        for (let i = 0; i < 4; i++) {
          const next = (i + 1) % 4;
          ctx.moveTo(innerVerts[i].x, innerVerts[i].y);
          ctx.lineTo(innerVerts[next].x, innerVerts[next].y);
        }
        ctx.stroke();

        for (let i = 0; i < 4; i++) {
          ctx.fillStyle = primaryStroke;
          ctx.beginPath();
          ctx.arc(outerVerts[i].x, outerVerts[i].y, 4.5 + (b.phase - 1) * 0.8, 0, Math.PI * 2);
          ctx.fill();

          ctx.fillStyle = secondaryStroke;
          ctx.beginPath();
          ctx.arc(innerVerts[i].x, innerVerts[i].y, 3.2 + (b.phase - 1) * 0.6, 0, Math.PI * 2);
          ctx.fill();
        }

        // Progressive Dimensional Fracture Lines when cracking in the middle
        if (isDying && crackProgress > 0 && b.deathCracks) {
          ctx.save();
          ctx.lineCap = 'round';
          ctx.lineJoin = 'miter';

          // Expanding white-hot core leak
          const coreLeakR = 8 + crackProgress * 34;
          const leakGrad = ctx.createRadialGradient(0, 0, 2, 0, 0, coreLeakR);
          leakGrad.addColorStop(0, '#ffffff');
          leakGrad.addColorStop(0.55, 'rgba(255, 0, 0, 0.85)');
          leakGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');
          ctx.fillStyle = leakGrad;
          ctx.beginPath();
          ctx.arc(0, 0, coreLeakR, 0, Math.PI * 2);
          ctx.fill();

          for (const cr of b.deathCracks) {
            if (crackProgress < cr.appearAt) continue;
            const localProg = Math.min(1, (crackProgress - cr.appearAt) / 0.38);
            const totalSegs = cr.points.length - 1;
            const exactSegs = totalSegs * localProg;
            const fullSegs = Math.floor(exactSegs);
            const partial = exactSegs - fullSegs;

            const drawPts = [];
            for (let k = 0; k <= Math.min(fullSegs, totalSegs); k++) {
              drawPts.push(cr.points[k]);
            }
            if (fullSegs < totalSegs && partial > 0) {
              const pA = cr.points[fullSegs];
              const pB = cr.points[fullSegs + 1];
              drawPts.push({
                x: pA.x + (pB.x - pA.x) * partial,
                y: pA.y + (pB.y - pA.y) * partial
              });
            }

            if (drawPts.length < 2) continue;

            // Crimson outer fracture aura
            ctx.shadowColor = '#ff0000';
            ctx.shadowBlur = 16 + crackProgress * 14;
            ctx.strokeStyle = '#ff0000';
            ctx.lineWidth = 4 + crackProgress * 5;
            ctx.beginPath();
            drawPts.forEach((pt, idx) => {
              if (idx === 0) ctx.moveTo(pt.x, pt.y);
              else ctx.lineTo(pt.x, pt.y);
            });
            ctx.stroke();

            // Crisp white inner fracture fissure
            ctx.shadowBlur = 0;
            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = 1.6 + crackProgress * 2.4;
            ctx.beginPath();
            drawPts.forEach((pt, idx) => {
              if (idx === 0) ctx.moveTo(pt.x, pt.y);
              else ctx.lineTo(pt.x, pt.y);
            });
            ctx.stroke();
          }
          ctx.restore();
        }

        ctx.fillStyle = primaryStroke;
        ctx.beginPath();
        ctx.arc(0, 0, 5.5 + (b.phase - 1) * 1.4, 0, Math.PI * 2);
        ctx.fill();

        if (b.phase >= 3) {
          ctx.fillStyle = '#ffffff';
          ctx.beginPath();
          ctx.arc(0, 0, 3.2, 0, Math.PI * 2);
          ctx.fill();
        }

        // Visual Telegraph Ring & Trajectory Spokes while ATK 03 Arc Lightning is Charging
        if (b.arcChargeTimer > 0 && b.arcChargeMax > 0) {
          const chargeProgress = 1 - Math.max(0, b.arcChargeTimer / b.arcChargeMax);
          const previewCount = b.isUlt ? 8 : (b.phase === 4 ? 6 : 6 + (b.phase - 1) * 2);
          const ringR = b.radius + 44 * (1 - chargeProgress * 0.72);

          ctx.strokeStyle = chargeProgress > 0.75 ? '#ffffff' : '#ff0000';
          ctx.lineWidth = 2;
          ctx.setLineDash([6, 4]);
          ctx.beginPath();
          ctx.arc(0, 0, ringR, 0, Math.PI * 2);
          ctx.stroke();
          ctx.setLineDash([]);

          ctx.strokeStyle = 'rgba(255, 255, 255, 0.45)';
          ctx.lineWidth = 1.2;
          for (let k = 0; k < previewCount; k++) {
            const pAng = b.arcChargeAngle + (k * Math.PI * 2) / previewCount;
            ctx.beginPath();
            ctx.moveTo(Math.cos(pAng) * (b.radius + 4), Math.sin(pAng) * (b.radius + 4));
            ctx.lineTo(Math.cos(pAng) * (b.radius + 26 + chargeProgress * 22), Math.sin(pAng) * (b.radius + 26 + chargeProgress * 22));
            ctx.stroke();
          }
        }

        ctx.font = '800 10px "JetBrains Mono", monospace';
        ctx.fillStyle = b.isUlt ? '#ff0000' : (b.phase >= 3 ? '#ff0000' : '#ffffff');
        ctx.textAlign = 'center';
        const statusPrefix = isDying ? 'COLLAPSING' : (b.isUlt ? 'ULT FORM' : `PHASE 0${b.phase}`);
        const statusText = isDying
          ? (crackProgress > 0 ? `CORE FRACTURE // ${Math.round(crackProgress * 100)}%` : 'RECENTERING SINGULARITY...')
          : b.voidAttackLabel;
        const labelOffset = b.isUlt ? 70 : (b.phase >= 3 ? 44 : 26);
        ctx.fillText(`${statusPrefix} // ${statusText}`, 0, -b.radius - labelOffset);

        ctx.restore();
        return;
      }

      // ------------------------------------------------------------------------
      // SPECIAL RENDERER: LEVEL 5 FINAL BOSS — RGB PRISM TRIANGLE
      // ------------------------------------------------------------------------
      if (b.level === 5) {
        const hue = b.rgbHue;
        const rgbPrimary = `hsl(${hue}, 100%, 54%)`;
        const rgbSecondary = `hsl(${(hue + 120) % 360}, 100%, 58%)`;
        const rgbTertiary = `hsl(${(hue + 240) % 360}, 100%, 58%)`;

        ctx.save();
        ctx.translate(b.x, b.y);

        const glowGrad = ctx.createRadialGradient(0, 0, b.radius * 0.25, 0, 0, b.radius + 32);
        glowGrad.addColorStop(0, `hsla(${hue}, 100%, 55%, 0.28)`);
        glowGrad.addColorStop(0.55, `hsla(${(hue + 140) % 360}, 100%, 50%, 0.12)`);
        glowGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');
        ctx.fillStyle = glowGrad;
        ctx.beginPath();
        ctx.arc(0, 0, b.radius + 32, 0, Math.PI * 2);
        ctx.fill();

        ctx.save();
        ctx.rotate(b.angle);

        ctx.fillStyle = '#000000';
        ctx.beginPath();
        for (let v = 0; v < 3; v++) {
          const va = (v * Math.PI * 2) / 3;
          const vx = Math.cos(va) * b.radius;
          const vy = Math.sin(va) * b.radius;
          if (v === 0) ctx.moveTo(vx, vy);
          else ctx.lineTo(vx, vy);
        }
        ctx.closePath();
        ctx.fill();

        ctx.lineWidth = 3.5;
        ctx.strokeStyle = b.hitFlash > 0 ? '#ffffff' : rgbPrimary;
        ctx.stroke();

        ctx.lineWidth = 2;
        ctx.strokeStyle = b.hitFlash > 0 ? '#ffffff' : rgbSecondary;
        ctx.beginPath();
        for (let v = 0; v < 3; v++) {
          const va = (v * Math.PI * 2) / 3 + Math.PI;
          const vx = Math.cos(va) * (b.radius * 0.56);
          const vy = Math.sin(va) * (b.radius * 0.56);
          if (v === 0) ctx.moveTo(vx, vy);
          else ctx.lineTo(vx, vy);
        }
        ctx.closePath();
        ctx.stroke();

        for (let v = 0; v < 3; v++) {
          const va = (v * Math.PI * 2) / 3;
          const vx = Math.cos(va) * b.radius;
          const vy = Math.sin(va) * b.radius;
          const vColor = `hsl(${(hue + v * 120) % 360}, 100%, 55%)`;

          ctx.strokeStyle = vColor;
          ctx.lineWidth = 1.6;
          ctx.beginPath();
          ctx.moveTo(0, 0);
          ctx.lineTo(vx, vy);
          ctx.stroke();

          ctx.fillStyle = vColor;
          ctx.beginPath();
          ctx.arc(vx, vy, 5.5, 0, Math.PI * 2);
          ctx.fill();

          ctx.fillStyle = '#ffffff';
          ctx.beginPath();
          ctx.arc(vx, vy, 2.4, 0, Math.PI * 2);
          ctx.fill();
        }

        ctx.fillStyle = rgbTertiary;
        ctx.beginPath();
        ctx.arc(0, 0, 9, 0, Math.PI * 2);
        ctx.fill();

        ctx.fillStyle = '#ffffff';
        ctx.beginPath();
        ctx.arc(0, 0, 4, 0, Math.PI * 2);
        ctx.fill();

        ctx.restore();
        ctx.restore();
        return;
      }

      // ------------------------------------------------------------------------
      // STANDARD RENDERER: SECTORS 1–4 (#000000, #ff0000, #ffffff)
      // ------------------------------------------------------------------------
      ctx.save();
      ctx.translate(b.x, b.y);

      if (b.phase === 2) {
        ctx.save();
        ctx.rotate(-b.angle * 1.5);
        ctx.strokeStyle = 'rgba(255, 0, 0, 0.5)';
        ctx.lineWidth = 1.5;
        ctx.setLineDash([10, 8]);
        ctx.beginPath();
        ctx.arc(0, 0, b.radius + 14, 0, Math.PI * 2);
        ctx.stroke();
        ctx.restore();
      }

      ctx.save();
      ctx.rotate(b.angle);

      const primaryStroke = b.hitFlash > 0 ? '#ffffff' : '#ff0000';
      const innerStroke = b.hitFlash > 0 ? '#ff0000' : '#ffffff';

      if (b.level === 2) {
        ctx.fillStyle = '#000000';
        ctx.strokeStyle = primaryStroke;
        ctx.lineWidth = 3;
        ctx.beginPath();
        ctx.moveTo(b.radius + 10, 0);
        ctx.lineTo(-b.radius * 0.72, b.radius * 0.88);
        ctx.lineTo(-b.radius * 0.35, 0);
        ctx.lineTo(-b.radius * 0.72, -b.radius * 0.88);
        ctx.closePath();
        ctx.fill();
        ctx.stroke();

        ctx.strokeStyle = innerStroke;
        ctx.lineWidth = 1.8;
        ctx.beginPath();
        ctx.moveTo(b.radius * 0.45, 0);
        ctx.lineTo(-b.radius * 0.5, b.radius * 0.48);
        ctx.lineTo(-b.radius * 0.22, 0);
        ctx.lineTo(-b.radius * 0.5, -b.radius * 0.48);
        ctx.closePath();
        ctx.stroke();

        ctx.fillStyle = '#ff0000';
        ctx.beginPath();
        ctx.moveTo(-b.radius * 0.35, -7);
        ctx.lineTo(-b.radius * 0.35 - (10 + Math.random() * 8), 0);
        ctx.lineTo(-b.radius * 0.35, 7);
        ctx.closePath();
        ctx.fill();

        ctx.fillStyle = primaryStroke;
        ctx.beginPath();
        ctx.arc(2, 0, 6, 0, Math.PI * 2);
        ctx.fill();
      } else {
        ctx.fillStyle = '#000000';
        ctx.strokeStyle = primaryStroke;
        ctx.lineWidth = 3;
        drawPolygon(ctx, b.sides, b.radius);
        ctx.fill();
        ctx.stroke();

        ctx.strokeStyle = innerStroke;
        ctx.lineWidth = 1.8;
        ctx.rotate(-b.angle * 2);
        drawPolygon(ctx, b.sides, b.radius * 0.56);

        ctx.fillStyle = primaryStroke;
        ctx.beginPath();
        ctx.arc(0, 0, 8, 0, Math.PI * 2);
        ctx.fill();

        ctx.fillStyle = '#ffffff';
        ctx.beginPath();
        ctx.arc(0, 0, 3.5, 0, Math.PI * 2);
        ctx.fill();
      }

      ctx.restore();
      ctx.restore();
    }

    function drawEntities() {
      // 1. Player Bullets
      for (const pb of state.playerBullets) {
        const ang = Math.atan2(pb.vy, pb.vx);
        ctx.save();
        ctx.translate(pb.x, pb.y);
        ctx.rotate(ang);
        ctx.fillStyle = '#ffffff';
        ctx.fillRect(-10, -2.2, 20, 4.4);
        ctx.fillStyle = '#ff0000';
        ctx.fillRect(-10, -1, 7, 2);
        ctx.restore();
      }

      // 2. Enemy Bullets
      for (const eb of state.enemyBullets) {
        const isRgb = typeof eb.rgbHue === 'number';
        const bulletCol = isRgb ? `hsl(${eb.rgbHue}, 100%, 55%)` : (eb.color || '#ff0000');
        ctx.save();
        ctx.translate(eb.x, eb.y);
        ctx.fillStyle = bulletCol;
        ctx.beginPath();
        ctx.arc(0, 0, eb.radius, 0, Math.PI * 2);
        ctx.fill();

        ctx.fillStyle = '#ffffff';
        ctx.beginPath();
        ctx.arc(0, 0, eb.radius * 0.45, 0, Math.PI * 2);
        ctx.fill();
        ctx.restore();
      }

      // 3. Sector Drones & Tesseract Void 4-Class Fleet
      for (const dr of state.drones) {
        const shipPhase = dr.phase || 1;
        if (dr.type === 'speeder' && dr.afterimages) {
          for (const ai of dr.afterimages) {
            ctx.save();
            ctx.translate(ai.x, ai.y);
            ctx.rotate(ai.angle);
            ctx.globalAlpha = (ai.life / 0.18) * (0.45 + shipPhase * 0.08);
            ctx.strokeStyle = shipPhase >= 3 ? '#ffffff' : '#ff0000';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            ctx.moveTo(15, 0);
            ctx.lineTo(-10, 7);
            ctx.lineTo(-6, 0);
            ctx.lineTo(-10, -7);
            ctx.closePath();
            ctx.stroke();
            ctx.restore();
          }
        }

        ctx.save();
        ctx.translate(dr.x, dr.y);

        if (dr.type === 'teleporter') {
          if (dr.warpAnimTimer > 0) {
            const wt = dr.warpAnimTimer / 0.3;
            ctx.scale(1 - wt * 0.6, 1 + wt * 0.7);
          }

          if (shipPhase >= 2) {
            ctx.save();
            ctx.rotate(state.elapsedTime * 3.5);
            ctx.strokeStyle = shipPhase >= 4 ? '#ffffff' : 'rgba(255, 0, 0, 0.7)';
            ctx.lineWidth = 1.2;
            ctx.setLineDash([4, 3]);
            ctx.beginPath();
            ctx.arc(0, 0, dr.radius + 6, 0, Math.PI * 2);
            ctx.stroke();
            ctx.restore();
          }

          ctx.rotate(dr.angle || 0);
          ctx.fillStyle = '#000000';
          ctx.strokeStyle = '#ffffff';
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.moveTo(14, 0);
          ctx.lineTo(0, 11);
          ctx.lineTo(-12, 0);
          ctx.lineTo(0, -11);
          ctx.closePath();
          ctx.fill();
          ctx.stroke();

          ctx.strokeStyle = '#ff0000';
          ctx.lineWidth = 1.5;
          ctx.beginPath();
          ctx.arc(0, 0, 6.5, 0, Math.PI * 2);
          ctx.stroke();

          ctx.fillStyle = '#ff0000';
          ctx.beginPath();
          ctx.arc(2, 0, 2.8, 0, Math.PI * 2);
          ctx.fill();
        } else if (dr.type === 'speeder') {
          if (dr.subState === 'TELEGRAPH') {
            ctx.strokeStyle = 'rgba(255, 0, 0, 0.75)';
            ctx.lineWidth = 1.6 + (shipPhase - 1) * 0.4;
            ctx.setLineDash([8, 6]);
            ctx.beginPath();
            ctx.moveTo(0, 0);
            ctx.lineTo(Math.cos(dr.angle) * 650, Math.sin(dr.angle) * 650);
            ctx.stroke();
            ctx.setLineDash([]);
          }

          ctx.rotate(dr.angle || 0);

          if (shipPhase >= 3 && dr.subState === 'CHARGE') {
            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = 1.5;
            ctx.beginPath();
            ctx.moveTo(20, 0);
            ctx.lineTo(-4, 15);
            ctx.moveTo(20, 0);
            ctx.lineTo(-4, -15);
            ctx.stroke();
          }

          ctx.fillStyle = '#000000';
          ctx.strokeStyle = '#ff0000';
          ctx.lineWidth = 2.2;
          ctx.beginPath();
          ctx.moveTo(16, 0);
          ctx.lineTo(-11, 8);
          ctx.lineTo(-6, 0);
          ctx.lineTo(-11, -8);
          ctx.closePath();
          ctx.fill();
          ctx.stroke();

          ctx.fillStyle = '#ffffff';
          ctx.beginPath();
          ctx.moveTo(-8, -3);
          ctx.lineTo(-16 - Math.random() * (6 + shipPhase * 3), 0);
          ctx.lineTo(-8, 3);
          ctx.closePath();
          ctx.fill();
        } else if (dr.type === 'bomber') {
          const fuseProgress = 1 - Math.max(0, (dr.fuseTimer || 0) / (dr.maxFuse || 3.3));
          const blink = Math.sin(state.elapsedTime * (12 + fuseProgress * 26)) > 0;
          const blastR = (dr.blastRadius || 105) + (shipPhase - 1) * 14;

          ctx.strokeStyle = blink ? 'rgba(255, 0, 0, 0.45)' : 'rgba(255, 255, 255, 0.25)';
          ctx.lineWidth = 1.2 + (shipPhase - 1) * 0.3;
          ctx.setLineDash([5, 5]);
          ctx.beginPath();
          ctx.arc(0, 0, blastR * (0.45 + fuseProgress * 0.55), 0, Math.PI * 2);
          ctx.stroke();
          ctx.setLineDash([]);

          ctx.rotate(state.elapsedTime * (2.5 + shipPhase * 0.8));
          ctx.fillStyle = '#000000';
          ctx.strokeStyle = blink ? '#ff0000' : '#ffffff';
          ctx.lineWidth = 2.2;
          drawPolygon(ctx, 6, dr.radius);
          ctx.fill();
          ctx.stroke();

          if (shipPhase >= 3) {
            ctx.strokeStyle = '#ffffff';
            ctx.lineWidth = 1.4;
            ctx.beginPath();
            ctx.arc(0, 0, dr.radius + 4, 0, Math.PI * 2);
            ctx.stroke();
          }

          ctx.fillStyle = blink ? '#ffffff' : '#ff0000';
          ctx.beginPath();
          ctx.arc(0, 0, 4.5 + fuseProgress * 3.5 + (shipPhase - 1) * 0.6, 0, Math.PI * 2);
          ctx.fill();
        } else if (dr.type === 'shooter') {
          ctx.rotate(dr.angle || 0);
          ctx.fillStyle = '#000000';
          ctx.strokeStyle = '#ff0000';
          ctx.lineWidth = 2;
          ctx.beginPath();
          ctx.moveTo(14, 0);
          ctx.lineTo(2, 12);
          ctx.lineTo(-11, 9);
          ctx.lineTo(-7, 0);
          ctx.lineTo(-11, -9);
          ctx.lineTo(2, -12);
          ctx.closePath();
          ctx.fill();
          ctx.stroke();

          ctx.strokeStyle = '#ffffff';
          ctx.lineWidth = 1.8;
          ctx.beginPath();
          ctx.moveTo(4, -5);
          ctx.lineTo(17 + (shipPhase >= 3 ? 3 : 0), -5);
          ctx.moveTo(4, 5);
          ctx.lineTo(17 + (shipPhase >= 3 ? 3 : 0), 5);
          ctx.stroke();
        } else {
          ctx.fillStyle = '#000000';
          ctx.strokeStyle = '#ff0000';
          ctx.lineWidth = 2;
          drawPolygon(ctx, 4, dr.radius);
          ctx.fill();
          ctx.stroke();

          ctx.fillStyle = '#ffffff';
          ctx.beginPath();
          ctx.arc(0, 0, 3, 0, Math.PI * 2);
          ctx.fill();
        }

        ctx.restore();
      }

      // 4. Level 2 Proximity Mines
      for (const m of state.mines) {
        const pulseR = m.radius + Math.sin(m.pulse) * 2.5;
        ctx.save();
        ctx.translate(m.x, m.y);
        ctx.strokeStyle = 'rgba(255, 0, 0, 0.22)';
        ctx.setLineDash([4, 4]);
        ctx.beginPath();
        ctx.arc(0, 0, m.triggerRadius, 0, Math.PI * 2);
        ctx.stroke();
        ctx.setLineDash([]);

        ctx.fillStyle = '#000000';
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = 2;
        ctx.beginPath();
        ctx.arc(0, 0, pulseR, 0, Math.PI * 2);
        ctx.fill();
        ctx.stroke();

        ctx.fillStyle = '#ffffff';
        ctx.beginPath();
        ctx.arc(0, 0, 3, 0, Math.PI * 2);
        ctx.fill();
        ctx.restore();
      }

      // 5. Combat Health Pack Drop (15-30s cooldown, restores 25%-50% HP)
      if (state.healthPack) {
        const hpPack = state.healthPack;
        const bob = Math.sin(hpPack.pulse) * 3;
        const ringR = hpPack.radius + 5 + Math.sin(hpPack.pulse * 1.4) * 3;

        ctx.save();
        ctx.translate(hpPack.x, hpPack.y + bob);

        ctx.strokeStyle = 'rgba(255, 255, 255, 0.45)';
        ctx.lineWidth = 1.2;
        ctx.setLineDash([6, 4]);
        ctx.beginPath();
        ctx.arc(0, 0, ringR, 0, Math.PI * 2);
        ctx.stroke();
        ctx.setLineDash([]);

        ctx.save();
        ctx.rotate(hpPack.pulse * 0.35);
        ctx.fillStyle = '#000000';
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 2;
        drawPolygon(ctx, 8, hpPack.radius);
        ctx.fill();
        ctx.stroke();
        ctx.restore();

        ctx.fillStyle = '#ff0000';
        ctx.fillRect(-3.5, -9, 7, 18);
        ctx.fillRect(-9, -3.5, 18, 7);

        ctx.fillStyle = '#ffffff';
        ctx.fillRect(-1.5, -6, 3, 12);
        ctx.fillRect(-6, -1.5, 12, 3);

        ctx.font = '800 9px "JetBrains Mono", monospace';
        ctx.fillStyle = '#ffffff';
        ctx.textAlign = 'center';
        ctx.fillText('REPAIR +25-50%', 0, hpPack.radius + 15);

        ctx.restore();
      }

      // 6. Combat Upgrade Pack Drop (30s or 60s cooldown: Fire Rate / DMG / Max HP)
      if (state.upgradePack) {
        const upg = state.upgradePack;
        const bob = Math.cos(upg.pulse) * 3.5;
        const outerR = upg.radius + 6 + Math.sin(upg.pulse * 1.5) * 3.5;

        ctx.save();
        ctx.translate(upg.x, upg.y + bob);

        ctx.save();
        ctx.rotate(-upg.pulse * 0.45);
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = 1.6;
        ctx.setLineDash([7, 5]);
        drawPolygon(ctx, 4, outerR);
        ctx.setLineDash([]);
        ctx.restore();

        ctx.save();
        ctx.rotate(upg.pulse * 0.35);
        ctx.fillStyle = '#000000';
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = 2.2;
        drawPolygon(ctx, 6, upg.radius);
        ctx.fill();
        ctx.stroke();

        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 1.4;
        drawPolygon(ctx, 6, upg.radius * 0.65);
        ctx.restore();

        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 2.4;
        ctx.beginPath();
        ctx.moveTo(-6, -1);
        ctx.lineTo(0, -7);
        ctx.lineTo(6, -1);
        ctx.moveTo(-6, 5);
        ctx.lineTo(0, -1);
        ctx.lineTo(6, 5);
        ctx.stroke();

        ctx.font = '800 9px "JetBrains Mono", monospace';
        ctx.fillStyle = '#ff0000';
        ctx.textAlign = 'center';
        ctx.fillText('SYSTEM UPGRADE', 0, upg.radius + 16);

        ctx.restore();
      }
    }

    function drawPlayer() {
      const p = state.player;

      for (const ai of p.afterimages) {
        ctx.save();
        ctx.translate(ai.x, ai.y);
        ctx.rotate(ai.angle);
        ctx.globalAlpha = (ai.life / 0.18) * 0.35;
        ctx.strokeStyle = '#ffffff';
        ctx.lineWidth = 1.5;
        ctx.beginPath();
        ctx.moveTo(18, 0);
        ctx.lineTo(-12, 11);
        ctx.lineTo(-7, 0);
        ctx.lineTo(-12, -11);
        ctx.closePath();
        ctx.stroke();
        ctx.restore();
      }

      if (p.aimPoint && state.screen === 'PLAYING') {
        const ap = p.aimPoint;
        ctx.save();
        ctx.strokeStyle = 'rgba(255, 0, 0, 0.32)';
        ctx.setLineDash([6, 6]);
        ctx.lineWidth = 1;
        ctx.beginPath();
        ctx.moveTo(p.x, p.y);
        ctx.lineTo(ap.x, ap.y);
        ctx.stroke();
        ctx.setLineDash([]);

        ctx.translate(ap.x, ap.y);
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = 1.8;
        const r = 13;
        ctx.beginPath();
        ctx.moveTo(-r, -4); ctx.lineTo(-r, -r); ctx.lineTo(-4, -r);
        ctx.moveTo(4, -r); ctx.lineTo(r, -r); ctx.lineTo(r, -4);
        ctx.moveTo(-r, 4); ctx.lineTo(-r, r); ctx.lineTo(-4, r);
        ctx.moveTo(4, r); ctx.lineTo(r, r); ctx.lineTo(r, 4);
        ctx.stroke();
        ctx.restore();
      }

      if (p.invulnTimer > 0 && p.dashTimer <= 0 && Math.floor(p.invulnTimer * 22) % 2 === 0) {
        return;
      }

      ctx.save();
      ctx.translate(p.x, p.y);
      ctx.rotate(p.angle);

      const flameLen = 10 + Math.random() * 8;
      ctx.fillStyle = '#ff0000';
      ctx.beginPath();
      ctx.moveTo(-10, -4.5);
      ctx.lineTo(-10 - flameLen, 0);
      ctx.lineTo(-10, 4.5);
      ctx.closePath();
      ctx.fill();

      ctx.fillStyle = '#000000';
      ctx.strokeStyle = '#ffffff';
      ctx.lineWidth = 2.4;
      ctx.beginPath();
      ctx.moveTo(19, 0);
      ctx.lineTo(-13, 12);
      ctx.lineTo(-8, 0);
      ctx.lineTo(-13, -12);
      ctx.closePath();
      ctx.fill();
      ctx.stroke();

      ctx.fillStyle = '#ff0000';
      ctx.beginPath();
      ctx.moveTo(6, 0);
      ctx.lineTo(-3, 4);
      ctx.lineTo(-3, -4);
      ctx.closePath();
      ctx.fill();

      if (p.muzzleFlash > 0) {
        ctx.fillStyle = '#ffffff';
        ctx.beginPath();
        ctx.arc(21, 0, 5.5, 0, Math.PI * 2);
        ctx.fill();
      }

      ctx.restore();
    }

    function drawFX() {
      for (const sw of state.shockwaves) {
        ctx.save();
        ctx.globalAlpha = Math.max(0, sw.life / sw.maxLife);
        ctx.strokeStyle = sw.color;
        ctx.lineWidth = 2.2;
        ctx.beginPath();
        ctx.arc(sw.x, sw.y, sw.radius, 0, Math.PI * 2);
        ctx.stroke();
        ctx.restore();
      }

      for (const pt of state.particles) {
        ctx.save();
        ctx.globalAlpha = Math.max(0, pt.life / pt.maxLife);
        ctx.fillStyle = pt.color;
        ctx.fillRect(pt.x - pt.size / 2, pt.y - pt.size / 2, pt.size, pt.size);
        ctx.restore();
      }

      for (const ft of state.floatingTexts) {
        ctx.save();
        ctx.globalAlpha = Math.max(0, Math.min(1, ft.life / ft.maxLife));
        ctx.font = '800 13px "Orbitron", monospace';
        ctx.fillStyle = ft.color;
        ctx.textAlign = 'center';
        ctx.fillText(ft.text, ft.x, ft.y);
        ctx.restore();
      }
    }

    function drawHUD() {
      const w = GAME_CONFIG.ARENA_WIDTH;
      const p = state.player;
      const b = state.boss;

      ctx.fillStyle = '#000000';
      ctx.fillRect(0, 0, w, 62);
      ctx.strokeStyle = 'rgba(255, 255, 255, 0.25)';
      ctx.lineWidth = 1;
      ctx.beginPath();
      ctx.moveTo(0, 62);
      ctx.lineTo(w, 62);
      ctx.stroke();

      // Left: Player Hull Integrity
      ctx.font = '800 11px "JetBrains Mono", monospace';
      ctx.fillStyle = '#ffffff';
      ctx.textAlign = 'left';
      ctx.fillText(
        state.practiceMode ? 'HULL // IMMUNE [PRACTICE]' : `HULL // ${Math.ceil(p.hp)}/${p.maxHp}`,
        16,
        20
      );

      const hpRatio = state.practiceMode ? 1 : Math.max(0, p.hp / p.maxHp);
      ctx.strokeStyle = '#ffffff';
      ctx.lineWidth = 1.5;
      ctx.strokeRect(16, 27, 210, 14);
      ctx.fillStyle = hpRatio <= 0.3 ? '#ff0000' : '#ffffff';
      ctx.fillRect(18, 29, 206 * hpRatio, 10);

      const dashRatio = 1 - Math.max(0, p.dashCooldown / GAME_CONFIG.PLAYER_DASH_COOLDOWN);
      ctx.fillStyle = dashRatio >= 1 ? '#ffffff' : 'rgba(255,255,255,0.45)';
      ctx.fillText(dashRatio >= 1 ? 'DASH [READY]' : 'DASH [CHARGING]', 16, 54);

      const bonusRof = Math.round(((p.rateMult || 1) - 1) * 100);
      const bonusDmg = Math.round(((p.dmgMult || 1) - 1) * 100);
      if (bonusRof > 0 || bonusDmg > 0) {
        ctx.fillStyle = '#ff0000';
        ctx.fillText(`MODS // ROF +${bonusRof}% DMG +${bonusDmg}%`, 148, 54);
      }

      // Center: Boss Name & Health Bar
      if (b) {
        ctx.textAlign = 'center';
        ctx.font = '900 13px "Orbitron", monospace';
        ctx.fillStyle = b.isVoid && b.isUlt ? '#ff0000' : '#ffffff';
        const modeTag = state.practiceMode ? ' [PRACTICE]' : (state.hardMode ? ' [HARD]' : '');
        const phaseLabel = b.isVoid
          ? (b.isUlt
              ? ` [ULT FORM // DMG x${(b.voidDmgMult || 1).toFixed(2)}]`
              : ` [PHASE 0${b.phase} // DMG x${(b.voidDmgMult || 1).toFixed(2)}]`)
          : (b.phase === 2 ? ' [OVERDRIVE]' : '');
        const worldTag = b.isVoid ? 'THE TESSERACTS VOID' : 'THE GUARDIANS';
        const bossHeader = b.isVoid
          ? `${worldTag} // ${b.name}${phaseLabel}${modeTag}`
          : `${worldTag} // 0${b.level} ${b.name}${phaseLabel}${modeTag}`;
        ctx.fillText(bossHeader, w / 2, 20);

        const barW = 440;
        const barX = w / 2 - barW / 2;
        const innerX = barX + 2;
        const innerW = barW - 4;
        const bossRatio = Math.max(0, b.hp / b.maxHp);
        ctx.strokeStyle = b.isVoid && b.isUlt ? '#ffffff' : '#ff0000';
        ctx.lineWidth = b.isVoid && b.isUlt ? 2 : 1.5;
        ctx.strokeRect(barX, 27, barW, 14);
        ctx.fillStyle = '#ff0000';
        ctx.fillRect(innerX, 29, innerW * bossRatio, 10);

        // Draw Next-Phase Threshold Line on THE TESSERACTS VOID HP Bar
        let nextVoidPhase = null;
        if (b.isVoid) {
          const voidMilestones = [
            { ratio: GAME_CONFIG.VOID_PHASE_THRESHOLDS[1], label: 'PHASE 02' },
            { ratio: GAME_CONFIG.VOID_PHASE_THRESHOLDS[2], label: 'PHASE 03' },
            { ratio: GAME_CONFIG.VOID_PHASE_THRESHOLDS[3], label: 'PHASE 04' }
          ];
          if (!b.ultTriggered) {
            voidMilestones.push({ ratio: GAME_CONFIG.VOID_ULT_THRESHOLD, label: 'ULT FORM' });
          }

          // Identify the immediate upcoming phase threshold
          if (b.phase === 1) nextVoidPhase = voidMilestones[0];
          else if (b.phase === 2) nextVoidPhase = voidMilestones[1];
          else if (b.phase === 3) nextVoidPhase = voidMilestones[2];
          else if (b.phase === 4 && !b.ultTriggered) nextVoidPhase = voidMilestones[3];

          // Subtle internal tick marks for subsequent future thresholds
          for (const ms of voidMilestones) {
            if (nextVoidPhase && ms.ratio < nextVoidPhase.ratio) {
              const futureX = Math.round(innerX + innerW * ms.ratio);
              ctx.fillStyle = 'rgba(0, 0, 0, 0.75)';
              ctx.fillRect(futureX - 1, 29, 2, 10);
            }
          }

          // Prominent high-contrast marker line for the immediate next phase
          if (nextVoidPhase) {
            const lineX = Math.round(innerX + innerW * nextVoidPhase.ratio);

            // Black contrast casing so the white line cuts cleanly through the red HP fill
            ctx.fillStyle = '#000000';
            ctx.fillRect(lineX - 2, 24, 4, 20);

            // Crisp white vertical threshold line + top/bottom T-caps
            ctx.fillStyle = '#ffffff';
            ctx.fillRect(lineX - 1, 24, 2, 20);
            ctx.fillRect(lineX - 4, 24, 8, 2);
            ctx.fillRect(lineX - 4, 42, 8, 2);

            // Compact next-phase readout right beside the HP bar
            const nextHpVal = Math.round(b.maxHp * nextVoidPhase.ratio);
            ctx.save();
            ctx.font = '800 9px "JetBrains Mono", monospace';
            ctx.fillStyle = '#ffffff';
            ctx.textAlign = 'left';
            ctx.fillText(`NEXT: ${nextVoidPhase.label} (${nextHpVal.toLocaleString()})`, barX + barW + 10, 37);
            ctx.restore();
          }
        }

        ctx.font = '600 10px "JetBrains Mono", monospace';
        ctx.fillStyle = 'rgba(255, 255, 255, 0.72)';
        ctx.fillText(
          `${b.hp.toLocaleString()} / ${b.maxHp.toLocaleString()} HP  |  TIME ${formatTime(state.elapsedTime)}  |  SCORE ${String(state.score).padStart(5, '0')}`,
          w / 2,
          54
        );
      }

      // Non-blocking Tactical Alert Banner on Sector Entry
      if (state.bannerTimer > 0 && state.screen === 'PLAYING') {
        const alpha = Math.min(1, state.bannerTimer * 1.6);
        ctx.save();
        ctx.globalAlpha = alpha;
        const bw = 540;
        const bh = 86;
        const bx = w / 2 - bw / 2;
        const by = 82;

        ctx.fillStyle = 'rgba(0, 0, 0, 0.88)';
        ctx.fillRect(bx, by, bw, bh);
        ctx.strokeStyle = '#ff0000';
        ctx.lineWidth = 2;
        ctx.strokeRect(bx, by, bw, bh);

        ctx.textAlign = 'center';
        ctx.font = '800 11px "JetBrains Mono", monospace';
        ctx.fillStyle = '#ff0000';
        ctx.fillText(state.bannerTag, w / 2, by + 23);

        ctx.font = '900 22px "Orbitron", monospace';
        ctx.fillStyle = '#ffffff';
        ctx.fillText(state.bannerTitle, w / 2, by + 51);

        ctx.font = '600 11px "JetBrains Mono", monospace';
        ctx.fillStyle = 'rgba(255, 255, 255, 0.75)';
        ctx.fillText(state.bannerSubtitle, w / 2, by + 72);
        ctx.restore();
      }
    }

    /* ============================================================================
       14. ANIMATED TOP-DOWN GALAXY STAR-MAP RENDERER (START SCREEN)
       ============================================================================ */
    function drawGalaxyStartMap() {
      const w = GAME_CONFIG.ARENA_WIDTH;
      const h = GAME_CONFIG.ARENA_HEIGHT;
      const g = getGalaxyGeometry();
      const t = performance.now() * 0.001;

      ctx.fillStyle = '#000000';
      ctx.fillRect(0, 0, w, h);

      // 1. Deep-Space Twinkling Starfield
      for (const st of state.galaxyStars) {
        const pulse = 0.55 + 0.45 * Math.sin(t * st.twinkle + st.x);
        ctx.fillStyle = st.isRed
          ? `rgba(255, 0, 0, ${st.alpha * pulse})`
          : `rgba(255, 255, 255, ${st.alpha * pulse})`;
        ctx.fillRect(st.x, st.y, st.size, st.size);
      }

      // 2. Top-Down Rotating Spiral Galaxy Arms
      ctx.save();
      ctx.translate(g.cx, g.cy);
      const armCount = 4;
      for (let arm = 0; arm < armCount; arm++) {
        const baseAngle = (arm * Math.PI * 2) / armCount + t * 0.14;
        ctx.beginPath();
        for (let step = 12; step <= 115; step += 2) {
          const r = step * 3.6;
          const theta = baseAngle + step * 0.032;
          const px = Math.cos(theta) * r;
          const py = Math.sin(theta) * r * 0.64;
          if (step === 12) ctx.moveTo(px, py);
          else ctx.lineTo(px, py);
        }
        ctx.strokeStyle = arm % 2 === 0 ? 'rgba(255, 0, 0, 0.22)' : 'rgba(255, 255, 255, 0.14)';
        ctx.lineWidth = 2.2;
        ctx.stroke();

        for (let step = 16; step <= 110; step += 6) {
          const r = step * 3.6;
          const theta = baseAngle + step * 0.032 + Math.sin(step * 0.4) * 0.08;
          const px = Math.cos(theta) * r;
          const py = Math.sin(theta) * r * 0.64;
          ctx.fillStyle = (step + arm) % 3 === 0 ? 'rgba(255, 0, 0, 0.55)' : 'rgba(255, 255, 255, 0.48)';
          ctx.fillRect(px - 1.2, py - 1.2, 2.4, 2.4);
        }
      }

      // 3. Outer Galactic Scanner Rings & Elliptical Orbit Track for THE GUARDIANS
      ctx.strokeStyle = 'rgba(255, 255, 255, 0.12)';
      ctx.lineWidth = 1;
      ctx.beginPath();
      ctx.ellipse(0, 0, 390, 245, 0, 0, Math.PI * 2);
      ctx.stroke();

      ctx.strokeStyle = state.selectedTarget === 'guardians' ? 'rgba(255, 0, 0, 0.72)' : 'rgba(255, 255, 255, 0.34)';
      ctx.lineWidth = state.selectedTarget === 'guardians' ? 1.8 : 1.2;
      ctx.setLineDash([8, 8]);
      ctx.beginPath();
      ctx.ellipse(0, 0, g.orbitRx, g.orbitRy, 0, 0, Math.PI * 2);
      ctx.stroke();
      ctx.setLineDash([]);

      // 4. Galactic Center Singularity: THE TESSERACTS VOID (Black Hole)
      const voidSelected = state.selectedTarget === 'void';
      const voidHovered = state.galaxyHovered === 'void';

      const bhGrad = ctx.createRadialGradient(0, 0, 14, 0, 0, 74);
      bhGrad.addColorStop(0, 'rgba(0, 0, 0, 1)');
      bhGrad.addColorStop(0.45, 'rgba(255, 0, 0, 0.55)');
      bhGrad.addColorStop(0.75, 'rgba(255, 255, 255, 0.16)');
      bhGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');
      ctx.fillStyle = bhGrad;
      ctx.beginPath();
      ctx.arc(0, 0, 74, 0, Math.PI * 2);
      ctx.fill();

      ctx.save();
      ctx.rotate(-t * 1.4);
      ctx.strokeStyle = '#ff0000';
      ctx.lineWidth = 2.4;
      ctx.setLineDash([14, 7]);
      ctx.beginPath();
      ctx.arc(0, 0, 36, 0, Math.PI * 2);
      ctx.stroke();
      ctx.restore();

      ctx.save();
      ctx.rotate(t * 2.1);
      ctx.strokeStyle = '#ffffff';
      ctx.lineWidth = 1.4;
      ctx.setLineDash([6, 10]);
      ctx.beginPath();
      ctx.arc(0, 0, 44, 0, Math.PI * 2);
      ctx.stroke();
      ctx.restore();

      ctx.fillStyle = '#000000';
      ctx.strokeStyle = '#ffffff';
      ctx.lineWidth = 2.2;
      ctx.beginPath();
      ctx.arc(0, 0, 22, 0, Math.PI * 2);
      ctx.fill();
      ctx.stroke();

      ctx.save();
      ctx.rotate(t * 0.9);
      ctx.strokeStyle = '#ff0000';
      ctx.lineWidth = 1.5;
      drawPolygon(ctx, 4, 14);
      ctx.restore();

      if (voidSelected || voidHovered) {
        drawTargetBrackets(ctx, 0, 0, 56, voidSelected ? '#ff0000' : '#ffffff');
      }

      ctx.textAlign = 'center';
      ctx.font = '900 12px "Orbitron", monospace';
      ctx.fillStyle = voidSelected ? '#ff0000' : '#ffffff';
      ctx.fillText('THE TESSERACTS VOID', 0, 76);
      ctx.font = '700 10px "JetBrains Mono", monospace';
      ctx.fillStyle = 'rgba(255, 255, 255, 0.78)';
      ctx.fillText('[GALACTIC CORE // 1 BOSS // 50,000 HP]', 0, 91);

      ctx.restore();

      // 5. Orbiting Star System: THE GUARDIANS (5-Boss Gauntlet)
      const starSelected = state.selectedTarget === 'guardians';
      const starHovered = state.galaxyHovered === 'guardians';

      ctx.save();
      ctx.translate(g.starX, g.starY);

      const starGrad = ctx.createRadialGradient(0, 0, 4, 0, 0, 46);
      starGrad.addColorStop(0, 'rgba(255, 255, 255, 0.95)');
      starGrad.addColorStop(0.4, 'rgba(255, 0, 0, 0.5)');
      starGrad.addColorStop(1, 'rgba(0, 0, 0, 0)');
      ctx.fillStyle = starGrad;
      ctx.beginPath();
      ctx.arc(0, 0, 46, 0, Math.PI * 2);
      ctx.fill();

      ctx.strokeStyle = 'rgba(255, 255, 255, 0.35)';
      ctx.lineWidth = 1;
      ctx.beginPath();
      ctx.arc(0, 0, 26, 0, Math.PI * 2);
      ctx.stroke();

      for (let m = 0; m < 5; m++) {
        const mAng = t * 1.9 + (m * Math.PI * 2) / 5;
        const mx = Math.cos(mAng) * 26;
        const my = Math.sin(mAng) * 26;
        ctx.fillStyle = m % 2 === 0 ? '#ffffff' : '#ff0000';
        ctx.beginPath();
        ctx.arc(mx, my, 2.6, 0, Math.PI * 2);
        ctx.fill();
      }

      ctx.fillStyle = '#ffffff';
      ctx.beginPath();
      ctx.arc(0, 0, 11, 0, Math.PI * 2);
      ctx.fill();

      ctx.strokeStyle = '#ff0000';
      ctx.lineWidth = 2;
      ctx.beginPath();
      ctx.arc(0, 0, 14, 0, Math.PI * 2);
      ctx.stroke();

      if (starSelected || starHovered) {
        drawTargetBrackets(ctx, 0, 0, 38, starSelected ? '#ff0000' : '#ffffff');
      }

      ctx.textAlign = 'center';
      ctx.font = '900 12px "Orbitron", monospace';
      ctx.fillStyle = starSelected ? '#ff0000' : '#ffffff';
      ctx.fillText('THE GUARDIANS', 0, -48);
      ctx.font = '700 10px "JetBrains Mono", monospace';
      ctx.fillStyle = 'rgba(255, 255, 255, 0.82)';
      ctx.fillText('[ORBITING STAR // 5 BOSSES]', 0, -34);

      ctx.restore();
    }

    function drawTargetBrackets(context, cx, cy, r, color) {
      const len = 12;
      context.save();
      context.strokeStyle = color;
      context.lineWidth = 2.4;
      context.beginPath();
      context.moveTo(cx - r, cy - r + len); context.lineTo(cx - r, cy - r); context.lineTo(cx - r + len, cy - r);
      context.moveTo(cx + r - len, cy - r); context.lineTo(cx + r, cy - r); context.lineTo(cx + r, cy - r + len);
      context.moveTo(cx - r, cy + r - len); context.lineTo(cx - r, cy + r); context.lineTo(cx - r + len, cy + r);
      context.moveTo(cx + r - len, cy + r); context.lineTo(cx + r, cy + r); context.lineTo(cx + r, cy + r - len);
      context.stroke();
      context.restore();
    }

    /* ============================================================================
       15. FIXED-TIMESTEP-SAFE MAIN LOOP
       ============================================================================ */
    let lastTimestamp = performance.now();

    function gameLoop(now) {
      const dt = Math.min((now - lastTimestamp) / 1000, 0.05);
      lastTimestamp = now;

      if (state.screen === 'START') {
        state.galaxyAngle = (state.galaxyAngle + dt * 0.24) % (Math.PI * 2);
      }

      if (state.screen === 'PLAYING') {
        state.elapsedTime += dt;
        if (state.bannerTimer > 0) state.bannerTimer -= dt;
        updatePlayer(dt);
        updateBoss(dt);
        updateEntities(dt);
        updateLasers(dt);
        updateCracks(dt);
        updateLightnings(dt);
        updateTeleports(dt);
        updateHealthPacks(dt);
        updateUpgradePacks(dt);
      }

      updateFX(dt);

      ctx.save();
      ctx.scale(canvasScale, canvasScale);

      if (state.screen === 'START') {
        drawGalaxyStartMap();
      } else {
        if (state.shakeTime > 0) {
          state.shakeTime -= dt;
          const sx = (Math.random() - 0.5) * state.shakeMagnitude * 2;
          const sy = (Math.random() - 0.5) * state.shakeMagnitude * 2;
          ctx.translate(sx, sy);
        }

        drawArenaGrid();
        drawVoidRealityBG();
        drawCracks();
        drawLasers();
        drawTeleports();
        drawEntities();
        drawLightnings();
        drawBoss();
        drawPlayer();
        drawFX();

        if (state.flashAlpha > 0) {
          ctx.fillStyle = `rgba(255, 255, 255, ${state.flashAlpha})`;
          ctx.fillRect(0, 0, GAME_CONFIG.ARENA_WIDTH, GAME_CONFIG.ARENA_HEIGHT);
          state.flashAlpha = Math.max(0, state.flashAlpha - dt * 2.4);
        }

        drawHUD();
      }

      ctx.restore();
      requestAnimationFrame(gameLoop);
    }

    // Boot Default Target & Render Loop
    selectTarget('guardians');
    requestAnimationFrame(gameLoop);
  </script>
</body>
</html># game1
