[index.html](https://github.com/user-attachments/files/26386876/index.html)
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FaGe Escape Room</title>
  <style>
    @import url("https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&family=Manrope:wght@400;500;700;800&display=swap");

    :root {
      --bg-1: #06111f;
      --bg-2: #0d2037;
      --panel: rgba(8, 18, 33, 0.78);
      --line: rgba(255, 255, 255, 0.1);
      --text: #eef5ff;
      --muted: #9eb0cb;
      --cyan: #67e8f9;
      --blue: #60a5fa;
      --gold: #fbbf24;
      --good: #34d399;
      --bad: #fb7185;
      --room-glow: rgba(96, 165, 250, 0.18);
    }

    * { box-sizing: border-box; }

    html, body {
      margin: 0;
      min-height: 100%;
      overflow: hidden;
      font-family: "Manrope", sans-serif;
      color: var(--text);
      background:
        radial-gradient(circle at 20% 15%, rgba(103, 232, 249, 0.12), transparent 22%),
        radial-gradient(circle at 85% 10%, rgba(251, 191, 36, 0.08), transparent 18%),
        linear-gradient(180deg, #030813, #081321 45%, #030813 100%);
    }

    body::before {
      content: "";
      position: fixed;
      inset: 0;
      background-image:
        linear-gradient(rgba(255,255,255,0.02) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.02) 1px, transparent 1px);
      background-size: 42px 42px;
      mask-image: linear-gradient(180deg, rgba(0,0,0,0.9), transparent 92%);
      pointer-events: none;
    }

    button {
      font: inherit;
    }

    .game {
      position: relative;
      width: 100vw;
      height: 100vh;
      overflow: hidden;
      perspective: 1400px;
    }

    .hud {
      position: absolute;
      inset: 18px 18px auto 18px;
      z-index: 20;
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      gap: 16px;
      pointer-events: none;
    }

    .hud-block {
      max-width: min(560px, calc(100vw - 36px));
      padding: 16px 18px;
      border: 1px solid var(--line);
      border-radius: 20px;
      background: linear-gradient(180deg, rgba(8, 18, 33, 0.76), rgba(8, 18, 33, 0.52));
      backdrop-filter: blur(12px);
      box-shadow: 0 12px 34px rgba(0, 0, 0, 0.28);
      pointer-events: auto;
    }

    .hud-kicker,
    .tiny-label,
    .door-state {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      padding: 8px 12px;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,0.1);
      background: rgba(255,255,255,0.04);
      text-transform: uppercase;
      letter-spacing: 0.15em;
      font-family: "Space Grotesk", sans-serif;
      font-size: 11px;
      color: var(--muted);
    }

    .hud-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: var(--good);
      box-shadow: 0 0 0 0 rgba(52, 211, 153, 0.55);
      animation: pulse 2s infinite;
    }

    @keyframes pulse {
      0% { box-shadow: 0 0 0 0 rgba(52, 211, 153, 0.45); }
      70% { box-shadow: 0 0 0 14px rgba(52, 211, 153, 0); }
      100% { box-shadow: 0 0 0 0 rgba(52, 211, 153, 0); }
    }

    h1, h2, h3 {
      margin: 0;
      font-family: "Space Grotesk", sans-serif;
      letter-spacing: -0.04em;
    }

    .hud h1 {
      margin-top: 10px;
      font-size: clamp(1.5rem, 2.5vw, 2.2rem);
    }

    .hud p {
      margin: 8px 0 0;
      color: var(--muted);
      line-height: 1.45;
    }

    .hud-stats {
      display: grid;
      gap: 10px;
      width: min(320px, 32vw);
    }

    .hud-block.main {
      max-width: min(560px, 52vw);
    }

    .hud-block.side {
      min-width: min(320px, 32vw);
      max-width: min(320px, 32vw);
    }

    .hud-stat {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      padding: 12px 14px;
      border-radius: 16px;
      border: 1px solid rgba(255,255,255,0.08);
      background: rgba(255,255,255,0.04);
      font-size: 12px;
      color: #d7e4f8;
      letter-spacing: 0.06em;
      text-transform: uppercase;
      font-family: "Space Grotesk", sans-serif;
    }

    .hud-stat strong {
      font-size: 15px;
      letter-spacing: 0.02em;
      text-transform: none;
      color: #f2f7ff;
    }

    .wellbeing-card {
      padding: 14px;
      border-radius: 18px;
      border: 1px solid rgba(103,232,249,0.16);
      background:
        linear-gradient(180deg, rgba(103,232,249,0.08), rgba(255,255,255,0.03)),
        rgba(255,255,255,0.03);
      box-shadow: inset 0 1px 0 rgba(255,255,255,0.04);
    }

    .wellbeing-head {
      display: flex;
      align-items: end;
      justify-content: space-between;
      gap: 12px;
      margin-bottom: 10px;
    }

    .wellbeing-label {
      font-size: 12px;
      color: #cfe3fb;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      font-family: "Space Grotesk", sans-serif;
    }

    .wellbeing-value {
      font-family: "Space Grotesk", sans-serif;
      font-size: 1.8rem;
      font-weight: 700;
      color: #f2f7ff;
    }

    .wellbeing-track {
      height: 18px;
      padding: 3px;
      border-radius: 999px;
      background: rgba(255,255,255,0.08);
      border: 1px solid rgba(255,255,255,0.08);
      overflow: hidden;
    }

    .wellbeing-fill {
      height: 100%;
      width: 100%;
      border-radius: 999px;
      background: linear-gradient(90deg, #34d399, #67e8f9);
      box-shadow: 0 0 22px rgba(103,232,249,0.24);
      transition: width 0.35s ease, background 0.35s ease;
    }

    .wellbeing-note {
      margin-top: 8px;
      font-size: 12px;
      color: var(--muted);
      line-height: 1.45;
    }

    .room {
      position: absolute;
      inset: 0;
      overflow: hidden;
      transform-style: preserve-3d;
    }

    .scene {
      position: absolute;
      inset: 0;
      transform-style: preserve-3d;
      transition: transform 0.9s cubic-bezier(0.22, 1, 0.36, 1), filter 0.9s ease, opacity 0.9s ease;
    }

    .game.room-transition .scene {
      transform: translateZ(110px) rotateX(12deg) scale(1.06);
      filter: blur(1.5px) saturate(1.15);
    }

    .room::before {
      content: "";
      position: absolute;
      inset: 0;
      background:
        radial-gradient(circle at 50% 12%, var(--room-glow), transparent 24%),
        linear-gradient(180deg, rgba(19, 43, 69, 0.9) 0 24%, rgba(9, 18, 31, 0.95) 24.1% 100%);
    }

    .back-wall {
      position: absolute;
      left: 18%;
      right: 18%;
      top: 11%;
      height: 20%;
      border-radius: 22px 22px 34px 34px;
      background:
        radial-gradient(circle at 50% 0%, rgba(255,255,255,0.14), transparent 38%),
        linear-gradient(180deg, rgba(38, 68, 102, 0.8), rgba(13, 26, 42, 0.95));
      border: 1px solid rgba(255,255,255,0.08);
      box-shadow: inset 0 -18px 40px rgba(0,0,0,0.22);
      z-index: 1;
    }

    .wall {
      position: absolute;
      top: 20%;
      bottom: 6%;
      width: 20%;
      z-index: 1;
      opacity: 0.88;
    }

    .wall.left {
      left: 3%;
      clip-path: polygon(100% 2%, 24% 16%, 10% 100%, 100% 100%);
      background: linear-gradient(180deg, rgba(27, 54, 83, 0.9), rgba(6, 12, 23, 0.92));
      box-shadow: inset -50px 0 80px rgba(0,0,0,0.28);
    }

    .wall.right {
      right: 3%;
      clip-path: polygon(0 2%, 76% 16%, 90% 100%, 0 100%);
      background: linear-gradient(180deg, rgba(27, 54, 83, 0.9), rgba(6, 12, 23, 0.92));
      box-shadow: inset 50px 0 80px rgba(0,0,0,0.28);
    }

    .ceiling-lights {
      position: absolute;
      left: 12%;
      right: 12%;
      top: 7%;
      display: flex;
      justify-content: space-between;
      z-index: 1;
    }

    .light {
      width: 160px;
      height: 22px;
      border-radius: 999px;
      background: linear-gradient(180deg, rgba(255,255,255,0.9), rgba(255,255,255,0.45));
      box-shadow: 0 0 40px rgba(255,255,255,0.2);
      opacity: 0.92;
    }

    .floor {
      position: absolute;
      inset: 24% 6% 6%;
      border-radius: 38px;
      background:
        linear-gradient(180deg, rgba(255,255,255,0.05), transparent 16%),
        radial-gradient(circle at center, rgba(255,255,255,0.04), transparent 62%),
        linear-gradient(180deg, #10253d, #091320);
      box-shadow:
        inset 0 1px 0 rgba(255,255,255,0.08),
        inset 0 -60px 100px rgba(0,0,0,0.35);
      clip-path: polygon(8% 0, 92% 0, 100% 100%, 0 100%);
    }

    .floor::before {
      content: "";
      position: absolute;
      inset: 0;
      border-radius: 38px;
      background-image:
        linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
      background-size: 56px 56px;
      mask-image: linear-gradient(180deg, rgba(0,0,0,0.92), rgba(0,0,0,0.78));
    }

    .floor::after {
      content: "";
      position: absolute;
      inset: 0;
      background:
        linear-gradient(77deg, transparent 0 18%, rgba(255,255,255,0.035) 18.4%, transparent 19%),
        linear-gradient(103deg, transparent 0 18%, rgba(255,255,255,0.035) 18.4%, transparent 19%),
        radial-gradient(circle at 50% 10%, rgba(255,255,255,0.05), transparent 55%);
      opacity: 0.8;
      pointer-events: none;
    }

    .door-frame {
      position: absolute;
      left: 50%;
      top: 16%;
      width: 150px;
      height: 192px;
      transform: translateX(-50%);
      border-radius: 30px 30px 18px 18px;
      border: 1px solid rgba(255,255,255,0.15);
      background:
        linear-gradient(180deg, rgba(255,255,255,0.08), rgba(255,255,255,0.02)),
        linear-gradient(135deg, rgba(251,191,36,0.18), rgba(96,165,250,0.16));
      box-shadow: 0 20px 44px rgba(0,0,0,0.28);
      z-index: 2;
    }

    .door-glow {
      position: absolute;
      left: 50%;
      top: 13%;
      width: 240px;
      height: 280px;
      transform: translateX(-50%);
      background: radial-gradient(circle, rgba(103,232,249,0.18), transparent 68%);
      filter: blur(20px);
      z-index: 1;
      pointer-events: none;
    }

    .door-frame::before {
      content: "";
      position: absolute;
      inset: 16px 22px 20px;
      border-radius: 20px;
      border: 1px solid rgba(255,255,255,0.1);
    }

    .door-frame::after {
      content: "";
      position: absolute;
      right: 28px;
      top: 92px;
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: rgba(255,255,255,0.62);
      box-shadow: 0 0 18px rgba(255,255,255,0.36);
    }

    .object {
      position: absolute;
      z-index: 3;
      pointer-events: none;
    }

    .desk {
      left: 11%;
      top: 48%;
      width: 220px;
      height: 112px;
      border-radius: 30px;
      background: linear-gradient(180deg, #7b5537, #49301f);
      box-shadow: 0 26px 32px rgba(0,0,0,0.25);
    }

    .desk::before,
    .desk::after {
      content: "";
      position: absolute;
      bottom: -28px;
      width: 16px;
      height: 44px;
      border-radius: 8px;
      background: linear-gradient(180deg, #654228, #341f13);
    }

    .desk::before { left: 28px; }
    .desk::after { right: 28px; }

    .desk-file {
      position: absolute;
      left: 28px;
      top: 18px;
      width: 70px;
      height: 48px;
      border-radius: 10px;
      background: linear-gradient(180deg, #e9eef7, #cdd7e4);
      box-shadow: 0 12px 18px rgba(0,0,0,0.14);
    }

    .terminal {
      right: 13%;
      top: 46%;
      width: 196px;
      height: 148px;
      border-radius: 28px;
      background: linear-gradient(180deg, #162b44, #0a1523);
      border: 1px solid rgba(255,255,255,0.08);
      box-shadow: 0 24px 34px rgba(0,0,0,0.28);
    }

    .terminal::before {
      content: "";
      position: absolute;
      inset: 16px 18px 26px;
      border-radius: 18px;
      background: linear-gradient(180deg, rgba(52,211,153,0.26), rgba(34,211,238,0.14));
      border: 1px solid rgba(103,232,249,0.18);
    }

    .terminal::after {
      content: "";
      position: absolute;
      left: 50%;
      bottom: -24px;
      transform: translateX(-50%);
      width: 72px;
      height: 24px;
      border-radius: 0 0 18px 18px;
      background: linear-gradient(180deg, #203852, #0c1623);
    }

    .bed {
      left: 37%;
      bottom: 13%;
      width: 230px;
      height: 100px;
      border-radius: 26px;
      background: linear-gradient(180deg, #f1f5f9, #aebdce);
      box-shadow: 0 24px 32px rgba(0,0,0,0.22);
    }

    .bed::before {
      content: "+";
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      font-size: 52px;
      font-weight: 800;
      color: rgba(23, 37, 54, 0.4);
    }

    .hotspot {
      position: absolute;
      z-index: 10;
      width: 56px;
      height: 56px;
      padding: 0;
      border-radius: 50%;
      border: 0;
      background: transparent;
      cursor: pointer;
      transition: transform 0.18s ease, opacity 0.18s ease, filter 0.18s ease;
    }

    .marker-name {
      position: absolute;
      left: 50%;
      top: calc(100% + 8px);
      transform: translateX(-50%);
      padding: 4px 8px;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,0.08);
      background: rgba(5, 13, 23, 0.72);
      color: #d7e4f8;
      font-size: 11px;
      line-height: 1;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      font-family: "Space Grotesk", sans-serif;
      white-space: nowrap;
      pointer-events: none;
    }

    .hotspot::before,
    .hotspot::after {
      content: "";
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      border-radius: 50%;
      pointer-events: none;
    }

    .hotspot::before {
      width: 22px;
      height: 22px;
      background: rgba(103,232,249,0.18);
      border: 2px solid rgba(103,232,249,0.6);
      box-shadow: 0 0 18px rgba(103,232,249,0.22);
    }

    .hotspot::after {
      width: 54px;
      height: 54px;
      border: 1px solid rgba(103,232,249,0.2);
      background: radial-gradient(circle, rgba(103,232,249,0.08), transparent 68%);
    }

    .hotspot:hover,
    .hotspot.near {
      transform: scale(1.12);
      filter: brightness(1.08);
    }

    .hotspot.near::before {
      animation: marker-pulse 1s ease-in-out infinite alternate;
      background: rgba(251,191,36,0.26);
      border-color: rgba(251,191,36,0.85);
      box-shadow: 0 0 24px rgba(251,191,36,0.34);
    }

    .hotspot.near::after {
      border-color: rgba(251,191,36,0.34);
      background: radial-gradient(circle, rgba(251,191,36,0.12), transparent 68%);
    }

    @keyframes marker-pulse {
      from { transform: translate(-50%, -50%) scale(0.94); }
      to { transform: translate(-50%, -50%) scale(1.12); }
    }

    .hint-bar {
      position: absolute;
      left: 50%;
      bottom: 18px;
      transform: translateX(-50%);
      z-index: 15;
      padding: 10px 16px;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,0.1);
      background: rgba(5, 13, 23, 0.54);
      color: #dce8f6;
      font-size: 13px;
      letter-spacing: 0.03em;
      max-width: min(720px, calc(100vw - 40px));
      text-align: center;
      backdrop-filter: blur(10px);
    }

    .player {
      position: absolute;
      width: 64px;
      height: 118px;
      transform: translate(-50%, -50%) scale(1);
      z-index: 12;
      pointer-events: none;
      transition: left 0.04s linear, top 0.04s linear;
      filter: drop-shadow(0 18px 18px rgba(0,0,0,0.25));
    }

    .player-shadow {
      position: absolute;
      width: 60px;
      height: 18px;
      transform: translate(-50%, -50%);
      border-radius: 50%;
      background: rgba(0,0,0,0.28);
      filter: blur(4px);
      z-index: 11;
      transition: left 0.04s linear, top 0.04s linear;
    }

    .player-label {
      position: absolute;
      transform: translateX(-50%);
      z-index: 13;
      padding: 6px 10px;
      border-radius: 999px;
      border: 1px solid rgba(255,255,255,0.1);
      background: rgba(4, 11, 20, 0.8);
      font-size: 11px;
      font-family: "Space Grotesk", sans-serif;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      transition: left 0.04s linear, top 0.04s linear;
    }

    .player.walking .player-arm.left,
    .player.walking .player-leg.right {
      animation: swing-left 0.34s ease-in-out infinite alternate;
    }

    .player.walking .player-arm.right,
    .player.walking .player-leg.left {
      animation: swing-right 0.34s ease-in-out infinite alternate;
    }

    @keyframes swing-left {
      from { transform: rotate(18deg); }
      to { transform: rotate(-18deg); }
    }

    @keyframes swing-right {
      from { transform: rotate(-18deg); }
      to { transform: rotate(18deg); }
    }

    .player-head {
      position: absolute;
      left: 50%;
      top: 0;
      width: 30px;
      height: 34px;
      transform: translateX(-50%);
      border-radius: 16px 16px 14px 14px;
      background: linear-gradient(180deg, #ffd8b2, #eeb98d);
      z-index: 4;
    }

    .player-hair {
      position: absolute;
      inset: -3px 0 auto;
      height: 16px;
      border-radius: 16px 16px 10px 10px;
      background: linear-gradient(180deg, #2b211d, #5a4031);
    }

    .player-face {
      position: absolute;
      left: 50%;
      top: 15px;
      width: 16px;
      height: 8px;
      transform: translateX(-50%);
      display: flex;
      justify-content: space-between;
    }

    .player-face::before,
    .player-face::after {
      content: "";
      width: 3px;
      height: 3px;
      border-radius: 50%;
      background: #4b2e23;
    }

    .player-torso {
      position: absolute;
      left: 50%;
      top: 28px;
      width: 34px;
      height: 42px;
      transform: translateX(-50%);
      border-radius: 16px 16px 12px 12px;
      background: linear-gradient(180deg, #6ee7f9, #2563eb);
      z-index: 3;
    }

    .player-torso::before {
      content: "";
      position: absolute;
      left: 50%;
      top: 6px;
      width: 12px;
      height: 16px;
      transform: translateX(-50%);
      border-radius: 999px;
      background: rgba(255,255,255,0.16);
    }

    .player-arm,
    .player-leg {
      position: absolute;
      transform-origin: top center;
      border-radius: 999px;
    }

    .player-arm {
      top: 34px;
      width: 9px;
      height: 34px;
      background: linear-gradient(180deg, #ffd8b2, #dea271);
      z-index: 2;
    }

    .player-arm.left { left: 12px; }
    .player-arm.right { right: 12px; }

    .player-leg {
      top: 66px;
      width: 11px;
      height: 40px;
      background: linear-gradient(180deg, #0f1f36, #1d4ed8);
      z-index: 1;
    }

    .player-leg.left { left: 21px; }
    .player-leg.right { right: 21px; }

    .player-foot {
      position: absolute;
      bottom: -5px;
      width: 14px;
      height: 8px;
      border-radius: 999px;
      background: #0a0f17;
    }

    .player-leg.left .player-foot { left: -1px; }
    .player-leg.right .player-foot { right: -1px; }

    .overlay {
      position: absolute;
      inset: 0;
      display: none;
      align-items: flex-start;
      justify-content: center;
      padding: 140px 24px 24px;
      background: rgba(2, 6, 12, 0.72);
      backdrop-filter: blur(10px);
      z-index: 50;
    }

    .overlay.show {
      display: flex;
    }

    .modal {
      width: min(920px, 100%);
      max-height: 90vh;
      overflow: auto;
      padding: 26px;
      border-radius: 28px;
      border: 1px solid rgba(255,255,255,0.1);
      background: linear-gradient(180deg, rgba(10, 23, 40, 0.96), rgba(5, 12, 22, 0.94));
      box-shadow: 0 26px 70px rgba(0,0,0,0.45);
    }

    .modal-head {
      display: flex;
      justify-content: space-between;
      gap: 16px;
      align-items: flex-start;
      margin-bottom: 18px;
    }

    .close-btn,
    .action-btn,
    .answer-btn,
    .restart-btn {
      border: 0;
      cursor: pointer;
      border-radius: 20px;
    }

    .close-btn {
      width: 46px;
      height: 46px;
      color: white;
      background: rgba(255,255,255,0.06);
      border: 1px solid rgba(255,255,255,0.08);
    }

    .info-box,
    .result-box {
      padding: 20px;
      margin-bottom: 18px;
      border-radius: 24px;
      border: 1px solid rgba(255,255,255,0.08);
      background: rgba(255,255,255,0.045);
    }

    .info-box p,
    .result-box p {
      margin: 0;
      color: #dce8f6;
      line-height: 1.75;
    }

    .answers {
      display: grid;
      gap: 12px;
    }

    .answer-btn {
      width: 100%;
      padding: 16px 18px;
      text-align: left;
      display: grid;
      grid-template-columns: 46px 1fr;
      gap: 14px;
      color: white;
      background: rgba(255,255,255,0.045);
      border: 1px solid rgba(255,255,255,0.08);
      transition: transform 0.18s ease, border-color 0.18s ease, background 0.18s ease;
    }

    .answer-btn:hover {
      transform: translateY(-2px);
      border-color: rgba(96,165,250,0.35);
      background: rgba(96,165,250,0.08);
    }

    .answer-key {
      width: 46px;
      height: 46px;
      display: grid;
      place-items: center;
      border-radius: 14px;
      background: linear-gradient(180deg, rgba(96,165,250,0.24), rgba(103,232,249,0.16));
      border: 1px solid rgba(96,165,250,0.22);
      font-family: "Space Grotesk", sans-serif;
      font-weight: 700;
    }

    .answer-text {
      line-height: 1.65;
      color: #dce8f6;
      padding-top: 2px;
    }

    .result-box.success {
      border-color: rgba(52,211,153,0.25);
      background: rgba(52,211,153,0.09);
    }

    .result-box.fail {
      border-color: rgba(251,113,133,0.24);
      background: rgba(251,113,133,0.09);
    }

    .action-btn,
    .restart-btn {
      width: 100%;
      padding: 16px 18px;
      font-weight: 800;
      color: white;
      background: linear-gradient(135deg, #38bdf8, #2dd4bf);
      box-shadow: 0 18px 34px rgba(56, 189, 248, 0.22);
    }

    .action-btn.secondary {
      background: rgba(255,255,255,0.08);
      box-shadow: none;
      border: 1px solid rgba(255,255,255,0.08);
    }

    .ending {
      position: absolute;
      inset: 0;
      display: none;
      place-items: center;
      padding: 24px;
      background: rgba(2, 6, 12, 0.88);
      z-index: 60;
    }

    .ending.show {
      display: grid;
    }

    .travel-fx {
      position: absolute;
      inset: 0;
      opacity: 0;
      pointer-events: none;
      z-index: 40;
      background:
        radial-gradient(circle at 50% 45%, rgba(255,255,255,0.15), transparent 18%),
        linear-gradient(180deg, rgba(103,232,249,0), rgba(103,232,249,0.12), rgba(103,232,249,0));
      transition: opacity 0.35s ease;
    }

    .travel-fx::before,
    .travel-fx::after {
      content: "";
      position: absolute;
      inset: 0;
      background-repeat: no-repeat;
    }

    .travel-fx::before {
      background-image:
        linear-gradient(transparent, rgba(255,255,255,0.6), transparent),
        linear-gradient(transparent, rgba(255,255,255,0.45), transparent);
      background-size: 4px 100%, 4px 100%;
      background-position: 35% 0, 65% 0;
      filter: blur(1px);
    }

    .travel-fx::after {
      background: radial-gradient(circle at center, rgba(103,232,249,0.16), transparent 42%);
      animation: pulse-travel 1s ease-in-out infinite alternate;
    }

    .game.room-transition .travel-fx {
      opacity: 1;
    }

    @keyframes pulse-travel {
      from { transform: scale(0.9); opacity: 0.35; }
      to { transform: scale(1.08); opacity: 0.75; }
    }

    .ending-card {
      width: min(760px, 100%);
      padding: 34px;
      text-align: center;
      border-radius: 30px;
      border: 1px solid rgba(255,255,255,0.1);
      background: linear-gradient(180deg, rgba(10, 23, 40, 0.96), rgba(5, 12, 22, 0.94));
      box-shadow: 0 26px 70px rgba(0,0,0,0.48);
    }

    .ending-card p {
      color: var(--muted);
      line-height: 1.75;
    }

    .ending-score {
      margin: 22px 0;
      font-family: "Space Grotesk", sans-serif;
      font-size: clamp(3.2rem, 10vw, 5.2rem);
      font-weight: 800;
    }

    @media (max-width: 1000px) {
      .hud {
        flex-direction: column;
      }

      .hud-block.main,
      .hud-block.side,
      .hud-stats {
        max-width: min(100%, calc(100vw - 36px));
        width: min(100%, calc(100vw - 36px));
      }

      .hotspot {
        width: 48px;
        height: 48px;
      }
    }

    @media (max-width: 760px) {
      html, body {
        overflow: auto;
      }

      .game {
        min-height: 100vh;
        height: 980px;
      }

      .room {
        min-height: 980px;
      }

      .light {
        width: 100px;
      }

      .desk { width: 170px; height: 96px; }
      .terminal { width: 150px; height: 120px; }
      .bed { width: 176px; height: 86px; }
    }
  </style>
</head>
<body>
  <main class="game">
    <section class="hud">
      <div class="hud-block main">
        <div class="hud-kicker"><span class="hud-dot"></span> Escape Room Simulation</div>
        <h1 id="roomTitle">Raum 1: Schmerzentstehung & Schmerzarten</h1>
        <p id="roomSubtitle">Bewege deinen Avatar durch den Raum, untersuche die Stationen und öffne die Tür mit der richtigen klinischen Entscheidung.</p>
      </div>

      <div class="hud-block side">
        <div class="hud-kicker">Status</div>
        <div class="hud-stats">
          <div class="hud-stat"><span>Fortschritt</span><strong id="roomBadge">Raum 1 / 5</strong></div>
          <div class="hud-stat"><span>Türstatus</span><strong id="doorState">Verriegelt</strong></div>
          <div class="wellbeing-card">
            <div class="wellbeing-head">
              <div class="wellbeing-label">Patienten-Wohlbefinden</div>
              <div class="wellbeing-value" id="wellbeingValue">100%</div>
            </div>
            <div class="wellbeing-track">
              <div class="wellbeing-fill" id="wellbeingBar"></div>
            </div>
            <div class="wellbeing-note">Zeigt direkt an, wie stabil der Patient aktuell ist.</div>
          </div>
        </div>
      </div>
    </section>

    <section class="room" id="room">
      <div class="scene" id="scene">
        <div class="back-wall"></div>
        <div class="wall left"></div>
        <div class="wall right"></div>

        <div class="ceiling-lights">
          <div class="light"></div>
          <div class="light"></div>
          <div class="light"></div>
        </div>

        <div class="door-glow"></div>
        <div class="door-frame"></div>
        <div class="floor"></div>

        <div class="object desk">
          <div class="desk-file"></div>
        </div>
        <div class="object terminal"></div>
        <div class="object bed"></div>

        <button class="hotspot" aria-label="Fallakte" data-hotspot="briefing" style="left: 17%; top: 56%;"><span class="marker-name">Fallakte</span></button>
        <button class="hotspot" aria-label="Frage" data-hotspot="question" style="right: 18%; top: 67%;"><span class="marker-name">Frage</span></button>
        <button class="hotspot" aria-label="Türe" data-hotspot="door" style="left: calc(50% - 28px); top: 34%;"><span class="marker-name">Türe</span></button>

        <div class="player-shadow" id="playerShadow"></div>
        <div class="player" id="player">
          <div class="player-head">
            <div class="player-hair"></div>
            <div class="player-face"></div>
          </div>
          <div class="player-arm left"></div>
          <div class="player-arm right"></div>
          <div class="player-torso"></div>
          <div class="player-leg left"><div class="player-foot"></div></div>
          <div class="player-leg right"><div class="player-foot"></div></div>
        </div>
        <div class="player-label" id="playerLabel">Pflegekraft</div>

        <div class="hint-bar" id="hintBar">Steuerung mit WASD oder Pfeiltasten. Laufe nahe an eine Station heran und klicke sie an.</div>
      </div>
    </section>

    <div class="overlay" id="overlay">
      <div class="modal" id="modal"></div>
    </div>

    <div class="travel-fx" id="travelFx"></div>

    <div class="ending" id="ending">
      <div class="ending-card" id="endingCard"></div>
    </div>
  </main>

  <script>
    const rooms = [
      {
        themeGlow: "rgba(96, 165, 250, 0.18)",
        title: "Raum 1: Schmerzart erkennen",
        subtitle: "Du musst die Schmerzqualität korrekt deuten, um die erste Tür zu öffnen.",
        description: "Ein Patient beschreibt seine Schmerzen als 'brennend, elektrisierend und einschiessend'. Die Schmerzen strahlen ins Bein aus.",
        question: "Welche Schmerzart liegt vor?",
        options: [
          { text: "Neuropathischer Schmerz", correct: true, feedback: "Richtig! Diese Schmerzqualität ist typisch für Nervenschädigungen." },
          { text: "Nozizeptiver Schmerz", correct: false, feedback: "Falsch. Dieser ist eher dumpf, drückend oder stechend." },
          { text: "Psychosomatischer Schmerz", correct: false, feedback: "Falsch. Die Symptome sprechen klar für eine körperliche Ursache." }
        ]
      },
      {
        themeGlow: "rgba(103, 232, 249, 0.18)",
        title: "Raum 2: Schmerzassessment",
        subtitle: "Hier zählt die richtige Wahl des Assessment-Instruments.",
        description: "Du führst bei einem kognitiv fitten Patienten ein Schmerzassessment durch.",
        question: "Welches Instrument ist am besten geeignet?",
        options: [
          { text: "BESD-Skala", correct: false, feedback: "Falsch. Diese wird bei Menschen mit Demenz eingesetzt." },
          { text: "Numerische Rating-Skala (NRS)", correct: true, feedback: "Richtig! Standardinstrument zur Schmerzmessung." },
          { text: "Beobachtung ohne Skala", correct: false, feedback: "Falsch. Subjektive Einschätzung des Patienten ist entscheidend." }
        ]
      },
      {
        themeGlow: "rgba(251, 191, 36, 0.18)",
        title: "Raum 3: Opioidtherapie",
        subtitle: "Die dritte Tür verlangt Wissen über typische Opioid-Nebenwirkungen.",
        description: "Ein Patient erhält Morphin gegen starke Schmerzen.",
        question: "Welche Nebenwirkung tritt fast immer auf?",
        options: [
          { text: "Atembeschleunigung", correct: false, feedback: "Falsch. Eher droht eine Atemdepression." },
          { text: "Obstipation", correct: true, feedback: "Richtig! Sehr typische Nebenwirkung → Prophylaxe nötig." },
          { text: "Heisshunger", correct: false, feedback: "Falsch. Das ist nicht typisch für Opioide." }
        ]
      },
      {
        themeGlow: "rgba(244, 114, 182, 0.16)",
        title: "Raum 4: Lymphödem",
        subtitle: "Hier geht es um sicheres Handeln nach Lymphknotenentfernung.",
        description: "Nach einer Mastektomie wurden Lymphknoten entfernt.",
        question: "Was ist wichtig?",
        options: [
          { text: "Blutdruck am betroffenen Arm messen", correct: false, feedback: "Falsch! Gefahr eines Lymphödems." },
          { text: "Arm hochlagern und schützen", correct: true, feedback: "Richtig! Schutz und Hochlagerung sind zentral." },
          { text: "Kräftig massieren", correct: false, feedback: "Falsch. Nur spezielle Lymphdrainage ist erlaubt." }
        ]
      },
      {
        themeGlow: "rgba(167, 139, 250, 0.18)",
        title: "Raum 5: Ethik",
        subtitle: "Die Tür reagiert nur, wenn du das ethische Kernprinzip richtig benennst.",
        description: "Ein Patient lehnt eine lebensverlängernde Therapie ab.",
        question: "Welches Prinzip steht im Vordergrund?",
        options: [
          { text: "Autonomie", correct: true, feedback: "Richtig! Der Patient entscheidet selbst." },
          { text: "Gerechtigkeit", correct: false, feedback: "Falsch. Hier geht es nicht um Verteilung." },
          { text: "Nicht-Schädigung", correct: false, feedback: "Falsch. Autonomie steht hier im Vordergrund." }
        ]
      },
      {
        themeGlow: "rgba(34, 197, 94, 0.16)",
        title: "Raum 6: Schmerzbeurteilung",
        subtitle: "In diesem Raum musst du Schmerz professionell einschätzen.",
        description: "Ein Patient wirkt ruhig, gibt aber NRS 7 an.",
        question: "Was ist korrekt?",
        options: [
          { text: "Die Beobachtung ist wichtiger", correct: false, feedback: "Falsch. Schmerz ist subjektiv." },
          { text: "Der Patient übertreibt", correct: false, feedback: "Falsch. Schmerz darf nicht relativiert werden." },
          { text: "Schmerz ist subjektiv und ernst zu nehmen", correct: true, feedback: "Richtig! Aussage des Patienten zählt." }
        ]
      },
      {
        themeGlow: "rgba(59, 130, 246, 0.18)",
        title: "Raum 7: Kommunikation",
        subtitle: "Hier öffnet sich die Tür nur mit einer professionellen Gesprächsführung.",
        description: "Ein Patient sagt: 'Ich will keine Schmerzmittel mehr.'",
        question: "Was ist die beste Reaktion?",
        options: [
          { text: "Nach Gründen fragen", correct: true, feedback: "Richtig! Gespräch führen ist zentral." },
          { text: "Medikamente sofort absetzen", correct: false, feedback: "Falsch. Erst klären!" },
          { text: "Überreden", correct: false, feedback: "Falsch. Druck ist nicht professionell." }
        ]
      },
      {
        themeGlow: "rgba(14, 165, 233, 0.18)",
        title: "Raum 8: Bedarfsmedikation",
        subtitle: "Du musst wissen, wann Bedarfsmedikation korrekt gegeben werden darf.",
        description: "Ein Patient hat Bedarfsmedikation verordnet.",
        question: "Wann darfst du sie geben?",
        options: [
          { text: "Nur bei sichtbaren Schmerzen", correct: false, feedback: "Falsch. Schmerz ist subjektiv." },
          { text: "Wenn verordnet und Patient Schmerzen angibt", correct: true, feedback: "Richtig!" },
          { text: "Nur wenn Arzt anwesend ist", correct: false, feedback: "Falsch." }
        ]
      },
      {
        themeGlow: "rgba(251, 113, 133, 0.18)",
        title: "Raum 9: Atemdepression",
        subtitle: "Jetzt geht es um schnelles Erkennen einer gefährlichen Situation.",
        description: "Ein Patient unter Morphin wirkt schläfrig und atmet langsam.",
        question: "Was ist deine erste Handlung?",
        options: [
          { text: "Patient schlafen lassen", correct: false, feedback: "Falsch. Gefahr!" },
          { text: "Mehr Schmerzmittel geben", correct: false, feedback: "Falsch." },
          { text: "Atemfrequenz kontrollieren und melden", correct: true, feedback: "Richtig! Notfall erkennen." }
        ]
      },
      {
        themeGlow: "rgba(16, 185, 129, 0.18)",
        title: "Raum 10: Hygiene",
        subtitle: "Hier schützt du einen immungeschwächten Patienten.",
        description: "Ein Patient ist immungeschwächt.",
        question: "Was ist besonders wichtig?",
        options: [
          { text: "Viele Besucher zulassen", correct: false, feedback: "Falsch." },
          { text: "Konsequente Händehygiene", correct: true, feedback: "Richtig!" },
          { text: "Keine Schutzmassnahmen nötig", correct: false, feedback: "Falsch." }
        ]
      },
      {
        themeGlow: "rgba(245, 158, 11, 0.18)",
        title: "Raum 11: Ernährung",
        subtitle: "Die Tür öffnet sich, wenn du die passende Ernährungsstrategie wählst.",
        description: "Ein Patient hat wenig Appetit.",
        question: "Was ist sinnvoll?",
        options: [
          { text: "Essen erzwingen", correct: false, feedback: "Falsch." },
          { text: "Kleine, häufige Mahlzeiten", correct: true, feedback: "Richtig!" },
          { text: "Nur grosse Mahlzeiten", correct: false, feedback: "Falsch." }
        ]
      },
      {
        themeGlow: "rgba(45, 212, 191, 0.18)",
        title: "Raum 12: Mobilisation",
        subtitle: "In diesem Raum ist pflegerische Aktivierung gefragt.",
        description: "Ein Patient hat Schmerzen und bleibt im Bett.",
        question: "Was ist richtig?",
        options: [
          { text: "Schonende Mobilisation fördern", correct: true, feedback: "Richtig!" },
          { text: "Strikte Bettruhe", correct: false, feedback: "Falsch." },
          { text: "Bewegung vermeiden", correct: false, feedback: "Falsch." }
        ]
      },
      {
        themeGlow: "rgba(168, 85, 247, 0.18)",
        title: "Raum 13: Delir",
        subtitle: "Die nächste Tür testet dein Wissen zu akuten Verwirrtheitszuständen.",
        description: "Ein Patient ist plötzlich verwirrt.",
        question: "Was spricht für ein Delir?",
        options: [
          { text: "Langsamer Beginn", correct: false, feedback: "Falsch." },
          { text: "Immer gleichbleibend", correct: false, feedback: "Falsch." },
          { text: "Plötzlicher Beginn und wechselnd", correct: true, feedback: "Richtig!" }
        ]
      },
      {
        themeGlow: "rgba(6, 182, 212, 0.18)",
        title: "Raum 14: Teamarbeit",
        subtitle: "Hier zählt, dass du Beschwerden nicht für dich behältst.",
        description: "Du bemerkst zunehmende Schmerzen.",
        question: "Was tust du?",
        options: [
          { text: "Pflegefachperson informieren", correct: true, feedback: "Richtig!" },
          { text: "Abwarten", correct: false, feedback: "Falsch." },
          { text: "Erst morgen melden", correct: false, feedback: "Falsch." }
        ]
      },
      {
        themeGlow: "rgba(239, 68, 68, 0.18)",
        title: "Raum 15: FINAL",
        subtitle: "Im letzten Raum musst du einen opioidbezogenen Notfall richtig priorisieren.",
        description: "Patient mit Morphin: schläfrig, Atemfrequenz 8/min. Ehefrau besorgt.",
        question: "Was ist deine prioritäre Handlung?",
        options: [
          { text: "Medikamente absetzen", correct: false, feedback: "Falsch." },
          { text: "Atemfrequenz kontrollieren und sofort melden", correct: true, feedback: "Richtig! Notfall!" },
          { text: "Nur beobachten", correct: false, feedback: "Falsch." }
        ]
      }
    ];

    const state = {
      roomIndex: 0,
      wellbeing: 100,
      solved: false,
      briefingRead: false,
      canMove: true,
      transitioning: false,
      activeHotspot: null,
      avatar: { x: 50, y: 74 },
      keys: {}
    };

    const gameEl = document.querySelector(".game");
    const roomEl = document.getElementById("room");
    const roomTitle = document.getElementById("roomTitle");
    const roomSubtitle = document.getElementById("roomSubtitle");
    const wellbeingValue = document.getElementById("wellbeingValue");
    const wellbeingBar = document.getElementById("wellbeingBar");
    const roomBadge = document.getElementById("roomBadge");
    const doorState = document.getElementById("doorState");
    const hintBar = document.getElementById("hintBar");
    const player = document.getElementById("player");
    const playerShadow = document.getElementById("playerShadow");
    const playerLabel = document.getElementById("playerLabel");
    const overlay = document.getElementById("overlay");
    const modal = document.getElementById("modal");
    const travelFx = document.getElementById("travelFx");
    const ending = document.getElementById("ending");
    const endingCard = document.getElementById("endingCard");
    const hotspots = [...document.querySelectorAll(".hotspot")];

    function renderHUD() {
      const room = rooms[state.roomIndex];
      document.documentElement.style.setProperty("--room-glow", room.themeGlow);
      roomTitle.textContent = room.title;
      roomSubtitle.textContent = room.subtitle;
      roomBadge.textContent = `Raum ${state.roomIndex + 1} / ${rooms.length}`;
      doorState.textContent = state.solved ? "Entriegelt" : "Verriegelt";
      wellbeingValue.textContent = `${state.wellbeing}%`;
      wellbeingBar.style.width = `${state.wellbeing}%`;

      if (state.wellbeing > 60) {
        wellbeingValue.style.color = "#dffcf1";
        wellbeingBar.style.background = "linear-gradient(90deg, #34d399, #67e8f9)";
      } else if (state.wellbeing > 30) {
        wellbeingValue.style.color = "#fde9b1";
        wellbeingBar.style.background = "linear-gradient(90deg, #fbbf24, #fde68a)";
      } else {
        wellbeingValue.style.color = "#ffd2dc";
        wellbeingBar.style.background = "linear-gradient(90deg, #fb7185, #ef4444)";
      }
    }

    function positionPlayer() {
      const depthScale = 0.72 + ((state.avatar.y - 25) / 63) * 0.55;
      player.style.left = `${state.avatar.x}%`;
      player.style.top = `${state.avatar.y}%`;
      player.style.transform = `translate(-50%, -50%) scale(${depthScale})`;
      playerShadow.style.left = `${state.avatar.x}%`;
      playerShadow.style.top = `${Math.min(88, state.avatar.y + 8)}%`;
      playerShadow.style.transform = `translate(-50%, -50%) scale(${0.84 + ((state.avatar.y - 25) / 63) * 0.36})`;
      playerLabel.style.left = `${state.avatar.x}%`;
      playerLabel.style.top = `${state.avatar.y - 10}%`;
    }

    function getCenter(element) {
      const roomRect = roomEl.getBoundingClientRect();
      const rect = element.getBoundingClientRect();
      return {
        x: ((rect.left + rect.width / 2 - roomRect.left) / roomRect.width) * 100,
        y: ((rect.top + rect.height / 2 - roomRect.top) / roomRect.height) * 100
      };
    }

    function updateHotspots() {
      let nearest = null;
      let minDistance = Infinity;

      hotspots.forEach((spot) => {
        const isQuestion = spot.dataset.hotspot === "question";
        spot.classList.toggle("disabled", isQuestion && !state.briefingRead);

        const center = getCenter(spot);
        const dx = center.x - state.avatar.x;
        const dy = center.y - state.avatar.y;
        const distance = Math.hypot(dx, dy);
        const near = distance < 16;
        spot.classList.toggle("near", near);

        if (distance < minDistance) {
          minDistance = distance;
          nearest = spot;
        }
      });

      state.activeHotspot = minDistance < 16 ? nearest.dataset.hotspot : null;

      if (state.activeHotspot === "briefing") {
        hintBar.textContent = "Fallakte in Reichweite. Klicke den Marker an, um den Fall zu lesen.";
      } else if (state.activeHotspot === "question") {
        hintBar.textContent = !state.briefingRead
          ? "Lies zuerst die Fallakte. Danach wird die Frage freigeschaltet."
          : state.solved
            ? "Terminal gesichert. Deine Entscheidung steht. Du kannst jetzt zur Tür."
            : "Terminal in Reichweite. Klicke den Marker an, um die Frage zu beantworten.";
      } else if (state.activeHotspot === "door") {
        hintBar.textContent = state.solved
          ? "Die Tür ist offen. Klicke den Ausgang an, um den nächsten Raum zu betreten."
          : "Die Tür bleibt verriegelt. Löse zuerst die Aufgabe am Terminal.";
      } else {
        hintBar.textContent = "Steuerung mit WASD oder Pfeiltasten. Die leuchtenden Bodenmarker zeigen interaktive Stellen.";
      }
    }

    function openOverlay(content) {
      modal.innerHTML = content;
      overlay.classList.add("show");
      state.canMove = false;
      player.classList.remove("walking");
    }

    function closeOverlay() {
      overlay.classList.remove("show");
      modal.innerHTML = "";
      state.canMove = true;
    }

    function showBriefing() {
      const room = rooms[state.roomIndex];
      state.briefingRead = true;
      updateHotspots();
      openOverlay(`
        <div class="modal-head">
          <div>
            <div class="tiny-label">Fallakte geöffnet</div>
            <h2>${room.title}</h2>
          </div>
          <button class="close-btn" data-close>&times;</button>
        </div>
        <div class="info-box">
          <div class="tiny-label">Fallsituation</div>
          <p>${room.description}</p>
        </div>
        <button class="action-btn secondary" data-close>Zurück in den Raum</button>
      `);
    }

    function showQuestion() {
      if (!state.briefingRead) {
        openOverlay(`
          <div class="modal-head">
            <div>
              <div class="tiny-label">Zugriff gesperrt</div>
              <h2>Lies zuerst die Fallsituation</h2>
            </div>
            <button class="close-btn" data-close>&times;</button>
          </div>
          <div class="result-box fail">
            <div class="tiny-label">Hinweis</div>
            <p>Öffne zuerst die Fallakte. Erst wenn du die Fallsituation gelesen hast, wird die Frage freigegeben.</p>
          </div>
          <button class="action-btn secondary" data-close>Zurück in den Raum</button>
        `);
        return;
      }

      const room = rooms[state.roomIndex];
      openOverlay(`
        <div class="modal-head">
          <div>
            <div class="tiny-label">Entscheidungsterminal</div>
            <h2>${room.title}</h2>
          </div>
          <button class="close-btn" data-close>&times;</button>
        </div>
        <div class="info-box">
          <div class="tiny-label">Fallsituation</div>
          <p>${room.description}</p>
        </div>
        <div class="info-box">
          <div class="tiny-label">Rätsel / Entscheidung</div>
          <p>${room.question}</p>
        </div>
        <div class="answers">
          ${room.options.map((option, index) => `
            <button class="answer-btn" data-answer="${index}">
              <div class="answer-key">${String.fromCharCode(65 + index)}</div>
              <div class="answer-text">${option.text}</div>
            </button>
          `).join("")}
        </div>
      `);
    }

    function applyAnswer(index) {
      const option = rooms[state.roomIndex].options[index];

      if (option.correct) {
        state.solved = true;
        state.wellbeing = Math.min(100, state.wellbeing + 5);
      } else {
        state.wellbeing = Math.max(0, state.wellbeing - 20);
      }

      renderHUD();
      updateHotspots();

      openOverlay(`
        <div class="modal-head">
          <div>
            <div class="tiny-label">${option.correct ? "Türmechanismus reagiert" : "Warnsystem aktiv"}</div>
            <h2>${option.correct ? "Die Tür entriegelt sich" : "Die Tür bleibt verschlossen"}</h2>
          </div>
          <button class="close-btn" data-close>&times;</button>
        </div>
        <div class="result-box ${option.correct ? "success" : "fail"}">
          <div class="tiny-label">${option.correct ? "Korrekte Entscheidung" : "Falsche Entscheidung"}</div>
          <p>${option.feedback}</p>
        </div>
        <button class="action-btn ${option.correct ? "" : "secondary"}" data-close>
          ${option.correct ? "Zurück in den Raum" : "Erneut versuchen"}
        </button>
      `);

      if (state.wellbeing <= 0) {
        showEnding(false);
      }
    }

    function nextRoom() {
      if (!state.solved) {
        openOverlay(`
          <div class="modal-head">
            <div>
              <div class="tiny-label">Zugang verweigert</div>
              <h2>Die Tür ist noch verriegelt</h2>
            </div>
            <button class="close-btn" data-close>&times;</button>
          </div>
          <div class="result-box fail">
            <div class="tiny-label">Hinweis</div>
            <p>Beantworte zuerst die Frage am Terminal korrekt. Erst danach öffnet sich der Ausgang.</p>
          </div>
          <button class="action-btn secondary" data-close>Weiter spielen</button>
        `);
        return;
      }

      if (state.roomIndex === rooms.length - 1) {
        animateTransition(true);
        return;
      }
      animateTransition(false);
    }

    function animateAvatarTo(targetX, targetY, duration = 800) {
      return new Promise((resolve) => {
        const startX = state.avatar.x;
        const startY = state.avatar.y;
        const start = performance.now();
        player.classList.add("walking");

        function frame(now) {
          const progress = Math.min(1, (now - start) / duration);
          const eased = 1 - Math.pow(1 - progress, 3);

          state.avatar.x = startX + (targetX - startX) * eased;
          state.avatar.y = startY + (targetY - startY) * eased;
          positionPlayer();
          updateHotspots();

          if (progress < 1) {
            requestAnimationFrame(frame);
          } else {
            player.classList.remove("walking");
            resolve();
          }
        }

        requestAnimationFrame(frame);
      });
    }

    async function animateTransition(isFinalRoom) {
      if (state.transitioning) return;

      state.transitioning = true;
      state.canMove = false;
      overlay.classList.remove("show");
      modal.innerHTML = "";
      hintBar.textContent = isFinalRoom
        ? "Du läufst zum finalen Ausgang ..."
        : "Du läufst durch die Tür in den nächsten Raum ...";

      await animateAvatarTo(50, 31, 900);

      gameEl.classList.add("room-transition");
      travelFx.setAttribute("aria-hidden", "true");

      setTimeout(() => {
        if (isFinalRoom) {
          showEnding(true);
          gameEl.classList.remove("room-transition");
          state.transitioning = false;
          return;
        }

        state.roomIndex += 1;
        state.solved = false;
        state.briefingRead = false;
        state.avatar = { x: 50, y: 76 };
        renderHUD();
        positionPlayer();
        updateHotspots();
      }, 420);

      setTimeout(() => {
        gameEl.classList.remove("room-transition");
        state.canMove = true;
        state.transitioning = false;
      }, 980);
    }

    function showEnding(victory) {
      overlay.classList.remove("show");
      state.canMove = false;
      ending.classList.add("show");
      endingCard.innerHTML = victory
        ? `
          <div class="tiny-label" style="margin: 0 auto 14px;">Mission erfüllt</div>
          <h2>Escape Room geschafft</h2>
          <p>Du hast alle Räume gelöst und den Patienten mit guten klinischen Entscheidungen stabil durch die Simulation geführt.</p>
          <div class="ending-score">${state.wellbeing}%</div>
          <p>Finales Patienten-Wohlbefinden</p>
          <button class="restart-btn" onclick="window.location.reload()">Noch einmal spielen</button>
        `
        : `
          <div class="tiny-label" style="margin: 0 auto 14px;">Simulation gescheitert</div>
          <h2>Der Patient ist instabil geworden</h2>
          <p>Das Wohlbefinden ist auf 0% gefallen. Starte neu und löse die Räume präziser.</p>
          <div class="ending-score">${state.wellbeing}%</div>
          <p>Finales Patienten-Wohlbefinden</p>
          <button class="restart-btn" onclick="window.location.reload()">Neu starten</button>
        `;
    }

    function interactWith(spot) {
      if (!spot) return;
      if (spot === "briefing") showBriefing();
      if (spot === "question") showQuestion();
      if (spot === "door") nextRoom();
    }

    function movePlayer() {
      if (!state.canMove || state.transitioning || ending.classList.contains("show")) return;

      let dx = 0;
      let dy = 0;
      const speed = 0.42;

      if (state.keys["ArrowUp"] || state.keys["w"] || state.keys["W"]) dy -= speed;
      if (state.keys["ArrowDown"] || state.keys["s"] || state.keys["S"]) dy += speed;
      if (state.keys["ArrowLeft"] || state.keys["a"] || state.keys["A"]) dx -= speed;
      if (state.keys["ArrowRight"] || state.keys["d"] || state.keys["D"]) dx += speed;

      const walking = dx !== 0 || dy !== 0;
      player.classList.toggle("walking", walking);

      if (!walking) return;

      state.avatar.x = Math.max(8, Math.min(92, state.avatar.x + dx));
      state.avatar.y = Math.max(25, Math.min(88, state.avatar.y + dy));

      positionPlayer();
      updateHotspots();
    }

    document.addEventListener("keydown", (event) => {
      state.keys[event.key] = true;
      if (event.key === "Enter" && state.activeHotspot) {
        interactWith(state.activeHotspot);
      }
      if (["ArrowUp", "ArrowDown", "ArrowLeft", "ArrowRight", " "].includes(event.key)) {
        event.preventDefault();
      }
    });

    document.addEventListener("keyup", (event) => {
      state.keys[event.key] = false;
      if (!Object.values(state.keys).some(Boolean)) {
        player.classList.remove("walking");
      }
    });

    hotspots.forEach((spot) => {
      spot.addEventListener("click", () => {
        if (state.activeHotspot === spot.dataset.hotspot) {
          interactWith(spot.dataset.hotspot);
          return;
        }

        openOverlay(`
          <div class="modal-head">
            <div>
              <div class="tiny-label">Noch zu weit entfernt</div>
              <h2>Gehe näher an die Station</h2>
            </div>
            <button class="close-btn" data-close>&times;</button>
          </div>
          <div class="result-box fail">
            <div class="tiny-label">Hinweis</div>
            <p>Bewege den Avatar in die Nähe des Objekts. Sobald die Station markiert ist, kannst du sie aktivieren.</p>
          </div>
          <button class="action-btn secondary" data-close>Zurück ins Spiel</button>
        `);
      });
    });

    overlay.addEventListener("click", (event) => {
      if (event.target === overlay) closeOverlay();
    });

    modal.addEventListener("click", (event) => {
      const close = event.target.closest("[data-close]");
      if (close) {
        closeOverlay();
        return;
      }

      const answer = event.target.closest("[data-answer]");
      if (answer) {
        applyAnswer(Number(answer.dataset.answer));
      }
    });

    function loop() {
      movePlayer();
      requestAnimationFrame(loop);
    }

    renderHUD();
    positionPlayer();
    updateHotspots();
    loop();
  </script>
</body>
</html>
