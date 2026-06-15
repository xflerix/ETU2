
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no, viewport-fit=cover">
<style>
  html { touch-action: manipulation; }
</style>
<script>
  // Disable pinch-to-zoom and double-tap zoom
  document.addEventListener('gesturestart', function(e){ e.preventDefault(); }, {passive:false});
  document.addEventListener('gesturechange', function(e){ e.preventDefault(); }, {passive:false});
  document.addEventListener('gestureend', function(e){ e.preventDefault(); }, {passive:false});
  var lastTouchEnd = 0;
  document.addEventListener('touchend', function(e){
    var now = Date.now();
    if (now - lastTouchEnd <= 300) { e.preventDefault(); }
    lastTouchEnd = now;
  }, {passive:false});
  document.addEventListener('touchmove', function(e){
    if (e.touches.length > 1) e.preventDefault();
  }, {passive:false});
  document.addEventListener('wheel', function(e){
    if (e.ctrlKey) e.preventDefault();
  }, {passive:false});
  document.addEventListener('keydown', function(e){
    if ((e.ctrlKey || e.metaKey) && (e.key==='+' || e.key==='-' || e.key==='=' || e.key==='0')) {
      e.preventDefault();
    }
  });
</script>
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#080c14" id="metaThemeColor">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="ETU">
<meta name="application-name" content="ETU">
<meta name="description" content="English Trainer Ultimate — учи английские слова каждый день">
<link id="pwaManifest" rel="manifest" href="">
<link id="appleIcon" rel="apple-touch-icon" href="">
<title>English Trainer Ultimate</title>
<style>
/* ════════════════════════════════════
   ПЕРЕМЕННЫЕ И БАЗА
════════════════════════════════════ */
:root {
  --bg: #080c14;
  --bg2: #0d1525;
  --card: rgba(255,255,255,0.06);
  --card-border: rgba(255,255,255,0.10);
  --glass: rgba(255,255,255,0.04);
  --text: #f0f4ff;
  --text2: #8899bb;
  --accent: #4f8eff;
  --accent2: #7c5cfc;
  --ok: #22d88f;
  --err: #ff4f6d;
  --warn: #ffc247;
  --muted: rgba(255,255,255,0.08);
  --muted2: rgba(255,255,255,0.05);
  --r: 20px;
  --r2: 14px;
  --t: all 0.22s cubic-bezier(0.4,0,0.2,1);
  --glow-accent: 0 0 30px rgba(79,142,255,0.25);
  --glow-ok: 0 0 30px rgba(34,216,143,0.3);
  --glow-err: 0 0 30px rgba(255,79,109,0.3);
}
body.light {
  --bg: #f0f4ff;
  --bg2: #e4ebfa;
  --card: rgba(255,255,255,0.85);
  --card-border: rgba(100,130,200,0.2);
  --glass: rgba(255,255,255,0.6);
  --text: #0f1a35;
  --text2: #5566aa;
  --accent: #2563eb;
  --accent2: #6d28d9;
  --ok: #059669;
  --err: #dc2626;
  --warn: #d97706;
  --muted: rgba(0,0,0,0.06);
  --muted2: rgba(0,0,0,0.04);
}

*, *::before, *::after { box-sizing:border-box; margin:0; padding:0; }
button, .mode-card, .diff-option, .letter-tile, .answer-tile, .option-btn, .icon-btn {
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}

body {
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  min-height: 100dvh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  padding: 16px 12px 40px;
  padding-left: max(12px, env(safe-area-inset-left));
  padding-right: max(12px, env(safe-area-inset-right));
  padding-bottom: max(40px, env(safe-area-inset-bottom));
  overflow-x: hidden;
  transition: background .4s, color .3s;
  -webkit-tap-highlight-color: transparent;
  touch-action: manipulation;
}

/* Анимированный фон */
.bg-orbs {
  position: fixed; inset: 0; pointer-events: none; z-index: 0; overflow: hidden;
}
.bg-orb {
  position: absolute; border-radius: 50%; filter: blur(80px); opacity: 0.12;
  animation: orbFloat linear infinite;
}
.bg-orb:nth-child(1){ width:600px;height:600px;background:#4f8eff;top:-200px;left:-100px;animation-duration:20s; }
.bg-orb:nth-child(2){ width:500px;height:500px;background:#7c5cfc;bottom:-150px;right:-100px;animation-duration:26s;animation-direction:reverse; }
.bg-orb:nth-child(3){ width:300px;height:300px;background:#22d88f;top:40%;left:60%;animation-duration:32s; }
@keyframes orbFloat {
  0%,100%{ transform:translate(0,0) scale(1); }
  33%{ transform:translate(40px,-30px) scale(1.05); }
  66%{ transform:translate(-20px,40px) scale(0.97); }
}
body.light .bg-orb { opacity:0.07; }

.container { max-width:620px; width:100%; position:relative; z-index:1; }

/* ════════════════════════════════════
   ШАПКА
════════════════════════════════════ */
header {
  display: flex; justify-content: space-between; align-items: center;
  margin-bottom: 18px; padding: 0 2px;
}
.logo {
  font-size: 22px; font-weight: 900; letter-spacing: -0.5px;
  background: linear-gradient(135deg, #4f8eff, #a78bfa);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  display: flex; align-items: center; gap: 8px;
}
.logo-icon { font-size: 28px; -webkit-text-fill-color: initial; }
.header-actions { display: flex; gap: 8px; }
.icon-btn {
  background: var(--card); color: var(--text);
  border: 1px solid var(--card-border); padding: 9px 11px;
  border-radius: 12px; cursor: pointer; font-size: 16px; line-height: 1;
  backdrop-filter: blur(10px); transition: var(--t);
}
.icon-btn:hover { transform: scale(1.08); border-color: var(--accent); }

/* ════════════════════════════════════
   КАРТОЧКИ / ЭКРАНЫ
════════════════════════════════════ */
.card {
  background: var(--card); border: 1px solid var(--card-border);
  border-radius: var(--r); padding: 24px; margin-bottom: 16px;
  backdrop-filter: blur(20px); box-shadow: 0 8px 32px rgba(0,0,0,0.2);
  position: relative; overflow: hidden; width: 100%;
  transition: var(--t);
}
.screen {
  display: block;
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  position: absolute;
  top: 0; left: 0; right: 0;
  will-change: transform, opacity;
  transition: none;
}
.screen.active {
  opacity: 1;
  visibility: visible;
  pointer-events: auto;
  position: relative;
  animation: screenIn .3s cubic-bezier(.4,0,.2,1) both;
}
@keyframes screenIn {
  from { opacity:0; transform:translateY(10px) scale(0.99); }
  to   { opacity:1; transform:translateY(0) scale(1); }
}

/* ════════════════════════════════════
   ГЛАВНОЕ МЕНЮ
════════════════════════════════════ */
.menu-top {
  display: flex; align-items: center; gap: 16px; margin-bottom: 20px;
}
.avatar {
  width: 64px; height: 64px; border-radius: 18px;
  background: linear-gradient(135deg, #4f8eff, #7c5cfc);
  display: flex; align-items: center; justify-content: center;
  font-size: 30px; flex-shrink: 0; animation: pulse 3s ease-in-out infinite;
  overflow: hidden;
}
@keyframes pulse { 0%,100%{box-shadow:0 0 0 0 rgba(79,142,255,.4)} 50%{box-shadow:0 0 0 10px rgba(79,142,255,0)} }
.avatar img { width:100%; height:100%; object-fit:cover; border-radius:16px; display:block; }
.avatar-photo-btn {
  display:inline-flex; align-items:center; gap:6px;
  background:rgba(79,142,255,.15); color:var(--accent);
  border:1px solid rgba(79,142,255,.4); border-radius:10px;
  padding:7px 14px; font-size:13px; font-weight:800;
  cursor:pointer; transition:var(--t); margin-bottom:10px;
}
.avatar-photo-btn:hover { background:rgba(79,142,255,.28); }
.avatar-photo-del {
  display:inline-flex; align-items:center; gap:6px;
  background:rgba(255,79,109,.1); color:var(--err);
  border:1px solid rgba(255,79,109,.3); border-radius:10px;
  padding:7px 14px; font-size:13px; font-weight:800;
  cursor:pointer; transition:var(--t); margin-bottom:10px; margin-left:8px;
}
.avatar-photo-del:hover { background:rgba(255,79,109,.22); }
.menu-info { text-align: left; flex: 1; }
.menu-name { font-size: 20px; font-weight: 800; margin-bottom: 4px; }
.rank-badge {
  display: inline-block; padding: 3px 12px; border-radius: 20px;
  font-size: 12px; font-weight: 800; letter-spacing: .5px;
  background: linear-gradient(135deg, #ffc247, #ff8c42); color: #000;
}

/* Уровень и XP */
.level-bar-wrap {
  background: var(--muted); border-radius: 8px; overflow: hidden;
  height: 10px; margin: 10px 0 4px; position: relative;
}
.level-bar {
  height: 100%; border-radius: 8px;
  background: linear-gradient(90deg, #4f8eff, #a78bfa);
  transition: width .5s cubic-bezier(.4,0,.2,1);
  position: relative;
}
.level-bar::after {
  content:''; position:absolute; inset:0; border-radius:8px;
  background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,.3) 50%, transparent 100%);
  background-size: 200% 100%; animation: shimmer 2s infinite;
}
@keyframes shimmer { from{background-position:200% 0} to{background-position:-200% 0} }
.level-txt { font-size: 12px; color: var(--text2); font-weight: 600; margin-bottom: 14px; text-align: right; }

.menu-stats {
  display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; margin-bottom: 16px;
}
.stat-card {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 12px 14px; text-align: left;
}
.stat-card .s-label { font-size: 11px; color: var(--text2); font-weight: 700; text-transform: uppercase; letter-spacing: .5px; }
.stat-card .s-val { font-size: 24px; font-weight: 900; color: var(--accent); margin-top: 2px; }

/* Дневная цель */
.daily-box {
  background: linear-gradient(135deg, rgba(79,142,255,.1), rgba(124,92,252,.1));
  border: 1px dashed rgba(79,142,255,.4);
  border-radius: var(--r2); padding: 14px 16px; margin-bottom: 16px;
}
.daily-row { display: flex; justify-content: space-between; font-size: 13px; font-weight: 700; margin-bottom: 8px; }
.daily-xp { color: var(--accent); }

/* Сложность дневной цели */
.daily-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
.daily-title { font-size: 13px; font-weight: 700; }
.daily-change-btn {
  background: rgba(79,142,255,.15); color: var(--accent);
  border: 1px solid rgba(79,142,255,.3); padding: 3px 10px;
  border-radius: 8px; font-size: 11px; font-weight: 800;
  cursor: pointer; transition: var(--t); letter-spacing: .3px;
}
.daily-change-btn:hover { background: rgba(79,142,255,.28); }
.daily-difficulty-badge {
  display: inline-flex; align-items: center; gap: 4px;
  font-size: 11px; font-weight: 800; padding: 2px 9px;
  border-radius: 8px; margin-bottom: 6px;
}
.daily-progress-row { display: flex; justify-content: space-between; font-size: 12px; font-weight: 700; margin-bottom: 6px; }
.daily-xp-done { color: var(--ok); }
.daily-xp-goal { color: var(--text2); }
.daily-complete-banner {
  background: linear-gradient(135deg, rgba(34,216,143,.15), rgba(79,142,255,.1));
  border: 1px solid rgba(34,216,143,.4); border-radius: 10px;
  padding: 6px 12px; font-size: 12px; font-weight: 800; color: var(--ok);
  text-align: center; margin-top: 8px; display: none;
}

/* Модальное окно выбора сложности */
.diff-modal-overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(8,12,20,.88); backdrop-filter: blur(14px);
  z-index: 200; justify-content: center; align-items: center;
  animation: fadeIn .25s ease;
}
.diff-modal-overlay.open { display: flex; }
.diff-modal {
  background: var(--card); border: 1px solid var(--card-border);
  border-radius: 24px; padding: 28px 24px; max-width: 400px; width: 94%;
  backdrop-filter: blur(20px); box-shadow: 0 30px 60px rgba(0,0,0,.5);
  animation: screenIn .3s ease;
}
.diff-modal-title { font-size: 22px; font-weight: 900; margin-bottom: 4px; }
.diff-modal-sub { font-size: 13px; color: var(--text2); margin-bottom: 20px; }
.diff-options { display: flex; flex-direction: column; gap: 10px; margin-bottom: 20px; }
.diff-option {
  display: flex; align-items: center; gap: 14px;
  background: var(--muted2); border: 2px solid var(--card-border);
  border-radius: var(--r2); padding: 14px 16px;
  cursor: pointer; transition: var(--t);
}
.diff-option:hover { border-color: var(--accent); transform: translateX(4px); }
.diff-option.selected { border-color: var(--accent); background: rgba(79,142,255,.12); }
.diff-option-icon { font-size: 28px; flex-shrink: 0; }
.diff-option-info { flex: 1; text-align: left; }
.diff-option-name { font-size: 15px; font-weight: 800; }
.diff-option-desc { font-size: 12px; color: var(--text2); margin-top: 2px; }
.diff-option-xp {
  font-size: 14px; font-weight: 900; padding: 4px 10px;
  border-radius: 8px; white-space: nowrap; flex-shrink: 0;
}
.diff-modal-close {
  width: 100%; padding: 13px; background: var(--muted); color: var(--text);
  border: 1px solid var(--card-border); border-radius: var(--r2);
  font-weight: 700; font-size: 15px; cursor: pointer; transition: var(--t);
}
.diff-modal-close:hover { border-color: var(--accent); }

/* Цветовые схемы для уровней */
.diff-easy   { background: rgba(34,216,143,.12) !important; }
.diff-easy.selected, .diff-easy:hover { border-color: var(--ok) !important; }
.diff-easy .diff-option-xp { background: rgba(34,216,143,.2); color: var(--ok); }
.diff-medium { background: rgba(79,142,255,.08) !important; }
.diff-medium.selected, .diff-medium:hover { border-color: var(--accent) !important; }
.diff-medium .diff-option-xp { background: rgba(79,142,255,.2); color: var(--accent); }
.diff-hard   { background: rgba(255,194,71,.08) !important; }
.diff-hard.selected, .diff-hard:hover { border-color: var(--warn) !important; }
.diff-hard .diff-option-xp { background: rgba(255,194,71,.2); color: var(--warn); }
.diff-ultra  { background: rgba(255,79,109,.08) !important; }
.diff-ultra.selected, .diff-ultra:hover { border-color: var(--err) !important; }
.diff-ultra .diff-option-xp { background: rgba(255,79,109,.2); color: var(--err); }

/* Цитата дня */
.quote-box {
  background: var(--muted2); border-left: 3px solid var(--accent2);
  border-radius: 0 var(--r2) var(--r2) 0; padding: 12px 14px;
  margin-bottom: 16px; font-style: italic; font-size: 13px; color: var(--text2);
  line-height: 1.5;
}
.quote-box strong { color: var(--text); font-style: normal; }

.menu-btns { display: flex; flex-direction: column; gap: 10px; }

/* ════════════════════════════════════
   ВЫБОР РЕЖИМА
════════════════════════════════════ */
.mode-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 16px; }
.mode-card {
  background: var(--muted2); border: 2px solid var(--card-border);
  border-radius: var(--r2); padding: 20px 14px;
  cursor: pointer; transition: var(--t);
  font-weight: 700; font-size: 15px; text-align: center;
}
.mode-card:hover {
  border-color: var(--accent); transform: translateY(-4px);
  box-shadow: var(--glow-accent);
}
.mode-card .mc-icon { font-size: 36px; display: block; margin-bottom: 8px; }
.mode-card .mc-sub { font-size: 11px; color: var(--text2); font-weight: 500; margin-top: 4px; }
.mode-card.new-badge::after {
  content: 'NEW'; position: absolute; top: 8px; right: 8px;
  background: var(--ok); color: #000; font-size: 9px; font-weight: 900;
  padding: 2px 6px; border-radius: 6px; letter-spacing: .5px;
}
.mode-card { position: relative; }

/* Спринт опции */
.sprint-opts { display: flex; gap: 10px; justify-content: center; margin: 14px 0; }
.sprint-opt-btn {
  background: var(--muted); color: var(--text);
  border: 2px solid transparent; padding: 12px 20px;
  border-radius: 14px; font-size: 18px; font-weight: 800;
  cursor: pointer; transition: var(--t);
}
.sprint-opt-btn:hover, .sprint-opt-btn.active {
  background: var(--accent); color: #fff; border-color: var(--accent);
}

/* ════════════════════════════════════
   ИГРОВОЙ ЭКРАН
════════════════════════════════════ */
.game-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; gap: 8px; }
.pause-btn {
  background: var(--muted); color: var(--text); border: 1px solid var(--card-border);
  border-radius: 10px; padding: 8px 14px; font-weight: 700; font-size: 13px;
  cursor: pointer; transition: var(--t); white-space: nowrap;
}
.pause-btn:hover { background: var(--accent); color: #fff; border-color: var(--accent); }
.stats-bar { display: flex; gap: 6px; flex: 1; justify-content: flex-end; flex-wrap: wrap; }
.mini-stat {
  background: var(--muted); padding: 6px 10px; border-radius: 10px;
  font-size: 12px; font-weight: 700; white-space: nowrap;
}
.mini-stat span { color: var(--accent); }

/* Прогресс */
.prog-wrap { background: var(--muted); height: 7px; border-radius: 4px; overflow: hidden; margin-bottom: 12px; }
.prog-bar {
  height: 100%; width: 0; border-radius: 4px;
  background: linear-gradient(90deg, #4f8eff, #a78bfa);
  transition: width .35s cubic-bezier(.4,0,.2,1);
}
.prog-bar.sprint { background: linear-gradient(90deg, #22d88f, #4f8eff); }

.sprint-counter { font-size: 12px; font-weight: 700; color: var(--text2); margin-bottom: 6px; text-align: right; }

/* Переключатель режима */
.g-mode-selector { display: flex; gap: 6px; margin-bottom: 14px; background: var(--muted2); border-radius: 14px; padding: 4px; }
.g-mode-btn {
  flex: 1; padding: 9px; font-size: 12px; border-radius: 10px;
  background: transparent; color: var(--text2); cursor: pointer;
  font-weight: 700; transition: var(--t); border: none;
}
.g-mode-btn.active { background: var(--accent); color: #fff; }

/* Таймер */
.timer-box {
  font-size: 16px; font-weight: 800; color: var(--warn);
  margin-bottom: 10px; display: flex; align-items: center; justify-content: center; gap: 6px;
}
.timer-ring {
  width: 40px; height: 40px; position: relative; flex-shrink: 0;
}
.timer-ring svg { transform: rotate(-90deg); }
.timer-ring circle {
  fill: none; stroke-width: 3;
  stroke-linecap: round;
  transition: stroke-dashoffset .9s linear;
}
.timer-ring .bg-ring { stroke: var(--muted); stroke-dasharray: 100; stroke-dashoffset: 0; }
.timer-ring .fg-ring {
  stroke: var(--warn); stroke-dasharray: 100; stroke-dashoffset: 0;
  transition: stroke-dashoffset 1s linear, stroke .5s;
}
.timer-num { font-size: 14px; font-weight: 900; position: absolute; inset: 0; display: flex; align-items: center; justify-content: center; }

/* Слово */
.word-display {
  font-size: 34px; font-weight: 900; margin: 14px 0;
  min-height: 52px; letter-spacing: -.5px;
  display: flex; align-items: center; justify-content: center; gap: 10px;
  text-shadow: 0 2px 20px rgba(79,142,255,.15);
}
.speaker-btn {
  background: var(--muted); border: none; cursor: pointer;
  font-size: 20px; padding: 8px; border-radius: 50%; transition: var(--t);
}
.speaker-btn:hover { background: var(--accent); transform: scale(1.1); }


/* ── Карточка режима ошибок ── */
.mode-card.mistakes-card { border-color: rgba(255,79,109,.25); }
.mode-card.mistakes-card:hover { border-color: #ff4f6d; box-shadow: 0 0 24px rgba(255,79,109,.2); }
.mistakes-count-badge {
  display: inline-block; background: var(--err); color: #fff;
  font-size: 10px; font-weight: 900; padding: 2px 7px;
  border-radius: 8px; margin-left: 4px; vertical-align: middle;
}

/* Ввод */
.input-wrapper { position: relative; width: 100%; margin-bottom: 14px; }
input[type=text] {
  width: 100%; padding: 15px 18px; font-size: 18px; font-weight: 600;
  border-radius: var(--r2); border: 2px solid var(--card-border);
  background: var(--muted2); color: var(--text); outline: none; text-align: center;
  transition: var(--t); backdrop-filter: blur(10px);
}
input[type=text]:focus {
  border-color: var(--accent);
  box-shadow: 0 0 0 4px rgba(79,142,255,.12);
}
input[type=text].input-correct { border-color: var(--ok); box-shadow: 0 0 0 4px rgba(34,216,143,.12); }
input[type=text].input-wrong { border-color: var(--err); box-shadow: 0 0 0 4px rgba(255,79,109,.12); }

/* Варианты */
.options-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 14px; }
.option-btn {
  background: var(--muted); color: var(--text);
  border: 2px solid var(--card-border); padding: 15px 12px;
  font-size: 15px; font-weight: 600; border-radius: var(--r2);
  cursor: pointer; transition: var(--t); line-height: 1.3;
}
.option-btn:hover { border-color: var(--accent); background: rgba(79,142,255,.12); transform: translateY(-2px); }
.option-btn.correct-choice { background: rgba(34,216,143,.2) !important; border-color: var(--ok) !important; color: var(--ok) !important; }
.option-btn.incorrect-choice { background: rgba(255,79,109,.2) !important; border-color: var(--err) !important; color: var(--err) !important; }

/* Кнопки действий */
.actions { display: flex; gap: 8px; width: 100%; }
button { padding: 13px; font-size: 15px; font-weight: 700; border: none; border-radius: var(--r2); cursor: pointer; transition: var(--t); display: flex; align-items: center; justify-content: center; gap: 7px; }
.btn-primary {
  flex: 2; background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; min-width: 110px;
  box-shadow: 0 4px 16px rgba(79,142,255,.25);
}
.btn-primary:hover { filter: brightness(1.12); transform: translateY(-1px); }
.btn-primary:active { transform: translateY(0); }
.btn-secondary { flex: 1; background: var(--muted); color: var(--text); border: 1px solid var(--card-border); }
.btn-secondary:hover { background: var(--muted2); border-color: var(--accent); }
.btn-hint { background: rgba(255,194,71,.15); color: var(--warn); border: 1px solid rgba(255,194,71,.3); }
.btn-hint:hover { background: rgba(255,194,71,.25); }
.btn-danger { background: rgba(255,79,109,.15); color: var(--err); border: 1px solid rgba(255,79,109,.3); }
.btn-danger:hover { background: rgba(255,79,109,.25); }

@media(max-width:480px){
  .actions { flex-direction:column; gap:8px; }
  .actions button { width:100%; flex:none; padding:14px; }
}

/* Результат ответа */
.result {
  font-size: 16px; font-weight: 700; min-height: 26px;
  margin-top: 12px; transition: var(--t); text-align: center;
  line-height: 1.4;
}
.ok-text { color: var(--ok); }
.err-text { color: var(--err); }

/* ════════════════════════════════════
   РЕЖИМ ФЛЭШКАРТЫ
════════════════════════════════════ */
.flashcard-wrap {
  perspective: 1000px; height: 200px; margin: 16px 0; cursor: pointer;
}
.flashcard {
  width: 100%; height: 100%; position: relative;
  transform-style: preserve-3d; transition: transform .5s cubic-bezier(.4,0,.2,1);
}
.flashcard.flipped { transform: rotateY(180deg); }
.fc-face {
  position: absolute; inset: 0;
  backface-visibility: hidden; border-radius: var(--r);
  display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 8px;
  font-weight: 800; font-size: 28px; padding: 20px;
  border: 1px solid var(--card-border);
}
.fc-front {
  background: linear-gradient(135deg, rgba(79,142,255,.15), rgba(124,92,252,.15));
  border-color: rgba(79,142,255,.3);
}
.fc-back {
  background: linear-gradient(135deg, rgba(34,216,143,.1), rgba(79,142,255,.1));
  border-color: rgba(34,216,143,.3);
  transform: rotateY(180deg);
}
.fc-hint { font-size: 12px; font-weight: 600; color: var(--text2); margin-top: 4px; }
.fc-lang { font-size: 11px; text-transform: uppercase; letter-spacing: 1px; color: var(--text2); font-weight: 700; }

.flashcard-actions { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-top: 12px; }
.fc-btn-know {
  background: rgba(34,216,143,.15); color: var(--ok);
  border: 1px solid rgba(34,216,143,.3); border-radius: var(--r2);
  padding: 14px; font-weight: 800; cursor: pointer; transition: var(--t);
}
.fc-btn-know:hover { background: rgba(34,216,143,.25); transform: translateY(-2px); }
.fc-btn-unknown {
  background: rgba(255,79,109,.1); color: var(--err);
  border: 1px solid rgba(255,79,109,.25); border-radius: var(--r2);
  padding: 14px; font-weight: 800; cursor: pointer; transition: var(--t);
}
.fc-btn-unknown:hover { background: rgba(255,79,109,.2); transform: translateY(-2px); }
.fc-counter { font-size: 13px; color: var(--text2); font-weight: 700; text-align: center; margin-bottom: 8px; }
.fc-progress-row { display: flex; gap: 8px; margin-bottom: 12px; }
.fc-prog-item { flex: 1; padding: 8px; border-radius: 10px; font-size: 12px; font-weight: 700; text-align: center; }
.fc-prog-know { background: rgba(34,216,143,.1); color: var(--ok); }
.fc-prog-unkn { background: rgba(255,79,109,.1); color: var(--err); }
.fc-prog-left { background: var(--muted2); color: var(--text2); }

/* ════════════════════════════════════
   РЕЖИМ АНАГРАММА
════════════════════════════════════ */
.anagram-q { font-size: 16px; color: var(--text2); font-weight: 700; margin-bottom: 10px; }
.anagram-word { font-size: 22px; font-weight: 900; margin-bottom: 16px; color: var(--text); }
.anagram-answer {
  display: flex; gap: 8px; flex-wrap: wrap; justify-content: center;
  min-height: 52px; padding: 10px; background: var(--muted2);
  border: 2px dashed var(--card-border); border-radius: var(--r2);
  margin-bottom: 12px; cursor: pointer;
}
.anagram-letters {
  display: flex; gap: 8px; flex-wrap: wrap; justify-content: center; margin-bottom: 14px;
}
.letter-tile {
  background: var(--muted); border: 1px solid var(--card-border);
  border-radius: 10px; width: 42px; height: 48px;
  display: flex; align-items: center; justify-content: center;
  font-size: 18px; font-weight: 800; cursor: pointer; transition: var(--t);
  user-select: none;
}
.letter-tile:hover { border-color: var(--accent); background: rgba(79,142,255,.12); transform: translateY(-3px); }
.letter-tile.used { opacity: 0.3; pointer-events: none; }
.answer-tile {
  background: linear-gradient(135deg, rgba(79,142,255,.2), rgba(124,92,252,.2));
  border: 1px solid rgba(79,142,255,.4);
  border-radius: 10px; width: 42px; height: 48px;
  display: flex; align-items: center; justify-content: center;
  font-size: 18px; font-weight: 800; cursor: pointer; transition: var(--t);
  color: var(--accent);
}
.answer-tile:hover { opacity: 0.7; transform: scale(0.95); }
.anagram-hint-row { display: flex; align-items: center; gap: 8px; margin-bottom: 10px; }
.anagram-hint-word { font-size: 13px; color: var(--text2); font-weight: 600; letter-spacing: 1px; }

/* ════════════════════════════════════
   РЕЗУЛЬТАТЫ СПРИНТА
════════════════════════════════════ */
.result-hero { font-size: 80px; margin-bottom: 10px; animation: heroIn .5s cubic-bezier(.4,0,.2,1); }
@keyframes heroIn { from{transform:scale(.5) rotate(-10deg);opacity:0} to{transform:scale(1) rotate(0);opacity:1} }
.result-title { font-size: 28px; font-weight: 900; margin-bottom: 8px; }
.result-pct {
  font-size: 56px; font-weight: 900; margin-bottom: 16px;
  background: linear-gradient(135deg, #4f8eff, #a78bfa);
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
}
.result-stats { display: grid; grid-template-columns: repeat(2,1fr); gap: 10px; margin-bottom: 20px; }
.result-stat {
  background: var(--muted2); border: 1px solid var(--card-border);
  padding: 14px; border-radius: var(--r2); font-size: 12px; font-weight: 700;
  color: var(--text2); text-transform: uppercase; letter-spacing: .5px;
}
.result-stat strong { display:block; font-size:26px; color:var(--accent); margin-top:3px; letter-spacing:-1px; }
.result-verdict {
  background: linear-gradient(135deg, rgba(79,142,255,.08), rgba(124,92,252,.08));
  border: 1px solid rgba(79,142,255,.2);
  border-radius: var(--r2); padding: 14px 16px;
  margin-bottom: 20px; font-size: 14px; font-weight: 700; line-height: 1.6;
}

/* ════════════════════════════════════
   МОИ СЛОВА
════════════════════════════════════ */
.mw-list { max-height: 260px; overflow-y: auto; margin-bottom: 14px; }
.mw-list::-webkit-scrollbar { width: 4px; }
.mw-list::-webkit-scrollbar-track { background: var(--muted2); border-radius: 2px; }
.mw-list::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 2px; }
.mw-row {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: 12px; padding: 10px 14px; margin-bottom: 8px;
  display: flex; justify-content: space-between; align-items: center;
  font-size: 14px; font-weight: 600;
}
.mw-add-row { display: flex; gap: 8px; margin-bottom: 12px; }
.mw-add-row input { flex: 1; padding: 10px 12px; font-size: 14px; }
.mw-add-btn {
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; border: none; border-radius: var(--r2);
  padding: 10px 16px; font-weight: 700; cursor: pointer; font-size: 14px; white-space: nowrap;
}
.mw-del-btn {
  background: transparent; border: 1px solid rgba(255,79,109,.4); color: var(--err);
  border-radius: 8px; padding: 3px 9px; font-size: 11px; cursor: pointer; font-weight: 700;
}
.mw-del-btn:hover { background: rgba(255,79,109,.15); }

/* ════════════════════════════════════
   СТАТИСТИКА
════════════════════════════════════ */
.stats-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; margin-bottom: 16px; }
.stats-cell {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 14px 16px;
}
.stats-cell .sc-icon { font-size: 24px; margin-bottom: 6px; }
.stats-cell .sc-label { font-size: 11px; color: var(--text2); font-weight: 700; text-transform: uppercase; letter-spacing: .5px; }
.stats-cell .sc-val { font-size: 26px; font-weight: 900; color: var(--text); margin-top: 2px; }
.accuracy-visual {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 16px; margin-bottom: 12px;
}
.acc-bar-wrap { background: var(--muted); height: 12px; border-radius: 6px; overflow: hidden; margin-top: 8px; }
.acc-bar-ok { height: 100%; background: linear-gradient(90deg, var(--ok), #4f8eff); border-radius: 6px; transition: width .8s ease; }

/* Ранг-прогресс */
.rank-progress {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 16px; margin-bottom: 12px;
}
.rank-levels { display: flex; justify-content: space-between; flex-wrap: wrap; gap: 8px; }
.rank-item {
  display: flex; align-items: center; gap: 8px; font-size: 13px; font-weight: 700;
  padding: 8px 12px; border-radius: 10px; background: var(--muted);
}
.rank-item.earned { background: rgba(79,142,255,.12); border: 1px solid rgba(79,142,255,.3); }
.rank-item.current { background: rgba(34,216,143,.12); border: 1px solid rgba(34,216,143,.4); color: var(--ok); }

/* ════════════════════════════════════
   ТАБЫ
════════════════════════════════════ */
.tabs { display: flex; gap: 4px; background: var(--muted2); padding: 4px; border-radius: 14px; margin-bottom: 14px; }
.tab {
  flex: 1; padding: 10px; cursor: pointer; font-weight: 700; font-size: 13px;
  border-radius: 10px; transition: var(--t); color: var(--text2); text-align: center;
}
.tab.active { background: var(--card); color: var(--text); box-shadow: 0 2px 8px rgba(0,0,0,.15); }
.tab-content { display: none; max-height: 260px; overflow-y: auto; text-align: left; padding: 2px; }
.tab-content.active { display: block; }
.tab-content::-webkit-scrollbar { width: 4px; }
.tab-content::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 2px; }

ul { list-style: none; padding: 0; }
li {
  padding: 10px 14px; background: var(--muted2);
  border: 1px solid var(--card-border); border-radius: 12px;
  margin-bottom: 8px; font-size: 13px; font-weight: 600;
  display: flex; justify-content: space-between; align-items: center;
  transition: var(--t);
}
li:hover { border-color: rgba(79,142,255,.3); }
.err-ru { color: var(--err); }
.ok-en { color: var(--ok); }
.empty-state { text-align: center; color: var(--text2); font-style: italic; padding: 24px 0; font-size: 14px; }
.ach-icon { font-size: 20px; }

/* ════════════════════════════════════
   ПАУЗА / ОВЕРЛЕЙ
════════════════════════════════════ */
.overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(8,12,20,.85); backdrop-filter: blur(12px);
  z-index: 100; justify-content: center; align-items: center;
  animation: fadeIn .3s ease;
}
.overlay.active { display: flex; }
@keyframes fadeIn { from{opacity:0} to{opacity:1} }
.pause-card {
  background: var(--card); border: 1px solid var(--card-border);
  border-radius: 24px; padding: 28px; max-width: 440px; width: 92%;
  backdrop-filter: blur(20px); box-shadow: 0 30px 60px rgba(0,0,0,.4);
  max-height: 90vh; overflow-y: auto; animation: screenIn .3s ease;
}
.pause-title { font-size: 22px; font-weight: 900; margin-bottom: 16px; }
.pause-menu-actions { display: flex; flex-direction: column; gap: 10px; margin-top: 14px; }
.pause-menu-actions button { width: 100%; }
.pause-sound-toggle {
  width: 100%; padding: 12px 16px; border-radius: 12px; font-size: 14px; font-weight: 800;
  background: rgba(79,142,255,.12); color: var(--accent);
  border: 1.5px solid rgba(79,142,255,.35); cursor: pointer; transition: var(--t);
  display: flex; align-items: center; justify-content: center; gap: 8px;
}
.pause-sound-toggle:hover { background: rgba(79,142,255,.22); }
.pause-sound-toggle.muted {
  background: rgba(255,255,255,.05); color: var(--text2);
  border-color: var(--card-border);
}

select {
  width: 100%; padding: 12px 14px; border: 1px solid var(--card-border);
  border-radius: 12px; background: var(--muted2); color: var(--text);
  font-size: 13px; font-weight: 600; outline: none; cursor: pointer;
  transition: var(--t); backdrop-filter: blur(10px);
}
select:focus { border-color: var(--accent); }
.settings-row { display: flex; flex-direction: column; gap: 6px; text-align: left; margin-bottom: 12px; }
.settings-row label { font-size: 12px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .5px; }

/* Кнопки управления */
.clean-btn {
  background: transparent; border: 1px solid rgba(255,79,109,.4); color: var(--err);
  padding: 7px 12px; border-radius: 10px; font-size: 11px; cursor: pointer; font-weight: 800;
}
.clean-btn:hover { background: rgba(255,79,109,.12); }
.reset-btn {
  background: transparent; border: 1px solid rgba(255,79,109,.3); color: var(--err);
  padding: 9px 16px; border-radius: 12px; cursor: pointer; font-weight: 700; font-size: 12px;
  margin-top: 14px; display: inline-block;
}
.reset-btn:hover { background: rgba(255,79,109,.12); }

/* ════════════════════════════════════
   АНИМАЦИИ ОТВЕТОВ
════════════════════════════════════ */
.shake { animation: shake .45s ease; }
@keyframes shake { 0%,100%{transform:translateX(0)} 20%,60%{transform:translateX(-8px)} 40%,80%{transform:translateX(8px)} }
.flash-ok { animation: flashOk .35s ease; }
@keyframes flashOk {
  0%  { box-shadow:0 8px 32px rgba(0,0,0,.2); }
  50% { box-shadow:0 0 40px 15px rgba(34,216,143,.25); }
  100%{ box-shadow:0 8px 32px rgba(0,0,0,.2); }
}

/* XP поп */
.xp-pop {
  position: fixed; pointer-events: none; font-size: 20px; font-weight: 900;
  color: var(--ok); text-shadow: 0 2px 10px rgba(0,0,0,.5);
  animation: xpFly .9s ease forwards; z-index: 9999;
}
@keyframes xpFly { 0%{opacity:1;transform:translateY(0) scale(1)} 100%{opacity:0;transform:translateY(-80px) scale(1.5)} }

/* Уведомление */
.notif {
  position: fixed; top: 20px; left: 50%; transform: translateX(-50%);
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; padding: 12px 24px; border-radius: 16px;
  font-weight: 800; font-size: 14px; box-shadow: 0 10px 30px rgba(79,142,255,.3);
  z-index: 99999; white-space: nowrap; animation: notifIn .4s ease; max-width: calc(100vw - 32px);
}
@keyframes notifIn { from{opacity:0;transform:translateX(-50%) translateY(-10px)} to{opacity:1;transform:translateX(-50%) translateY(0)} }

/* Достижение поп */
.ach-pop {
  position: fixed; bottom: 30px; right: 20px;
  background: linear-gradient(135deg, rgba(34,216,143,.15), rgba(79,142,255,.15));
  border: 1px solid rgba(34,216,143,.4);
  backdrop-filter: blur(16px);
  color: var(--text); padding: 14px 18px; border-radius: 16px;
  font-weight: 700; font-size: 13px; box-shadow: 0 10px 30px rgba(0,0,0,.3);
  z-index: 99999; max-width: 280px; animation: achIn .4s ease;
}
@keyframes achIn { from{opacity:0;transform:translateX(20px)} to{opacity:1;transform:translateX(0)} }
.ach-pop .ach-pop-title { color: var(--ok); font-size: 11px; text-transform: uppercase; letter-spacing: .8px; margin-bottom: 4px; }

/* Серия-баннер */
.streak-banner {
  position: fixed; top: 50%; left: 50%; transform: translate(-50%,-50%);
  font-size: 48px; font-weight: 900; pointer-events: none; z-index: 9998;
  animation: streakPop .6s ease forwards;
  text-shadow: 0 4px 20px rgba(255,194,71,.5);
}
@keyframes streakPop {
  0%  { opacity:0; transform:translate(-50%,-50%) scale(.5); }
  30% { opacity:1; transform:translate(-50%,-50%) scale(1.1); }
  70% { opacity:1; transform:translate(-50%,-50%) scale(1); }
  100%{ opacity:0; transform:translate(-50%,-60%) scale(.9); }
}

/* Разделитель */
.divider { height: 1px; background: var(--card-border); margin: 16px 0; }

/* ════════════════════════════════════
   ПРОИЗНОШЕНИЕ
════════════════════════════════════ */
.pronun-input-row {
  display: flex; gap: 8px; margin-bottom: 14px;
}
.pronun-input-row input {
  flex: 1; font-size: 17px; text-align: left;
}
.pronun-speak-btn {
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; border: none; border-radius: var(--r2);
  padding: 0 20px; font-size: 22px; cursor: pointer;
  flex-shrink: 0; transition: var(--t);
  display: flex; align-items: center; justify-content: center;
}
.pronun-speak-btn:hover { filter: brightness(1.15); transform: scale(1.05); }
.pronun-speak-btn:active { transform: scale(0.97); }
.pronun-speak-btn.speaking { animation: speakPulse .6s ease-in-out infinite alternate; }
@keyframes speakPulse {
  from { box-shadow: 0 0 0 0 rgba(79,142,255,.4); }
  to   { box-shadow: 0 0 0 12px rgba(79,142,255,0); }
}
.pronun-voice-row {
  display: flex; gap: 8px; align-items: center; margin-bottom: 14px; flex-wrap: wrap;
}
.pronun-voice-row label { font-size: 12px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .5px; white-space: nowrap; }
.pronun-voice-row select { flex: 1; min-width: 140px; font-size: 13px; }
.pronun-speed-row {
  display: flex; gap: 8px; align-items: center; margin-bottom: 14px;
}
.pronun-speed-row label { font-size: 12px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .5px; white-space: nowrap; }
.pronun-speed-row input[type=range] {
  flex: 1; accent-color: var(--accent);
  background: transparent; border: none; padding: 0; outline: none; box-shadow: none;
  height: 6px; cursor: pointer;
}
.pronun-speed-val { font-size: 13px; font-weight: 800; color: var(--accent); width: 32px; text-align: right; }
.pronun-history {
  max-height: 160px; overflow-y: auto; margin-top: 6px;
}
.pronun-history::-webkit-scrollbar { width: 4px; }
.pronun-history::-webkit-scrollbar-thumb { background: var(--accent); border-radius: 2px; }
.pronun-hist-item {
  display: flex; align-items: center; justify-content: space-between;
  padding: 8px 12px; background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: 10px; margin-bottom: 6px; font-size: 14px; font-weight: 600; cursor: pointer;
  transition: var(--t);
}
.pronun-hist-item:hover { border-color: var(--accent); background: rgba(79,142,255,.08); }
.pronun-hist-repeat { font-size: 16px; color: var(--accent); }
.pronun-no-support {
  background: rgba(255,79,109,.1); border: 1px solid rgba(255,79,109,.3);
  border-radius: var(--r2); padding: 12px 16px; font-size: 13px; font-weight: 700;
  color: var(--err); text-align: center; margin-bottom: 14px;
}
.section-title { font-size: 13px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .8px; margin-bottom: 12px; }

/* Полоска ввода анаграммы */
.anagram-blank {
  display: flex; gap: 4px; justify-content: center; margin-bottom: 12px; flex-wrap: wrap;
}
.blank-slot {
  width: 28px; height: 4px; border-radius: 2px;
  background: var(--card-border);
}
.blank-slot.filled { background: var(--accent); }

/* ════════════════════════════════════
   АДАПТИВНАЯ ВЁРСТКА
════════════════════════════════════ */

/* ── Базовые улучшения для мобильных ── */
@media (max-width: 480px) {
  body { padding: 10px 8px 60px; }

  .container { max-width: 100%; }

  /* Шапка */
  header { margin-bottom: 12px; padding: 0; }
  .logo { font-size: 18px; gap: 6px; }
  .logo-icon { font-size: 22px; }
  .icon-btn { padding: 8px 10px; font-size: 14px; border-radius: 10px; }
  .header-actions { gap: 6px; }

  /* Карточки */
  .card { padding: 16px 14px; border-radius: 16px; margin-bottom: 12px; }

  /* Главное меню */
  .avatar { width: 52px; height: 52px; font-size: 24px; border-radius: 14px; }
  .menu-name { font-size: 17px; }
  .menu-stats { grid-template-columns: 1fr 1fr; gap: 8px; }
  .stat-card .s-val { font-size: 20px; }

  /* Сетка режимов */
  .mode-grid { grid-template-columns: 1fr 1fr; gap: 8px; }
  .mode-card { padding: 14px 8px; font-size: 13px; border-radius: 12px; }
  .mode-card .mc-icon { font-size: 28px; margin-bottom: 6px; }
  .mode-card .mc-sub { font-size: 10px; }

  /* Спринт кнопки */
  .sprint-opts { gap: 6px; }
  .sprint-opt-btn { padding: 10px 14px; font-size: 16px; border-radius: 12px; }

  /* Игровой экран */
  .game-header { gap: 6px; flex-wrap: wrap; }
  .pause-btn { padding: 7px 10px; font-size: 12px; }
  .stats-bar { gap: 4px; }
  .mini-stat { padding: 5px 8px; font-size: 11px; border-radius: 8px; }

  /* Слово */
  .word-display { font-size: 26px; margin: 10px 0; min-height: 44px; gap: 8px; }

  /* Переключатель режима */
  .g-mode-selector { gap: 4px; }
  .g-mode-btn { padding: 8px 6px; font-size: 11px; }

  /* Инпут */
  input[type=text] { padding: 13px 14px; font-size: 16px; border-radius: 12px; }

  /* Варианты ответов */
  .options-grid { grid-template-columns: 1fr 1fr; gap: 8px; }
  .option-btn { padding: 12px 8px; font-size: 13px; border-radius: 12px; }

  /* Кнопки действий */
  .actions { flex-direction: column; gap: 8px; }
  .actions button { width: 100%; flex: none; padding: 13px; font-size: 14px; }

  /* Флэшкарты */
  .flashcard-wrap { height: 160px; margin: 10px 0; }
  .fc-face { font-size: 22px; padding: 14px; }
  .fc-progress-row { gap: 6px; }
  .fc-prog-item { padding: 6px; font-size: 11px; border-radius: 8px; }
  .flashcard-actions { gap: 8px; }
  .fc-btn-know, .fc-btn-unknown { padding: 12px; font-size: 13px; border-radius: 12px; }

  /* Анаграмма */
  .letter-tile, .answer-tile { width: 36px; height: 42px; font-size: 16px; border-radius: 8px; }
  .anagram-word { font-size: 18px; }
  .anagram-letters, .anagram-answer { gap: 6px; }

  /* Результаты */
  .result-hero { font-size: 60px; }
  .result-title { font-size: 22px; }
  .result-pct { font-size: 44px; }
  .result-stats { grid-template-columns: 1fr 1fr; gap: 8px; }
  .result-stat { padding: 10px; }
  .result-stat strong { font-size: 20px; }

  /* Достижения/ошибки */
  .stats-grid { grid-template-columns: 1fr 1fr; gap: 8px; }
  .stats-cell .sc-val { font-size: 20px; }
  .rank-levels { gap: 6px; }
  .rank-item { font-size: 12px; padding: 6px 10px; }

  /* Мои слова */
  .mw-add-row { flex-direction: column; gap: 8px; }
  .mw-add-row input { width: 100%; }
  .mw-add-btn { width: 100%; padding: 12px; }

  /* Произношение */
  .pronun-input-row { gap: 6px; }
  .pronun-speak-btn { padding: 0 14px; font-size: 20px; }
  .pronun-voice-row { flex-direction: column; align-items: flex-start; gap: 6px; }
  .pronun-voice-row select { width: 100%; }
  .pronun-speed-row { flex-wrap: wrap; }

  /* Модальное окно */
  .diff-modal { padding: 20px 16px; border-radius: 18px; }
  .diff-modal-title { font-size: 18px; }
  .diff-option { gap: 10px; padding: 12px 12px; border-radius: 12px; }
  .diff-option-icon { font-size: 22px; }
  .diff-option-name { font-size: 13px; }
  .diff-option-xp { font-size: 12px; padding: 3px 8px; }

  /* Попап достижения */
  .ach-pop { bottom: 16px; right: 12px; left: 12px; max-width: none; font-size: 12px; padding: 12px 14px; }

  /* Уведомление */
  .notif { font-size: 13px; padding: 10px 18px; white-space: normal; text-align: center; max-width: 90vw; }

  /* Пауза */
  .pause-card { padding: 20px 16px; border-radius: 18px; }
  .pause-title { font-size: 18px; }

  /* Серия-баннер */
  .streak-banner { font-size: 36px; }

  /* Дневная цель */
  .daily-box { padding: 12px 12px; }
  .daily-row { font-size: 12px; }
  .daily-complete-banner { font-size: 11px; padding: 5px 10px; }

  /* Табы */
  .tab { padding: 8px 6px; font-size: 12px; }
  li { padding: 8px 10px; font-size: 12px; }

  /* Таймер */
  .timer-box { font-size: 14px; }
}

/* ── Средние планшеты (481–768px) ── */
@media (min-width: 481px) and (max-width: 768px) {
  body { padding: 14px 16px 50px; }

  .container { max-width: 540px; }

  .logo { font-size: 20px; }
  .card { padding: 20px 18px; }

  .avatar { width: 58px; height: 58px; font-size: 27px; }
  .menu-name { font-size: 18px; }

  .mode-grid { grid-template-columns: 1fr 1fr; gap: 10px; }
  .mode-card { padding: 16px 12px; }

  .word-display { font-size: 30px; }

  .flashcard-wrap { height: 180px; }
  .fc-face { font-size: 24px; }

  .result-pct { font-size: 48px; }
  .result-hero { font-size: 70px; }

  .mw-add-row { flex-wrap: wrap; }
  .mw-add-btn { flex-shrink: 0; }

  .ach-pop { right: 14px; max-width: 300px; }
}

/* ── Большие планшеты (769–1024px) ── */
@media (min-width: 769px) and (max-width: 1024px) {
  body { padding: 20px 24px 50px; }
  .container { max-width: 580px; }
  .card { padding: 26px 22px; }
  .word-display { font-size: 36px; }
}

/* ── Десктоп (1025px+) ── */
@media (min-width: 1025px) {
  body { padding: 28px 24px 60px; }
  .container { max-width: 640px; }
  .card { padding: 28px 26px; }
  .word-display { font-size: 38px; }
  .mode-grid { grid-template-columns: 1fr 1fr; }
  .card:hover { box-shadow: 0 12px 40px rgba(0,0,0,0.25); }
}

/* ── Широкие экраны (1440px+) ── */
@media (min-width: 1440px) {
  .container { max-width: 680px; }
  body { padding: 36px 24px 70px; }
}

/* ── Ориентация: альбомная на маленьких устройствах ── */
@media (max-width: 768px) and (orientation: landscape) {
  body { padding: 8px 12px 30px; }
  .card { padding: 14px 16px; margin-bottom: 10px; }
  .menu-top { gap: 12px; }
  .avatar { width: 44px; height: 44px; font-size: 20px; }
  .flashcard-wrap { height: 140px; }
  .word-display { font-size: 24px; margin: 8px 0; min-height: 38px; }
  .timer-ring { width: 32px; height: 32px; }
  .mode-grid { grid-template-columns: repeat(4, 1fr); gap: 8px; }
  .mode-card .mc-icon { font-size: 22px; margin-bottom: 4px; }
  .result-hero { font-size: 48px; margin-bottom: 6px; }
  .result-pct { font-size: 36px; margin-bottom: 10px; }
}

/* ── Тёмная тема системы ── */
@media (prefers-color-scheme: light) {
  body:not(.light):not(.dark-forced) {
    /* Уважаем явный выбор пользователя */
  }
}

/* ── Уменьшение анимаций ── */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
  .bg-orb { animation: none !important; }
  .level-bar::after { animation: none !important; }
}

/* ── Тонкие экраны (SE, fold) ── */
@media (max-width: 360px) {
  body { padding: 8px 6px 50px; }
  .logo { font-size: 16px; }
  .logo-icon { font-size: 18px; }
  .icon-btn { padding: 7px 8px; font-size: 13px; }
  .card { padding: 14px 10px; border-radius: 14px; }
  .word-display { font-size: 22px; }
  input[type=text] { font-size: 14px; padding: 11px 10px; }
  .letter-tile, .answer-tile { width: 32px; height: 38px; font-size: 14px; }
  .mode-grid { gap: 6px; }
  .mode-card { padding: 12px 6px; font-size: 12px; }
  .mode-card .mc-icon { font-size: 24px; }
  .stat-card .s-val { font-size: 18px; }
  .stats-grid { grid-template-columns: 1fr 1fr; }
  .result-pct { font-size: 38px; }
  .result-title { font-size: 18px; }
  .diff-option { padding: 10px 10px; gap: 8px; }
  .diff-option-icon { font-size: 18px; }
}

/* ════════════════════════════════════
   УЛУЧШЕНИЯ v4 — ДЕНЬ СЕРИИ, КОЛЬЦА, CONFETTI
════════════════════════════════════ */

/* Day streak badge */
.day-streak-box {
  display: flex; align-items: center; gap: 14px;
  background: linear-gradient(135deg, rgba(255,140,50,.12), rgba(255,194,71,.08));
  border: 1px solid rgba(255,140,50,.3);
  border-radius: var(--r2); padding: 12px 16px; margin-bottom: 16px;
}
.day-streak-flame { font-size: 32px; flex-shrink: 0; animation: flamePulse 1.5s ease-in-out infinite; }
@keyframes flamePulse { 0%,100%{transform:scale(1) rotate(-3deg)} 50%{transform:scale(1.1) rotate(3deg)} }
.day-streak-info { flex: 1; }
.day-streak-label { font-size: 11px; color: var(--text2); font-weight: 800; text-transform: uppercase; letter-spacing: .5px; }
.day-streak-val { font-size: 26px; font-weight: 900; color: var(--warn); line-height: 1.1; }
.day-streak-sub { font-size: 11px; color: var(--text2); font-weight: 600; margin-top: 1px; }

/* XP Ring progress for profile */
.profile-ring-wrap {
  position: relative; width: 110px; height: 110px; margin: 0 auto 10px;
}
.profile-ring-wrap svg { transform: rotate(-90deg); }
.profile-ring-wrap .ring-bg { fill: none; stroke: var(--muted); stroke-width: 6; }
.profile-ring-wrap .ring-fg {
  fill: none; stroke-width: 6; stroke-linecap: round;
  transition: stroke-dashoffset 1s ease, stroke .4s ease;
}
@keyframes rainbowStroke {
  0%   { stroke: #ff4f6d; }
  25%  { stroke: #ffc247; }
  50%  { stroke: #22d88f; }
  75%  { stroke: #4f8eff; }
  100% { stroke: #ff4f6d; }
}
.profile-avatar-center {
  position: absolute; inset: 8px; display: flex; align-items: center;
  justify-content: center; font-size: 48px; cursor: pointer;
  border-radius: 50%; overflow: hidden;
}
.profile-avatar-center img {
  width: 100%; height: 100%; object-fit: cover; border-radius: 50%; display: block;
}

/* Confetti particle */
.confetti-piece {
  position: fixed; pointer-events: none; z-index: 99999;
  width: 8px; height: 8px; border-radius: 2px;
  animation: confettiFall 1.2s ease forwards;
}
@keyframes confettiFall {
  0%   { opacity: 1; transform: translateY(-20px) rotate(0deg) scale(1); }
  100% { opacity: 0; transform: translateY(200px) rotate(720deg) scale(0.5); }
}

/* Level-up banner */
.lvlup-banner {
  position: fixed; top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  background: linear-gradient(135deg, rgba(79,142,255,.95), rgba(124,92,252,.95));
  backdrop-filter: blur(20px);
  color: #fff; padding: 24px 36px; border-radius: 24px;
  font-size: 22px; font-weight: 900; text-align: center;
  box-shadow: 0 20px 60px rgba(79,142,255,.4);
  z-index: 9999; pointer-events: none;
  animation: lvlupIn .5s cubic-bezier(.4,0,.2,1) forwards;
}
@keyframes lvlupIn {
  0%  { opacity:0; transform:translate(-50%,-50%) scale(.6); }
  60% { opacity:1; transform:translate(-50%,-50%) scale(1.05); }
  100%{ opacity:1; transform:translate(-50%,-50%) scale(1); }
}

/* Улучшенные кнопки */
.btn-primary {
  position: relative; overflow: hidden;
}
.btn-primary::after {
  content: ''; position: absolute; inset: 0;
  background: linear-gradient(to right, transparent 0%, rgba(255,255,255,.1) 50%, transparent 100%);
  transform: translateX(-100%); transition: transform .4s;
}
.btn-primary:hover::after { transform: translateX(100%); }

/* Красивее stat-card */
.stat-card { transition: var(--t); cursor: default; }
.stat-card:hover { transform: translateY(-2px); border-color: var(--accent); box-shadow: 0 4px 16px rgba(79,142,255,.15); }

/* Weekly progress mini-chart */
.weekly-chart {
  display: flex; align-items: flex-end; gap: 6px; height: 50px;
  margin-top: 10px;
}
.week-bar-wrap { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 4px; }
.week-bar {
  width: 100%; border-radius: 4px 4px 0 0;
  background: linear-gradient(to top, var(--accent), var(--accent2));
  transition: height .6s cubic-bezier(.4,0,.2,1);
  min-height: 4px;
}
.week-bar.today { box-shadow: 0 0 10px rgba(79,142,255,.4); }
.week-label { font-size: 9px; font-weight: 800; color: var(--text2); }

/* Улучшенная карточка достижений */
.ach-list-item {
  display: flex; align-items: center; gap: 10px;
  padding: 10px 12px; background: var(--muted2);
  border: 1px solid var(--card-border); border-radius: 12px;
  margin-bottom: 8px; transition: var(--t);
}
.ach-list-item:hover { border-color: rgba(79,142,255,.3); }
.ach-list-item.locked { opacity: 0.45; filter: grayscale(.5); }
.ach-list-icon { font-size: 22px; flex-shrink: 0; }
.ach-list-text { flex: 1; font-size: 13px; font-weight: 600; }
.ach-list-earned { font-size: 10px; color: var(--ok); font-weight: 800; }
.ach-check { font-size: 16px; }

/* Красивее quote box */
.quote-box {
  position: relative; overflow: hidden;
}
.quote-box::before {
  content: '"'; position: absolute; top: -8px; left: 8px;
  font-size: 60px; font-family: Georgia, serif; color: var(--accent2);
  opacity: 0.15; line-height: 1; pointer-events: none;
}

/* Градиент аватара в меню */
.avatar {
  background: linear-gradient(135deg, var(--accent), var(--accent2)) !important;
  overflow: hidden;
}

/* Улучшенный пустой экран достижений */
.ach-empty {
  text-align: center; padding: 24px 16px;
}
.ach-empty-icon { font-size: 40px; margin-bottom: 8px; }
.ach-empty-text { color: var(--text2); font-size: 13px; font-style: italic; }

/* Нижняя навигация (quick access) */
.bottom-nav {
  position: fixed; bottom: 0; left: 0; right: 0;
  background: rgba(8,12,20,0.92); border-top: 1px solid var(--card-border);
  backdrop-filter: blur(20px); display: flex; justify-content: space-around;
  padding: 8px 0 max(8px, env(safe-area-inset-bottom));
  z-index: 50;
  box-shadow: 0 -4px 20px rgba(0,0,0,.15);
}
body.light .bottom-nav {
  background: rgba(255,255,255,0.92);
  border-top: 1px solid rgba(100,130,200,0.25);
  box-shadow: 0 -4px 20px rgba(0,0,0,.08);
}
body.light .nav-item { color: #7788cc; }
body.light .nav-item.active { color: var(--accent); }
body { padding-bottom: max(90px, calc(env(safe-area-inset-bottom) + 80px)); }
.nav-item {
  display: flex; flex-direction: column; align-items: center; gap: 2px;
  font-size: 10px; font-weight: 700; color: var(--text2);
  padding: 4px 12px; border-radius: 10px; cursor: pointer;
  transition: var(--t); background: transparent; border: none;
  -webkit-tap-highlight-color: transparent;
}
.nav-item.active { color: var(--accent); }
.nav-item-icon { font-size: 20px; }

/* Кнопка «Начать» — более заметная */
.start-btn-mega {
  background: linear-gradient(135deg, #4f8eff, #7c5cfc);
  color: #fff; border: none; border-radius: 18px;
  padding: 18px 24px; font-size: 18px; font-weight: 900;
  width: 100%; cursor: pointer; transition: var(--t);
  box-shadow: 0 6px 24px rgba(79,142,255,.35);
  position: relative; overflow: hidden; letter-spacing: .3px;
  display: flex; align-items: center; justify-content: center; gap: 10px;
}
.start-btn-mega:hover { transform: translateY(-2px); box-shadow: 0 10px 32px rgba(79,142,255,.45); filter: brightness(1.08); }
.start-btn-mega:active { transform: translateY(0); }

/* Улучшенный таймер */
.timer-num { color: var(--warn); font-weight: 900; }

/* Тег версии */
.version-tag { opacity: .4; font-weight: 400; font-size: 12px; }

/* ────────────────────────────────
   v4 polish — финальные штрихи
──────────────────────────────── */

/* Убрать дублирование btn-primary::after (уже добавлено в extra_css) */

/* Улучшенный header */
header {
  padding: 4px 4px 0;
}
.logo {
  text-shadow: 0 2px 12px rgba(79,142,255,.2);
}

/* Плавное появление card */
.card {
  will-change: transform, opacity;
}

/* Улучшенный результат */
.result-hero {
  text-shadow: 0 4px 20px rgba(0,0,0,.3);
}

/* Mode card — добавить цветные фоны при hover */
.mode-card:nth-child(1):hover { border-color: #22d88f; box-shadow: 0 0 24px rgba(34,216,143,.2); }
.mode-card:nth-child(2):hover { border-color: #4f8eff; box-shadow: 0 0 24px rgba(79,142,255,.2); }
.mode-card:nth-child(3):hover { border-color: #ffc247; box-shadow: 0 0 24px rgba(255,194,71,.2); }
.mode-card:nth-child(4):hover { border-color: #ff4f6d; box-shadow: 0 0 24px rgba(255,79,109,.2); }

/* Улучшенный flashcard */
.fc-face {
  text-shadow: 0 2px 10px rgba(0,0,0,.15);
}

/* Красивее daily-box */
.daily-box {
  transition: var(--t);
}
.daily-box:hover {
  border-color: rgba(79,142,255,.6);
}

/* Streak box glow */
.day-streak-box {
  transition: var(--t);
}
.day-streak-box:hover {
  box-shadow: 0 4px 20px rgba(255,140,50,.15);
}

/* Улучшен скролл */
::-webkit-scrollbar { width: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: rgba(79,142,255,.3); border-radius: 3px; }
::-webkit-scrollbar-thumb:hover { background: rgba(79,142,255,.5); }

/* Улучшить timer ring цвет когда мало времени */
.timer-ring .fg-ring.danger { stroke: var(--err); }

/* Подсказка в анаграмме */
.anagram-q {
  background: var(--muted2); border-radius: 8px; padding: 6px 12px;
  display: inline-block;
}

/* Анимация появления букв в анаграмме */
@keyframes tileJump {
  0%  { opacity:0; transform: translateY(20px) scale(0.6); }
  60% { opacity:1; transform: translateY(-6px) scale(1.1); }
  80% { transform: translateY(3px) scale(0.95); }
  100%{ opacity:1; transform: translateY(0) scale(1); }
}
.letter-tile.jump { animation: tileJump 0.45s cubic-bezier(.4,0,.2,1) both; }

/* Поиск по словарю */
.dict-search-wrap { position:relative; margin-bottom:12px; }
.dict-search-input {
  width:100%; padding:12px 40px 12px 16px; font-size:15px; font-weight:600;
  border-radius:var(--r2); border:2px solid var(--card-border);
  background:var(--muted2); color:var(--text); outline:none; transition:var(--t);
}
.dict-search-input:focus { border-color:var(--accent); box-shadow:0 0 0 3px rgba(79,142,255,.1); }
.dict-search-clear {
  position:absolute; right:12px; top:50%; transform:translateY(-50%);
  background:none; border:none; color:var(--text2); font-size:18px; cursor:pointer; padding:0; line-height:1;
}
.dict-results {
  max-height:220px; overflow-y:auto; margin-bottom:10px;
}
.dict-result-item {
  display:flex; justify-content:space-between; align-items:center;
  padding:10px 14px; background:var(--muted2); border:1px solid var(--card-border);
  border-radius:10px; margin-bottom:6px; font-size:14px; font-weight:600;
  transition:var(--t);
}
.dict-result-item:hover { border-color:rgba(79,142,255,.3); }
.dict-result-ru { color:var(--text2); font-size:12px; }
.dict-no-result { text-align:center; color:var(--text2); padding:16px 0; font-style:italic; font-size:14px; }
.dict-result-cat { font-size:10px; color:var(--text2); background:var(--muted); padding:2px 7px; border-radius:6px; font-weight:700; letter-spacing:.3px; }

/* Экспорт прогресса */
.export-btn-row { display:flex; gap:8px; margin-top:10px; }

/* Слово дня */
.word-of-day-card {
  background: linear-gradient(135deg, rgba(124,92,252,.12), rgba(79,142,255,.08));
  border: 1px solid rgba(124,92,252,.3);
  border-radius: var(--r2); padding: 14px 16px; margin-bottom: 16px;
  position: relative; overflow: hidden;
}
.word-of-day-card::before {
  content: ''; position: absolute; top: -30px; right: -30px;
  width: 80px; height: 80px; background: rgba(124,92,252,.08); border-radius: 50%;
}
.wod-label { font-size: 10px; font-weight: 900; text-transform: uppercase; letter-spacing: 1px;
  color: var(--accent2); margin-bottom: 6px; }
.wod-word { font-size: 26px; font-weight: 900; color: var(--text); margin-bottom: 2px; }
.wod-pron { font-size: 12px; color: var(--text2); font-weight: 600; margin-bottom: 6px; letter-spacing: .5px; }
.wod-translation { font-size: 14px; font-weight: 700; color: var(--text2); margin-bottom: 8px; }
.wod-sentence { font-size: 12px; color: var(--text2); font-style: italic; line-height: 1.6; margin-bottom: 10px;
  border-left: 2px solid rgba(124,92,252,.4); padding-left: 8px; }
.wod-actions { display: flex; gap: 8px; align-items: center; }
.wod-btn-know {
  flex: 1; background: linear-gradient(135deg, var(--accent2), var(--accent));
  color: #fff; border: none; border-radius: 10px; padding: 9px 14px;
  font-size: 13px; font-weight: 800; cursor: pointer; transition: var(--t);
}
.wod-btn-know:hover { filter: brightness(1.1); }
.wod-btn-know.known { background: rgba(34,216,143,.15); color: var(--ok); border: 1px solid rgba(34,216,143,.3); }
.wod-speak-btn {
  background: var(--muted); border: 1px solid var(--card-border);
  border-radius: 10px; padding: 9px 12px; font-size: 18px;
  cursor: pointer; transition: var(--t);
}
.wod-speak-btn:hover { border-color: var(--accent); }

/* Онбординг — обучающий раунд */
.tutorial-overlay {
  position: fixed; inset: 0;
  background: rgba(8,12,20,.92); backdrop-filter: blur(16px);
  z-index: 400; display: flex; justify-content: center; align-items: center;
  animation: fadeIn .35s ease;
}
.tutorial-card {
  background: var(--card); border: 1px solid var(--card-border);
  border-radius: 28px; padding: 28px 24px; max-width: 380px; width: 94%;
  backdrop-filter: blur(20px); box-shadow: 0 30px 80px rgba(0,0,0,.5);
  text-align: center; animation: screenIn .4s ease;
}
.tutorial-step { font-size: 11px; font-weight: 800; color: var(--accent2);
  text-transform: uppercase; letter-spacing: 1px; margin-bottom: 8px; }
.tutorial-emoji { font-size: 52px; margin-bottom: 12px; display: block; }
.tutorial-title { font-size: 22px; font-weight: 900; margin-bottom: 8px; }
.tutorial-sub { font-size: 14px; color: var(--text2); line-height: 1.65; margin-bottom: 20px; }
.tutorial-demo {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: 14px; padding: 14px; margin-bottom: 20px;
  font-size: 16px; font-weight: 700; line-height: 1.6;
}
.tutorial-demo .demo-word { font-size: 28px; font-weight: 900; color: var(--accent); margin-bottom: 4px; }
.tutorial-demo .demo-arrow { color: var(--text2); font-size: 20px; }
.tutorial-demo .demo-answer { font-size: 22px; font-weight: 900; color: var(--ok); }
.tutorial-dots { display: flex; justify-content: center; gap: 6px; margin-bottom: 20px; }
.tutorial-dot { width: 8px; height: 8px; border-radius: 50%; background: var(--muted); transition: background .3s; }
.tutorial-dot.active { background: var(--accent); }

/* Статистика по категориям */
.cat-stats-grid { display: flex; flex-direction: column; gap: 8px; margin-bottom: 12px; }
.cat-stat-row {
  display: flex; align-items: center; gap: 10px;
}
.cat-stat-label { font-size: 12px; font-weight: 700; color: var(--text2); min-width: 80px; }
.cat-stat-bar-wrap { flex: 1; background: var(--muted); border-radius: 4px; height: 8px; overflow: hidden; }
.cat-stat-bar { height: 100%; border-radius: 4px; background: linear-gradient(90deg, var(--ok), var(--accent));
  transition: width .6s ease; }
.cat-stat-pct { font-size: 12px; font-weight: 900; color: var(--accent); min-width: 36px; text-align: right; }

/* Улучшенные result-stat карточки */
.result-stat:nth-child(1) strong { color: var(--ok); }
.result-stat:nth-child(2) strong { color: var(--err); }
.result-stat:nth-child(3) strong { color: var(--warn); }
.result-stat:nth-child(4) strong { color: var(--accent2); }

/* Screen titles */
#profileScreen > div:first-child,
#statsScreen > div:first-child,
#myWordsScreen > div:first-child,
#pronounceScreen > div:first-child {
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* Улучшенный select */
select {
  -webkit-appearance: none;
  appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%238899bb' stroke-width='1.5' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 14px center;
  padding-right: 36px;
}

/* Улучшенная статистика — ранг */
.rank-item {
  transition: var(--t);
}
.rank-item.current {
  animation: rankGlow 2s ease-in-out infinite;
}
@keyframes rankGlow {
  0%,100% { box-shadow: 0 0 0 0 rgba(34,216,143,.3); }
  50% { box-shadow: 0 0 12px 4px rgba(34,216,143,.15); }
}

/* Красивей мои слова */
.mw-row {
  transition: var(--t);
}
.mw-row:hover {
  border-color: rgba(79,142,255,.3);
  transform: translateX(3px);
}


/* ════════════════════════════════════
   ЭКРАН НАСТРОЕК
════════════════════════════════════ */
.settings-section {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 14px 16px; margin-bottom: 14px;
}
.settings-section-title {
  font-size: 11px; font-weight: 800; color: var(--text2);
  text-transform: uppercase; letter-spacing: .6px; margin-bottom: 14px;
}
.settings-row-item {
  display: flex; align-items: center; justify-content: space-between; gap: 12px;
}
.settings-row-info { flex: 1; }
.settings-row-label { font-size: 14px; font-weight: 700; margin-bottom: 2px; }
.settings-row-sub { font-size: 12px; color: var(--text2); font-weight: 500; }

/* Toggle switch */
.toggle-wrap {
  width: 50px; height: 28px; border-radius: 14px;
  background: var(--muted); position: relative; cursor: pointer;
  transition: background .25s; flex-shrink: 0; border: 2px solid var(--card-border);
}
.toggle-wrap.on { background: var(--ok); border-color: var(--ok); }
.toggle-knob {
  position: absolute; top: 2px; left: 2px;
  width: 20px; height: 20px; border-radius: 50%;
  background: #fff; box-shadow: 0 2px 6px rgba(0,0,0,.25);
  transition: transform .25s cubic-bezier(.4,0,.2,1);
}
.toggle-wrap.on .toggle-knob { transform: translateX(22px); }

/* Theme picker */
.theme-picker {
  display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-top: 4px;
}
.theme-option {
  border: 2px solid var(--card-border); border-radius: 12px;
  overflow: hidden; cursor: pointer; transition: var(--t);
}
.theme-option:hover { border-color: var(--accent); }
.theme-option.active { border-color: var(--accent); box-shadow: 0 0 0 3px rgba(79,142,255,.2); }
.theme-preview {
  height: 52px; padding: 10px; display: flex; flex-direction: column; gap: 6px;
}
.theme-preview-dark { background: #080c14; }
.theme-preview-light { background: #f0f4ff; }
.theme-preview-auto {
  display: flex; flex-direction: row; padding: 0; gap: 0; overflow: hidden;
}
.tp-half { flex: 1; height: 100%; }
.dark-half { background: #080c14; }
.light-half { background: #f0f4ff; }
.tp-bar {
  height: 6px; border-radius: 3px;
  background: rgba(255,255,255,.3);
}
.theme-preview-light .tp-bar { background: rgba(0,0,0,.15); }
.tp-bar.short { width: 60%; }
.theme-option-label {
  padding: 6px 8px; font-size: 11px; font-weight: 800;
  text-align: center; color: var(--text2);
}

/* Settings mini button */
.settings-mini-btn {
  background: var(--muted); color: var(--text);
  border: 1px solid var(--card-border); border-radius: 10px;
  padding: 7px 14px; font-size: 12px; font-weight: 800;
  cursor: pointer; transition: var(--t); white-space: nowrap; flex-shrink: 0;
}
.settings-mini-btn:hover { border-color: var(--accent); color: var(--accent); }
.danger-btn { color: var(--err); border-color: rgba(255,79,109,.3); }
.danger-btn:hover { background: rgba(255,79,109,.12); border-color: var(--err); }

/* Nav item — 5 кнопок */
.nav-item { flex: 1; min-width: 0; }
.nav-item-icon { font-size: 18px; }
.nav-item { font-size: 9px; }

/* ════════════════════════════════════
   ЖИЗНИ (HEARTS)
════════════════════════════════════ */
.lives-bar {
  display: flex; align-items: center; gap: 4px;
  font-size: 20px; letter-spacing: 2px;
}
.heart { transition: transform .2s, filter .2s; display: inline-block; }
.heart.lost { filter: grayscale(1); opacity: 0.3; transform: scale(0.8); }
.heart-pop { animation: heartPop .4s cubic-bezier(.4,0,.2,1); }
@keyframes heartPop {
  0%  { transform: scale(1.6); }
  60% { transform: scale(0.85); }
  100%{ transform: scale(0.8); }
}
.lives-danger { animation: livesDanger .6s ease-in-out; }
@keyframes livesDanger {
  0%,100%{ background: var(--card); }
  50%    { background: rgba(255,79,109,.15); }
}

/* ════════════════════════════════════
   БОНУС-РАУНД
════════════════════════════════════ */
.bonus-banner {
  position: fixed; top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  background: linear-gradient(135deg, rgba(255,194,71,.97), rgba(255,140,50,.97));
  color: #000; padding: 20px 32px; border-radius: 24px;
  font-size: 20px; font-weight: 900; text-align: center;
  box-shadow: 0 0 60px rgba(255,194,71,.5);
  z-index: 9999; pointer-events: none;
  animation: bonusIn .5s cubic-bezier(.4,0,.2,1) forwards;
}
@keyframes bonusIn {
  0%  { opacity:0; transform:translate(-50%,-50%) scale(.5) rotate(-5deg); }
  60% { opacity:1; transform:translate(-50%,-50%) scale(1.08) rotate(2deg); }
  100%{ opacity:1; transform:translate(-50%,-50%) scale(1) rotate(0); }
}
.bonus-active-badge {
  display: inline-flex; align-items: center; gap: 5px;
  background: linear-gradient(135deg, rgba(255,194,71,.2), rgba(255,140,50,.15));
  border: 1px solid rgba(255,194,71,.5);
  border-radius: 10px; padding: 4px 10px;
  font-size: 11px; font-weight: 900; color: var(--warn);
  animation: bonusBadgePulse 1s ease-in-out infinite;
}
@keyframes bonusBadgePulse {
  0%,100% { box-shadow: 0 0 0 0 rgba(255,194,71,.3); }
  50%     { box-shadow: 0 0 0 6px rgba(255,194,71,0); }
}

/* ════════════════════════════════════
   КАЛЕНДАРЬ АКТИВНОСТИ
════════════════════════════════════ */
.activity-cal-wrap {
  margin-bottom: 16px;
}
.activity-cal-title {
  font-size: 12px; font-weight: 800; color: var(--text2);
  text-transform: uppercase; letter-spacing: .6px; margin-bottom: 10px;
  display: flex; justify-content: space-between; align-items: center;
}
.activity-cal-title span { color: var(--accent); font-size: 13px; letter-spacing: 0; text-transform: none; }
.activity-grid {
  display: grid;
  grid-template-columns: repeat(13, 1fr);
  gap: 3px;
}
.act-cell {
  aspect-ratio: 1; border-radius: 3px;
  background: var(--muted);
  transition: transform .15s;
  cursor: default;
  position: relative;
}
.act-cell:hover { transform: scale(1.4); z-index: 2; }
.act-cell.lvl1 { background: rgba(79,142,255,.3); }
.act-cell.lvl2 { background: rgba(79,142,255,.55); }
.act-cell.lvl3 { background: rgba(79,142,255,.8); }
.act-cell.lvl4 { background: #4f8eff; box-shadow: 0 0 6px rgba(79,142,255,.4); }
.act-cell.today { outline: 2px solid var(--accent); outline-offset: 1px; border-radius: 4px; }
.act-months {
  display: flex; justify-content: space-between;
  font-size: 9px; font-weight: 700; color: var(--text2);
  margin-top: 5px; padding: 0 1px;
}

/* ════════════════════════════════════
   ЕЖЕДНЕВНЫЕ ЧЕЛЛЕНДЖИ
════════════════════════════════════ */
.challenges-box {
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 14px 16px; margin-bottom: 16px;
}
.challenges-title {
  font-size: 13px; font-weight: 800; margin-bottom: 12px;
  display: flex; justify-content: space-between; align-items: center;
}
.challenges-refresh {
  font-size: 11px; color: var(--text2); font-weight: 600;
}
.challenge-item {
  display: flex; align-items: center; gap: 12px;
  padding: 10px 12px; border-radius: 12px;
  border: 1px solid var(--card-border); background: var(--muted2);
  margin-bottom: 8px; transition: var(--t); cursor: pointer;
}
.challenge-item:last-child { margin-bottom: 0; }
.challenge-item.done {
  border-color: rgba(34,216,143,.3); background: rgba(34,216,143,.06);
  opacity: .7;
}
.challenge-item:not(.done):hover { border-color: var(--accent); }
.ch-icon { font-size: 22px; flex-shrink: 0; }
.ch-info { flex: 1; }
.ch-name { font-size: 13px; font-weight: 800; margin-bottom: 2px; }
.ch-desc { font-size: 11px; color: var(--text2); font-weight: 600; }
.ch-xp {
  font-size: 12px; font-weight: 900; color: var(--accent);
  background: rgba(79,142,255,.1); border-radius: 8px;
  padding: 3px 8px; white-space: nowrap; flex-shrink: 0;
}
.ch-xp.done-xp { color: var(--ok); background: rgba(34,216,143,.1); }
.ch-check { font-size: 18px; flex-shrink: 0; }

/* ════════════════════════════════════
   РЕЖИМ «СОСТАВЬ ПРЕДЛОЖЕНИЕ»
════════════════════════════════════ */
.sentence-word-row {
  display: flex; flex-wrap: wrap; gap: 8px;
  justify-content: center; margin: 14px 0;
  min-height: 44px;
}
.sentence-tile {
  padding: 8px 14px; border-radius: 10px;
  background: var(--muted2); border: 2px solid var(--card-border);
  font-size: 15px; font-weight: 800; cursor: pointer;
  transition: var(--t); user-select: none;
  touch-action: manipulation; -webkit-tap-highlight-color: transparent;
}
.sentence-tile:hover { border-color: var(--accent); background: rgba(79,142,255,.1); }
.sentence-tile.used { opacity: .3; pointer-events: none; }
.sentence-answer-row {
  display: flex; flex-wrap: wrap; gap: 8px;
  justify-content: center; min-height: 52px;
  background: var(--muted2); border: 2px dashed var(--card-border);
  border-radius: 14px; padding: 10px 12px; margin-bottom: 14px;
  transition: border-color .2s;
}
.sentence-answer-row.has-words { border-color: rgba(79,142,255,.3); }
.sentence-answer-tile {
  padding: 8px 14px; border-radius: 10px;
  background: rgba(79,142,255,.15); border: 2px solid var(--accent);
  font-size: 15px; font-weight: 800; cursor: pointer;
  transition: var(--t);
}
.sentence-answer-tile:hover { background: rgba(255,79,109,.15); border-color: var(--err); }
.sentence-ru {
  font-size: 20px; font-weight: 900; text-align: center;
  margin-bottom: 6px; color: var(--text); min-height: 32px;
}
.sentence-hint-line {
  font-size: 12px; color: var(--text2); text-align: center;
  margin-bottom: 10px; font-style: italic;
}
.sentence-result-screen .result-hero { font-size: 60px; }

/* ════════════════════════════════════
   СУНДУКИ С НАГРАДАМИ
════════════════════════════════════ */
.chest-box {
  background: linear-gradient(135deg, rgba(255,194,71,.10), rgba(255,140,50,.05));
  border: 1px solid rgba(255,194,71,.25); border-radius: var(--r2);
  padding: 14px 16px; margin-bottom: 16px;
}
.chest-title {
  font-size: 13px; font-weight: 800; margin-bottom: 10px;
  display: flex; justify-content: space-between; align-items: center;
}
.chest-row { display: flex; gap: 10px; align-items: center; }
.chest-track { display: flex; gap: 6px; flex: 1; }
.chest-day {
  flex: 1; aspect-ratio: 1; border-radius: 10px;
  background: var(--muted); border: 1px solid var(--card-border);
  display: flex; align-items: center; justify-content: center;
  font-size: 16px; font-weight: 900; color: var(--text2);
  transition: var(--t); position: relative;
}
.chest-day.filled {
  background: linear-gradient(135deg, rgba(255,194,71,.25), rgba(255,140,50,.15));
  border-color: rgba(255,194,71,.5); color: var(--warn);
}
.chest-day.today { outline: 2px solid var(--warn); outline-offset: 1px; }
.chest-reward-btn {
  flex-shrink: 0; padding: 10px 14px; border-radius: 12px;
  border: none; cursor: pointer; font-weight: 900; font-size: 13px;
  background: linear-gradient(135deg, #ffc247, #ff8c32); color: #1a1408;
  transition: var(--t); white-space: nowrap;
}
.chest-reward-btn:disabled { opacity: .35; cursor: default; }
.chest-reward-btn.ready { animation: chestPulse 1.2s infinite; }
@keyframes chestPulse {
  0%,100% { box-shadow: 0 0 0 0 rgba(255,194,71,.5); }
  50% { box-shadow: 0 0 0 8px rgba(255,194,71,0); }
}
.chest-sub { font-size: 11px; color: var(--text2); font-weight: 600; margin-top: 8px; }

/* ════════════════════════════════════
   МАГАЗИН
════════════════════════════════════ */
.shop-balance {
  display: flex; align-items: center; justify-content: space-between;
  background: var(--muted2); border: 1px solid var(--card-border);
  border-radius: var(--r2); padding: 14px 16px; margin-bottom: 16px;
}
.shop-balance-label { font-size: 12px; font-weight: 800; color: var(--text2); text-transform: uppercase; letter-spacing: .5px; }
.shop-balance-val { font-size: 24px; font-weight: 900; color: var(--warn); }
.shop-section-title {
  font-size: 12px; font-weight: 800; color: var(--text2);
  text-transform: uppercase; letter-spacing: .6px; margin: 16px 0 10px;
}
.shop-grid {
  display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px;
}
.shop-grid.themes { grid-template-columns: repeat(3, 1fr); }

/* ── Кроппер фото ── */
.cropper-modal {
  display: none; position: fixed; inset: 0;
  background: rgba(8,12,20,.95); backdrop-filter: blur(16px);
  z-index: 9999; flex-direction: column;
  align-items: center; justify-content: center; padding: 20px;
}
.cropper-modal.open { display: flex; }
.cropper-title { font-size: 18px; font-weight: 900; margin-bottom: 16px; color: var(--text); }
.cropper-canvas-wrap {
  position: relative; width: 280px; height: 280px;
  border-radius: 20px; overflow: hidden;
  background: #000; touch-action: none; cursor: grab; flex-shrink: 0;
}
.cropper-canvas-wrap:active { cursor: grabbing; }
.cropper-canvas-wrap canvas { display: block; width: 100%; height: 100%; }
.cropper-overlay {
  position: absolute; inset: 0; pointer-events: none;
  box-shadow: inset 0 0 0 2px rgba(255,255,255,0.5);
  border-radius: 20px;
}
.cropper-circle-mask {
  position: absolute; inset: 0; pointer-events: none;
  background: radial-gradient(circle 120px at center, transparent 119px, rgba(0,0,0,0.55) 120px);
}
.cropper-hint { font-size: 12px; color: var(--text2); margin: 12px 0 4px; font-weight: 600; }
.cropper-zoom-row {
  display: flex; align-items: center; gap: 12px; margin: 8px 0 16px; width: 280px;
}
.cropper-zoom-row span { font-size: 16px; }
.cropper-zoom-row input[type=range] {
  flex: 1; accent-color: var(--accent); height: 4px;
}
.cropper-btns { display: flex; gap: 10px; width: 280px; }
.cropper-btn-cancel {
  flex: 1; padding: 14px; border-radius: 14px;
  background: var(--muted); color: var(--text);
  border: 1px solid var(--card-border); font-size: 15px; font-weight: 700; cursor: pointer;
}
.cropper-btn-ok {
  flex: 1; padding: 14px; border-radius: 14px;
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  color: #fff; border: none; font-size: 15px; font-weight: 800; cursor: pointer;
}
.frame-preview-wrap {
  width: 48px; height: 48px; border-radius: 14px;
  margin: 0 auto 6px; position: relative; flex-shrink: 0;
  isolation: isolate;
}
.frame-preview-inner {
  width: 100%; height: 100%; border-radius: 12px;
  background: linear-gradient(135deg,#4f8eff,#7c5cfc);
  display: flex; align-items: center; justify-content: center;
  font-size: 22px; overflow: hidden; position: relative; z-index: 1;
}
.frame-preview-ring {
  position: absolute; inset: -3px; border-radius: 16px; z-index: 0;
}
@keyframes rainbowSpin {
  0%   { background-position: 0% 50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
.frame-preview-ring.animated {
  background: linear-gradient(270deg,#ff4f6d,#ffc247,#22d88f,#4f8eff,#a78bfa,#ff4f6d);
  background-size: 300% 300%;
  animation: rainbowSpin 3s ease infinite;
}

/* Применённая рамка на реальном аватаре */
.avatar-frame-wrap {
  position: relative; display: inline-flex; flex-shrink: 0;
}
.avatar-frame-wrap .avatar {
  position: relative; z-index: 1;
}
.avatar-frame-ring {
  position: absolute; inset: -4px; border-radius: 22px; z-index: 0; pointer-events: none;
}
.avatar-frame-ring.animated {
  background: linear-gradient(270deg,#ff4f6d,#ffc247,#22d88f,#4f8eff,#a78bfa,#ff4f6d);
  background-size: 300% 300%;
  animation: rainbowSpin 3s ease infinite;
  border-radius: 22px;
}
/* Профиль — рамка вокруг кольца XP */
.profile-frame-ring {
  position: absolute; inset: -4px; border-radius: 50%; z-index: 0; pointer-events: none;
}
.profile-frame-ring.animated {
  background: linear-gradient(270deg,#ff4f6d,#ffc247,#22d88f,#4f8eff,#a78bfa,#ff4f6d);
  background-size: 300% 300%;
  animation: rainbowSpin 3s ease infinite;
}
.shop-item {
  background: var(--muted2); border: 2px solid var(--card-border);
  border-radius: 14px; padding: 10px 6px; text-align: center;
  cursor: pointer; transition: var(--t); position: relative;
}
.shop-item:hover { border-color: var(--accent); }
.shop-item.owned { border-color: rgba(34,216,143,.4); }
.shop-item.equipped { border-color: var(--accent); background: rgba(79,142,255,.12); }
.shop-item.locked { opacity: .55; }
.shop-item-icon { font-size: 26px; margin-bottom: 6px; }
.shop-item-name { font-size: 10px; font-weight: 800; color: var(--text2); margin-bottom: 4px; }
.shop-item-price {
  font-size: 11px; font-weight: 900; color: var(--warn);
  background: rgba(255,194,71,.1); border-radius: 8px; padding: 2px 6px;
}
.shop-item-price.owned-tag { color: var(--ok); background: rgba(34,216,143,.1); }
.shop-item-price.equipped-tag { color: var(--accent); background: rgba(79,142,255,.1); }
.theme-swatch {
  width: 100%; height: 28px; border-radius: 8px; margin-bottom: 6px;
}
.shop-toast {
  position: fixed; top: 50%; left: 50%; transform: translate(-50%,-50%);
  background: linear-gradient(135deg, rgba(255,194,71,.97), rgba(255,140,50,.97));
  color: #1a1408; padding: 28px 36px; border-radius: 24px;
  font-size: 20px; font-weight: 900; text-align: center;
  box-shadow: 0 20px 60px rgba(255,140,50,.4);
  z-index: 9999; pointer-events: none;
  animation: lvlupIn .5s cubic-bezier(.4,0,.2,1) forwards;
}

.onboard-overlay {
  position: fixed; inset: 0;
  background: rgba(8,12,20,.94); backdrop-filter: blur(16px);
  z-index: 300; display: flex; justify-content: center; align-items: center;
  animation: fadeIn .35s ease;
}
.onboard-card {
  background: var(--card); border: 1px solid var(--card-border);
  border-radius: 28px; padding: 32px 24px; max-width: 400px; width: 94%;
  backdrop-filter: blur(20px); box-shadow: 0 30px 80px rgba(0,0,0,.5);
  text-align: center; animation: screenIn .4s ease;
}
.onboard-emoji { font-size: 64px; margin-bottom: 12px; display: block; }
.onboard-title { font-size: 26px; font-weight: 900; margin-bottom: 8px;
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  -webkit-background-clip: text; -webkit-text-fill-color: transparent;
}
.onboard-sub { font-size: 14px; color: var(--text2); margin-bottom: 24px; line-height: 1.6; }
.onboard-name-input {
  width: 100%; padding: 14px 16px; font-size: 17px; font-weight: 700;
  background: var(--muted2); border: 2px solid var(--card-border);
  border-radius: 16px; color: var(--text); outline: none; margin-bottom: 12px;
  text-align: center; transition: var(--t);
}
.onboard-name-input:focus { border-color: var(--accent); }
.onboard-avatar-row {
  display: flex; flex-wrap: wrap; gap: 8px; justify-content: center; margin-bottom: 20px;
}
.onboard-avatar-btn {
  font-size: 28px; padding: 8px; border-radius: 12px; cursor: pointer;
  border: 2px solid transparent; background: var(--muted2); transition: var(--t);
}
.onboard-avatar-btn.selected { border-color: var(--accent); background: rgba(79,142,255,.15); }
.onboard-steps { display: flex; justify-content: center; gap: 6px; margin-bottom: 20px; }
.onboard-dot {
  width: 8px; height: 8px; border-radius: 50%; background: var(--muted);
  transition: background .3s, transform .3s;
}
.onboard-dot.active { background: var(--accent); transform: scale(1.3); }

/* ════════════════════════════════════
   ТОП СЛОЖНЫХ СЛОВ
════════════════════════════════════ */
.hard-word-item {
  display: flex; align-items: center; justify-content: space-between;
  padding: 10px 14px; background: var(--muted2);
  border: 1px solid var(--card-border); border-radius: 12px; margin-bottom: 8px;
  font-size: 13px; font-weight: 600; transition: var(--t);
}
.hard-word-item:hover { border-color: rgba(255,79,109,.3); }
.hard-word-bar-wrap { background: var(--muted); border-radius: 4px; height: 4px; width: 60px; }
.hard-word-bar { height: 100%; border-radius: 4px;
  background: linear-gradient(90deg, var(--err), var(--warn)); }
.hard-word-pct { font-size: 11px; font-weight: 800; color: var(--err); min-width: 32px; text-align: right; }

/* ════════════════════════════════════
   РЕКОРД ДНЯ
════════════════════════════════════ */
.daily-record-box {
  display: inline-flex; align-items: center; gap: 6px;
  background: rgba(255,194,71,.1); border: 1px solid rgba(255,194,71,.3);
  border-radius: 10px; padding: 4px 10px;
  font-size: 12px; font-weight: 800; color: var(--warn);
}
.record-new { animation: recordPop .6s cubic-bezier(.4,0,.2,1); }
@keyframes recordPop {
  0%  { transform: scale(1); }
  40% { transform: scale(1.18); }
  100%{ transform: scale(1); }
}

/* ════════════════════════════════════
   РЕЖИМ ПОВТОРЕНИЯ ОШИБОК
════════════════════════════════════ */
.retry-badge {
  display: inline-flex; align-items: center; gap: 5px;
  background: rgba(255,79,109,.12); border: 1px solid rgba(255,79,109,.3);
  border-radius: 10px; padding: 5px 12px;
  font-size: 12px; font-weight: 900; color: var(--err);
  margin-bottom: 14px;
}

/* ════════════════════════════════════
   ЭКРАН ПОВТОРЕНИЯ ОШИБОК
════════════════════════════════════ */
#mistakeRetryScreen .result-hero { font-size: 60px; }

/* ════════════════════════════════════
   ПЕРЕХОД СВАЙПОМ
════════════════════════════════════ */
.screen { transform-origin: center; }
@keyframes slideInRight {
  from { opacity:0; transform: translateX(28px) scale(0.98); }
  to   { opacity:1; transform: translateX(0) scale(1); }
}
@keyframes slideInLeft {
  from { opacity:0; transform: translateX(-28px) scale(0.98); }
  to   { opacity:1; transform: translateX(0) scale(1); }
}
.screen.slide-in-right { animation: slideInRight .28s cubic-bezier(.4,0,.2,1) both; }
.screen.slide-in-left  { animation: slideInLeft  .28s cubic-bezier(.4,0,.2,1) both; }

</style>
</head>
<body onclick="unlockAudio()">
<button id="soundBtn" style="display:none;">🔊</button>
<button id="themeBtn" style="display:none;">🌙</button>
<svg width="0" height="0" style="position:absolute">
  <defs>
    <linearGradient id="ringGrad" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#4f8eff"/>
      <stop offset="100%" stop-color="#a78bfa"/>
    </linearGradient>
    <linearGradient id="frameGradGold" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#ffc247"/><stop offset="100%" stop-color="#ff8c42"/>
    </linearGradient>
    <linearGradient id="frameGradFire" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#ff4f6d"/><stop offset="100%" stop-color="#ff8c42"/>
    </linearGradient>
    <linearGradient id="frameGradNeon" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#22d88f"/><stop offset="100%" stop-color="#4f8eff"/>
    </linearGradient>
    <linearGradient id="frameGradGalaxy" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#7c5cfc"/><stop offset="50%" stop-color="#4f8eff"/><stop offset="100%" stop-color="#22d88f"/>
    </linearGradient>
    <linearGradient id="frameGradRainbow" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#ff4f6d"/><stop offset="33%" stop-color="#ffc247"/><stop offset="66%" stop-color="#22d88f"/><stop offset="100%" stop-color="#a78bfa"/>
    </linearGradient>
    <linearGradient id="frameGradDiamond" x1="0%" y1="0%" x2="100%" y2="100%">
      <stop offset="0%" stop-color="#e0f2ff"/><stop offset="50%" stop-color="#74c0fc"/><stop offset="100%" stop-color="#e0f2ff"/>
    </linearGradient>
  </defs>
</svg>

<!-- Фоновые сферы -->
<div class="bg-orbs">
  <div class="bg-orb"></div>
  <div class="bg-orb"></div>
  <div class="bg-orb"></div>
</div>

<div class="container">
  <!-- header removed — navigation moved to bottom nav -->

  <!-- ═══ ГЛАВНОЕ МЕНЮ ═══ -->
  <div id="menuScreen" class="card screen active">
    <div class="menu-top">
      <div class="avatar" id="menuAvatar">🦁</div>
      <div class="menu-info">
        <div class="menu-name">English Trainer</div>
        <div class="rank-badge" id="userRank">Новичок</div>
      </div>
    </div>

    <div style="display:flex;justify-content:space-between;font-size:13px;font-weight:700;margin-bottom:4px;">
      <span style="color:var(--text2)">Уровень <span id="menuLevel" style="color:var(--accent)">1</span></span>
      <span style="color:var(--text2)"><span id="menuXp">0</span> / <span id="nextLvlXp">100</span> XP</span>
    </div>
    <div class="level-bar-wrap"><div class="level-bar" id="levelBar"></div></div>
    <div class="level-txt" id="levelTxt"></div>

    <div class="day-streak-box" id="dayStreakBox">
      <div class="day-streak-flame">🔥</div>
      <div class="day-streak-info">
        <div class="day-streak-label">Дней подряд</div>
        <div class="day-streak-val" id="dayStreakVal">0</div>
        <div class="day-streak-sub" id="dayStreakSub">Занимайся каждый день!</div>
      </div>
      <div style="text-align:right;">
        <div style="font-size:11px;color:var(--text2);font-weight:700;">РЕКОРД</div>
        <div id="dayStreakBest" style="font-size:18px;font-weight:900;color:var(--accent)">0</div>
      </div>
    </div>

    <div class="menu-btns" style="margin-bottom:16px;">
      <button class="start-btn-mega" onclick="showModeSelect()"><span>🚀</span> Начать тренировку</button>
    </div>

    <!-- Ежедневные челленджи -->
    <div class="challenges-box">
      <div class="challenges-title">
        🎯 Челленджи на сегодня
        <span class="challenges-refresh" id="chalRefreshTime"></span>
      </div>
      <div id="challengesList"></div>
    </div>

    <div class="word-of-day-card" id="wodCard">
      <div class="wod-label">📖 Слово дня</div>
      <div class="wod-word" id="wodWord">—</div>
      <div class="wod-pron" id="wodPron"></div>
      <div class="wod-translation" id="wodTranslation">—</div>
      <div class="wod-sentence" id="wodSentence"></div>
      <div class="wod-actions">
        <button class="wod-btn-know" id="wodKnowBtn" onclick="wodMarkKnown()">✅ Запомнил!</button>
        <button class="wod-speak-btn" onclick="wodSpeak()">🔊</button>
      </div>
    </div>

    <div class="menu-btns">
      <button class="btn-secondary" onclick="openDictSearch()">🔍 Поиск слова</button>
      <button class="btn-secondary" onclick="showScreen('pronounceScreen');initPronunScreen();">🗣️ Произношение</button>
      <button class="btn-secondary" onclick="showShopScreen()">🛍️ Магазин</button>
    </div>
  </div>

  <!-- ═══ ВЫБОР РЕЖИМА ═══ -->
  <div id="modeSelectScreen" class="card screen">
    <div style="font-size:22px;font-weight:900;margin-bottom:4px;">Выбери режим</div>
    <div style="color:var(--text2);font-size:13px;margin-bottom:4px;">Как будем тренироваться?</div>

    <div class="mode-grid">
      <div class="mode-card" onclick="startEndless()">
        <span class="mc-icon">♾️</span>
        Бесконечный
        <div class="mc-sub">Слова идут подряд, сам останавливаешь</div>
      </div>
      <div class="mode-card" onclick="showSprintSelect()">
        <span class="mc-icon">⚡</span>
        Спринт
        <div class="mc-sub">10, 20 или 30 слов — итог в конце</div>
      </div>
      <div class="mode-card" onclick="startFlashcards()">
        <span class="mc-icon">🃏</span>
        Флэшкарты
        <div class="mc-sub">Смотришь слово, переворачиваешь — видишь перевод</div>
      </div>
      <div class="mode-card" onclick="startAnagram()">
        <span class="mc-icon">🧩</span>
        Анаграмма
        <div class="mc-sub">Буквы перемешаны — собери слово правильно</div>
      </div>
      <div class="mode-card" onclick="startDictant()" style="grid-column:span 2;">
        <span class="mc-icon">🎧</span>
        Диктант
        <div class="mc-sub">Слово произносится вслух — напиши перевод на русском</div>
      </div>
      <div class="mode-card" onclick="startSentence()" style="grid-column:span 2;">
        <span class="mc-icon">🗣️</span>
        Предложение
        <div class="mc-sub">Слова перемешаны — составь правильное предложение</div>
      </div>
      <div class="mode-card mistakes-card" onclick="startPersistentMistakeMode()" style="grid-column:span 2;" id="mistakesModeCard">
        <span class="mc-icon">💪</span>
        Только ошибки <span class="mistakes-count-badge" id="mistakesModeCount" style="display:none;"></span>
        <div class="mc-sub">Повторить слова, в которых были ошибки</div>
      </div>
    </div>

    <div id="sprintLenRow" style="display:none;margin-top:16px;">
      <div style="font-size:13px;font-weight:800;margin-bottom:10px;color:var(--text2);text-transform:uppercase;letter-spacing:.5px;">Количество слов</div>
      <div class="sprint-opts">
        <button class="sprint-opt-btn active" onclick="setSprintLen(10,this)">10</button>
        <button class="sprint-opt-btn" onclick="setSprintLen(20,this)">20</button>
        <button class="sprint-opt-btn" onclick="setSprintLen(30,this)">30</button>
      </div>
      <button class="btn-primary" style="width:100%;margin-top:12px;" onclick="startSprint()">🏁 Поехали!</button>
    </div>

    <button class="btn-secondary" style="width:100%;margin-top:14px;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>

  <!-- ═══ ТРЕНИРОВКА ═══ -->
  <div id="gameScreen" class="card screen">
    <div class="game-header">
      <button class="pause-btn" onclick="pauseGame()">⏸ Пауза</button>
      <div class="stats-bar">
        <div class="mini-stat">⭐ <span id="gameXp">0</span></div>
        <div class="mini-stat">🔥 <span id="gameStreak">0</span></div>
        <div class="mini-stat">🎯 <span id="gameAccuracy">—</span></div>
        <div class="mini-stat" id="livesBar" style="display:none;"><span id="livesDisplay">❤️❤️❤️</span></div>
        <div id="bonusActiveBadge" style="display:none;"><span class="bonus-active-badge">⚡ ×2 XP!</span></div>
      </div>
    </div>

    <div class="prog-wrap"><div class="prog-bar" id="xpBar"></div></div>
    <div id="sprintCounterRow" class="sprint-counter" style="display:none;">
      Слово <span id="sprintCurrent">1</span> из <span id="sprintTotal">10</span>
    </div>
    <div class="prog-wrap" id="sprintProgWrap" style="display:none;">
      <div class="prog-bar sprint" id="sprintBar"></div>
    </div>

    <div class="g-mode-selector">
      <button class="g-mode-btn active" id="modeInputBtn" onclick="setGameMode('write')">⌨️ Ввод</button>
      <button class="g-mode-btn" id="modeTestBtn" onclick="setGameMode('test')">🔘 Тест</button>
    </div>

    <div class="timer-box">
      <div class="timer-ring">
        <svg viewBox="0 0 36 36" width="40" height="40">
          <circle class="bg-ring" cx="18" cy="18" r="15.9" stroke-dasharray="100" stroke-dashoffset="0"/>
          <circle class="fg-ring" id="timerCircle" cx="18" cy="18" r="15.9"/>
        </svg>
        <div class="timer-num" id="timer">15</div>
      </div>
    </div>

    <div class="word-display">
      <span id="word"></span>
      <button class="speaker-btn" onclick="speakCurrent()" id="speakBtn" style="display:none;">🔊</button>
    </div>
    <div id="dictantRepeatWrap" style="display:none;text-align:center;margin:-4px 0 10px;">
      <button onclick="speakDictantAgain()" style="background:var(--muted);border:1px solid var(--card-border);color:var(--accent);border-radius:12px;padding:8px 22px;font-size:13px;font-weight:800;cursor:pointer;transition:var(--t);">🔊 Повторить слово</button>
    </div>

    <div id="inputSection">
      <div class="input-wrapper">
        <input type="text" id="answer" placeholder="Введите ответ…" autocomplete="off" spellcheck="false">
      </div>
    </div>
    <div id="testSection" style="display:none;">
      <div class="options-grid" id="optionsGrid"></div>
    </div>

    <div class="actions">
      <button class="btn-secondary btn-hint" onclick="getHint()" id="hintBtn">💡 −5XP</button>
      <button class="btn-secondary" onclick="skipWord()" id="skipBtn">⏭ Пропуск</button>
      <button class="btn-primary" onclick="checkAnswer()" id="checkBtn">✅ Проверить <span style="opacity:.5;font-size:11px;font-weight:600;">[Enter]</span></button>
    </div>
    <div class="result" id="result"></div>
  </div>

  <!-- ═══ ФЛЭШКАРТЫ ═══ -->
  <div id="flashcardScreen" class="card screen">
    <div class="game-header">
      <button class="pause-btn" onclick="exitFlashcards()">← Выйти</button>
      <div class="stats-bar">
        <div class="mini-stat">✅ <span id="fcKnow">0</span></div>
        <div class="mini-stat">❌ <span id="fcUnknown">0</span></div>
      </div>
    </div>

    <div class="fc-counter" id="fcCounter">Карточка 1 из 20</div>
    <div class="fc-progress-row">
      <div class="fc-prog-item fc-prog-know">✅ Знаю: <span id="fcKnowCount">0</span></div>
      <div class="fc-prog-item fc-prog-unkn">❌ Учить: <span id="fcUnknCount">0</span></div>
      <div class="fc-prog-item fc-prog-left">📋 Осталось: <span id="fcLeftCount">0</span></div>
    </div>

    <div class="prog-wrap">
      <div class="prog-bar sprint" id="fcBar"></div>
    </div>

    <div class="flashcard-wrap" onclick="flipCard()">
      <div class="flashcard" id="flashcard">
        <div class="fc-face fc-front">
          <div class="fc-lang">🇷🇺 Русский</div>
          <div id="fcFrontWord">слово</div>
          <div class="fc-hint">Нажми чтобы перевернуть 👆</div>
        </div>
        <div class="fc-face fc-back">
          <div class="fc-lang">🇬🇧 English</div>
          <div id="fcBackWord">word</div>
          <div class="fc-hint">Ты знаешь это слово?</div>
        </div>
      </div>
    </div>

    <div class="flashcard-actions">
      <button class="fc-btn-unknown" onclick="fcAnswer(false)">❌ Учить</button>
      <button class="fc-btn-know" onclick="fcAnswer(true)">✅ Знаю!</button>
    </div>
  </div>

  <!-- ═══ РЕЗУЛЬТАТ ФЛЭШКАРТ ═══ -->
  <div id="flashcardResultScreen" class="card screen">
    <div class="result-hero" id="fcResultEmoji">🃏</div>
    <div class="result-title" id="fcResultTitle">Раунд завершён!</div>
    <div class="result-stats">
      <div class="result-stat">✅ Знаю<strong id="fcResKnow">0</strong></div>
      <div class="result-stat">📚 Учить<strong id="fcResUnknown">0</strong></div>
    </div>
    <div class="result-verdict" id="fcVerdict"></div>
    <div style="display:flex;flex-direction:column;gap:10px;">
      <button class="btn-primary" onclick="startFlashcards()">🔄 Ещё раз</button>
      <button class="btn-secondary" onclick="fcRepeatUnknown()" id="fcRepeatBtn">📚 Повторить ошибки</button>
      <button class="btn-secondary" onclick="showScreen('menuScreen');updateMenu();">🏠 В меню</button>
    </div>
  </div>

  <!-- ═══ АНАГРАММА ═══ -->
  <div id="anagramScreen" class="card screen">
    <div class="game-header">
      <button class="pause-btn" onclick="exitAnagram()">← Выйти</button>
      <div class="stats-bar">
        <div class="mini-stat">⭐ <span id="agXp">0</span></div>
        <div class="mini-stat">🔥 <span id="agStreak">0</span></div>
      </div>
    </div>

    <div class="prog-wrap"><div class="prog-bar sprint" id="agBar"></div></div>
    <div class="sprint-counter" id="agCounter" style="text-align:right;margin-bottom:6px;">Слово 1 из 10</div>

    <div class="timer-box">⏱️ <span id="agTimer" style="font-size:20px;font-weight:900;">20</span></div>

    <div class="anagram-q">Составь слово по переводу:</div>
    <div class="anagram-word" id="agWord">слово</div>

    <div class="anagram-blank" id="agBlank"></div>

    <div class="anagram-answer" id="agAnswer" onclick="anagramAnswerClick(event)"></div>

    <div class="anagram-letters" id="agLetters"></div>

    <div class="actions">
      <button class="btn-secondary btn-hint" onclick="agHint()" id="agHintBtn">💡 Подсказка</button>
      <button class="btn-secondary" onclick="agSkip()">⏭ Пропуск</button>
      <button class="btn-primary" onclick="agCheck()">✅ Проверить</button>
    </div>
    <div class="result" id="agResult"></div>
  </div>

  <!-- ═══ РЕЗУЛЬТАТЫ СПРИНТА ═══ -->
  <div id="resultsScreen" class="card screen">
    <div class="result-hero" id="resultsEmoji">🏆</div>
    <div class="result-title" id="resultsTitle">Спринт завершён!</div>
    <div class="result-pct" id="resultsPct">0%</div>
    <div id="dailyRecordBadge" style="text-align:center;margin-bottom:12px;display:none;">
      <span class="daily-record-box">🏅 Рекорд дня!</span>
    </div>
    <div class="result-stats">
      <div class="result-stat">✅ Правильно<strong id="resCorrect">0</strong></div>
      <div class="result-stat">❌ Ошибки<strong id="resErrors">0</strong></div>
      <div class="result-stat">⭐ XP<strong id="resXp">0</strong></div>
      <div class="result-stat">🔥 Серия<strong id="resStreak">0</strong></div>
    </div>
    <div class="result-verdict" id="resultsVerdict"></div>
    <div style="display:flex;flex-direction:column;gap:10px;">
      <button class="btn-primary" id="resultsAgainBtn" onclick="resultsPlayAgain()">🔄 Ещё раз</button>
      <button class="btn-danger" id="retryMistakesBtn" onclick="startMistakeRetry()" style="display:none;border-radius:var(--r2);padding:14px;font-weight:800;font-size:15px;cursor:pointer;transition:var(--t);">💪 Повторить ошибки</button>
      <button class="btn-secondary" onclick="showScreen('menuScreen');updateMenu();">🏠 В меню</button>
      <button class="btn-secondary" onclick="showScreen('modeSelectScreen')">← Выбор режима</button>
    </div>
  </div>

  <!-- ═══ МОИ СЛОВА ═══ -->
  <div id="myWordsScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:12px;">📝 Мои слова</div>
    <div style="font-size:13px;color:var(--text2);margin-bottom:14px;">Добавь свои пары — они войдут в категорию «Мои слова».</div>
    <div class="mw-add-row">
      <input type="text" id="mwRu" placeholder="🇷🇺 Русский">
      <input type="text" id="mwEn" placeholder="🇬🇧 English">
      <button class="mw-add-btn" onclick="addMyWord()">+ Добавить</button>
    </div>
    <div class="mw-list" id="mwList"></div>
    <button class="btn-secondary" style="width:100%;margin-top:6px;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>

  <!-- ═══ ПРОФИЛЬ ═══ -->
  <div id="profileScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:16px;">👤 Мой профиль</div>
    <div style="text-align:center;margin-bottom:20px;">
      <div class="profile-ring-wrap">
        <svg width="110" height="110" viewBox="0 0 110 110">
          <circle class="ring-bg" cx="55" cy="55" r="48"/>
          <circle id="profileFrameRing" cx="55" cy="55" r="48"
            fill="none" stroke-width="5" stroke-linecap="round"
            stroke-dasharray="301.6" stroke-dashoffset="0"
            stroke="transparent" style="transition:stroke .4s ease;"/>
          <circle class="ring-fg" id="profileRingFg" cx="55" cy="55" r="48"
            stroke-dasharray="301.6" stroke-dashoffset="301.6"/>
        </svg>
        <div class="profile-avatar-center" onclick="openAvatarPicker()" title="Сменить аватар">
          <span id="profileAvatarBig">🦁</span>
        </div>
      </div>
      <div style="font-size:12px;color:var(--text2);font-weight:600;margin-top:4px;">Нажми чтобы сменить аватар</div>
    </div>
    <div id="avatarPicker" style="display:none;margin-bottom:16px;">
      <div style="font-size:13px;font-weight:800;color:var(--text2);margin-bottom:10px;text-transform:uppercase;letter-spacing:.5px;">Выбери аватар</div>
      <div style="margin-bottom:12px;">
        <button class="avatar-photo-btn" onclick="document.getElementById('avatarPhotoInput').click()">📷 Загрузить фото</button>
        <button class="avatar-photo-del" id="avatarPhotoDelBtn" style="display:none;" onclick="removePhotoAvatar()">🗑 Удалить фото</button>
        <input type="file" id="avatarPhotoInput" accept="image/*" style="display:none;" onchange="handleAvatarPhoto(this)">
      </div>
      <div id="avatarGrid" style="display:grid;grid-template-columns:repeat(6,1fr);gap:8px;"></div>
    </div>
    <div style="margin-bottom:14px;">
      <label style="font-size:12px;font-weight:800;color:var(--text2);text-transform:uppercase;letter-spacing:.5px;display:block;margin-bottom:6px;">👤 Имя</label>
      <input type="text" id="profileName" placeholder="Введи своё имя…" maxlength="24" style="width:100%;padding:12px 14px;background:var(--muted2);border:1px solid var(--card-border);border-radius:var(--r2);color:var(--text);font-size:15px;font-weight:700;outline:none;">
    </div>
    <div style="margin-bottom:14px;">
      <label style="font-size:12px;font-weight:800;color:var(--text2);text-transform:uppercase;letter-spacing:.5px;display:block;margin-bottom:6px;">🎯 Цель обучения</label>
      <select id="profileGoal" style="width:100%;padding:12px 14px;background:var(--muted2);border:1px solid var(--card-border);border-radius:var(--r2);color:var(--text);font-size:14px;font-weight:700;outline:none;">
        <option value="">— Не указано —</option>
        <option value="school">🏫 Для школы / учёбы</option>
        <option value="work">💼 Для работы</option>
        <option value="travel">✈️ Для путешествий</option>
        <option value="exam">📋 Подготовка к экзамену</option>
        <option value="fun">🎮 Просто для себя</option>
        <option value="emigration">🌍 Переезд за рубеж</option>
      </select>
    </div>
    <div style="margin-bottom:14px;">
      <label style="font-size:12px;font-weight:800;color:var(--text2);text-transform:uppercase;letter-spacing:.5px;display:block;margin-bottom:6px;">📊 Уровень английского</label>
      <div id="levelPicker" style="display:grid;grid-template-columns:repeat(3,1fr);gap:8px;"></div>
    </div>
    <div style="margin-bottom:20px;">
      <label style="font-size:12px;font-weight:800;color:var(--text2);text-transform:uppercase;letter-spacing:.5px;display:block;margin-bottom:6px;">📝 О себе</label>
      <textarea id="profileBio" placeholder="Расскажи немного о себе…" maxlength="120" rows="3" style="width:100%;padding:12px 14px;background:var(--muted2);border:1px solid var(--card-border);border-radius:var(--r2);color:var(--text);font-size:14px;font-weight:600;outline:none;resize:none;font-family:inherit;line-height:1.5;"></textarea>
      <div id="bioCounter" style="font-size:11px;color:var(--text2);text-align:right;margin-top:3px;">0 / 120</div>
    </div>
    <div style="background:var(--muted2);border:1px solid var(--card-border);border-radius:var(--r2);padding:14px 16px;margin-bottom:20px;">
      <div style="font-size:12px;font-weight:800;color:var(--text2);text-transform:uppercase;letter-spacing:.5px;margin-bottom:12px;">🏅 Статистика</div>
      <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:10px;text-align:center;">
        <div style="background:rgba(79,142,255,.08);border-radius:10px;padding:10px 6px;">
          <div id="pStXp" style="font-size:22px;font-weight:900;color:var(--accent)">0</div>
          <div style="font-size:11px;color:var(--text2);font-weight:700;">XP всего</div>
        </div>
        <div style="background:rgba(255,194,71,.08);border-radius:10px;padding:10px 6px;">
          <div id="pStLvl" style="font-size:22px;font-weight:900;color:var(--warn)">1</div>
          <div style="font-size:11px;color:var(--text2);font-weight:700;">Уровень</div>
        </div>
        <div style="background:rgba(34,216,143,.08);border-radius:10px;padding:10px 6px;">
          <div id="pStStreak" style="font-size:22px;font-weight:900;color:var(--ok)">0</div>
          <div style="font-size:11px;color:var(--text2);font-weight:700;">Макс. серия</div>
        </div>
      </div>
      <div style="margin-top:12px;padding-top:12px;border-top:1px solid var(--card-border);text-align:center;">
        <div id="pRankLabel" style="font-size:16px;font-weight:900;">🌱 Новичок</div>
        <div style="font-size:11px;color:var(--text2);margin-top:2px;">Текущий ранг</div>
        <div style="margin-top:8px;">
          <div style="font-size:11px;color:var(--text2);margin-bottom:4px;">Прогресс уровня</div>
          <div style="background:var(--muted);border-radius:6px;overflow:hidden;height:6px;">
            <div id="pProfileBar" style="height:100%;border-radius:6px;background:linear-gradient(90deg,var(--accent),var(--accent2));transition:width .5s;"></div>
          </div>
          <div id="pProfileBarTxt" style="font-size:10px;color:var(--text2);margin-top:3px;text-align:right;font-weight:700;"></div>
        </div>
      </div>
    </div>

    <!-- Календарь активности -->
    <div style="background:var(--muted2);border:1px solid var(--card-border);border-radius:var(--r2);padding:14px 16px;margin-bottom:20px;">
      <div class="activity-cal-title">
        📆 Активность за 3 месяца
        <span id="actCalStreak"></span>
      </div>
      <div class="activity-grid" id="activityGrid"></div>
      <div class="act-months" id="actMonths"></div>
    </div>

    <button class="btn-primary" style="width:100%;margin-bottom:10px;" onclick="saveProfile()">💾 Сохранить профиль</button>
    <button class="btn-secondary" style="width:100%;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>

  <!-- ═══ СТАТИСТИКА ═══ -->
  <div id="statsScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:16px;">📊 Статистика</div>

    <div class="daily-box">
      <div class="daily-header">
        <span class="daily-title">📅 Дневная цель</span>
        <button class="daily-change-btn" onclick="openDiffModal();event.stopPropagation();">⚙️ Сложность</button>
      </div>
      <div class="daily-difficulty-badge" id="dailyDiffBadge">🌱 Легко</div>
      <div class="daily-progress-row">
        <span class="daily-xp-done" id="dailyXpDone">0 XP набрано</span>
        <span class="daily-xp-goal" id="dailyGoalText">цель: 50 XP</span>
      </div>
      <div class="prog-wrap" style="margin-bottom:0;height:10px;">
        <div class="prog-bar" id="dailyGoalBar"></div>
      </div>
      <div class="daily-complete-banner" id="dailyCompleteBanner">🎉 Дневная цель выполнена! Отличная работа!</div>
    </div>

    <div class="stats-grid">
      <div class="stats-cell">
        <div class="sc-icon">⭐</div>
        <div class="sc-label">Всего XP</div>
        <div class="sc-val" id="stXp">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">📖</div>
        <div class="sc-label">Уровень</div>
        <div class="sc-val" id="stLevel">1</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">✅</div>
        <div class="sc-label">Правильно</div>
        <div class="sc-val" id="stCorrect">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">📝</div>
        <div class="sc-label">Всего ответов</div>
        <div class="sc-val" id="stTotal">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">🔥</div>
        <div class="sc-label">Макс. серия</div>
        <div class="sc-val" id="stStreak">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">📅</div>
        <div class="sc-label">XP сегодня</div>
        <div class="sc-val" id="stDailyXp">0</div>
      </div>
      <div class="stats-cell">
        <div class="sc-icon">🗓️</div>
        <div class="sc-label">Дней подряд</div>
        <div class="sc-val" id="stDayStreak">0</div>
      </div>
      <div class="stats-cell" style="grid-column:span 2;">
        <div class="sc-icon">📈</div>
        <div class="sc-label">XP за неделю</div>
        <div id="statsWeeklyChart" class="weekly-chart" style="height:44px;margin-top:8px;"></div>
      </div>
    </div>

    <div class="accuracy-visual">
      <div class="section-title">Точность ответов</div>
      <div style="display:flex;justify-content:space-between;font-size:22px;font-weight:900;margin-bottom:6px;">
        <span id="stAccPct">0%</span>
        <span style="color:var(--text2);font-size:14px;font-weight:700;align-self:flex-end" id="stAccDetail"></span>
      </div>
      <div class="acc-bar-wrap">
        <div class="acc-bar-ok" id="stAccBar" style="width:0%"></div>
      </div>
    </div>

    <div class="rank-progress">
      <div class="section-title">Путь к мастерству</div>
      <div class="rank-levels" id="rankLevels"></div>
    </div>

    <div class="accuracy-visual" id="hardWordsSection" style="display:none;">
      <div class="section-title">🔴 Топ сложных слов</div>
      <div id="hardWordsList"></div>
    </div>

    <div class="accuracy-visual" id="catAccSection">
      <div class="section-title">📂 Точность по категориям</div>
      <div class="cat-stats-grid" id="catStatsList">
        <div class="empty-state" style="padding:10px 0;font-size:13px;">Пока нет данных по категориям</div>
      </div>
    </div>

    <button class="btn-secondary" style="width:100%;margin-bottom:10px;" onclick="showScreen('menuScreen')">← Назад</button>
    <div style="text-align:center;">
      <button class="reset-btn" onclick="confirmReset()">⚠️ Сбросить прогресс</button>
    </div>
  </div>

  <!-- ═══ ДОСТИЖЕНИЯ И ОШИБКИ ═══ -->
  <div class="card screen" id="dashboardCard">
    <div style="font-size:20px;font-weight:900;margin-bottom:16px;background:linear-gradient(135deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;">🏆 Достижения</div>

    <!-- Сундук с наградами -->
    <div class="chest-box">
      <div class="chest-title">
        🎁 Сундук недели
        <span style="font-size:11px;color:var(--text2);font-weight:700;">за выполнение всех челленджей</span>
      </div>
      <div class="chest-row">
        <div class="chest-track" id="chestTrack"></div>
        <button class="chest-reward-btn" id="chestClaimBtn" onclick="claimChest()" disabled>Открыть 🔒</button>
      </div>
      <div class="chest-sub" id="chestSub">Выполни все 3 челленджа сегодня, чтобы продвинуться</div>
    </div>

    <div class="tabs">
      <div class="tab active" onclick="switchTab('tab-ach')" id="achBtn">🏆 Достижения <span id="achCountBadge" style="background:var(--ok);color:#000;font-size:10px;padding:1px 6px;border-radius:6px;font-weight:900;"></span></div>
      <div class="tab" onclick="switchTab('tab-mis')" id="misBtn">❌ Ошибки <span id="misCountBadge" style="background:var(--err);color:#fff;font-size:10px;padding:1px 6px;border-radius:6px;font-weight:900;display:none;"></span></div>
    </div>
    <div class="tab-content active" id="tab-ach"><ul id="achievements"></ul></div>
    <div class="tab-content" id="tab-mis">
      <ul id="mistakes"></ul>
      <div style="display:flex;gap:8px;margin-top:6px;">
        <button class="btn-danger" id="retryAllMistakesBtn" onclick="startMistakeRetry()" style="flex:1;border-radius:var(--r2);padding:11px;font-weight:800;font-size:13px;cursor:pointer;transition:var(--t);display:none;">💪 Тренировать</button>
        <button class="clean-btn" onclick="clearMistakes()" style="flex:1;">Очистить</button>
      </div>
    </div>
  </div>


  <!-- ═══ ПРОИЗНОШЕНИЕ ═══ -->
  <div id="pronounceScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:6px;">🗣️ Произношение</div>
    <div style="font-size:13px;color:var(--text2);margin-bottom:16px;">Введи слово, букву или фразу — и услышишь, как это звучит по-английски.</div>

    <div id="pronunNoSupport" class="pronun-no-support" style="display:none;">
      ⚠️ Твой браузер не поддерживает синтез речи. Попробуй Chrome или Edge.
    </div>

    <div class="pronun-input-row">
      <input type="text" id="pronunInput" placeholder="Введи слово или букву…" autocomplete="off" spellcheck="false" onkeydown="if(event.key==='Enter') pronounceWord()">
      <button class="pronun-speak-btn" id="pronunSpeakBtn" onclick="pronounceWord()" title="Произнести">🔊</button>
    </div>

    <div class="pronun-voice-row">
      <label>Голос</label>
      <select id="pronunVoice"></select>
    </div>

    <div class="pronun-speed-row">
      <label>Скорость</label>
      <input type="range" id="pronunSpeed" min="0.5" max="2" step="0.1" value="1" oninput="document.getElementById('pronunSpeedVal').textContent=parseFloat(this.value).toFixed(1)+'×'">
      <span class="pronun-speed-val" id="pronunSpeedVal">1.0×</span>
    </div>

    <div class="pronun-speed-row" style="margin-bottom:6px;">
      <label>Тональность</label>
      <input type="range" id="pronunPitch" min="0.5" max="2" step="0.1" value="1" oninput="document.getElementById('pronunPitchVal').textContent=parseFloat(this.value).toFixed(1)+'×'">
      <span class="pronun-speed-val" id="pronunPitchVal">1.0×</span>
    </div>

    <div class="divider"></div>
    <div class="section-title">История 🕐</div>
    <div class="pronun-history" id="pronunHistory">
      <div class="empty-state" id="pronunHistEmpty">Ещё ничего не произносилось.</div>
    </div>

    <button class="btn-secondary" style="width:100%;margin-top:14px;" onclick="showScreen('menuScreen')">← Назад</button>
  </div>


  <!-- ═══ НАСТРОЙКИ ═══ -->
  <div id="settingsScreen" class="card screen">
    <div style="font-size:20px;font-weight:900;margin-bottom:20px;background:linear-gradient(135deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent;">⚙️ Настройки</div>

    <!-- Звук -->
    <div class="settings-section">
      <div class="settings-section-title">🔊 Звук</div>
      <div class="settings-row-item">
        <div class="settings-row-info">
          <div class="settings-row-label">Звуковые эффекты</div>
          <div class="settings-row-sub">Звуки при ответах и действиях</div>
        </div>
        <div class="toggle-wrap" onclick="toggleSoundSetting()" id="soundToggleWrap">
          <div class="toggle-knob" id="soundToggleKnob"></div>
        </div>
      </div>
    </div>

    <!-- Тема -->
    <div class="settings-section">
      <div class="settings-section-title">🎨 Оформление</div>
      <div class="settings-row-item" style="margin-bottom:10px;">
        <div class="settings-row-info">
          <div class="settings-row-label">Тема оформления</div>
          <div class="settings-row-sub" id="themeSubLabel">Тёмная тема активна</div>
        </div>
      </div>
      <div class="theme-picker">
        <div class="theme-option" id="themeOptDark" onclick="setTheme('dark')">
          <div class="theme-preview theme-preview-dark">
            <div class="tp-bar"></div><div class="tp-bar short"></div>
          </div>
          <div class="theme-option-label">🌙 Тёмная</div>
        </div>
        <div class="theme-option" id="themeOptLight" onclick="setTheme('light')">
          <div class="theme-preview theme-preview-light">
            <div class="tp-bar"></div><div class="tp-bar short"></div>
          </div>
          <div class="theme-option-label">☀️ Светлая</div>
        </div>
        <div class="theme-option" id="themeOptAuto" onclick="setTheme('auto')">
          <div class="theme-preview theme-preview-auto">
            <div class="tp-half dark-half"></div>
            <div class="tp-half light-half"></div>
          </div>
          <div class="theme-option-label">🔄 Авто</div>
        </div>
      </div>
    </div>

    <!-- Дневная цель -->
    <div class="settings-section">
      <div class="settings-section-title">🎯 Дневная цель</div>
      <div class="settings-row-item">
        <div class="settings-row-info">
          <div class="settings-row-label">Сложность</div>
          <div class="settings-row-sub" id="settingsDiffSub">Лёгкая — 30 XP/день</div>
        </div>
        <button class="settings-mini-btn" onclick="openDiffModal()">Изменить</button>
      </div>
    </div>

    <!-- Игра -->
    <div class="settings-section">
      <div class="settings-section-title">🎮 Игра</div>
      <div class="settings-row-item">
        <div class="settings-row-info">
          <div class="settings-row-label">Направление перевода</div>
          <div class="settings-row-sub">По умолчанию для тренировок</div>
        </div>
      </div>
      <select id="settingsMode" onchange="document.getElementById('mode').value=this.value;savePrefs();" style="margin-bottom:10px;">
        <option value="ru-en">🇷🇺 → 🇬🇧 Русский → Английский</option>
        <option value="en-ru">🇬🇧 → 🇷🇺 Английский → Русский</option>
        <option value="mixed">🔀 Смешанный</option>
      </select>
      <div class="settings-row-item">
        <div class="settings-row-info">
          <div class="settings-row-label">Категория слов</div>
          <div class="settings-row-sub">Какие слова учить</div>
        </div>
      </div>
      <select id="settingsCategory" onchange="document.getElementById('category').value=this.value;savePrefs();" style="margin-bottom:0;">
        <option value="all">🌍 Все категории</option>
        <option value="jobs">💼 Профессии</option>
        <option value="workplaces">🏭 Места работы</option>
        <option value="dailyRoutines">🌅 Распорядок дня</option>
        <option value="activities">📅 Занятия и дни недели</option>
        <option value="aroundTown">🏙️ По городу</option>
        <option value="directions">🗺️ Ориентирование</option>
        <option value="adjectives">🎨 Прилагательные</option>
        <option value="scenery">🌄 Природа и пейзажи</option>
        <option value="aroundHouse">🏠 Дом и квартира</option>
        <option value="myWords">⭐ Мои слова</option>
      </select>
    </div>

    <!-- Данные -->
    <div class="settings-section">
      <div class="settings-section-title">📁 Данные</div>
      <div class="settings-row-item" style="margin-bottom:10px;">
        <div class="settings-row-info">
          <div class="settings-row-label">Мои слова</div>
          <div class="settings-row-sub">Добавляй свои пары для тренировки</div>
        </div>
        <button class="settings-mini-btn" onclick="showScreen('myWordsScreen')">Добавить</button>
      </div>
      <div class="settings-row-item">
        <div class="settings-row-info">
          <div class="settings-row-label">Сертификат прогресса</div>
          <div class="settings-row-sub">Скачать HTML-файл со своей статистикой</div>
        </div>
        <button class="settings-mini-btn" onclick="exportProgress()">🎓 Сертификат</button>
      </div>
    </div>

    <!-- Опасная зона -->
    <div class="settings-section" style="border-color:rgba(255,79,109,.2);">
      <div class="settings-section-title" style="color:var(--err);">⚠️ Опасная зона</div>
      <div class="settings-row-item">
        <div class="settings-row-info">
          <div class="settings-row-label">Сбросить прогресс</div>
          <div class="settings-row-sub">Удалит весь XP, достижения и ошибки</div>
        </div>
        <button class="settings-mini-btn danger-btn" onclick="confirmReset()">Сброс</button>
      </div>
    </div>

    <div style="text-align:center;font-size:12px;color:var(--text2);margin-top:8px;opacity:.5;">ETU by BV · v1.0</div>
  </div>

</div><!-- /container -->

<!-- ═══ ПОИСК ПО СЛОВАРЮ ═══ -->
<div class="overlay" id="dictOverlay" onclick="if(event.target===this)closeDictSearch()">
  <div class="pause-card" style="max-height:80vh;overflow-y:auto;">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;">
      <div class="pause-title" style="margin-bottom:0;">🔍 Поиск по словарю</div>
      <button onclick="closeDictSearch()" style="background:none;border:none;font-size:22px;color:var(--text2);cursor:pointer;padding:4px;">✕</button>
    </div>
    <div class="dict-search-wrap">
      <input class="dict-search-input" id="dictSearchInput" type="text" placeholder="Введи слово на рус. или англ." oninput="dictSearch(this.value)" autocomplete="off" spellcheck="false">
      <button class="dict-search-clear" onclick="clearDictSearch()">✕</button>
    </div>
    <div id="dictResults" class="dict-results">
      <div class="dict-no-result">Начни вводить слово для поиска</div>
    </div>
    <div style="font-size:11px;color:var(--text2);text-align:center;margin-top:6px;">Найдено слов: <span id="dictCount">—</span></div>
  </div>
</div>

<!-- ═══ ОБУЧАЮЩИЙ ТЬЮТОРИАЛ ═══ -->
<div class="tutorial-overlay" id="tutorialOverlay" style="display:none;">
  <div class="tutorial-card">
    <div id="tutStep1">
      <div class="tutorial-step">Шаг 1 из 3</div>
      <span class="tutorial-emoji">📖</span>
      <div class="tutorial-title" id="tutGreetTitle">Тебе покажут слово</div>
      <div class="tutorial-sub">На экране появится слово на русском. Твоя задача — написать его перевод на английский.</div>
      <div class="tutorial-demo">
        <div class="demo-word">кошка</div>
        <div class="demo-arrow">↓</div>
        <div class="demo-answer">cat</div>
      </div>
      <div class="tutorial-dots">
        <div class="tutorial-dot active"></div>
        <div class="tutorial-dot"></div>
        <div class="tutorial-dot"></div>
      </div>
      <button class="btn-primary" style="width:100%;" onclick="tutNext(2)">Понятно →</button>
    </div>
    <div id="tutStep2" style="display:none;">
      <div class="tutorial-step">Шаг 2 из 3</div>
      <span class="tutorial-emoji">⏱️</span>
      <div class="tutorial-title">Отвечай быстрее</div>
      <div class="tutorial-sub">На каждый вопрос есть 15 секунд. Чем быстрее и точнее отвечаешь — тем больше серия и XP!</div>
      <div class="tutorial-demo" style="text-align:left;font-size:13px;line-height:1.8;">
        ✅ Правильно → <strong style="color:var(--ok)">+10 XP</strong><br>
        🔥 Серия × 5 → <strong style="color:var(--warn)">Бонус +10 XP</strong><br>
        ⚡ Серия × 10 → <strong style="color:var(--warn)">×2 XP на 30 сек!</strong>
      </div>
      <div class="tutorial-dots">
        <div class="tutorial-dot"></div>
        <div class="tutorial-dot active"></div>
        <div class="tutorial-dot"></div>
      </div>
      <button class="btn-primary" style="width:100%;" onclick="tutNext(3)">Дальше →</button>
    </div>
    <div id="tutStep3" style="display:none;">
      <div class="tutorial-step">Шаг 3 из 3</div>
      <span class="tutorial-emoji">🎯</span>
      <div class="tutorial-title">Режимы игры</div>
      <div class="tutorial-sub">Можно печатать ответ или выбрать из вариантов. Есть флэшкарты и анаграммы — пробуй всё!</div>
      <div class="tutorial-demo" style="text-align:left;font-size:13px;line-height:1.9;">
        ⌨️ <strong>Ввод</strong> — напечатай перевод<br>
        🔘 <strong>Тест</strong> — выбери из 4 вариантов<br>
        🃏 <strong>Флэшкарты</strong> — листай и запоминай<br>
        🧩 <strong>Анаграмма</strong> — собери слово из букв
      </div>
      <div class="tutorial-dots">
        <div class="tutorial-dot"></div>
        <div class="tutorial-dot"></div>
        <div class="tutorial-dot active"></div>
      </div>
      <button class="btn-primary" style="width:100%;" onclick="tutFinish()">🚀 Начать тренировку!</button>
    </div>
  </div>
</div>

<!-- ═══ ОНБОРДИНГ ═══ -->
<div class="onboard-overlay" id="onboardOverlay" style="display:none;">
  <div class="onboard-card">
    <!-- Шаг 1 -->
    <div id="onboardStep1">
      <span class="onboard-emoji">🚀</span>
      <div class="onboard-title">English Trainer</div>
      <div class="onboard-sub">Учи английские слова каждый день — быстро, весело и с геймификацией!</div>
      <div class="onboard-avatar-row" id="onboardAvatarRow"></div>
      <input class="onboard-name-input" id="onboardName" placeholder="Как тебя зовут?" maxlength="20">
      <div class="onboard-steps">
        <div class="onboard-dot active"></div>
        <div class="onboard-dot"></div>
      </div>
      <button class="btn-primary" style="width:100%;" onclick="onboardNext()">Далее →</button>
    </div>
    <!-- Шаг 2 -->
    <div id="onboardStep2" style="display:none;">
      <span class="onboard-emoji">🎯</span>
      <div class="onboard-title">Выбери цель</div>
      <div class="onboard-sub">Сколько XP ты готов зарабатывать каждый день?</div>
      <div style="display:flex;flex-direction:column;gap:10px;margin-bottom:20px;" id="onboardDiffList"></div>
      <div class="onboard-steps">
        <div class="onboard-dot"></div>
        <div class="onboard-dot active"></div>
      </div>
      <button class="btn-primary" style="width:100%;" onclick="onboardFinish()">🚀 Начать!</button>
    </div>
  </div>
</div>

<!-- ═══ ЭКРАН ПОВТОРЕНИЯ ОШИБОК ═══ -->
<div id="mistakeRetryScreen" class="card screen" style="display:none;"></div>

<!-- ═══ РЕЖИМ «ПРЕДЛОЖЕНИЕ» ═══ -->
<div id="sentenceScreen" class="card screen">
  <div class="game-header">
    <button class="pause-btn" onclick="exitSentence()">← Выйти</button>
    <div class="stats-bar">
      <div class="mini-stat">⭐ <span id="senXp">0</span></div>
      <div class="mini-stat">🔥 <span id="senStreak">0</span></div>
    </div>
  </div>
  <div class="prog-wrap"><div class="prog-bar sprint" id="senBar"></div></div>
  <div class="sprint-counter" id="senCounter" style="text-align:right;margin-bottom:8px;">Предложение 1 из 8</div>

  <div style="font-size:11px;font-weight:800;color:var(--text2);text-transform:uppercase;letter-spacing:.6px;margin-bottom:6px;">Переведи на английский:</div>
  <div class="sentence-ru" id="senRu">—</div>
  <div class="sentence-hint-line" id="senHintLine"></div>

  <div style="font-size:11px;font-weight:800;color:var(--text2);text-transform:uppercase;letter-spacing:.6px;margin-bottom:6px;">Твой ответ:</div>
  <div class="sentence-answer-row" id="senAnswer" onclick="senAnswerClick(event)"></div>

  <div style="font-size:11px;font-weight:800;color:var(--text2);text-transform:uppercase;letter-spacing:.6px;margin-bottom:8px;">Слова:</div>
  <div class="sentence-word-row" id="senWords"></div>

  <div class="result" id="senResult"></div>
  <div class="actions" style="margin-top:10px;">
    <button class="btn-secondary btn-hint" onclick="senHint()">💡 Подсказка</button>
    <button class="btn-secondary" onclick="senSkip()">⏭ Пропуск</button>
    <button class="btn-primary" onclick="senCheck()">✅ Проверить</button>
  </div>
</div>

<!-- ═══ РЕЗУЛЬТАТЫ «ПРЕДЛОЖЕНИЕ» ═══ -->
<div id="sentenceResultScreen" class="card screen sentence-result-screen">
  <div class="result-hero" id="senResEmoji">🗣️</div>
  <div class="result-title" id="senResTitle">Отличная работа!</div>
  <div class="result-pct" id="senResPct">0%</div>
  <div class="result-stats">
    <div class="result-stat">✅ Верно<strong id="senResCorrect">0</strong></div>
    <div class="result-stat">❌ Ошибки<strong id="senResErrors">0</strong></div>
    <div class="result-stat">⭐ XP<strong id="senResXp">0</strong></div>
    <div class="result-stat">🔥 Серия<strong id="senResStreak">0</strong></div>
  </div>
  <div class="result-verdict" id="senResVerdict"></div>
  <div style="display:flex;flex-direction:column;gap:10px;margin-top:16px;">
    <button class="btn-primary" onclick="startSentence()">🔄 Ещё раз</button>
    <button class="btn-secondary" onclick="showScreen('menuScreen');updateMenu();">🏠 В меню</button>
  </div>
</div>

<!-- ═══ МАГАЗИН ═══ -->
<div id="shopScreen" class="card screen">
  <div style="font-size:20px;font-weight:900;margin-bottom:4px;">🛍️ Магазин</div>
  <div style="color:var(--text2);font-size:13px;margin-bottom:14px;">Трать XP на рамки, темы и звуки</div>

  <div class="shop-balance">
    <div>
      <div class="shop-balance-label">Твой баланс</div>
      <div class="shop-balance-val" id="shopXpBalance">0 XP</div>
    </div>
    <div style="font-size:36px;">💰</div>
  </div>

  <div class="shop-section-title">🖼️ Рамки аватара</div>
  <div class="shop-grid" id="shopFrames" style="grid-template-columns:repeat(4,1fr);"></div>

  <div class="shop-section-title">🎨 Цветовые темы</div>
  <div class="shop-grid themes" id="shopThemes"></div>

  <div class="shop-section-title">🔊 Звуки правильного ответа</div>
  <div class="shop-grid" id="shopSounds"></div>

  <button class="btn-secondary" style="width:100%;margin-top:16px;" onclick="goToMainMenu()">← Назад</button>
</div>

<!-- Нижняя навигация — всегда видима -->
<nav class="bottom-nav" id="bottomNav">
  <button class="nav-item active" id="navHome" onclick="navTo('home')">
    <span class="nav-item-icon">🏠</span>
    Главная
  </button>
  <button class="nav-item" id="navStats" onclick="navTo('stats')">
    <span class="nav-item-icon">📊</span>
    Статистика
  </button>
  <button class="nav-item" id="navAch" onclick="navTo('ach')">
    <span class="nav-item-icon">🏆</span>
    Достижения
  </button>
  <button class="nav-item" id="navSettings" onclick="navTo('settings')">
    <span class="nav-item-icon">⚙️</span>
    Настройки
  </button>
  <button class="nav-item" id="navProfile" onclick="navTo('profile')">
    <span class="nav-item-icon">👤</span>
    Профиль
  </button>
</nav>

<!-- ═══ МОДАЛЬНОЕ ОКНО СЛОЖНОСТИ ═══ -->
<div class="diff-modal-overlay" id="diffModalOverlay" onclick="if(event.target===this)closeDiffModal()">
  <div class="diff-modal">
    <div class="diff-modal-title">🎯 Дневная цель</div>
    <div class="diff-modal-sub">Выбери нагрузку — XP сбрасываются каждый день</div>
    <div class="diff-options">
      <div class="diff-option diff-easy" id="diff-easy" onclick="selectDifficulty('easy')">
        <span class="diff-option-icon">🌱</span>
        <div class="diff-option-info">
          <div class="diff-option-name">Лёгкая</div>
          <div class="diff-option-desc">Для начинающих и занятых людей</div>
        </div>
        <span class="diff-option-xp">30 XP</span>
      </div>
      <div class="diff-option diff-medium" id="diff-medium" onclick="selectDifficulty('medium')">
        <span class="diff-option-icon">⚡</span>
        <div class="diff-option-info">
          <div class="diff-option-name">Обычная</div>
          <div class="diff-option-desc">Оптимальный ежедневный прогресс</div>
        </div>
        <span class="diff-option-xp">75 XP</span>
      </div>
      <div class="diff-option diff-hard" id="diff-hard" onclick="selectDifficulty('hard')">
        <span class="diff-option-icon">🔥</span>
        <div class="diff-option-info">
          <div class="diff-option-name">Усиленная</div>
          <div class="diff-option-desc">Серьёзный подход к изучению</div>
        </div>
        <span class="diff-option-xp">150 XP</span>
      </div>
      <div class="diff-option diff-ultra" id="diff-ultra" onclick="selectDifficulty('ultra')">
        <span class="diff-option-icon">💀</span>
        <div class="diff-option-info">
          <div class="diff-option-name">Ультра</div>
          <div class="diff-option-desc">Только для настоящих чемпионов</div>
        </div>
        <span class="diff-option-xp">300 XP</span>
      </div>
    </div>
    <button class="diff-modal-close" onclick="closeDiffModal()">Закрыть</button>
  </div>
</div>

<!-- ═══ ПАУЗА ═══ -->
<div class="overlay" id="pauseOverlay">
  <div class="pause-card">
    <div class="pause-title">⏸ Пауза</div>
    <div class="settings-row">
      <label>Направление перевода</label>
      <select id="mode" onchange="savePrefs();">
        <option value="ru-en">🇷🇺 → 🇬🇧 Русский → Английский</option>
        <option value="en-ru">🇬🇧 → 🇷🇺 Английский → Русский</option>
        <option value="mixed">🔀 Смешанный</option>
      </select>
    </div>
    <div class="settings-row">
      <label>Категория</label>
      <select id="category" onchange="savePrefs();">
        <option value="all">🌍 Все категории</option>
        <option value="jobs">💼 Профессии (Jobs)</option>
        <option value="workplaces">🏭 Места работы</option>
        <option value="dailyRoutines">🌅 Распорядок дня</option>
        <option value="activities">📅 Занятия и дни недели</option>
        <option value="aroundTown">🏙️ По городу</option>
        <option value="directions">🗺️ Ориентирование</option>
        <option value="adjectives">🎨 Прилагательные</option>
        <option value="scenery">🌄 Природа и пейзажи</option>
        <option value="aroundHouse">🏠 Дом и квартира</option>
        <option value="myWords">⭐ Мои слова</option>
      </select>
    </div>
    <div class="settings-row">
      <label>Звук</label>
      <button id="pauseSoundToggle" class="pause-sound-toggle" onclick="toggleSoundFromPause()">🔊 Звук включён</button>
    </div>
    <div class="pause-menu-actions">
      <button class="btn-primary" onclick="resumeGame()">▶️ Продолжить</button>
      <button class="btn-secondary" onclick="goToMainMenu()">🏠 Главное меню</button>
    </div>
  </div>
</div>

<script>
console.log("ETU v3 - profile build OK");
// ════════════════════════════════════
//  ДАННЫЕ — СЛОВАРИ
// ════════════════════════════════════
const categories = {
  // Theme 9 — Jobs
  jobs:[
    {ru:"уборщик / уборщица",en:"cleaner"},{ru:"водитель",en:"driver"},
    {ru:"продавец-консультант",en:"sales assistant"},{ru:"парикмахер",en:"hairdresser"},
    {ru:"шеф-повар",en:"chef"},{ru:"садовник",en:"gardener"},
    {ru:"ветеринар",en:"vet"},{ru:"актёр",en:"actor"},
    {ru:"врач",en:"doctor"},{ru:"медсестра / медбрат",en:"nurse"},
    {ru:"стоматолог",en:"dentist"},{ru:"полицейский",en:"police officer"},
    {ru:"пожарный",en:"firefighter"},{ru:"фермер",en:"farmer"},
    {ru:"строитель",en:"construction worker / builder"},{ru:"художник",en:"artist"},
    {ru:"администратор / секретарь",en:"receptionist"},{ru:"механик",en:"mechanic"},
    {ru:"инженер",en:"engineer"},{ru:"учёный",en:"scientist"},
    {ru:"учитель",en:"teacher"},{ru:"бизнесвумен",en:"businesswoman"},
    {ru:"бизнесмен",en:"businessman"},{ru:"официант",en:"waiter"},
    {ru:"официантка",en:"waitress"},{ru:"электрик",en:"electrician"},
    {ru:"пилот",en:"pilot"},{ru:"судья",en:"judge"}
  ],
  // Theme 10 — Workplaces / Work with
  workplaces:[
    {ru:"ферма",en:"farm"},{ru:"офис",en:"office"},
    {ru:"театр",en:"theater / theatre"},{ru:"школа",en:"school"},
    {ru:"лаборатория",en:"laboratory"},{ru:"ресторан",en:"restaurant"},
    {ru:"строительная площадка",en:"construction site"},{ru:"больница",en:"hospital"},
    {ru:"животные",en:"animals"},{ru:"дети",en:"children"},
    {ru:"пациенты",en:"patients"},{ru:"растения",en:"plants"},
    {ru:"еда",en:"food"},{ru:"люди",en:"people"}
  ],
  // Theme 11 — Daily Routines / Times of Day
  dailyRoutines:[
    {ru:"просыпаться",en:"wake up"},{ru:"вставать",en:"get up"},
    {ru:"принимать душ",en:"take a shower / have a shower"},
    {ru:"принимать ванну",en:"take a bath / have a bath"},
    {ru:"расчесывать волосы",en:"brush your hair"},
    {ru:"завтракать",en:"have breakfast / eat breakfast"},
    {ru:"идти на работу",en:"go to work"},{ru:"идти в школу",en:"go to school"},
    {ru:"покупать продукты",en:"buy groceries"},{ru:"идти домой",en:"go home"},
    {ru:"готовить ужин",en:"cook dinner"},{ru:"ужинать",en:"have dinner / eat dinner"},
    {ru:"гладить рубашку",en:"iron a shirt"},{ru:"одеваться",en:"get dressed"},
    {ru:"чистить зубы",en:"brush your teeth"},{ru:"умываться",en:"wash your face"},
    {ru:"начинать работу",en:"start work"},{ru:"обедать",en:"have lunch / eat lunch"},
    {ru:"заканчивать работу",en:"finish work"},{ru:"уходить с работы",en:"leave work"},
    {ru:"убирать со стола",en:"clear the table"},
    {ru:"мыть посуду",en:"do the dishes / wash the dishes"},
    {ru:"выгуливать собаку",en:"walk the dog"},{ru:"ложиться спать",en:"go to bed"},
    {ru:"день",en:"day"},{ru:"ночь",en:"night"},
    {ru:"рассвет",en:"dawn"},{ru:"утро",en:"morning"},
    {ru:"после обеда",en:"afternoon"},{ru:"сумерки",en:"dusk"},
    {ru:"вечер",en:"evening"},{ru:"поздний вечер",en:"late evening"}
  ],
  // Theme 14 — Activities / Days of the Week
  activities:[
    {ru:"понедельник",en:"Monday"},{ru:"вторник",en:"Tuesday"},
    {ru:"среда",en:"Wednesday"},{ru:"четверг",en:"Thursday"},
    {ru:"пятница",en:"Friday"},{ru:"суббота",en:"Saturday"},
    {ru:"воскресенье",en:"Sunday"},{ru:"выходные",en:"weekend"},
    {ru:"ходить в спортзал",en:"go to the gym"},
    {ru:"ходить плавать",en:"go swimming"},
    {ru:"играть в теннис",en:"play tennis"},
    {ru:"играть в футбол",en:"play soccer"},
    {ru:"читать газету",en:"read the newspaper"},
    {ru:"принимать ванну",en:"take a bath"}
  ],
  // Theme 20 — Around Town
  aroundTown:[
    {ru:"деревня",en:"village"},{ru:"небольшой город",en:"town"},
    {ru:"крупный город",en:"city"},{ru:"больница",en:"hospital"},
    {ru:"полицейский участок",en:"police station"},{ru:"автовокзал",en:"bus station"},
    {ru:"автобусная остановка",en:"bus stop"},{ru:"железнодорожный вокзал",en:"train station"},
    {ru:"аэропорт",en:"airport"},{ru:"школа",en:"school"},
    {ru:"фабрика / завод",en:"factory"},{ru:"супермаркет",en:"supermarket"},
    {ru:"магазин",en:"store / shop"},{ru:"аптека",en:"pharmacy"},
    {ru:"банк",en:"bank"},{ru:"почтовое отделение",en:"post office"},
    {ru:"библиотека",en:"library"},{ru:"музей",en:"museum"},
    {ru:"мэрия / городская администрация",en:"town hall"},{ru:"замок",en:"castle"},
    {ru:"офисное здание",en:"office building"},{ru:"парк",en:"park"},
    {ru:"здесь",en:"here"},{ru:"мост",en:"bridge"},
    {ru:"плавательный бассейн",en:"swimming pool"},{ru:"ресторан",en:"restaurant"},
    {ru:"кафе",en:"cafe"},{ru:"там",en:"there"},
    {ru:"бар",en:"bar"},{ru:"кинотеатр",en:"movie theater / cinema"},
    {ru:"театр",en:"theater / theatre"},{ru:"отель",en:"hotel"},
    {ru:"рядом",en:"near"},{ru:"церковь",en:"church"},
    {ru:"мечеть",en:"mosque"},{ru:"синагога",en:"synagogue"},
    {ru:"храм",en:"temple"},{ru:"далеко",en:"far"}
  ],
  // Theme 21 — Directions
  directions:[
    {ru:"идти прямо",en:"go straight ahead"},
    {ru:"повернуть налево",en:"turn left"},
    {ru:"повернуть направо",en:"turn right"},
    {ru:"пройти мимо",en:"go past"},
    {ru:"повернуть на первом повороте направо",en:"take the first right"},
    {ru:"повернуть на втором повороте направо",en:"take the second right"},
    {ru:"рядом с",en:"next to"},
    {ru:"напротив",en:"opposite"},
    {ru:"между",en:"between"},
    {ru:"на углу",en:"on the corner"},
    {ru:"позади",en:"behind"},
    {ru:"перед",en:"in front of"},
    {ru:"справа",en:"on the right"},
    {ru:"слева",en:"on the left"},
    {ru:"перекрёсток",en:"intersection / crossroads"},
    {ru:"квартал",en:"block"}
  ],
  // Theme 22 — Adjectives
  adjectives:[
    {ru:"старый",en:"old"},
    {ru:"новый",en:"new"},
    {ru:"красивый",en:"beautiful"},
    {ru:"ужасный",en:"horrible"},
    {ru:"оживлённый",en:"busy"},
    {ru:"тихий",en:"quiet"},
    {ru:"маленький",en:"small"},
    {ru:"большой",en:"big"},
    {ru:"старый дом",en:"old house"},
    {ru:"новый дом",en:"new house"},
    {ru:"красивый дом",en:"beautiful house"},
    {ru:"ужасный дом",en:"horrible house"},
    {ru:"оживлённая улица",en:"busy street"},
    {ru:"тихая улица",en:"quiet street"},
    {ru:"маленький дом",en:"small house"},
    {ru:"большой дом",en:"big house"}
  ],
  // Theme 23 — Places and Scenery
  scenery:[
    {ru:"пляж",en:"beach"},
    {ru:"море",en:"sea"},
    {ru:"песок",en:"sand"},
    {ru:"сельская местность, деревня",en:"countryside"},
    {ru:"дерево",en:"tree"},
    {ru:"холм",en:"hill"},
    {ru:"гора",en:"mountain"},
    {ru:"озеро",en:"lake"},
    {ru:"небо",en:"sky"},
    {ru:"трава",en:"grass"},
    {ru:"река",en:"river"},
    {ru:"облако",en:"cloud"}
  ],
  // Theme 24 — Around the House
  aroundHouse:[
    {ru:"многоквартирный дом",en:"block of flats"},
    {ru:"дом",en:"house"},
    {ru:"гараж",en:"garage"},
    {ru:"ванная комната",en:"bathroom"},
    {ru:"гостиная",en:"living room"},
    {ru:"кабинет",en:"study"},
    {ru:"спальня",en:"bedroom"},
    {ru:"кухня",en:"kitchen"},
    {ru:"столовая",en:"dining room"},
    {ru:"книжный шкаф",en:"bookcase"},
    {ru:"письменный стол",en:"desk"},
    {ru:"кресло",en:"armchair"},
    {ru:"диван",en:"sofa"},
    {ru:"телевизор",en:"television"},
    {ru:"шкаф для одежды",en:"wardrobe"},
    {ru:"лампа",en:"lamp"},
    {ru:"кровать",en:"bed"},
    {ru:"холодильник",en:"fridge"},
    {ru:"плита",en:"cooker"},
    {ru:"раковина",en:"sink"},
    {ru:"стол",en:"table"},
    {ru:"стул",en:"chair"},
    {ru:"душ",en:"shower"},
    {ru:"туалет",en:"toilet"},
    {ru:"ванна",en:"bathtub"},
    {ru:"дверь",en:"door"},
    {ru:"крыша",en:"roof"},
    {ru:"лестница",en:"stairs"},
    {ru:"чердак",en:"attic"},
    {ru:"верхний этаж",en:"upstairs"},
    {ru:"нижний этаж",en:"downstairs"},
    {ru:"подвал",en:"basement"},
    {ru:"сад, двор",en:"garden"},
    {ru:"окно",en:"window"}
  ],
  myWords:[]
};

// ════════════════════════════════════
//  РЕЖИМ "ТОЛЬКО ОШИБКИ" (persistent)
// ════════════════════════════════════
function updateMistakesModeCard() {
  const badge = document.getElementById('mistakesModeCount');
  const card = document.getElementById('mistakesModeCard');
  if (!badge || !card) return;
  const count = mistakes.length;
  if (count > 0) {
    badge.textContent = count;
    badge.style.display = 'inline-block';
    card.style.opacity = '1';
    card.style.pointerEvents = 'auto';
  } else {
    badge.style.display = 'none';
    card.style.opacity = '0.45';
    card.style.pointerEvents = 'none';
  }
}

function startPersistentMistakeMode() {
  if (!mistakes.length) { alertPop('Нет сохранённых ошибок! Сначала потренируйся 💪'); return; }
  startMistakeRetry();
}


// ════════════════════════════════════
//  ЦИТАТЫ
// ════════════════════════════════════
const quotes = [
  { text:"The limits of my language are the limits of my world.", author:"Ludwig Wittgenstein" },
  { text:"One language sets you in a corridor for life. Two languages open every door.", author:"Frank Smith" },
  { text:"To have another language is to possess a second soul.", author:"Charlemagne" },
  { text:"Language is the road map of a culture.", author:"Rita Mae Brown" },
  { text:"The more languages you know, the more you are human.", author:"Tomáš Masaryk" },
  { text:"Learning a language is the discovery of a new world.", author:"Unknown" },
  { text:"Every word learned is a step forward.", author:"Unknown" },
  { text:"Practice makes perfect — especially with language.", author:"Unknown" },
  { text:"A different language is a different vision of life.", author:"Federico Fellini" },
  { text:"You live a new life for every new language you speak.", author:"Czech Proverb" },
  { text:"The brain is like a muscle. When it is in use we feel very good.", author:"Carl Sagan" },
  { text:"Consistency is more important than perfection.", author:"Unknown" },
  { text:"Every expert was once a beginner.", author:"Helen Hayes" },
  { text:"Small daily improvements over time lead to stunning results.", author:"Robin Sharma" },
  { text:"It does not matter how slowly you go as long as you do not stop.", author:"Confucius" },
];

// ════════════════════════════════════
//  ПРОГРЕСС
// ════════════════════════════════════
let xp        = parseInt(localStorage.getItem("xp"))        || 0;
let correct   = parseInt(localStorage.getItem("correct"))   || 0;
let total     = parseInt(localStorage.getItem("total"))     || 0;
let maxStreak = parseInt(localStorage.getItem("maxStreak")) || 0;
let streak    = parseInt(localStorage.getItem("streak"))    || 0;
let soundEnabled = localStorage.getItem("soundEnabled") !== "false";

const TODAY = new Date().toDateString();
const savedDay = localStorage.getItem("etu_day");
let dailyXp = savedDay === TODAY ? (parseInt(localStorage.getItem("etu_dailyXp"))||0) : 0;
if (savedDay !== TODAY) {
  localStorage.setItem("etu_day", TODAY);
  localStorage.setItem("etu_dailyXp", 0);
}

try { categories.myWords = JSON.parse(localStorage.getItem("myWords")) || []; } catch(e){}

// ════════════════════════════════════
//  СИСТЕМА СЛОЖНОСТИ ДНЕВНОЙ ЦЕЛИ
// ════════════════════════════════════
const DAILY_GOALS = {
  easy:   { xp: 30,  name: "Лёгкая",   icon: "🌱", color: "var(--ok)",   bg: "rgba(34,216,143,.15)" },
  medium: { xp: 75,  name: "Обычная",  icon: "⚡",  color: "var(--accent)",bg: "rgba(79,142,255,.15)" },
  hard:   { xp: 150, name: "Усиленная",icon: "🔥",  color: "var(--warn)",  bg: "rgba(255,194,71,.15)" },
  ultra:  { xp: 300, name: "Ультра",   icon: "💀",  color: "var(--err)",   bg: "rgba(255,79,109,.15)" },
};
let dailyDifficulty = localStorage.getItem("etu_difficulty") || "easy";

function openDiffModal() {
  playClick();
  document.getElementById("diffModalOverlay").classList.add("open");
  // Подсветить текущий
  ['easy','medium','hard','ultra'].forEach(d => {
    document.getElementById("diff-"+d).classList.toggle("selected", d === dailyDifficulty);
  });
}
function closeDiffModal() {
  document.getElementById("diffModalOverlay").classList.remove("open");
}
function selectDifficulty(d) {
  dailyDifficulty = d;
  localStorage.setItem("etu_difficulty", d);
  ['easy','medium','hard','ultra'].forEach(k => {
    document.getElementById("diff-"+k).classList.toggle("selected", k === d);
  });
  playCorrect();
  updateMenu();
  setTimeout(closeDiffModal, 350);
  alertPop(`Цель: ${DAILY_GOALS[d].icon} ${DAILY_GOALS[d].name} — ${DAILY_GOALS[d].xp} XP`);
  // Sync settings screen subtitle
  const diffSub = document.getElementById('settingsDiffSub');
  if (diffSub) diffSub.textContent = `${DAILY_GOALS[d].icon} ${DAILY_GOALS[d].name} — ${DAILY_GOALS[d].xp} XP/день`;
}

let earnedAchievements = [];
try { earnedAchievements = JSON.parse(localStorage.getItem("achievements")) || []; } catch(e){}
let mistakes = [];
try { mistakes = JSON.parse(localStorage.getItem("mistakes")) || []; } catch(e){}
let wordWeights = {};
try { wordWeights = JSON.parse(localStorage.getItem("wordWeights")) || {}; } catch(e){}

// ════════════════════════════════════
//  СОСТОЯНИЕ
// ════════════════════════════════════
let timerVal = 15, interval;
let currentWordObj, correctAnswerString, currentLanguage = 'ru';
let gameMode = localStorage.getItem("gameMode") || "write";
let isPaused = false, isAnswering = false, isGameActive = false;
let isSprint = false, sprintLen = 10, sprintIdx = 0;
let sprintCorrect = 0, sprintErrors = 0, sprintXpEarned = 0, sprintMaxStreak = 0;

// Флэшкарты
let fcPool = [], fcIdx = 0, fcKnown = 0, fcUnknown = 0, fcUnknownCards = [];

// Анаграмма
let agPool = [], agIdx = 0, agWord = "", agTarget = "", agInterval;
let agTimer = 20, agCorrect = 0, agErrors = 0, agXpEarned = 0, agStreak = 0;
const AG_TOTAL = 10;

// ════════════════════════════════════
//  НОВЫЕ ФИЧИ v5
// ════════════════════════════════════

// Жизни (Hearts)
const MAX_LIVES = 3;
let lives = MAX_LIVES;
let livesMode = false; // включены только в спринте

// Бонус-раунд (×2 XP на 30 сек после 10 подряд)
let bonusRoundActive = false;
let bonusRoundTimer = 0;
let bonusInterval = null;
const BONUS_TRIGGER = 10; // серия для активации
const BONUS_DURATION = 30; // секунд

// Рекорд дня
let dailyRecord = parseInt(localStorage.getItem("etu_dailyRecord_" + new Date().toDateString())) || 0;

// Повторение ошибок
let mistakeRetryPool = [];
let mistakeRetryIdx = 0;
let isMistakeRetry = false;

// Статистика по словам (сколько раз ошибался)
let wordErrorCount = {};
try { wordErrorCount = JSON.parse(localStorage.getItem("etu_wordErrors")) || {}; } catch(e){}

// Онбординг
let onboardSelectedAvatar = "🦁";
let onboardSelectedDiff = "medium";

// ════════════════════════════════════
//  ДОСТИЖЕНИЯ
// ════════════════════════════════════
const achievementList = {
  "ach_10":      "🎯 Знаток: 10 правильных ответов",
  "ach_50":      "🏆 Магистр: 50 правильных ответов",
  "ach_100":     "👑 Лингвист: 100 правильных ответов",
  "ach_250":     "🧠 Эрудит: 250 правильных ответов",
  "ach_str10":   "🔥 Серия 10",
  "ach_str25":   "⚡ Неудержимый: серия 25",
  "ach_str50":   "🌪️ Ураган: серия 50",
  "ach_lvl5":    "⭐ Уровень 5",
  "ach_lvl10":   "💎 Уровень 10",
  "ach_sprint":  "🏁 Первый спринт завершён",
  "ach_perfect": "✨ Идеальный спринт без ошибок",
  "ach_myword":  "📝 Добавил своё слово",
  "ach_flash":   "🃏 Первый раунд флэшкарт",
  "ach_anagram": "🧩 Первая анаграмма",
  "ach_phrases": "💬 Знаток фраз",
};

// ════════════════════════════════════
//  АУДИО
// ════════════════════════════════════
let audioCtx = null;
function unlockAudio() {
  if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  if (audioCtx.state === 'suspended') audioCtx.resume();
}
function beep(freq, type, dur, vol=0.07) {
  if (!soundEnabled || !audioCtx) return;
  try {
    const o = audioCtx.createOscillator(), g = audioCtx.createGain();
    o.connect(g); g.connect(audioCtx.destination);
    o.frequency.value = freq; o.type = type;
    g.gain.setValueAtTime(vol, audioCtx.currentTime);
    g.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + dur);
    o.start(); o.stop(audioCtx.currentTime + dur);
  } catch(e){}
}
function _pack(){ return localStorage.getItem('etu_sound_pack') || 'default'; }
function playCorrect(){
  const pack = _pack();
  if (pack === 'bells')  { beep(880,'sine',.12); setTimeout(()=>beep(1108,'sine',.14),60); setTimeout(()=>beep(1318,'sine',.25),130); return; }
  if (pack === 'arcade') { beep(660,'square',.06,.05); setTimeout(()=>beep(880,'square',.06,.05),50); setTimeout(()=>beep(1100,'square',.12,.06),100); return; }
  if (pack === 'chime')  { beep(784,'triangle',.18,.08); setTimeout(()=>beep(1047,'triangle',.22,.07),90); setTimeout(()=>beep(1568,'triangle',.3,.05),180); return; }
  beep(523,'sine',.1); setTimeout(()=>beep(659,'sine',.12),70); setTimeout(()=>beep(784,'sine',.2),140);
}
function playWrong(){
  const pack = _pack();
  if (pack === 'arcade') { beep(220,'square',.1,.08); setTimeout(()=>beep(180,'square',.15,.1),100); return; }
  if (pack === 'chime')  { beep(350,'triangle',.12,.08); setTimeout(()=>beep(280,'triangle',.2,.1),120); return; }
  beep(300,'sawtooth',.15,.1); setTimeout(()=>beep(200,'sawtooth',.25,.12),150);
}
function playClick(){
  const pack = _pack();
  if (pack === 'bells')  { beep(1200,'sine',.04,.04); return; }
  if (pack === 'arcade') { beep(700,'square',.03,.04); return; }
  if (pack === 'chime')  { beep(900,'triangle',.05,.04); return; }
  beep(600,'square',.05,.04);
}
function playStreak(){
  const pack = _pack();
  if (pack === 'bells')  { [880,1108,1318,1568].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.12,.1),i*80)); return; }
  if (pack === 'arcade') { [660,770,880,1100].forEach((f,i)=>setTimeout(()=>beep(f,'square',.08,.06),i*70)); return; }
  if (pack === 'chime')  { [784,987,1175,1568].forEach((f,i)=>setTimeout(()=>beep(f,'triangle',.15,.08),i*85)); return; }
  beep(659,'sine',.08); setTimeout(()=>beep(784,'sine',.08),60); setTimeout(()=>beep(987,'sine',.08),120); setTimeout(()=>beep(1047,'sine',.2),180);
}
function playAchievement(){
  const pack = _pack();
  if (pack === 'bells')  { [880,1108,1318,1568].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.3,.12),i*90)); return; }
  if (pack === 'arcade') { [523,659,784,1047,1318].forEach((f,i)=>setTimeout(()=>beep(f,'square',.25,.1),i*75)); return; }
  if (pack === 'chime')  { [784,1047,1318,1568].forEach((f,i)=>setTimeout(()=>beep(f,'triangle',.28,.1),i*85)); return; }
  [523,659,784,1047].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.25,.12),i*80));
}
function playTick(){
  const pack = _pack();
  if (pack === 'bells')  { beep(1400,'sine',.03,.03); return; }
  if (pack === 'arcade') { beep(1000,'square',.03,.03); return; }
  if (pack === 'chime')  { beep(1200,'triangle',.04,.03); return; }
  beep(880,'square',.05,.04);
}
function playHint(){
  const pack = _pack();
  if (pack === 'bells')  { beep(660,'sine',.1,.07); setTimeout(()=>beep(880,'sine',.12,.07),80); return; }
  if (pack === 'arcade') { beep(500,'square',.08,.06); setTimeout(()=>beep(650,'square',.1,.06),70); return; }
  if (pack === 'chime')  { beep(587,'triangle',.12,.07); setTimeout(()=>beep(784,'triangle',.15,.07),80); return; }
  beep(440,'triangle',.15,.07);
}
function playSprint(){
  const pack = _pack();
  if (pack === 'bells')  { [880,988,1108,1175,1318,1480,1568,1760].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.18,.1),i*90)); return; }
  if (pack === 'arcade') { [523,587,659,698,784,880,988,1047].forEach((f,i)=>setTimeout(()=>beep(f,'square',.15,.08),i*80)); return; }
  if (pack === 'chime')  { [784,880,987,1047,1175,1319,1480,1568].forEach((f,i)=>setTimeout(()=>beep(f,'triangle',.2,.1),i*90)); return; }
  [523,587,659,698,784,880,988,1047].forEach((f,i)=>setTimeout(()=>beep(f,'sine',.2,.1),i*90));
}
function playStart(){
  const pack = _pack();
  if (pack === 'bells')  { beep(880,'sine',.12); setTimeout(()=>beep(1108,'sine',.14),120); setTimeout(()=>beep(1318,'sine',.22),240); return; }
  if (pack === 'arcade') { beep(440,'square',.08); setTimeout(()=>beep(550,'square',.1),100); setTimeout(()=>beep(660,'square',.18),200); return; }
  if (pack === 'chime')  { beep(659,'triangle',.1); setTimeout(()=>beep(784,'triangle',.12),110); setTimeout(()=>beep(987,'triangle',.2),220); return; }
  beep(440,'sine',.1); setTimeout(()=>beep(550,'sine',.1),100); setTimeout(()=>beep(660,'sine',.2),200);
}
function playFlip(){
  const pack = _pack();
  if (pack === 'bells')  { beep(1108,'sine',.06,.05); setTimeout(()=>beep(1318,'sine',.08,.06),60); return; }
  if (pack === 'arcade') { beep(600,'square',.04,.04); setTimeout(()=>beep(900,'square',.06,.05),55); return; }
  if (pack === 'chime')  { beep(784,'triangle',.06,.05); setTimeout(()=>beep(1047,'triangle',.09,.06),65); return; }
  beep(500,'sine',.05,.06); setTimeout(()=>beep(900,'sine',.07,.07),55); setTimeout(()=>beep(1200,'triangle',.09,.05),110);
}

function speak(text) {
  if (!soundEnabled) return;           // не озвучивать если звук выключен
  if ('speechSynthesis' in window) {
    speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    u.lang = "en-US"; u.rate = 0.9;
    speechSynthesis.speak(u);
  }
}
function speakCurrent() {
  unlockAudio();
  if (currentWordObj && currentLanguage === 'en') speak(currentWordObj.en.split('/')[0].trim());
}

function _syncPauseSoundBtn() {
  const btn = document.getElementById('pauseSoundToggle');
  if (!btn) return;
  if (soundEnabled) {
    btn.textContent = '🔊 Звук включён';
    btn.classList.remove('muted');
  } else {
    btn.textContent = '🔇 Звук выключен';
    btn.classList.add('muted');
  }
}

function toggleSoundFromPause() {
  unlockAudio();
  soundEnabled = !soundEnabled;
  if (!soundEnabled) speechSynthesis.cancel();   // сразу останавливаем текущую озвучку
  localStorage.setItem('soundEnabled', soundEnabled);
  // синхронизируем все кнопки звука в интерфейсе
  const mainBtn = document.getElementById('soundBtn');
  if (mainBtn) mainBtn.textContent = soundEnabled ? '🔊' : '🔇';
  const settingsWrap = document.getElementById('soundToggleWrap');
  if (settingsWrap) settingsWrap.classList.toggle('on', soundEnabled);
  _syncPauseSoundBtn();
  if (soundEnabled) playClick();
}

// ════════════════════════════════════
//  УТИЛИТЫ
// ════════════════════════════════════
function norm(str) {
  if (!str) return [];
  return str.toLowerCase().replace(/ё/g,'е').replace(/\([^)]*\)/g,'')
    .split(/[\/,;+]/).map(s=>s.trim()).filter(Boolean);
}
function verify(input, correct) {
  return norm(input).some(t => norm(correct).includes(t));
}
function getPool() {
  const cat = document.getElementById("category").value;
  if (cat === "all") return Object.values(categories).flat();
  return categories[cat] || [];
}
function pickWord(pool) {
  const weighted = [];
  pool.forEach(w => {
    const wt = (wordWeights[w.en]||0) + 1;
    for (let i=0;i<wt;i++) weighted.push(w);
  });
  let c = weighted[Math.floor(Math.random()*weighted.length)];
  let tries = 0;
  while (tries++ < 20 && pool.length > 1 && currentWordObj && c.en === currentWordObj.en) {
    c = weighted[Math.floor(Math.random()*weighted.length)];
  }
  return c;
}
// Прогрессивная кривая: уровень N требует N*50 XP (суммарно: N*(N+1)/2*50)
// Уровень 1 = 0 XP, уровень 2 = 50, уровень 5 = 500, уровень 10 = 2250, уровень 50 = 63750, уровень 100 = 252500
function getLevel(x) {
  // Решаем N*(N+1)/2 * 50 <= x => N^2+N-2x/50 <= 0 => N = floor((-1+sqrt(1+8x/50))/2)+1
  if (x <= 0) return 1;
  return Math.floor((-1 + Math.sqrt(1 + 8 * x / 50)) / 2) + 1;
}
function _xpForLevel(n) {
  // Суммарный XP, нужный чтобы достигнуть уровня n (n>=1)
  const lvl = Math.max(1, n) - 1;
  return lvl * (lvl + 1) / 2 * 50;
}
function getNextLvlXp(x) {
  const lvl = getLevel(x);
  return _xpForLevel(lvl + 1);
}
function _getLevelProgress(x) {
  // Прогресс внутри текущего уровня в % (0..100)
  const lvl = getLevel(x);
  const cur = _xpForLevel(lvl);
  const next = _xpForLevel(lvl + 1);
  return Math.round((x - cur) / (next - cur) * 100);
}
function _xpInCurrentLevel(x) {
  return x - _xpForLevel(getLevel(x));
}
function _xpNeededForNextLevel(x) {
  const lvl = getLevel(x);
  return _xpForLevel(lvl + 1) - _xpForLevel(lvl);
}

// ════════════════════════════════════
//  СОХРАНЕНИЕ
// ════════════════════════════════════
function save() {
  localStorage.setItem("xp", xp);
  localStorage.setItem("correct", correct);
  localStorage.setItem("total", total);
  localStorage.setItem("streak", streak);
  if (streak > maxStreak){ maxStreak = streak; localStorage.setItem("maxStreak", maxStreak); }
  localStorage.setItem("achievements", JSON.stringify(earnedAchievements));
  localStorage.setItem("mistakes", JSON.stringify(mistakes));
  localStorage.setItem("soundEnabled", soundEnabled);
  localStorage.setItem("wordWeights", JSON.stringify(wordWeights));
  localStorage.setItem("etu_dailyXp", dailyXp);
  localStorage.setItem("etu_wordErrors", JSON.stringify(wordErrorCount));
  // v4: weekly chart data
  if (typeof saveWeeklyData === 'function') saveWeeklyData();
  // OPTIMIZATION v14: renderWeeklyChart() вызывается только при переходе в меню/статистику,
  // а не при каждом сохранении (каждый правильный ответ). Убираем из save().
  // if (typeof renderWeeklyChart === 'function') renderWeeklyChart();
  // v7: sprint state persistence
  saveSprintState();
}
function savePrefs() {
  localStorage.setItem("etu_mode", document.getElementById("mode").value);
  localStorage.setItem("etu_category", document.getElementById("category").value);
}
function loadPrefs() {
  const m = localStorage.getItem("etu_mode"), c = localStorage.getItem("etu_category");
  if (m) document.getElementById("mode").value = m;
  if (c) document.getElementById("category").value = c;
}

// ════════════════════════════════════
//  РАНГИ
// ════════════════════════════════════
const ranks = [
  {min:0,   label:"🌱 Новичок",   avatar:"🌱"},
  {min:100, label:"💪 Энтузиаст", avatar:"💪"},
  {min:300, label:"🔥 Продвинутый",avatar:"🔥"},
  {min:700, label:"🏆 Мастер слов",avatar:"🏆"},
  {min:1500,label:"🧙 Эрудит",    avatar:"🧙"},
  {min:3000,label:"⚡ Легенда",   avatar:"⚡"},
];
function getRankObj(x){ return [...ranks].reverse().find(r=>x>=r.min) || ranks[0]; }

// ════════════════════════════════════
//  ЭКРАНЫ
// ════════════════════════════════════
function showScreen(id) {
  const prev = document.querySelector('.screen.active');
  const prevId = prev ? prev.id : null;
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active','slide-in-right','slide-in-left'));
  const next = document.getElementById(id);
  next.classList.add('active');

  // Slide direction based on nav order
  const prevOrder = _tabOrder[prevId] ?? -1;
  const nextOrder = _tabOrder[id] ?? -1;
  if (prevOrder >= 0 && nextOrder >= 0 && prevOrder !== nextOrder) {
    const dir = nextOrder > prevOrder ? 'slide-in-right' : 'slide-in-left';
    next.classList.add(dir);
  }
  updateBottomNav(id);
}
function goToMainMenu() {
  playClick();
  clearInterval(interval);
  clearInterval(agInterval);
  clearInterval(bonusInterval); bonusRoundActive = false;
  isPaused=false; isAnswering=false; isGameActive=false; isDictant=false;
  document.getElementById("dictantRepeatWrap").style.display = 'none';
  if (isSprint) clearSprintState(); // v7: don't leave orphaned sprint state
  isSprint=false;
  document.getElementById("pauseOverlay").classList.remove("active");
  updateMenu();
  showScreen("menuScreen");
  document.getElementById("dashboardCard").style.display = "";
  renderActivityCalendar();
  renderChallenges();
  renderChest();
  // OPTIMIZATION v14: рендерим недельный график при возврате в меню (раньше вызывалось из save())
  if (typeof renderWeeklyChart === 'function') renderWeeklyChart();
  // Показать отложенный баннер уровня
  if (pendingLevelUp > 0) {
    const lvl = pendingLevelUp;
    pendingLevelUp = 0;
    setTimeout(() => { showLevelUpBanner(lvl); spawnConfetti(50); }, 400);
  }
}
function showModeSelect() {
  playClick();
  document.getElementById("sprintLenRow").style.display = "none";
  if (typeof updateMistakesModeCard === 'function') updateMistakesModeCard();
  showScreen("modeSelectScreen");
}
function showSprintSelect() {
  playClick();
  document.getElementById("sprintLenRow").style.display = "block";
}
function showStatsScreen() {
  playClick();
  updateStats();
  renderHardWords();
  renderCatStats();
  showScreen("statsScreen");
  document.getElementById("dashboardCard").style.display = "none";
}

// ════════════════════════════════════
//  МЕНЮ / ДАШБОРД
// ════════════════════════════════════
function updateMenu() {
  const lvl = getLevel(xp);
  const nextXp = getNextLvlXp(xp);
  const pct = _getLevelProgress(xp);
  const xpLeft = nextXp - xp;
  document.getElementById("menuXp").textContent = xp;
  document.getElementById("menuLevel").textContent = lvl;
  document.getElementById("nextLvlXp").textContent = nextXp;
  document.getElementById("levelBar").style.width = pct + "%";
  document.getElementById("levelTxt").textContent = `${xpLeft} XP до уровня ${lvl+1}`;
  const rank = getRankObj(xp);
  document.getElementById("userRank").textContent = rank.label;
  _applyAvatarToEl(document.getElementById("menuAvatar"), profileData.avatar || rank.avatar, profileData.avatarPhoto);
  if (profileData.name) document.querySelector(".menu-name").textContent = profileData.name;
  document.getElementById("dailyGoalText").textContent = `цель: ${DAILY_GOALS[dailyDifficulty].xp} XP`;
  document.getElementById("dailyXpDone").textContent = `${dailyXp} XP набрано`;
  const pctDaily = Math.min(dailyXp / DAILY_GOALS[dailyDifficulty].xp * 100, 100);
  document.getElementById("dailyGoalBar").style.width = pctDaily + "%";
  const d = DAILY_GOALS[dailyDifficulty];
  const badge = document.getElementById("dailyDiffBadge");
  badge.textContent = `${d.icon} ${d.name}`;
  badge.style.background = d.bg; badge.style.color = d.color;
  const done = dailyXp >= DAILY_GOALS[dailyDifficulty].xp;
  document.getElementById("dailyCompleteBanner").style.display = done ? "block" : "none";
  document.getElementById("dailyGoalBar").style.background = done
    ? "linear-gradient(90deg, #22d88f, #4f8eff)"
    : "linear-gradient(90deg, #4f8eff, #a78bfa)";
}

function updateStats() {
  document.getElementById("stXp").textContent = xp;
  document.getElementById("stLevel").textContent = getLevel(xp);
  document.getElementById("stCorrect").textContent = correct;
  document.getElementById("stTotal").textContent = total;
  document.getElementById("stStreak").textContent = maxStreak;
  document.getElementById("stDailyXp").textContent = `${dailyXp} / ${DAILY_GOALS[dailyDifficulty].xp}`;
  const acc = total ? Math.round(correct/total*100) : 0;
  document.getElementById("stAccPct").textContent = acc + "%";
  document.getElementById("stAccDetail").textContent = `${correct} из ${total}`;
  document.getElementById("stAccBar").style.width = acc + "%";

  // Day streak
  const dsEl = document.getElementById("stDayStreak");
  if (dsEl) {
    const cur = parseInt(localStorage.getItem("etu_ds_current")) || 0;
    dsEl.textContent = cur;
  }
  
  // Weekly chart in stats
  const swc = document.getElementById("statsWeeklyChart");
  if (swc) {
    const days = ['Пн','Вт','Ср','Чт','Пт','Сб','Вс'];
    const todayIdx = (new Date().getDay() + 6) % 7;
    let data = [];
    try { data = JSON.parse(localStorage.getItem("etu_weekData")) || []; } catch(e){}
    while (data.length < 7) data.unshift(0);
    if (data.length > 7) data = data.slice(-7);
    const maxVal = Math.max(...data, 1);
    swc.innerHTML = data.map((v, i) => {
      const heightPct = Math.round((v / maxVal) * 100);
      const isToday = i === todayIdx;
      return `<div class="week-bar-wrap">
        <div class="week-bar${isToday ? ' today' : ''}" style="height:${Math.max(4, heightPct * 0.36)}px;" title="${v} XP"></div>
        <div class="week-label">${days[i]}</div>
      </div>`;
    }).join("");
  }

  // Ранг лестница
  const c = document.getElementById("rankLevels");
  c.innerHTML = ranks.map(r=>{
    const earned = xp >= r.min;
    const current = getRankObj(xp).min === r.min;
    return `<div class="rank-item ${earned?'earned':''} ${current?'current':''}">${r.label}</div>`;
  }).join("");
}

// ════════════════════════════════════
//  ИГРА — ЗАПУСК
// ════════════════════════════════════
function startEndless() { startEndlessGame(); }

let sprintLenSelected = 10;
function setSprintLen(n, btn) {
  sprintLenSelected = n;
  document.querySelectorAll('.sprint-opt-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
}
function startSprint() {
  playStart(); isSprint=true;
  sprintLen=sprintLenSelected||10; sprintIdx=0;
  sprintCorrect=0; sprintErrors=0; sprintXpEarned=0; sprintMaxStreak=0;
  isGameActive=true;
  // Жизни
  livesMode=true; lives=MAX_LIVES; updateLivesDisplay();
  document.getElementById("livesBar").style.display="flex";
  // Бонус
  stopBonusRound();
  document.getElementById("sprintCounterRow").style.display="block";
  document.getElementById("sprintProgWrap").style.display="block";
  document.getElementById("dashboardCard").style.display="none";
  showScreen("gameScreen"); nextWord();
}
function startEndlessGame() {
  playStart(); isSprint=false; isGameActive=true;
  livesMode=false;
  document.getElementById("livesBar").style.display="none";
  stopBonusRound();
  document.getElementById("sprintCounterRow").style.display="none";
  document.getElementById("sprintProgWrap").style.display="none";
  document.getElementById("dashboardCard").style.display="none";
  showScreen("gameScreen"); nextWord();
}

// ════════════════════════════════════
//  РЕЖИМ ВВОДА
// ════════════════════════════════════
function setGameMode(mode) {
  gameMode = mode; localStorage.setItem("gameMode", mode);
  document.getElementById("modeInputBtn").classList.toggle("active", mode==='write');
  document.getElementById("modeTestBtn").classList.toggle("active", mode==='test');
  document.getElementById("inputSection").style.display = mode==='write'?'block':'none';
  document.getElementById("testSection").style.display = mode==='test'?'block':'none';
  document.getElementById("checkBtn").style.display = mode==='write'?'flex':'none';
  if (isGameActive) nextWord();
}

// ════════════════════════════════════
//  СЛОВО
// ════════════════════════════════════
function nextWord() {
  if (isPaused) return;
  if (isDictant) { nextDictantWord(); return; }
  if (isMistakeRetry) { nextMistakeRetryWord(); return; }
  if (isSprint && sprintIdx >= sprintLen) { showResults(); return; }
  isAnswering=false; toggleBtns(false);
  const pool = getPool();
  if (!pool.length){ alertPop("В категории нет слов!"); return; }
  currentWordObj = pickWord(pool);
  const dir = document.getElementById("mode").value;
  if (dir==="ru-en" || (dir==="mixed" && Math.random()<.5)) {
    document.getElementById("word").textContent = currentWordObj.ru;
    correctAnswerString = currentWordObj.en; currentLanguage='ru';
    document.getElementById("speakBtn").style.display='none';
  } else {
    document.getElementById("word").textContent = currentWordObj.en;
    correctAnswerString = currentWordObj.ru; currentLanguage='en';
    document.getElementById("speakBtn").style.display='inline-flex';
  }
  document.getElementById("answer").value="";
  document.getElementById("answer").className="";
  document.getElementById("result").innerHTML="";
  document.getElementById("result").className="result";
  if (isSprint){
    document.getElementById("sprintCurrent").textContent = sprintIdx+1;
    document.getElementById("sprintTotal").textContent = sprintLen;
    document.getElementById("sprintBar").style.width = (sprintIdx/sprintLen*100)+"%";
  }
  if (gameMode==='test') genOptions();
  if (gameMode==='write' && window.innerWidth>768) document.getElementById("answer").focus();
  resetTimer();
}

// ════════════════════════════════════
//  ТЕСТ: ВАРИАНТЫ
// ════════════════════════════════════
function genOptions() {
  const pool = getPool();
  // В диктанте: слышим английское, отвечаем русским
  const key = isDictant ? 'ru' : (currentLanguage==='ru'?'en':'ru');
  const corr = currentWordObj[key];
  let opts = [corr];
  const uniq = [...new Set(pool.map(w=>w[key]))].filter(v=>v!==corr);
  while (opts.length < Math.min(4, uniq.length+1)){
    const r = uniq[Math.floor(Math.random()*uniq.length)];
    if (!opts.includes(r)) opts.push(r);
  }
  opts.sort(()=>Math.random()-.5);
  const grid = document.getElementById("optionsGrid");
  grid.innerHTML="";
  opts.forEach(opt=>{
    const btn = document.createElement("button");
    btn.className="option-btn";
    btn.textContent = opt.charAt(0).toUpperCase()+opt.slice(1);
    btn.onclick=()=>{ unlockAudio(); selectOpt(opt,btn); };
    grid.appendChild(btn);
  });
}
function selectOpt(val, btn) {
  if (isAnswering) return;
  isAnswering=true; clearInterval(interval); toggleBtns(true);
  const ok = verify(val, correctAnswerString);
  document.querySelectorAll(".option-btn").forEach(b=>{
    b.style.pointerEvents="none";
    if (verify(b.textContent, correctAnswerString)) b.classList.add("correct-choice");
  });
  if (!ok && btn) btn.classList.add("incorrect-choice");
  ok ? onCorrect() : onWrong();
}

// ════════════════════════════════════
//  ПРОВЕРКА
// ════════════════════════════════════
function checkAnswer() {
  unlockAudio();
  if (gameMode!=='write'||isAnswering) return;
  const val = document.getElementById("answer").value.trim();
  if (!val) return;
  isAnswering=true; clearInterval(interval); toggleBtns(true);
  const ok = verify(val, correctAnswerString);
  document.getElementById("answer").classList.add(ok?'input-correct':'input-wrong');
  ok ? onCorrect() : onWrong();
}

function onCorrect() {
  total++; correct++; streak++;
  if (isSprint){ sprintCorrect++; sprintMaxStreak = Math.max(sprintMaxStreak,streak); }
  // Диктант
  if (isDictant) { dictantCorrect++; }
  const key = currentWordObj.en;
  wordWeights[key] = Math.max((wordWeights[key]||0)-1, 0);
  trackCatStat(true); // v7: category stats

  // Бонус-раунд
  if (streak === BONUS_TRIGGER && !bonusRoundActive) { activateBonusRound(); playBonusSound(); }

  const bonusMult = bonusRoundActive ? 2 : 1;
  const mult = streak>=15?3:streak>=5?2:1;
  const earned = 10 * mult * bonusMult;
  xp+=earned; dailyXp+=earned;
  if (isSprint) sprintXpEarned+=earned;

  // Рекорд дня
  if (isSprint && sprintXpEarned > dailyRecord) {
    dailyRecord = sprintXpEarned;
    localStorage.setItem("etu_dailyRecord_" + new Date().toDateString(), dailyRecord);
  }

  const res = document.getElementById("result");
  res.className="result ok-text";
  let xpLabel = `+${earned} XP`;
  if (bonusMult>1) xpLabel += ` ⚡`;
  if (mult>1) xpLabel += ` <b>(×${mult} 🔥)</b>`;
  res.innerHTML = `✅ Правильно! ${xpLabel}`;
  spawnXpPop(earned);
  document.getElementById("gameScreen").classList.add("flash-ok");
  setTimeout(()=>document.getElementById("gameScreen").classList.remove("flash-ok"),350);
  if (mult>1){ playStreak(); showStreakBanner(streak); }
  else playCorrect();
  // Haptic
  if (navigator.vibrate) navigator.vibrate(30);
  if (currentLanguage==='ru') speak(currentWordObj.en.split('/')[0].trim());
  if (streak>maxStreak) maxStreak=streak;
  const _prevXp = xp - earned;
  update(); save(); checkAchievements();
  if (typeof checkLevelUp === 'function') checkLevelUp(_prevXp, xp);
  if (typeof updateProfileRing === 'function') updateProfileRing();
  if (isDictant) {
    // Reveal the English word that was spoken
    const enWord = currentWordObj.en.split('/')[0].trim();
    res.innerHTML += ` 🔤 <em style="color:var(--accent)">${enWord}</em>`;
    dictantXp += earned; dictantIdx++;
    document.getElementById("dictantRepeatWrap").style.display = 'none';
    setTimeout(nextWord, 800);
  } else if (isSprint){ sprintIdx++; if(isMistakeRetry) mistakeRetryIdx++; setTimeout(nextWord, sprintIdx>=sprintLen?600:250); }
  else setTimeout(nextWord,250);
}
function onWrong() {
  total++; streak=0;
  if (isSprint) sprintErrors++;
  const key = currentWordObj.en;
  wordWeights[key]=(wordWeights[key]||0)+2;
  // Трекинг ошибок по словам
  wordErrorCount[key] = (wordErrorCount[key]||0)+1;
  trackCatStat(false); // v7: category stats

  const res = document.getElementById("result");
  res.className="result err-text";
  res.innerHTML = `❌ Ошибка. Верно: <strong>${correctAnswerString}</strong>`;
  document.getElementById("gameScreen").classList.add("shake");
  setTimeout(()=>document.getElementById("gameScreen").classList.remove("shake"),500);
  playWrong();
  // Haptic — двойной импульс для ошибки
  if (navigator.vibrate) navigator.vibrate([40,30,40]);

  const isEN = currentLanguage==='en';
  const mstr = isEN?`${currentWordObj.en} — ${currentWordObj.ru}`:`${currentWordObj.ru} — ${currentWordObj.en}`;
  if (!mistakes.includes(mstr)){ mistakes.push(mstr); renderMistakes(); }

  // Жизни
  if (livesMode) {
    lives = Math.max(0, lives - 1);
    updateLivesDisplay();
    if (lives <= 0) {
      update(); save(); checkAchievements();
      if (isSprint){ sprintIdx++; setTimeout(()=>{ showResults(); }, 800); }
      return;
    }
    if (lives === 1) {
      document.getElementById("gameScreen").classList.add("lives-danger");
      setTimeout(()=>document.getElementById("gameScreen").classList.remove("lives-danger"),600);
    }
  }

  update(); save(); checkAchievements();
  if (isDictant) {
    dictantErrors++; dictantIdx++;
    document.getElementById("dictantRepeatWrap").style.display = 'none';
    setTimeout(nextWord, 1400);
  } else if (isSprint){ sprintIdx++; if(isMistakeRetry) mistakeRetryIdx++; setTimeout(nextWord,sprintIdx>=sprintLen?600:1200); }
  else setTimeout(nextWord,1500);
}

// ════════════════════════════════════
//  ПОДСКАЗКА / ПРОПУСК
// ════════════════════════════════════
function getHint() {
  unlockAudio(); if (isAnswering) return;
  if (xp<5){ alertPop("Нужно минимум 5 XP для подсказки!"); return; }
  xp-=5; playHint(); update(); save();
  if (gameMode==='write'){
    const inp = document.getElementById("answer");
    const clean = norm(correctAnswerString)[0];
    if (clean){ inp.value=clean.substring(0,Math.max(inp.value.length+1,1)); inp.focus(); }
  } else {
    let removed=0;
    document.querySelectorAll(".option-btn").forEach(btn=>{
      if (removed<1&&!verify(btn.textContent,correctAnswerString)&&btn.style.opacity!=="0.3"){
        btn.style.opacity="0.3"; btn.style.pointerEvents="none"; removed++;
      }
    });
  }
}
function skipWord() {
  if (isAnswering) return;
  playClick(); if (isSprint) sprintIdx++; nextWord();
}

// ════════════════════════════════════
//  ТАЙМЕР
// ════════════════════════════════════
const TIMER_MAX = 15;
function resetTimer() {
  clearInterval(interval); timerVal=TIMER_MAX;
  const el = document.getElementById("timer");
  const circle = document.getElementById("timerCircle");
  el.textContent = timerVal;
  if (circle){ circle.style.stroke="var(--warn)"; circle.style.strokeDashoffset="0"; }
  interval = setInterval(()=>{
    if (isPaused||isAnswering) return;
    timerVal--;
    el.textContent=timerVal;
    const offset = (1-timerVal/TIMER_MAX)*100;
    if (circle){ circle.style.strokeDashoffset=offset; }
    if (timerVal<=5){ if (circle) { circle.style.stroke="var(--err)"; circle.classList.add('danger'); } playTick(); }
    if (timerVal>5 && circle) { circle.style.stroke="var(--warn)"; circle.classList.remove('danger'); }
    if (timerVal<=0){
      clearInterval(interval); streak=0; update(); save();
      if (isSprint) sprintIdx++;
      nextWord();
    }
  },1000);
}

// ════════════════════════════════════
//  ПАУЗА
// ════════════════════════════════════
function pauseGame() {
  clearInterval(interval); isPaused=true;
  document.getElementById("pauseOverlay").classList.add("active");
  _syncPauseSoundBtn();
}
function resumeGame() {
  isPaused=false; savePrefs();
  document.getElementById("pauseOverlay").classList.remove("active");
  nextWord();
}

function toggleBtns(disabled) {
  ['answer','skipBtn','hintBtn','checkBtn'].forEach(id=>{
    const el=document.getElementById(id); if(el) el.disabled=disabled;
  });
}

// ════════════════════════════════════
//  UPDATE UI
// ════════════════════════════════════
function update() {
  document.getElementById("gameXp").textContent=xp;
  document.getElementById("gameStreak").textContent=streak;
  document.getElementById("gameAccuracy").textContent=(total?Math.round(correct/total*100):0)+"%";
  document.getElementById("xpBar").style.width=_getLevelProgress(xp)+"%";
  updateMenu();
}

// ════════════════════════════════════
//  РЕЗУЛЬТАТЫ СПРИНТА
// ════════════════════════════════════
function showResults() {
  clearInterval(interval); isGameActive=false; playSprint();
  _lastResultMode = 'sprint';
  clearSprintState();
  stopBonusRound();
  livesMode=false;
  document.getElementById("livesBar").style.display="none";
  document.getElementById("resCorrect").textContent=sprintCorrect;
  document.getElementById("resErrors").textContent=sprintErrors;
  document.getElementById("resXp").textContent=sprintXpEarned;
  document.getElementById("resStreak").textContent=sprintMaxStreak;
  const pct = sprintLen>0?Math.round(sprintCorrect/sprintLen*100):0;
  document.getElementById("resultsPct").textContent=pct+"%";
  let emoji="😅", title="Не сдавайся!", verdict="";
  if (pct===100){emoji="🏆";title="Идеально! 🎉";triggerAch("ach_perfect");}
  else if(pct>=80){emoji="🌟";title="Отличный результат!";}
  else if(pct>=60){emoji="👍";title="Хорошая работа!";}
  else if(pct>=40){emoji="💪";title="Продолжай тренироваться!";}
  verdict=`Правильных: <b>${sprintCorrect} из ${sprintLen}</b> (${pct}%)<br>Заработано: <b>+${sprintXpEarned} XP</b> • Серия: <b>${sprintMaxStreak}</b>`;
  document.getElementById("resultsEmoji").textContent=emoji;
  document.getElementById("resultsTitle").textContent=title;
  document.getElementById("resultsVerdict").innerHTML=verdict;
  triggerAch("ach_sprint");
  trackChStat('sprints10');
  if (sprintErrors === 0) trackChStat('perfectSprint');
  claimChallengeXp();
  renderActivityCalendar();

  // Рекорд дня
  const recBadge = document.getElementById("dailyRecordBadge");
  const todayKey = "etu_dailyRecord_" + new Date().toDateString();
  const prevRecord = parseInt(localStorage.getItem(todayKey)) || 0;
  if (sprintXpEarned > 0 && sprintXpEarned >= prevRecord) {
    localStorage.setItem(todayKey, sprintXpEarned);
    dailyRecord = sprintXpEarned;
    recBadge.style.display="block";
    recBadge.querySelector("span").classList.add("record-new");
    setTimeout(()=>recBadge.querySelector("span").classList.remove("record-new"),700);
  } else {
    recBadge.style.display="none";
  }

  // Кнопка повторения ошибок
  const retryBtn = document.getElementById("retryMistakesBtn");
  // Собираем слова из текущего спринта с ошибками
  _lastSprintMistakes = mistakes.slice(-sprintErrors);
  retryBtn.style.display = sprintErrors > 0 ? "block" : "none";

  showScreen("resultsScreen");
  document.getElementById("dashboardCard").style.display="none";
  spawnConfetti(30);
}

// ════════════════════════════════════
//  ФЛЭШКАРТЫ
// ════════════════════════════════════
function startFlashcards(customPool) {
  playStart();
  const pool = customPool || getPool().sort(()=>Math.random()-.5).slice(0, 20);
  fcPool = pool; fcIdx=0; fcKnown=0; fcUnknown=0; fcUnknownCards=[];
  document.getElementById("dashboardCard").style.display="none";
  showScreen("flashcardScreen");
  renderFlashcard();
  triggerAch("ach_flash");
}
function renderFlashcard() {
  if (fcIdx >= fcPool.length){ showFlashcardResult(); return; }
  const w = fcPool[fcIdx];
  const card = document.getElementById("flashcard");
  card.classList.remove("flipped");
  document.getElementById("fcFrontWord").textContent = w.ru;
  document.getElementById("fcBackWord").textContent = w.en;
  document.getElementById("fcCounter").textContent = `Карточка ${fcIdx+1} из ${fcPool.length}`;
  document.getElementById("fcBar").style.width = (fcIdx/fcPool.length*100)+"%";
  document.getElementById("fcKnowCount").textContent = fcKnown;
  document.getElementById("fcUnknCount").textContent = fcUnknown;
  document.getElementById("fcLeftCount").textContent = fcPool.length - fcIdx;
}
function flipCard() {
  playFlip(); unlockAudio();
  document.getElementById("flashcard").classList.toggle("flipped");
}
function fcAnswer(known) {
  unlockAudio();
  if (known){ fcKnown++; playCorrect(); }
  else { fcUnknown++; fcUnknownCards.push(fcPool[fcIdx]); playWrong(); }
  fcIdx++;
  setTimeout(renderFlashcard, 200);
}
function showFlashcardResult() {
  const total = fcKnown + fcUnknown;
  const pct = total?Math.round(fcKnown/total*100):0;
  let emoji="📚", title="Продолжай!";
  if(pct===100){emoji="🎉";title="Все слова знаешь!";}
  else if(pct>=80){emoji="🌟";title="Отлично!";}
  else if(pct>=60){emoji="👍";title="Хорошо!";}
  document.getElementById("fcResultEmoji").textContent=emoji;
  document.getElementById("fcResultTitle").textContent=title;
  document.getElementById("fcResKnow").textContent=fcKnown;
  document.getElementById("fcResUnknown").textContent=fcUnknown;
  document.getElementById("fcVerdict").innerHTML=`Знаешь <b>${fcKnown}</b> из <b>${total}</b> (${pct}%)${fcUnknownCards.length>0?`<br>Рекомендуем повторить: <b>${fcUnknownCards.length}</b> слов`:'.'}`;
  const repBtn = document.getElementById("fcRepeatBtn");
  repBtn.style.display = fcUnknownCards.length>0?'flex':'none';
  showScreen("flashcardResultScreen");
  xp += fcKnown*3; dailyXp += fcKnown*3; save(); updateMenu();
  trackChStat('flashDone');
  claimChallengeXp();
  renderActivityCalendar();
}
function fcRepeatUnknown() {
  if (fcUnknownCards.length>0) startFlashcards(fcUnknownCards);
}
function exitFlashcards() { goToMainMenu(); }

// ════════════════════════════════════
//  АНАГРАММА
// ════════════════════════════════════
function startAnagram() {
  playStart();
  const pool = getPool().filter(w=>w.en.split('/')[0].trim().length<=10 && !w.en.includes(' '));
  if (pool.length < 5){ alertPop("Нет подходящих слов! Выбери категорию с простыми словами."); return; }
  agPool = pool.sort(()=>Math.random()-.5).slice(0, AG_TOTAL);
  agIdx=0; agCorrect=0; agErrors=0; agXpEarned=0; agStreak=0;
  document.getElementById("dashboardCard").style.display="none";
  showScreen("anagramScreen");
  nextAnagram();
  triggerAch("ach_anagram");
}
function nextAnagram() {
  clearInterval(agInterval);
  if (agIdx >= agPool.length){ showAnagramResult(); return; }
  const w = agPool[agIdx];
  agWord = w.ru;
  agTarget = w.en.split('/')[0].trim().toLowerCase();
  document.getElementById("agWord").textContent = agWord;
  document.getElementById("agCounter").textContent = `Слово ${agIdx+1} из ${AG_TOTAL}`;
  document.getElementById("agBar").style.width=(agIdx/AG_TOTAL*100)+"%";
  document.getElementById("agResult").innerHTML="";
  document.getElementById("agResult").className="result";
  document.getElementById("agXp").textContent=xp;
  document.getElementById("agStreak").textContent=agStreak;
  renderAnagramLetters();
  agTimer=20; agTick();
  agInterval = setInterval(()=>{
    agTimer--;
    document.getElementById("agTimer").textContent=agTimer;
    if(agTimer<=5) playTick();
    if(agTimer<=0){ clearInterval(agInterval); agOnWrong(); }
  },1000);
}
function renderAnagramLetters(){
  const shuffled = agTarget.split('').sort(()=>Math.random()-.5);
  const lettersEl = document.getElementById("agLetters");
  const answerEl = document.getElementById("agAnswer");
  const blankEl = document.getElementById("agBlank");
  lettersEl.innerHTML = shuffled.map((l,i)=>
    `<div class="letter-tile jump" data-letter="${l}" data-idx="${i}" onclick="agAddLetter(this)" style="animation-delay:${i*55}ms">${l.toUpperCase()}</div>`
  ).join("");
  answerEl.innerHTML="";
  blankEl.innerHTML = agTarget.split('').map(()=>`<div class="blank-slot"></div>`).join("");
}
function agAddLetter(tile) {
  if (tile.classList.contains('used')) return;
  playClick();
  tile.classList.add('used');
  const answerEl = document.getElementById("agAnswer");
  const div = document.createElement("div");
  div.className="answer-tile";
  div.textContent=tile.dataset.letter.toUpperCase();
  div.dataset.srcIdx=tile.dataset.idx;
  answerEl.appendChild(div);
  updateAnagramBlank();
}
function anagramAnswerClick(e){
  const tile = e.target.closest('.answer-tile');
  if (!tile) return;
  playClick();
  const srcIdx = tile.dataset.srcIdx;
  document.querySelector(`.letter-tile[data-idx="${srcIdx}"]`)?.classList.remove('used');
  tile.remove();
  updateAnagramBlank();
}
function updateAnagramBlank(){
  const filled = document.getElementById("agAnswer").children.length;
  document.querySelectorAll(".blank-slot").forEach((s,i)=>{
    s.classList.toggle('filled', i<filled);
  });
}
function agHint(){
  playHint();
  const answerEl = document.getElementById("agAnswer");
  const pos = answerEl.children.length;
  if (pos >= agTarget.length) return;
  const nextChar = agTarget[pos];
  const tile = [...document.querySelectorAll('.letter-tile')].find(t=>!t.classList.contains('used')&&t.dataset.letter===nextChar);
  // BUG FIX v14: XP списывался даже если подходящая буква не найдена (тайл уже использован или отсутствует).
  if (!tile) return;
  xp=Math.max(0,xp-3); update(); save();
  agAddLetter(tile);
}
function agSkip(){
  clearInterval(agInterval);
  agErrors++; agStreak=0;
  document.getElementById("agResult").innerHTML=`⏭ Пропущено. Ответ: <strong>${agTarget}</strong>`;
  document.getElementById("agResult").className="result err-text";
  agIdx++;
  setTimeout(nextAnagram, 1200);
}
function agCheck(){
  unlockAudio();
  const answer = [...document.getElementById("agAnswer").children].map(t=>t.textContent.toLowerCase()).join('');
  if (!answer){ alertPop("Составь слово из букв!"); return; }
  clearInterval(agInterval);
  if (answer===agTarget){
    agCorrect++; agStreak++;
    const earned=10+(agStreak>=5?10:0); agXpEarned+=earned;
    xp+=earned; dailyXp+=earned;
    document.getElementById("agResult").innerHTML=`✅ Верно! +${earned} XP`;
    document.getElementById("agResult").className="result ok-text";
    spawnXpPop(earned);
    if(agStreak>=5) playStreak(); else playCorrect();
    if (navigator.vibrate) navigator.vibrate(30);
    speak(agTarget);
  } else {
    agErrors++; agStreak=0;
    document.getElementById("agResult").innerHTML=`❌ Неверно. Правильно: <strong>${agTarget}</strong>`;
    document.getElementById("agResult").className="result err-text";
    playWrong();
    if (navigator.vibrate) navigator.vibrate([40,30,40]);
  }
  document.getElementById("agXp").textContent=xp;
  document.getElementById("agStreak").textContent=agStreak;
  update(); save(); checkAchievements();
  agIdx++;
  setTimeout(nextAnagram, 1300);
}
function agTick(){ document.getElementById("agTimer").textContent=agTimer; }
function agOnWrong(){
  agErrors++; agStreak=0;
  document.getElementById("agResult").innerHTML=`⏱ Время вышло! Ответ: <strong>${agTarget}</strong>`;
  document.getElementById("agResult").className="result err-text";
  playWrong();
  agIdx++;
  setTimeout(nextAnagram, 1200);
}
function showAnagramResult(){
  _lastResultMode = 'anagram';
  const pct = AG_TOTAL>0?Math.round(agCorrect/AG_TOTAL*100):0;
  let emoji="🧩",title="Анаграмма завершена!";
  if(pct===100){emoji="🏆";title="Все слова угаданы!";}
  else if(pct>=70){emoji="🌟";title="Отлично!";}
  sprintCorrect=agCorrect; sprintErrors=agErrors; sprintXpEarned=agXpEarned; sprintMaxStreak=agStreak;
  sprintLen=AG_TOTAL;
  document.getElementById("resCorrect").textContent=agCorrect;
  document.getElementById("resErrors").textContent=agErrors;
  document.getElementById("resXp").textContent=agXpEarned;
  document.getElementById("resStreak").textContent=agStreak;
  document.getElementById("resultsPct").textContent=pct+"%";
  document.getElementById("resultsEmoji").textContent=emoji;
  document.getElementById("resultsTitle").textContent=title;
  document.getElementById("resultsVerdict").innerHTML=`Угадано: <b>${agCorrect} из ${AG_TOTAL}</b> (${pct}%)<br>Заработано: <b>+${agXpEarned} XP</b>`;
  playSprint();
  showScreen("resultsScreen");
  document.getElementById("dashboardCard").style.display="none";
  trackChStat('anagramDone');
  claimChallengeXp();
  renderActivityCalendar();
}
function exitAnagram(){
  clearInterval(agInterval); goToMainMenu();
}

// ════════════════════════════════════
//  ДОСТИЖЕНИЯ
// ════════════════════════════════════
function checkAchievements() {
  const lvl=getLevel(xp);
  if(correct>=10)  triggerAch("ach_10");
  if(correct>=50)  triggerAch("ach_50");
  if(correct>=100) triggerAch("ach_100");
  if(correct>=250) triggerAch("ach_250");
  if(streak>=10)   triggerAch("ach_str10");
  if(streak>=25)   triggerAch("ach_str25");
  if(streak>=50)   triggerAch("ach_str50");
  if(lvl>=5)       triggerAch("ach_lvl5");
  if(lvl>=10)      triggerAch("ach_lvl10");
  renderAchievements();
}
function triggerAch(id) {
  if (!earnedAchievements.includes(id) && achievementList[id]){
    earnedAchievements.push(id); save(); playAchievement();
    showAchPop(achievementList[id]);
  }
}
function _renderAchievementsOld() {
  // replaced by v4 version below
}

// ════════════════════════════════════
//  ОШИБКИ
// ════════════════════════════════════
function renderMistakes() {
  const c=document.getElementById("mistakes");
  if (typeof updateMistakesModeCard === 'function') updateMistakesModeCard();
  const mBadge = document.getElementById("misCountBadge");
  if (mBadge) {
    mBadge.textContent = mistakes.length || '';
    mBadge.style.display = mistakes.length ? 'inline' : 'none';
  }
  const retryAllBtn = document.getElementById("retryAllMistakesBtn");
  if (retryAllBtn) retryAllBtn.style.display = mistakes.length ? "block" : "none";

  if(!mistakes.length){ c.innerHTML=`<li class="empty-state">Ошибок нет. Хорошая работа!</li>`;return; }
  c.innerHTML=mistakes.map((m,i)=>{
    const [a,b]=m.split(" — ");
    return `<li>
      <div><span class="err-ru">${a||''}</span> → <span class="ok-en">${b||''}</span></div>
      <button class="clean-btn" onclick="studyMistake('${(a||'').replace(/'/g,"\\'")}','${(b||'').replace(/'/g,"\\'")}',${i})">Учить</button>
    </li>`;
  }).join("");
}
function studyMistake(src,tgt,idx) {
  unlockAudio(); isGameActive=true; isSprint=false;
  showScreen("gameScreen");
  document.getElementById("dashboardCard").style.display="none";
  document.getElementById("sprintCounterRow").style.display="none";
  document.getElementById("sprintProgWrap").style.display="none";
  // BUG FIX v14: src — всегда русское слово, tgt — английское.
  // Старый код некорректно использовал currentLanguage (который мог быть от предыдущей игры).
  currentWordObj = { ru: src, en: tgt };
  currentLanguage = 'ru';
  correctAnswerString = tgt;
  document.getElementById("word").textContent=src;
  document.getElementById("answer").value="";
  document.getElementById("result").innerHTML="💪 Режим отработки ошибки!";
  document.getElementById("result").className="result";
  if(gameMode==='test') genOptions();
  mistakes.splice(idx,1); save(); renderMistakes();
  isAnswering=false; toggleBtns(false); resetTimer();
}
function clearMistakes(){ mistakes=[];save();renderMistakes(); }

// ════════════════════════════════════
//  МОИ СЛОВА
// ════════════════════════════════════
function renderMyWords() {
  const list=document.getElementById("mwList");
  if(!categories.myWords.length){
    list.innerHTML=`<div class="empty-state" style="padding:16px;font-size:14px;">Пока нет слов. Добавь первое!</div>`;return;
  }
  list.innerHTML=categories.myWords.map((w,i)=>
    `<div class="mw-row">
      <span>🇷🇺 <b>${w.ru}</b> → 🇬🇧 <b>${w.en}</b></span>
      <button class="mw-del-btn" onclick="deleteMyWord(${i})">✕</button>
    </div>`
  ).join("");
}
function addMyWord() {
  const ru=document.getElementById("mwRu").value.trim();
  const en=document.getElementById("mwEn").value.trim();
  if(!ru||!en){ alertPop("Введи оба слова!"); return; }
  categories.myWords.push({ru,en});
  localStorage.setItem("myWords",JSON.stringify(categories.myWords));
  document.getElementById("mwRu").value="";
  document.getElementById("mwEn").value="";
  playCorrect(); triggerAch("ach_myword"); renderMyWords();
}
function deleteMyWord(i){
  categories.myWords.splice(i,1);
  localStorage.setItem("myWords",JSON.stringify(categories.myWords));
  renderMyWords();
}
document.getElementById("mwEn").addEventListener("keydown",e=>{ if(e.key==="Enter") addMyWord(); });

// ════════════════════════════════════
//  ТАБЫ
// ════════════════════════════════════
function switchTab(id) {
  ['tab-ach','tab-mis'].forEach(t=>document.getElementById(t).classList.remove('active'));
  ['achBtn','misBtn'].forEach(b=>document.getElementById(b).classList.remove('active'));
  document.getElementById(id).classList.add('active');
  document.getElementById(id==='tab-ach'?'achBtn':'misBtn').classList.add('active');
}

// ════════════════════════════════════
//  ТЕМА / ЗВУК
// ════════════════════════════════════
function toggleTheme() {
  document.body.classList.toggle("light");
  const l=document.body.classList.contains("light");
  localStorage.setItem("etu_theme",l?"light":"dark");
  document.getElementById("themeBtn").textContent=l?"☀️":"🌙";
  _syncThemeColor();
}
function loadTheme() {
  const t=localStorage.getItem("etu_theme");
  if(t==="light"){ document.body.classList.add("light"); document.getElementById("themeBtn").textContent="☀️"; }
  _syncThemeColor();
}
function toggleSound() {
  unlockAudio(); soundEnabled=!soundEnabled;
  localStorage.setItem("soundEnabled",soundEnabled);
  document.getElementById("soundBtn").textContent=soundEnabled?"🔊":"🔇";
  if(soundEnabled) playClick();
}

// ════════════════════════════════════
//  ПОПАПЫ
// ════════════════════════════════════
function spawnXpPop(amount) {
  const el=document.getElementById("gameXp");
  if(!el) return;
  const rect=el.getBoundingClientRect();
  const pop=document.createElement("div");
  pop.className="xp-pop"; pop.textContent=`+${amount} XP`;
  pop.style.left=(rect.left+rect.width/2-30)+"px";
  pop.style.top=(rect.top+window.scrollY-10)+"px";
  document.body.appendChild(pop);
  setTimeout(()=>pop.remove(),950);
}
function showAchPop(text) {
  const pop=document.createElement("div");
  pop.className="ach-pop";
  pop.innerHTML=`<div class="ach-pop-title">🎉 Достижение получено!</div>${text}`;
  document.body.appendChild(pop);
  setTimeout(()=>{ pop.style.opacity="0"; pop.style.transition="opacity .5s"; setTimeout(()=>pop.remove(),500); },3000);
}
function showStreakBanner(n) {
  // streak отображается в шапке через gameStreak — всплывашка убрана
}
function alertPop(msg) {
  const d=document.createElement("div");
  d.className="notif"; d.textContent=msg;
  document.body.appendChild(d);
  setTimeout(()=>{ d.style.opacity="0"; d.style.transition="opacity .4s"; setTimeout(()=>d.remove(),400); },3000);
}

// ════════════════════════════════════
//  СБРОС
// ════════════════════════════════════
function confirmReset() {
  shopConfirm("⚠️ Сбросить весь прогресс, XP, достижения и ошибки?<br><span style='font-size:13px;font-weight:600;opacity:.7'>Покупки в магазине сохранятся</span>", () => {
    const savedDiff  = localStorage.getItem("etu_difficulty");
    const savedTheme = localStorage.getItem("etu_theme");
    const savedAccent= localStorage.getItem("etu_accent_theme");
    const savedShop  = localStorage.getItem("etu_shop_owned");
    const savedSound = localStorage.getItem("etu_sound_pack");
    localStorage.clear();
    if (savedDiff)   localStorage.setItem("etu_difficulty",   savedDiff);
    if (savedTheme)  localStorage.setItem("etu_theme",        savedTheme);
    if (savedAccent) localStorage.setItem("etu_accent_theme", savedAccent);
    if (savedShop)   localStorage.setItem("etu_shop_owned",   savedShop);
    if (savedSound)  localStorage.setItem("etu_sound_pack",   savedSound);
    xp=correct=total=maxStreak=streak=dailyXp=0;
    earnedAchievements=[]; mistakes=[]; wordWeights={};
    categories.myWords=[];
    save(); goToMainMenu(); update();
    alertPop("Прогресс сброшен!");
  }, '⚠️ Сбросить');
}

// ════════════════════════════════════
//  КЛАВИШИ
// ════════════════════════════════════
document.addEventListener("keydown",e=>{
  if(e.key==="Escape"){
    if(document.getElementById("pauseOverlay").classList.contains("active")) resumeGame();
    else if(isGameActive) pauseGame();
  }
  if(e.key==="Enter"){ unlockAudio(); checkAnswer(); }
});


// ════════════════════════════════════
//  ПРОИЗНОШЕНИЕ
// ════════════════════════════════════
let pronunHistory = [];
let pronunVoicesLoaded = false;

function initPronunScreen() {
  if (!('speechSynthesis' in window)) {
    document.getElementById('pronunNoSupport').style.display = 'block';
    return;
  }
  loadPronunVoices();
  renderPronunHistory();
  setTimeout(() => document.getElementById('pronunInput').focus(), 200);
}

function loadPronunVoices() {
  const sel = document.getElementById('pronunVoice');
  const populate = () => {
    const voices = speechSynthesis.getVoices();
    const engVoices = voices.filter(v => v.lang.startsWith('en'));
    sel.innerHTML = '';
    if (engVoices.length === 0) {
      const opt = document.createElement('option');
      opt.textContent = 'По умолчанию'; opt.value = '';
      sel.appendChild(opt);
    } else {
      engVoices.forEach((v, i) => {
        const opt = document.createElement('option');
        opt.value = i;
        opt.textContent = `${v.name} (${v.lang})`;
        sel.appendChild(opt);
      });
    }
    pronunVoicesLoaded = true;
  };
  if (speechSynthesis.getVoices().length) { populate(); }
  else { speechSynthesis.onvoiceschanged = populate; }
}

function pronounceWord(text) {
  if (!('speechSynthesis' in window)) return;
  const input = text || document.getElementById('pronunInput').value.trim();
  if (!input) { alertPop('Введи слово или букву!'); return; }

  speechSynthesis.cancel();

  const utt = new SpeechSynthesisUtterance(input);
  const sel = document.getElementById('pronunVoice');
  const voices = speechSynthesis.getVoices().filter(v => v.lang.startsWith('en'));
  if (voices.length && sel.value !== '') {
    utt.voice = voices[parseInt(sel.value)] || voices[0];
  }
  utt.rate  = parseFloat(document.getElementById('pronunSpeed').value);
  utt.pitch = parseFloat(document.getElementById('pronunPitch').value);
  utt.lang  = 'en-US';

  const btn = document.getElementById('pronunSpeakBtn');
  btn.classList.add('speaking');
  utt.onend = () => btn.classList.remove('speaking');
  utt.onerror = () => btn.classList.remove('speaking');

  speechSynthesis.speak(utt);

  // сохранить в историю (без дублей подряд)
  if (!pronunHistory.length || pronunHistory[0] !== input) {
    pronunHistory.unshift(input);
    if (pronunHistory.length > 20) pronunHistory.pop();
    renderPronunHistory();
  }
}

function renderPronunHistory() {
  const wrap = document.getElementById('pronunHistory');
  const empty = document.getElementById('pronunHistEmpty');
  if (!pronunHistory.length) {
    empty.style.display = 'block';
    // remove old items
    wrap.querySelectorAll('.pronun-hist-item').forEach(el => el.remove());
    return;
  }
  empty.style.display = 'none';
  wrap.querySelectorAll('.pronun-hist-item').forEach(el => el.remove());
  pronunHistory.forEach(word => {
    const div = document.createElement('div');
    div.className = 'pronun-hist-item';
    div.innerHTML = `<span>${word}</span><span class="pronun-hist-repeat">🔊</span>`;
    div.onclick = () => {
      document.getElementById('pronunInput').value = word;
      pronounceWord(word);
    };
    wrap.appendChild(div);
  });
}


// ════════════════════════════════════
//  ПРОФИЛЬ
// ════════════════════════════════════
const AVATARS = ["🦁","🐯","🦊","🐺","🐻","🐼","🐨","🦝","🐸","🐙","🦋","🐬","🦄","🐉","👾","🤖","🧙","🥷","🦸","🎩","🌟","🔥","⚡","🎯"];
const ENG_LEVELS = [
  {code:"A1", label:"A1 — Начинающий"},
  {code:"A2", label:"A2 — Элементарный"},
  {code:"B1", label:"B1 — Средний"},
  {code:"B2", label:"B2 — Выше среднего"},
  {code:"C1", label:"C1 — Продвинутый"},
  {code:"C2", label:"C2 — Свободный"},
];

let profileData = { name:"", avatar:"🦁", goal:"", engLevel:"", bio:"", avatarPhoto: null, frame: "frame_blue" };
try { profileData = Object.assign(profileData, JSON.parse(localStorage.getItem("etu_profile")) || {}); } catch(e){}
// Load photo separately (stored outside JSON due to size)
try { const _ph = localStorage.getItem("etu_avatarPhoto"); if (_ph) profileData.avatarPhoto = _ph; } catch(e){}

function loadProfile() {
  // Apply name to main menu
  if (profileData.name) {
    document.querySelector(".menu-name").textContent = profileData.name;
  }
  _applyAvatarToEl(document.getElementById("menuAvatar"), profileData.avatar, profileData.avatarPhoto);
}

function _applyAvatarToEl(el, emoji, photoData) {
  if (!el) return;
  if (photoData) {
    el.innerHTML = `<img src="${photoData}" alt="avatar">`;
  } else {
    el.innerHTML = '';
    el.textContent = emoji || '🦁';
  }
}

function showProfileScreen() {
  playClick();
  renderActivityCalendar();
  // Fill fields
  document.getElementById("profileName").value = profileData.name || "";
  document.getElementById("profileGoal").value = profileData.goal || "";
  document.getElementById("profileBio").value = profileData.bio || "";
  // Apply avatar (photo or emoji)
  _applyAvatarToEl(document.getElementById("profileAvatarBig"), profileData.avatar, profileData.avatarPhoto);
  // Show/hide delete photo button
  const delBtn = document.getElementById("avatarPhotoDelBtn");
  if (delBtn) delBtn.style.display = profileData.avatarPhoto ? "inline-flex" : "none";
  document.getElementById("bioCounter").textContent = (profileData.bio||"").length + " / 120";

  // Stats
  document.getElementById("pStXp").textContent = xp;
  document.getElementById("pStLvl").textContent = getLevel(xp);
  document.getElementById("pStStreak").textContent = maxStreak;
  document.getElementById("pRankLabel").textContent = getRankObj(xp).label;
  const pPct = _getLevelProgress(xp);
  const pBar = document.getElementById("pProfileBar");
  const pTxt = document.getElementById("pProfileBarTxt");
  const _xpCur = _xpInCurrentLevel(xp);
  const _xpNeed = _xpNeededForNextLevel(xp);
  if (pBar) pBar.style.width = pPct + "%";
  if (pTxt) pTxt.textContent = _xpCur + " / " + _xpNeed + " XP";

  // Level picker
  const lp = document.getElementById("levelPicker");
  lp.innerHTML = ENG_LEVELS.map(l => `
    <div data-code="${l.code}"
      style="padding:10px 6px;border-radius:12px;border:2px solid ${profileData.engLevel===l.code?'var(--accent)':'var(--card-border)'};
      background:${profileData.engLevel===l.code?'rgba(79,142,255,.15)':'var(--muted2)'};
      text-align:center;cursor:pointer;transition:all .2s;font-weight:800;font-size:13px;">
      <div style="font-size:18px;font-weight:900;color:var(--accent)">${l.code}</div>
      <div style="font-size:10px;color:var(--text2);font-weight:600;margin-top:2px;">${l.label.split('—')[1].trim()}</div>
    </div>`).join("");
  lp.querySelectorAll("div").forEach(d => {
    d.onclick = () => selectEngLevel(d.dataset.code);
  });

  // Avatar grid
  const ag = document.getElementById("avatarGrid");
  ag.innerHTML = AVATARS.map((a, i) => `
    <div data-idx="${i}"
      style="font-size:28px;text-align:center;padding:8px 4px;border-radius:12px;cursor:pointer;
      border:2px solid ${profileData.avatar===a?'var(--accent)':'transparent'};
      background:${profileData.avatar===a?'rgba(79,142,255,.15)':'var(--muted2)'};
      transition:all .2s;">${a}</div>`).join("");
  ag.querySelectorAll("div").forEach(d => {
    d.onclick = () => selectAvatar(AVATARS[parseInt(d.dataset.idx)]);
  });

  // Bio counter
  document.getElementById("profileBio").oninput = function() {
    document.getElementById("bioCounter").textContent = this.value.length + " / 120";
  };

  document.getElementById("dashboardCard").style.display = "none";
  showScreen("profileScreen");
  updateProfileRing();
}

function openAvatarPicker() {
  const p = document.getElementById("avatarPicker");
  p.style.display = p.style.display === "none" ? "block" : "none";
}

function selectAvatar(emoji) {
  profileData.avatar = emoji;
  profileData.avatarPhoto = null; // clear photo
  _applyAvatarToEl(document.getElementById("profileAvatarBig"), emoji, null);
  const delBtn = document.getElementById("avatarPhotoDelBtn");
  if (delBtn) delBtn.style.display = "none";
  // refresh grid highlight
  document.getElementById("avatarGrid").querySelectorAll("div").forEach(d => {
    const match = d.textContent === emoji;
    d.style.borderColor = match ? "var(--accent)" : "transparent";
    d.style.background = match ? "rgba(79,142,255,.15)" : "var(--muted2)";
  });
}

function handleAvatarPhoto(input) {
  const file = input.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = function(e) { openCropper(e.target.result); };
  reader.readAsDataURL(file);
  input.value = '';
}

function removePhotoAvatar() {
  profileData.avatarPhoto = null;
  localStorage.removeItem("etu_avatarPhoto");
  _applyAvatarToEl(document.getElementById("profileAvatarBig"), profileData.avatar, null);
  _applyAvatarToEl(document.getElementById("menuAvatar"), profileData.avatar, null);
  const delBtn = document.getElementById("avatarPhotoDelBtn");
  if (delBtn) delBtn.style.display = "none";
}

function selectEngLevel(code) {
  profileData.engLevel = code;
  document.getElementById("levelPicker").querySelectorAll("div[data-code]").forEach(d => {
    const isMe = d.dataset.code === code;
    d.style.borderColor = isMe ? "var(--accent)" : "var(--card-border)";
    d.style.background = isMe ? "rgba(79,142,255,.15)" : "var(--muted2)";
  });
}

function saveProfile() {
  profileData.name = document.getElementById("profileName").value.trim();
  profileData.goal = document.getElementById("profileGoal").value;
  profileData.bio  = document.getElementById("profileBio").value.trim();
  localStorage.setItem("etu_profile", JSON.stringify(profileData));

  // Update main menu name & avatar
  if (profileData.name) {
    document.querySelector(".menu-name").textContent = profileData.name;
  } else {
    document.querySelector(".menu-name").textContent = "English Trainer";
  }
  _applyAvatarToEl(document.getElementById("menuAvatar"), profileData.avatar, profileData.avatarPhoto);

  playCorrect();
  alertPop("✅ Профиль сохранён!");
  setTimeout(() => { showScreen("menuScreen"); }, 800);
}


// ════════════════════════════════════
//  СЕРИЯ ДНЕЙ (DAY STREAK)
// ════════════════════════════════════
const DS_KEY_LAST  = "etu_ds_lastDate";
const DS_KEY_CUR   = "etu_ds_current";
const DS_KEY_BEST  = "etu_ds_best";

function updateDayStreak() {
  const todayStr = new Date().toDateString();
  const yesterday = new Date(Date.now() - 86400000).toDateString();
  const lastDate = localStorage.getItem(DS_KEY_LAST) || "";
  let cur  = parseInt(localStorage.getItem(DS_KEY_CUR))  || 0;
  let best = parseInt(localStorage.getItem(DS_KEY_BEST)) || 0;

  if (lastDate === todayStr) {
    // уже занимались сегодня — ничего не меняем
  } else if (lastDate === yesterday) {
    // занимались вчера — streak продолжается
    cur++;
    localStorage.setItem(DS_KEY_LAST, todayStr);
    localStorage.setItem(DS_KEY_CUR, cur);
    if (cur > best) { best = cur; localStorage.setItem(DS_KEY_BEST, best); }
  } else {
    // пропуск — streak сбрасывается
    cur = 1;
    localStorage.setItem(DS_KEY_LAST, todayStr);
    localStorage.setItem(DS_KEY_CUR, cur);
    if (cur > best) { best = cur; localStorage.setItem(DS_KEY_BEST, best); }
  }
  renderDayStreak(cur, best);
  return { cur, best };
}

function renderDayStreak(cur, best) {
  const el = document.getElementById("dayStreakVal");
  const sub = document.getElementById("dayStreakSub");
  const bestEl = document.getElementById("dayStreakBest");
  if (!el) return;
  el.textContent = cur;
  bestEl.textContent = best;
  if (cur === 0) sub.textContent = "Начни сегодня!";
  else if (cur === 1) sub.textContent = "День 1 — отличное начало!";
  else if (cur < 7) sub.textContent = `${cur} дня подряд 💪`;
  else if (cur < 14) sub.textContent = `${cur} дней — продолжай!`;
  else if (cur < 30) sub.textContent = `${cur} дней — ты огонь! 🔥`;
  else sub.textContent = `${cur} дней — легендарно! ⚡`;
}

// ════════════════════════════════════
//  НЕДЕЛЬНЫЙ МИНИ-ГРАФИК
// ════════════════════════════════════
function renderWeeklyChart() {
  const chart = document.getElementById("weeklyChart");
  if (!chart) return;
  const days = ['Пн','Вт','Ср','Чт','Пт','Сб','Вс'];
  const today = new Date();
  const todayIdx = (today.getDay() + 6) % 7; // 0=Mon
  let data = [];
  try { data = JSON.parse(localStorage.getItem("etu_weekData")) || []; } catch(e){}
  // fill to 7 days
  while (data.length < 7) data.unshift(0);
  if (data.length > 7) data = data.slice(-7);

  const maxVal = Math.max(...data, 1);
  chart.innerHTML = data.map((v, i) => {
    const heightPct = Math.round((v / maxVal) * 100);
    const isToday = i === todayIdx;
    return `<div class="week-bar-wrap">
      <div class="week-bar${isToday ? ' today' : ''}" style="height:${Math.max(4, heightPct * 0.42)}px;" title="${v} XP"></div>
      <div class="week-label">${days[i]}</div>
    </div>`;
  }).join("");
}

function saveWeeklyData() {
  let data = [];
  try { data = JSON.parse(localStorage.getItem("etu_weekData")) || []; } catch(e){}
  while (data.length < 7) data.unshift(0);
  if (data.length > 7) data = data.slice(-7);
  const todayIdx = (new Date().getDay() + 6) % 7;
  data[todayIdx] = dailyXp;
  localStorage.setItem("etu_weekData", JSON.stringify(data));
}

// ════════════════════════════════════
//  CONFETTI
// ════════════════════════════════════
function spawnConfetti(count = 30) {
  const colors = ['#4f8eff','#7c5cfc','#22d88f','#ffc247','#ff4f6d','#a78bfa'];
  for (let i = 0; i < count; i++) {
    setTimeout(() => {
      const el = document.createElement('div');
      el.className = 'confetti-piece';
      el.style.left = (20 + Math.random() * 60) + 'vw';
      el.style.top = '20vh';
      el.style.background = colors[Math.floor(Math.random() * colors.length)];
      el.style.animationDelay = (Math.random() * .4) + 's';
      el.style.animationDuration = (.8 + Math.random() * .6) + 's';
      el.style.transform = `rotate(${Math.random()*360}deg)`;
      document.body.appendChild(el);
      setTimeout(() => el.remove(), 1500);
    }, i * 30);
  }
}

// ════════════════════════════════════
//  LEVEL UP BANNER
// ════════════════════════════════════
let _lastLevel = 0;
let pendingLevelUp = 0;
function checkLevelUp(prevXp, newXp) {
  const prev = getLevel(prevXp);
  const next = getLevel(newXp);
  if (next > prev && next > _lastLevel) {
    _lastLevel = next;
    if (isGameActive) {
      // Отложить баннер до выхода в главное меню
      pendingLevelUp = next;
    } else {
      showLevelUpBanner(next);
      spawnConfetti(50);
    }
  }
}

function showLevelUpBanner(lvl) {
  // Haptic on level up — triple pulse
  if (navigator.vibrate) navigator.vibrate([60, 40, 60, 40, 120]);
  const el = document.createElement('div');
  el.className = 'lvlup-banner';
  el.innerHTML = `⬆️ Уровень ${lvl}!<br><span style="font-size:14px;opacity:.85;font-weight:700;">Ты растёшь 🚀</span>`;
  document.body.appendChild(el);
  setTimeout(() => {
    el.style.transition = 'opacity .6s, transform .6s';
    el.style.opacity = '0';
    el.style.transform = 'translate(-50%,-60%) scale(.9)';
    setTimeout(() => el.remove(), 600);
  }, 2200);
}

// ════════════════════════════════════
//  НИЖНЯЯ НАВИГАЦИЯ
// ════════════════════════════════════
function navTo(tab) {
  ['navHome','navTrain','navStats','navAch','navProfile','navSettings'].forEach(id => {
    document.getElementById(id)?.classList.remove('active');
  });
  if (tab === 'home') {
    document.getElementById('navHome')?.classList.add('active');
    goToMainMenu();
  } else if (tab === 'train') {
    document.getElementById('navTrain')?.classList.add('active');
    showModeSelect();
  } else if (tab === 'stats') {
    document.getElementById('navStats')?.classList.add('active');
    showStatsScreen();
  } else if (tab === 'ach') {
    document.getElementById('navAch')?.classList.add('active');
    showAchScreen();
  } else if (tab === 'profile') {
    document.getElementById('navProfile')?.classList.add('active');
    showProfileScreen();
  } else if (tab === 'settings') {
    document.getElementById('navSettings')?.classList.add('active');
    showSettingsScreen();
  }
}

function updateBottomNav(screen) {
  ['navHome','navTrain','navStats','navAch','navProfile','navSettings'].forEach(id => {
    document.getElementById(id)?.classList.remove('active');
  });
  if (screen === 'menuScreen') document.getElementById('navHome')?.classList.add('active');
  else if (screen === 'modeSelectScreen' || screen === 'gameScreen') document.getElementById('navTrain')?.classList.add('active');
  else if (screen === 'statsScreen') document.getElementById('navStats')?.classList.add('active');
  else if (screen === 'dashboardCard') document.getElementById('navAch')?.classList.add('active');
  else if (screen === 'profileScreen') document.getElementById('navProfile')?.classList.add('active');
  else if (screen === 'settingsScreen') document.getElementById('navSettings')?.classList.add('active');
}

// updateBottomNav вызывается напрямую из showScreen (см. выше — патч снизу убран)

// ════════════════════════════════════
//  ПРОФИЛЬ — КОЛЬЦО XP
// ════════════════════════════════════
function updateProfileRing() {
  const ring = document.getElementById("profileRingFg");
  if (!ring) return;
  const pct = _getLevelProgress(xp) / 100;
  const circumference = 301.6;
  ring.style.strokeDashoffset = circumference * (1 - pct);

  // Color XP ring with frame's gradient
  const frameId = profileData.frame || 'frame_blue';
  const frameGradMap = {
    frame_none:    'url(#ringGrad)',
    frame_blue:    'url(#ringGrad)',
    frame_gold:    'url(#frameGradGold)',
    frame_fire:    'url(#frameGradFire)',
    frame_neon:    'url(#frameGradNeon)',
    frame_galaxy:  'url(#frameGradGalaxy)',
    frame_rainbow: 'url(#frameGradRainbow)',
    frame_diamond: 'url(#frameGradDiamond)',
  };
  const frameGlowMap = {
    frame_none:    '',
    frame_blue:    'drop-shadow(0 0 6px rgba(79,142,255,0.6))',
    frame_gold:    'drop-shadow(0 0 8px rgba(255,194,71,0.8))',
    frame_fire:    'drop-shadow(0 0 10px rgba(255,79,109,0.8))',
    frame_neon:    'drop-shadow(0 0 10px rgba(34,216,143,0.9))',
    frame_galaxy:  'drop-shadow(0 0 10px rgba(124,92,252,0.8))',
    frame_rainbow: 'drop-shadow(0 0 12px rgba(167,139,250,0.7))',
    frame_diamond: 'drop-shadow(0 0 12px rgba(116,192,252,0.9))',
  };
  ring.style.stroke = frameGradMap[frameId] ?? 'url(#ringGrad)';
  ring.style.filter = frameGlowMap[frameId] ?? '';

  // Hide the separate frame ring (not needed anymore)
  const frameRing = document.getElementById("profileFrameRing");
  if (frameRing) frameRing.setAttribute('stroke', 'transparent');
}

// ════════════════════════════════════
//  LEVEL UP интегрирован в onCorrect выше
// ════════════════════════════════════
// save() патч убран — вызов saveWeeklyData встроен в оригинальный save()

// Улучшенная renderAchievements с иконками и locked
function renderAchievements() {
  const c = document.getElementById("achievements");
  if (!Object.keys(achievementList).length) return;
  
  const items = Object.entries(achievementList).map(([id, text]) => {
    const earned = earnedAchievements.includes(id);
    return `<div class="ach-list-item${earned ? '' : ' locked'}">
      <span class="ach-list-icon">${text.split(' ')[0]}</span>
      <div class="ach-list-text">
        <div>${text.substring(text.indexOf(' ')+1)}</div>
        ${earned ? '<div class="ach-list-earned">✅ Получено</div>' : ''}
      </div>
      <span class="ach-check">${earned ? '✅' : '🔒'}</span>
    </div>`;
  });
  
  if (items.length === 0) {
    c.innerHTML = '<div class="ach-empty"><div class="ach-empty-icon">🏆</div><div class="ach-empty-text">Продолжай тренироваться!</div></div>';
  } else {
    c.innerHTML = items.join('');
  }
  const badge = document.getElementById("achCountBadge");
  if (badge) {
    badge.textContent = earnedAchievements.length ? earnedAchievements.length : '';
    badge.style.display = earnedAchievements.length ? 'inline' : 'none';
  }
}


// ════════════════════════════════════
//  НАСТРОЙКИ
// ════════════════════════════════════
function showAchScreen() {
  playClick();
  document.getElementById('dashboardCard').style.display = '';
  renderAchievements();
  renderMistakes();
  renderChest();
  showScreen('dashboardCard');
}

function showSettingsScreen() {
  playClick();
  // Sync sound toggle
  const wrap = document.getElementById('soundToggleWrap');
  if (wrap) wrap.classList.toggle('on', soundEnabled);

  // Sync theme picker
  const theme = localStorage.getItem('etu_theme') || 'dark';
  ['dark','light','auto'].forEach(t => {
    document.getElementById('themeOpt' + t.charAt(0).toUpperCase() + t.slice(1))?.classList.toggle('active', t === theme);
  });
  const labels = { dark: 'Тёмная тема активна', light: 'Светлая тема активна', auto: 'Авто (системная тема)' };
  const subEl = document.getElementById('themeSubLabel');
  if (subEl) subEl.textContent = labels[theme] || labels.dark;

  // Sync difficulty
  const d = DAILY_GOALS[dailyDifficulty];
  const diffSub = document.getElementById('settingsDiffSub');
  if (diffSub) diffSub.textContent = `${d.icon} ${d.name} — ${d.xp} XP/день`;

  // Sync selects
  const sm = document.getElementById('settingsMode');
  const sc = document.getElementById('settingsCategory');
  if (sm) sm.value = document.getElementById('mode').value;
  if (sc) sc.value = document.getElementById('category').value;

  document.getElementById('dashboardCard').style.display = 'none';
  showScreen('settingsScreen');
}

function toggleSoundSetting() {
  unlockAudio();
  soundEnabled = !soundEnabled;
  localStorage.setItem('soundEnabled', soundEnabled);
  // Sync hidden soundBtn (для совместимости)
  const btn = document.getElementById('soundBtn');
  if (btn) btn.textContent = soundEnabled ? '🔊' : '🔇';
  // Update toggle UI
  const wrap = document.getElementById('soundToggleWrap');
  if (wrap) wrap.classList.toggle('on', soundEnabled);
  if (soundEnabled) playClick();
}

function _syncThemeColor() {
  const isLight = document.body.classList.contains('light');
  const color = isLight ? '#f0f4ff' : '#080c14';
  let meta = document.getElementById('metaThemeColor');
  if (!meta) {
    meta = document.createElement('meta');
    meta.name = 'theme-color';
    meta.id = 'metaThemeColor';
    document.head.appendChild(meta);
  }
  meta.setAttribute('content', color);
}

function setTheme(theme) {
  playClick();
  if (theme === 'dark') {
    document.body.classList.remove('light');
    localStorage.setItem('etu_theme', 'dark');
  } else if (theme === 'light') {
    document.body.classList.add('light');
    localStorage.setItem('etu_theme', 'light');
  } else {
    // auto — follow system
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    document.body.classList.toggle('light', !prefersDark);
    localStorage.setItem('etu_theme', 'auto');
  }
  _syncThemeColor();
  // Update labels and active states
  const labels = { dark: 'Тёмная тема активна', light: 'Светлая тема активна', auto: 'Авто (системная тема)' };
  const subEl = document.getElementById('themeSubLabel');
  if (subEl) subEl.textContent = labels[theme];
  const themeBtn = document.getElementById('themeBtn');
  if (themeBtn) themeBtn.textContent = document.body.classList.contains('light') ? '☀️' : '🌙';
  ['dark','light','auto'].forEach(t => {
    document.getElementById('themeOpt' + t.charAt(0).toUpperCase() + t.slice(1))?.classList.toggle('active', t === theme);
  });
}

// ════════════════════════════════════
//  ЖИЗНИ (HEARTS)
// ════════════════════════════════════
function updateLivesDisplay() {
  const el = document.getElementById("livesDisplay");
  if (!el) return;
  let html = '';
  for (let i = 0; i < MAX_LIVES; i++) {
    html += `<span class="heart${i >= lives ? ' lost' : ''}">❤️</span>`;
  }
  el.innerHTML = html;
}

// ════════════════════════════════════
//  БОНУС-РАУНД
// ════════════════════════════════════
function activateBonusRound() {
  bonusRoundActive = true;
  bonusRoundTimer = BONUS_DURATION;
  document.getElementById("bonusActiveBadge").style.display = "flex";
  spawnConfetti(20);

  clearInterval(bonusInterval);
  bonusInterval = setInterval(() => {
    bonusRoundTimer--;
    const badge = document.querySelector(".bonus-active-badge");
    if (badge) badge.textContent = `⚡ ×2 XP (${bonusRoundTimer}с)`;
    if (bonusRoundTimer <= 0) stopBonusRound();
  }, 1000);
}
function stopBonusRound() {
  bonusRoundActive = false;
  clearInterval(bonusInterval);
  const el = document.getElementById("bonusActiveBadge");
  if (el) el.style.display = "none";
}

// ════════════════════════════════════
//  СОХРАНЕНИЕ ПРОГРЕССА СПРИНТА
// ════════════════════════════════════
function saveSprintState() {
  if (!isSprint || !isGameActive) return;
  const pool = getPool();
  const state = {
    isSprint, sprintLen, sprintIdx, sprintCorrect, sprintErrors,
    sprintXpEarned, sprintMaxStreak, livesMode, lives,
    currentWordEn: currentWordObj ? currentWordObj.en : null,
    currentWordRu: currentWordObj ? currentWordObj.ru : null,
    currentLanguage, correctAnswerString,
    gameMode, streak,
    ts: Date.now()
  };
  try { localStorage.setItem('etu_sprint_state', JSON.stringify(state)); } catch(e){}
}
function loadSprintState() {
  try {
    const raw = localStorage.getItem('etu_sprint_state');
    if (!raw) return null;
    const s = JSON.parse(raw);
    // Expire after 2 hours
    if (Date.now() - s.ts > 2 * 3600 * 1000) { clearSprintState(); return null; }
    return s;
  } catch(e){ return null; }
}
function clearSprintState() {
  localStorage.removeItem('etu_sprint_state');
}
function checkResumeSprint() {
  const s = loadSprintState();
  if (!s) return;
  const minutesAgo = Math.round((Date.now() - s.ts) / 60000);
  shopConfirm(
    `▶ Найден незавершённый спринт<br><span style="font-size:14px;font-weight:600;">${minutesAgo} мин. назад — ${s.sprintIdx} из ${s.sprintLen} слов пройдено</span>`,
    () => resumeSprintFromState(s),
    '▶ Продолжить'
  );
  // patch cancel to also clear state
  setTimeout(() => {
    const noBtn = document.getElementById('shopConfirmNo');
    if (noBtn) { const orig = noBtn.onclick; noBtn.onclick = () => { clearSprintState(); if(orig) orig(); }; }
  }, 0);
}
function resumeSprintFromState(s) {
  playStart();
  isSprint = true; sprintLen = s.sprintLen; sprintIdx = s.sprintIdx;
  sprintCorrect = s.sprintCorrect; sprintErrors = s.sprintErrors;
  sprintXpEarned = s.sprintXpEarned; sprintMaxStreak = s.sprintMaxStreak;
  livesMode = s.livesMode; lives = s.lives; streak = s.streak;
  isGameActive = true;
  if (livesMode) { updateLivesDisplay(); document.getElementById("livesBar").style.display="flex"; }
  document.getElementById("sprintCounterRow").style.display = "block";
  document.getElementById("sprintProgWrap").style.display = "block";
  document.getElementById("dashboardCard").style.display = "none";
  showScreen("gameScreen"); nextWord();
  alertPop(`▶ Спринт возобновлён! Осталось ${sprintLen - sprintIdx} слов`);
}

// ════════════════════════════════════
//  СЛОВО ДНЯ
// ════════════════════════════════════
const WOD_DATA = [
  { word:"persevere", pron:"[ˌpɜːsɪˈvɪər]", ru:"настойчиво продолжать", sentence:"You must persevere if you want to achieve your goals." },
  { word:"diligent",  pron:"[ˈdɪlɪdʒənt]", ru:"прилежный, усердный",   sentence:"She is a diligent student who never misses a class." },
  { word:"eloquent",  pron:"[ˈeləkwənt]",  ru:"красноречивый",          sentence:"His eloquent speech moved the entire audience." },
  { word:"resilient", pron:"[rɪˈzɪliənt]", ru:"стойкий, упругий",       sentence:"Children are often more resilient than adults think." },
  { word:"ambiguous", pron:"[æmˈbɪɡjuəs]", ru:"двусмысленный",          sentence:"The contract contained several ambiguous clauses." },
  { word:"concise",   pron:"[kənˈsaɪs]",   ru:"краткий, лаконичный",    sentence:"Please be concise — we have only five minutes left." },
  { word:"meticulous",pron:"[məˈtɪkjələs]",ru:"тщательный, дотошный",   sentence:"The surgeon was meticulous in every step of the procedure." },
  { word:"steadfast", pron:"[ˈstedfɑːst]", ru:"твёрдый, верный",        sentence:"She remained steadfast in her belief despite the criticism." },
  { word:"astute",    pron:"[əˈstjuːt]",   ru:"проницательный, умный",  sentence:"An astute investor knows when to wait." },
  { word:"candid",    pron:"[ˈkændɪd]",    ru:"откровенный, искренний", sentence:"I appreciate your candid feedback." },
  { word:"empathy",   pron:"[ˈempəθi]",    ru:"сочувствие, эмпатия",    sentence:"Good leaders show empathy towards their team." },
  { word:"frugal",    pron:"[ˈfruːɡəl]",   ru:"бережливый, экономный",  sentence:"Living a frugal lifestyle helped him save money." },
  { word:"gratitude", pron:"[ˈɡrætɪtjuːd]",ru:"благодарность",         sentence:"She expressed gratitude for all the help she received." },
  { word:"humble",    pron:"[ˈhʌmbəl]",    ru:"скромный, смиренный",    sentence:"Despite his success, he remained humble." },
  { word:"integrity", pron:"[ɪnˈteɡrəti]", ru:"честность, цельность",   sentence:"Integrity is the foundation of trust in any relationship." },
  { word:"curious",   pron:"[ˈkjʊəriəs]",  ru:"любопытный",             sentence:"Curious minds learn the fastest." },
  { word:"passion",   pron:"[ˈpæʃən]",     ru:"страсть, увлечение",     sentence:"Follow your passion and success will follow." },
  { word:"subtle",    pron:"[ˈsʌtəl]",     ru:"тонкий, едва заметный",  sentence:"There is a subtle difference between the two options." },
  { word:"thrive",    pron:"[θraɪv]",       ru:"процветать, расцветать", sentence:"Plants thrive in sunlight and water." },
  { word:"vivid",     pron:"[ˈvɪvɪd]",     ru:"яркий, живой",           sentence:"She has vivid memories of her childhood." },
  { word:"eager",     pron:"[ˈiːɡər]",     ru:"нетерпеливый, горящий",  sentence:"The eager students raised their hands." },
  { word:"genuine",   pron:"[ˈdʒenjuɪn]",  ru:"настоящий, искренний",   sentence:"His smile was genuine and warm." },
  { word:"flourish",  pron:"[ˈflʌrɪʃ]",   ru:"процветать, развиваться",sentence:"Creativity flourishes in a supportive environment." },
  { word:"observe",   pron:"[əbˈzɜːv]",    ru:"наблюдать, замечать",     sentence:"Scientists observe changes in the data carefully." },
  { word:"triumph",   pron:"[ˈtraɪʌmf]",  ru:"триумф, победа",          sentence:"Their team's triumph was celebrated by thousands." },
  { word:"venture",   pron:"[ˈventʃər]",  ru:"рисковать, предприятие",  sentence:"She decided to venture into unknown territory." },
  { word:"wisdom",    pron:"[ˈwɪzdəm]",   ru:"мудрость",                sentence:"Wisdom comes with experience and reflection." },
  { word:"yearn",     pron:"[jɜːn]",       ru:"стремиться, тосковать",   sentence:"He yearned for adventure after years in the office." },
  { word:"zeal",      pron:"[ziːl]",       ru:"рвение, энтузиазм",       sentence:"She approached every task with great zeal." },
  { word:"adapt",     pron:"[əˈdæpt]",     ru:"адаптировать, приспосабливаться",sentence:"We must adapt to survive in a changing world." },
  { word:"brilliant", pron:"[ˈbrɪliənt]", ru:"блестящий, великолепный", sentence:"It was a brilliant solution to a complex problem." },
];
function initWordOfDay() {
  const dayIdx = Math.floor(Date.now() / 86400000) % WOD_DATA.length;
  const w = WOD_DATA[dayIdx];
  document.getElementById("wodWord").textContent = w.word;
  document.getElementById("wodPron").textContent = w.pron;
  document.getElementById("wodTranslation").textContent = "🇷🇺 " + w.ru;
  document.getElementById("wodSentence").textContent = '"' + w.sentence + '"';
  const key = "etu_wod_known_" + new Date().toDateString();
  const known = localStorage.getItem(key) === "1";
  const btn = document.getElementById("wodKnowBtn");
  if (known) { btn.textContent = "✅ Уже запомнил!"; btn.classList.add("known"); }
  else { btn.textContent = "✅ Запомнил!"; btn.classList.remove("known"); }
}
function wodMarkKnown() {
  const key = "etu_wod_known_" + new Date().toDateString();
  localStorage.setItem(key, "1");
  const btn = document.getElementById("wodKnowBtn");
  btn.textContent = "✅ Уже запомнил!"; btn.classList.add("known");
  xp += 5; dailyXp += 5; save(); updateMenu();
  spawnConfetti(15); playCorrect();
  alertPop("🌟 +5 XP за слово дня!");
  claimChallengeXp();
}
function wodSpeak() {
  unlockAudio();
  const w = document.getElementById("wodWord").textContent;
  if (w && w !== "—") speak(w);
}

// ════════════════════════════════════
//  ПОИСК ПО СЛОВАРЮ
// ════════════════════════════════════
function openDictSearch() {
  playClick();
  document.getElementById("dictOverlay").classList.add("active");
  setTimeout(()=>document.getElementById("dictSearchInput").focus(), 150);
}
function closeDictSearch() {
  document.getElementById("dictOverlay").classList.remove("active");
}
function clearDictSearch() {
  document.getElementById("dictSearchInput").value = "";
  dictSearch("");
}
function dictSearch(q) {
  const resultsEl = document.getElementById("dictResults");
  const countEl = document.getElementById("dictCount");
  q = q.trim().toLowerCase();
  if (!q) {
    resultsEl.innerHTML = '<div class="dict-no-result">Начни вводить слово для поиска</div>';
    countEl.textContent = "—";
    return;
  }
  const catLabels = {
    jobs:"💼 Профессии", workplaces:"🏭 Места работы", dailyRoutines:"🌅 Распорядок",
    activities:"📅 Занятия", aroundTown:"🏙️ Город", directions:"🗺️ Ориентирование", adjectives:"🎨 Прилагательные", scenery:"🌄 Природа", aroundHouse:"🏠 Дом", myWords:"⭐ Мои слова"
  };
  let results = [];
  Object.entries(categories).forEach(([cat, words]) => {
    if (!Array.isArray(words)) return;
    words.forEach(w => {
      const matchRu = w.ru && w.ru.toLowerCase().includes(q);
      const matchEn = w.en && w.en.toLowerCase().includes(q);
      if (matchRu || matchEn) results.push({...w, cat});
    });
  });
  countEl.textContent = results.length;
  if (!results.length) {
    resultsEl.innerHTML = `<div class="dict-no-result">Ничего не найдено по «${q}»</div>`;
    return;
  }
  resultsEl.innerHTML = results.slice(0,30).map(r => `
    <div class="dict-result-item">
      <div>
        <div style="font-weight:800;color:var(--text);">${r.en}</div>
        <div class="dict-result-ru">${r.ru}</div>
      </div>
      <span class="dict-result-cat">${catLabels[r.cat]||r.cat}</span>
    </div>`).join("");
}

// ════════════════════════════════════
//  ЭКСПОРТ ПРОГРЕССА — СЕРТИФИКАТ
// ════════════════════════════════════
function exportProgress() {
  playClick();
  const level = getLevel(xp);
  const rank  = getRankObj(xp).label;
  const acc   = total ? Math.round(correct / total * 100) : 0;
  const name  = profileData.name || "Тренер";
  const avatar= profileData.avatar || "🦁";
  const date  = new Date().toLocaleDateString('ru-RU', {day:'numeric', month:'long', year:'numeric'});
  const achCount = earnedAchievements.length;

  const html = `<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Сертификат — ${name}</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap');
  *{box-sizing:border-box;margin:0;padding:0;}
  body{
    font-family:'Inter',system-ui,sans-serif;
    background:linear-gradient(135deg,#080c14 0%,#0d1525 50%,#080c14 100%);
    min-height:100vh; display:flex; align-items:center; justify-content:center;
    padding:16px;
  }
  .cert{
    background:linear-gradient(145deg,rgba(255,255,255,.07),rgba(255,255,255,.03));
    border:1px solid rgba(255,255,255,.12);
    border-radius:24px; padding:36px 24px; max-width:520px; width:100%;
    box-shadow:0 40px 80px rgba(0,0,0,.6), inset 0 1px 0 rgba(255,255,255,.1);
    position:relative; overflow:hidden; text-align:center;
  }
  .cert::before{
    content:''; position:absolute; top:-120px; left:-80px;
    width:400px; height:400px; border-radius:50%;
    background:radial-gradient(circle,rgba(79,142,255,.18),transparent 70%);
    pointer-events:none;
  }
  .cert::after{
    content:''; position:absolute; bottom:-100px; right:-60px;
    width:320px; height:320px; border-radius:50%;
    background:radial-gradient(circle,rgba(124,92,252,.15),transparent 70%);
    pointer-events:none;
  }
  .logo-line{
    font-size:10px; font-weight:800; letter-spacing:3px; text-transform:uppercase;
    color:rgba(255,255,255,.35); margin-bottom:20px;
  }
  .badge-wrap{
    display:inline-flex; align-items:center; gap:8px;
    background:linear-gradient(135deg,rgba(79,142,255,.15),rgba(124,92,252,.1));
    border:1px solid rgba(79,142,255,.3); border-radius:40px;
    padding:6px 16px; margin-bottom:20px; font-size:12px; font-weight:800;
    color:#a78bfa; letter-spacing:.5px; text-transform:uppercase;
  }
  .avatar-big{font-size:64px; display:block; margin-bottom:10px; line-height:1;}
  .cert-name{
    font-size:30px; font-weight:900; color:#fff;
    background:linear-gradient(135deg,#f0f4ff,#a78bfa);
    -webkit-background-clip:text; -webkit-text-fill-color:transparent;
    margin-bottom:8px; letter-spacing:-.5px; word-break:break-word;
  }
  .cert-sub{font-size:13px; color:rgba(255,255,255,.45); margin-bottom:24px; font-weight:600; line-height:1.5;}
  .divider{
    height:1px; background:linear-gradient(90deg,transparent,rgba(255,255,255,.12),transparent);
    margin:20px 0;
  }
  .stats-grid{
    display:grid; grid-template-columns:repeat(2,1fr); gap:10px; margin-bottom:20px;
  }
  .stat-box{
    background:rgba(255,255,255,.05); border:1px solid rgba(255,255,255,.08);
    border-radius:14px; padding:14px 8px;
  }
  .stat-val{
    font-size:26px; font-weight:900;
    background:linear-gradient(135deg,#4f8eff,#a78bfa);
    -webkit-background-clip:text; -webkit-text-fill-color:transparent;
    margin-bottom:4px;
  }
  .stat-label{font-size:10px; font-weight:700; color:rgba(255,255,255,.35); text-transform:uppercase; letter-spacing:.6px;}
  .rank-box{
    background:linear-gradient(135deg,rgba(255,194,71,.12),rgba(255,140,50,.08));
    border:1px solid rgba(255,194,71,.3); border-radius:14px;
    padding:14px 16px; margin-bottom:20px;
    display:flex; align-items:center; justify-content:center; gap:10px;
  }
  .rank-label{font-size:17px; font-weight:900; color:#ffc247;}
  .ach-line{
    font-size:13px; color:rgba(255,255,255,.4); font-weight:600; margin-bottom:24px; line-height:1.5;
  }
  .ach-line b{color:#22d88f;}
  .seal{
    display:inline-flex; align-items:center; justify-content:center; flex-direction:column;
    width:80px; height:80px; border-radius:50%;
    background:linear-gradient(135deg,#4f8eff,#7c5cfc);
    box-shadow:0 0 0 6px rgba(79,142,255,.15), 0 0 30px rgba(79,142,255,.3);
    margin-bottom:14px;
  }
  .seal-icon{font-size:32px;}
  .date-line{font-size:11px; color:rgba(255,255,255,.25); font-weight:600; letter-spacing:.5px;}
  @media print{
    body{background:#080c14!important; -webkit-print-color-adjust:exact; print-color-adjust:exact;}
  }
</style>
</head>
<body>
<div class="cert">
  <div class="logo-line">English Trainer Ultimate · ETU by BV</div>
  <div class="badge-wrap">🏅 Сертификат прогресса</div>
  <span class="avatar-big">${avatar}</span>
  <div class="cert-name">${name}</div>
  <div class="cert-sub">достиг уровня ${level} и продолжает учиться</div>

  <div class="divider"></div>

  <div class="stats-grid">
    <div class="stat-box">
      <div class="stat-val">${xp}</div>
      <div class="stat-label">⭐ Всего XP</div>
    </div>
    <div class="stat-box">
      <div class="stat-val">${level}</div>
      <div class="stat-label">📈 Уровень</div>
    </div>
    <div class="stat-box">
      <div class="stat-val">${acc}%</div>
      <div class="stat-label">🎯 Точность</div>
    </div>
    <div class="stat-box">
      <div class="stat-val">${correct}</div>
      <div class="stat-label">✅ Правильно</div>
    </div>
    <div class="stat-box">
      <div class="stat-val">${maxStreak}</div>
      <div class="stat-label">🔥 Макс. серия</div>
    </div>
    <div class="stat-box">
      <div class="stat-val">${achCount}</div>
      <div class="stat-label">🏆 Достижений</div>
    </div>
  </div>

  <div class="rank-box">
    <div class="rank-label">${rank}</div>
  </div>

  <div class="ach-line">Разблокировано достижений: <b>${achCount} из ${Object.keys(achievementList).length}</b></div>

  <div class="seal"><span class="seal-icon">✦</span></div>
  <div class="date-line">Выдан ${date} · ETU v1.0</div>
</div>
<style>
#pwaGate {
  position: fixed; inset: 0; z-index: 999999;
  background: #080c14;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  padding: 32px 24px; text-align: center;
  font-family: 'Segoe UI', system-ui, sans-serif;
  transition: opacity 0.3s;
}
#pwaGate.hidden { opacity: 0; pointer-events: none; }
.gate-icon { font-size: 80px; margin-bottom: 24px; animation: gp 2s ease-in-out infinite; }
@keyframes gp { 0%,100%{transform:scale(1)} 50%{transform:scale(1.1)} }
.gate-title { font-size: 26px; font-weight: 900; color: #f0f4ff; margin-bottom: 10px; }
.gate-sub { font-size: 15px; color: #8899bb; line-height: 1.6; margin-bottom: 28px; max-width: 280px; }
.gate-box {
  background: rgba(255,255,255,0.05); border: 1px solid rgba(255,255,255,0.1);
  border-radius: 20px; padding: 22px; width: 100%; max-width: 320px; margin-bottom: 24px; text-align: left;
}
.gate-step { display: flex; align-items: flex-start; gap: 12px; margin-bottom: 16px; }
.gate-step:last-child { margin-bottom: 0; }
.gate-num {
  background: linear-gradient(135deg,#4f8eff,#7c5cfc); color:#fff;
  font-size: 12px; font-weight: 900; width: 26px; height: 26px; border-radius: 8px;
  display: flex; align-items: center; justify-content: center; flex-shrink: 0;
}
.gate-step-text { font-size: 14px; color: #ccd6f6; line-height: 1.5; padding-top: 3px; }
.gate-btn {
  background: linear-gradient(135deg,#4f8eff,#7c5cfc);
  border: none; border-radius: 16px; padding: 17px 0;
  color: #fff; font-size: 17px; font-weight: 900;
  cursor: pointer; width: 100%; max-width: 320px;
  box-shadow: 0 8px 30px rgba(79,142,255,0.45);
}
.gate-note { margin-top: 18px; font-size: 12px; color: #445577; max-width: 280px; line-height: 1.6; }
</style>

<div id="pwaGate">
  <div class="gate-icon">📲</div>
  <div class="gate-title">Установи приложение</div>
  <div class="gate-sub">English Trainer Ultimate работает только как установленное приложение</div>

  <!-- iOS инструкция (показывается всегда на iOS) -->
  <div id="gateIosBox" class="gate-box" style="display:none">
    <div class="gate-step">
      <div class="gate-num">1</div>
      <div class="gate-step-text">Нажми кнопку <strong style="color:#f0f4ff">«Поделиться»</strong> (□↑) внизу Safari</div>
    </div>
    <div class="gate-step">
      <div class="gate-num">2</div>
      <div class="gate-step-text">Выбери <strong style="color:#f0f4ff">«На экран «Домой»»</strong></div>
    </div>
    <div class="gate-step">
      <div class="gate-num">3</div>
      <div class="gate-step-text">Нажми <strong style="color:#f0f4ff">«Добавить»</strong></div>
    </div>
    <div class="gate-step">
      <div class="gate-num">4</div>
      <div class="gate-step-text">Открой приложение <strong style="color:#f0f4ff">с главного экрана</strong></div>
    </div>
  </div>

  <!-- Android кнопка -->
  <div id="gateAndroidBox" style="display:none; width:100%; max-width:320px;">
    <button class="gate-btn" id="gateBtn" onclick="gateInstall()">📲 Установить приложение</button>
    <div id="gateWait" style="display:none;margin-top:14px;font-size:13px;color:#8899bb">⏳ Подожди секунду...</div>
  </div>

  <div class="gate-note" id="gateNote"></div>
</div>

<script>
(function() {
  // Определяем платформу
  var isIos = /iphone|ipad|ipod/i.test(navigator.userAgent) && !window.MSStream;
  var isAndroid = /android/i.test(navigator.userAgent);

  // Проверяем standalone
  var isStandalone = false;
  if (isIos) {
    isStandalone = (window.navigator.standalone === true);
  } else {
    try {
      isStandalone = window.matchMedia('(display-mode: standalone)').matches
        || window.matchMedia('(display-mode: fullscreen)').matches;
    } catch(e) {}
  }

  var gate = document.getElementById('pwaGate');

  if (isStandalone) {
    // Запущено как PWA — скрываем гейт
    gate.style.display = 'none';
    return;
  }

  // Показываем нужную инструкцию
  if (isIos) {
    document.getElementById('gateIosBox').style.display = 'block';
    document.getElementById('gateNote').textContent = 'После добавления открывай только с главного экрана 🏠';
  } else if (isAndroid) {
    document.getElementById('gateAndroidBox').style.display = 'block';
    document.getElementById('gateNote').textContent = 'После установки открывай с главного экрана 🏠';
  } else {
    // Десктоп — просто показываем сообщение
    document.getElementById('gateNote').innerHTML = 'Приложение предназначено для мобильных устройств.<br>Открой на iPhone или Android.';
  }

  // Перехватываем промпт установки (Android Chrome)
  window._pwaPrompt = null;
  window.addEventListener('beforeinstallprompt', function(e) {
    e.preventDefault();
    window._pwaPrompt = e;
    document.getElementById('gateAndroidBox').style.display = 'block';
  });

  window.addEventListener('appinstalled', function() {
    gate.style.display = 'none';
  });

  // Блокируем скролл под гейтом
  document.body.style.overflow = 'hidden';
})();

function gateInstall() {
  if (window._pwaPrompt) {
    document.getElementById('gateBtn').style.display = 'none';
    document.getElementById('gateWait').style.display = 'block';
    window._pwaPrompt.prompt();
    window._pwaPrompt.userChoice.then(function(r) {
      window._pwaPrompt = null;
      if (r.outcome !== 'accepted') {
        document.getElementById('gateBtn').style.display = 'block';
        document.getElementById('gateWait').style.display = 'none';
      }
    });
  } else {
    document.getElementById('gateWait').style.display = 'block';
    document.getElementById('gateWait').textContent = '⏳ Браузер ещё готовится. Попробуй через секунду.';
    setTimeout(function(){
      document.getElementById('gateBtn').style.display = 'block';
      document.getElementById('gateWait').style.display = 'none';
    }, 2500);
  }
}
</script>
</body>
</html>`;

  const blob = new Blob([html], {type:"text/html;charset=utf-8"});
  const url  = URL.createObjectURL(blob);
  const a    = document.createElement("a");
  a.href = url;
  a.download = `ETU_certificate_${name}_${new Date().toDateString().replace(/ /g,'_')}.html`;
  a.click();
  URL.revokeObjectURL(url);
  alertPop("🎓 Сертификат скачан!");
}

// ════════════════════════════════════
//  СТАТИСТИКА ПО КАТЕГОРИЯМ
// ════════════════════════════════════
function renderCatStats() {
  const el = document.getElementById("catStatsList");
  if (!el) return;
  let raw = {};
  try { raw = JSON.parse(localStorage.getItem("etu_cat_stats")) || {}; } catch(e){}
  const catLabels = {
    jobs:"💼 Профессии", workplaces:"🏭 Места работы", dailyRoutines:"🌅 Распорядок",
    activities:"📅 Занятия", aroundTown:"🏙️ Город", directions:"🗺️ Ориентирование", adjectives:"🎨 Прилагательные", scenery:"🌄 Природа", aroundHouse:"🏠 Дом", myWords:"⭐ Мои слова"
  };
  const entries = Object.entries(raw).filter(([k,v])=>(v.total||0)>=3)
    .sort((a,b)=>b[1].total-a[1].total);
  if (!entries.length) {
    el.innerHTML = '<div class="empty-state" style="padding:10px 0;font-size:13px;">Начни тренировку, чтобы увидеть статистику по категориям</div>';
    return;
  }
  // Find easiest/hardest by accuracy
  const byAcc = [...entries].sort((a,b) => (b[1].correct/b[1].total) - (a[1].correct/a[1].total));
  const easiestKey = byAcc[0][0];
  const hardestKey = byAcc[byAcc.length-1][0];
  const showBadges = byAcc.length >= 2 && (byAcc[0][1].correct/byAcc[0][1].total) !== (byAcc[byAcc.length-1][1].correct/byAcc[byAcc.length-1][1].total);

  el.innerHTML = entries.map(([cat, v]) => {
    const pct = v.total ? Math.round(v.correct / v.total * 100) : 0;
    const color = pct >= 80 ? 'var(--ok)' : pct >= 60 ? 'var(--accent)' : 'var(--warn)';
    let badge = '';
    if (showBadges && cat === easiestKey) badge = ' <span style="color:var(--ok);font-size:11px;font-weight:800;">✓ Легче всех</span>';
    else if (showBadges && cat === hardestKey) badge = ' <span style="color:var(--err);font-size:11px;font-weight:800;">⚠ Сложнее всех</span>';
    return `<div class="cat-stat-row">
      <div class="cat-stat-label">${catLabels[cat]||cat}${badge}</div>
      <div class="cat-stat-bar-wrap"><div class="cat-stat-bar" style="width:${pct}%;background:linear-gradient(90deg,${color},var(--accent));"></div></div>
      <div class="cat-stat-pct" style="color:${color};">${pct}%</div>
    </div>`;
  }).join("");
}
function trackCatStat(correct) {
  const cat = document.getElementById("category").value || "all";
  if (cat === "all") return; // don't track "all" as a category
  let raw = {};
  try { raw = JSON.parse(localStorage.getItem("etu_cat_stats")) || {}; } catch(e){}
  if (!raw[cat]) raw[cat] = {correct:0, total:0};
  raw[cat].total++;
  if (correct) raw[cat].correct++;
  localStorage.setItem("etu_cat_stats", JSON.stringify(raw));
}

// ════════════════════════════════════
//  ТЬЮТОРИАЛ
// ════════════════════════════════════
function showTutorial() {
  document.getElementById("tutorialOverlay").style.display = "flex";
  ['tutStep1','tutStep2','tutStep3'].forEach((id,i)=>{
    document.getElementById(id).style.display = i===0?"block":"none";
  });
  // Персональное приветствие по имени
  const greetEl = document.getElementById("tutGreetTitle");
  if (greetEl && profileData.name) {
    greetEl.textContent = `${profileData.name}, тебе покажут слово`;
  } else if (greetEl) {
    greetEl.textContent = "Тебе покажут слово";
  }
}
function tutNext(step) {
  playClick();
  ['tutStep1','tutStep2','tutStep3'].forEach((id,i)=>{ document.getElementById(id).style.display="none"; });
  document.getElementById("tutStep"+step).style.display = "block";
}
function tutFinish() {
  playCorrect();
  localStorage.setItem("etu_tutorial_done","1");
  document.getElementById("tutorialOverlay").style.display = "none";
  showModeSelect();
}

// ════════════════════════════════════
//  RESULTS — REPLAY CONTEXT-AWARE
// ════════════════════════════════════
let _lastResultMode = 'sprint'; // 'sprint' | 'anagram'
function resultsPlayAgain() {
  if (_lastResultMode === 'anagram') startAnagram();
  else if (_lastResultMode === 'dictant') startDictant();
  else startSprint();
}

// ════════════════════════════════════
//  БОНУС-РАУНД — ЗВУК
// ════════════════════════════════════
function playBonusSound() {
  if (!soundEnabled || !audioCtx) return;
  // Ascending arpeggio + crash
  const freqs = [261,329,392,523,659,784,1047];
  freqs.forEach((f,i) => setTimeout(()=>beep(f,'sine',.18,.1),i*60));
  setTimeout(()=>{
    beep(1047,'square',.3,.12);
    beep(880,'square',.3,.1);
  }, freqs.length*60);
}


function startMistakeRetry() {
  if (!mistakes.length) { alertPop("Нет ошибок для повторения!"); return; }
  playStart();
  isMistakeRetry = true;
  isSprint = true;
  livesMode = false;
  stopBonusRound();
  document.getElementById("livesBar").style.display = "none";

  // Парсим mistakes в пары слов
  mistakeRetryPool = mistakes.map(m => {
    const parts = m.split(" — ");
    return { ru: parts[0]||'', en: parts[1]||'' };
  }).filter(w => w.ru && w.en);

  if (!mistakeRetryPool.length) { alertPop("Нет слов для повторения!"); return; }
  mistakeRetryPool = mistakeRetryPool.sort(()=>Math.random()-.5);
  mistakeRetryIdx = 0;
  sprintLen = mistakeRetryPool.length;
  sprintIdx = 0; sprintCorrect=0; sprintErrors=0; sprintXpEarned=0; sprintMaxStreak=0;
  isGameActive = true;

  document.getElementById("sprintCounterRow").style.display="block";
  document.getElementById("sprintProgWrap").style.display="block";
  document.getElementById("dashboardCard").style.display="none";
  showScreen("gameScreen");
  nextMistakeRetryWord();
  alertPop(`💪 Повторяем ${mistakeRetryPool.length} слов!`);
}

function nextMistakeRetryWord() {
  if (mistakeRetryIdx >= mistakeRetryPool.length) {
    isMistakeRetry = false;
    showResults();
    return;
  }
  const w = mistakeRetryPool[mistakeRetryIdx];
  currentWordObj = w;
  currentLanguage = 'ru';
  correctAnswerString = w.en;
  document.getElementById("word").textContent = w.ru;
  document.getElementById("answer").value = "";
  document.getElementById("result").innerHTML = `<span class="retry-badge">💪 Режим повторения ошибок</span>`;
  document.getElementById("result").className = "result";
  document.getElementById("sprintCurrent").textContent = mistakeRetryIdx+1;
  document.getElementById("sprintTotal").textContent = mistakeRetryPool.length;
  document.getElementById("sprintBar").style.width = (mistakeRetryIdx/mistakeRetryPool.length*100)+"%";
  if (gameMode==='test') genOptions();
  isAnswering=false; toggleBtns(false); resetTimer();
}

// ════════════════════════════════════
//  СТАТИСТИКА СЛОЖНЫХ СЛОВ
// ════════════════════════════════════
function renderHardWords() {
  const section = document.getElementById("hardWordsSection");
  const list = document.getElementById("hardWordsList");
  if (!section || !list) return;

  const entries = Object.entries(wordErrorCount)
    .filter(([k,v]) => v > 0)
    .sort((a,b) => b[1]-a[1])
    .slice(0, 7);

  if (!entries.length) { section.style.display="none"; return; }
  section.style.display = "block";

  const maxErr = entries[0][1] || 1;
  list.innerHTML = entries.map(([word, count]) => {
    const pct = Math.round(count/maxErr*100);
    // Найти перевод
    const pool = getPool();
    const found = pool.find(w => w.en === word || w.en.split('/')[0].trim() === word);
    const ru = found ? found.ru : '?';
    return `<div class="hard-word-item">
      <div>
        <div style="font-weight:800;color:var(--text);">${word}</div>
        <div style="font-size:11px;color:var(--text2);">${ru}</div>
      </div>
      <div style="display:flex;align-items:center;gap:8px;">
        <div class="hard-word-bar-wrap"><div class="hard-word-bar" style="width:${pct}%"></div></div>
        <span class="hard-word-pct">${count}×</span>
      </div>
    </div>`;
  }).join("");
}

// ════════════════════════════════════
//  ОНБОРДИНГ
// ════════════════════════════════════
function initOnboarding() {
  const done = localStorage.getItem("etu_onboard_done");
  if (done) return;

  const overlay = document.getElementById("onboardOverlay");
  overlay.style.display = "flex";

  // Аватары
  const avatarRow = document.getElementById("onboardAvatarRow");
  const avatarList = ["🦁","🐯","🦊","🐺","🐻","🐼","🐨","🦝","🐸","🐙","🦋","👾"];
  avatarRow.innerHTML = avatarList.map(a =>
    `<button class="onboard-avatar-btn${a===onboardSelectedAvatar?' selected':''}" onclick="selectOnboardAvatar('${a}',this)">${a}</button>`
  ).join("");

  // Сложности
  const diffList = document.getElementById("onboardDiffList");
  diffList.innerHTML = Object.entries(DAILY_GOALS).map(([k,d]) =>
    `<div class="diff-option diff-${k}${k===onboardSelectedDiff?' selected':''}" onclick="selectOnboardDiff('${k}',this)" style="cursor:pointer;">
      <span class="diff-option-icon">${d.icon}</span>
      <div class="diff-option-info">
        <div class="diff-option-name">${d.name}</div>
        <div class="diff-option-desc">${d.xp} XP в день</div>
      </div>
    </div>`
  ).join("");
}

function selectOnboardAvatar(emoji, btn) {
  onboardSelectedAvatar = emoji;
  document.querySelectorAll(".onboard-avatar-btn").forEach(b=>b.classList.remove("selected"));
  btn.classList.add("selected");
  playClick();
}

function selectOnboardDiff(key, el) {
  onboardSelectedDiff = key;
  document.querySelectorAll("#onboardDiffList .diff-option").forEach(d=>d.classList.remove("selected"));
  el.classList.add("selected");
  playClick();
}

function onboardNext() {
  playClick();
  document.getElementById("onboardStep1").style.display="none";
  document.getElementById("onboardStep2").style.display="block";
}

function onboardFinish() {
  playCorrect();
  const name = document.getElementById("onboardName").value.trim();

  // Сохранить имя и аватар
  profileData.name = name || "Тренер";
  profileData.avatar = onboardSelectedAvatar;
  localStorage.setItem("etu_profile", JSON.stringify(profileData));

  // Сохранить сложность
  dailyDifficulty = onboardSelectedDiff;
  localStorage.setItem("etu_difficulty", dailyDifficulty);

  // Применить
  document.querySelector(".menu-name").textContent = profileData.name;
  _applyAvatarToEl(document.getElementById("menuAvatar"), profileData.avatar, profileData.avatarPhoto);
  updateMenu();

  localStorage.setItem("etu_onboard_done", "1");
  document.getElementById("onboardOverlay").style.display="none";
  spawnConfetti(40);
  alertPop(`Добро пожаловать, ${profileData.name}! 🎉`);
  // Show tutorial unless already done
  setTimeout(()=>{
    if (!localStorage.getItem("etu_tutorial_done")) showTutorial();
  }, 800);
}

// ════════════════════════════════════
//  ПЛАВНЫЙ ПЕРЕХОД МЕЖДУ ЭКРАНАМИ
// ════════════════════════════════════
let _lastNavTab = 0; // 0=home,1=stats,2=ach,3=settings,4=profile
const _tabOrder = { menuScreen:0, statsScreen:1, dashboardCard:2, settingsScreen:3, profileScreen:4 };

// ════════════════════════════════════
//  ИНИЦИАЛИЗАЦИЯ
// ════════════════════════════════════
//  РЕЖИМ «ДИКТАНТ»
// ════════════════════════════════════
let isDictant = false;
let dictantPool = [];
let dictantIdx = 0;
let dictantCorrect = 0;
let dictantErrors = 0;
let dictantXp = 0;
const DICTANT_LEN = 10;

function startDictant() {
  playStart();
  const pool = getPool().filter(w => w.en && w.en.trim());
  if (pool.length < 5) { alertPop("Мало слов для диктанта!"); return; }
  isDictant = true;
  dictantPool = pool.sort(() => Math.random() - .5).slice(0, DICTANT_LEN);
  dictantIdx = 0; dictantCorrect = 0; dictantErrors = 0; dictantXp = 0;
  document.getElementById("dashboardCard").style.display = "none";
  showScreen("gameScreen");
  document.getElementById("sprintCounterRow").style.display = "block";
  document.getElementById("sprintProgWrap").style.display = "block";
  isGameActive = true; isSprint = false;
  nextDictantWord();
}

function nextDictantWord() {
  if (dictantIdx >= dictantPool.length) { showDictantResult(); return; }
  const w = dictantPool[dictantIdx];
  currentWordObj = w;
  // Диктант: произносим английское слово, пользователь пишет РУССКИЙ перевод
  correctAnswerString = w.ru;
  currentLanguage = 'ru'; // ответ на русском → speak() не сработает в onCorrect автоматически

  // Show mic icon, hide Russian word
  document.getElementById("word").innerHTML = `<span style="font-size:42px;letter-spacing:0;">🎧</span>`;
  document.getElementById("speakBtn").style.display = 'none';
  document.getElementById("dictantRepeatWrap").style.display = 'block';

  document.getElementById("answer").value = "";
  document.getElementById("answer").placeholder = "Напечатай услышанное слово…";
  document.getElementById("result").innerHTML = `<span style="font-size:12px;color:var(--text2);">Диктант — слушай внимательно 🎧</span>`;
  document.getElementById("result").className = "result";
  document.getElementById("sprintCurrent").textContent = dictantIdx + 1;
  document.getElementById("sprintTotal").textContent = dictantPool.length;
  document.getElementById("sprintBar").style.width = (dictantIdx / dictantPool.length * 100) + "%";

  // Pronounce the word automatically
  setTimeout(() => {
    speak(currentWordObj.en.split('/')[0].trim());
    if (navigator.vibrate) navigator.vibrate(30);
  }, 400);

  if (gameMode === 'test') genOptions();
  isAnswering = false; toggleBtns(false); resetTimer();
}

function speakDictantAgain() {
  if (currentWordObj && isDictant) {
    speak(currentWordObj.en.split('/')[0].trim());
  }
}

function showDictantResult() {
  clearInterval(interval);
  isGameActive = false; isDictant = false;
  _lastResultMode = 'dictant';
  playSprint();
  const pct = dictantPool.length > 0 ? Math.round(dictantCorrect / dictantPool.length * 100) : 0;
  let emoji = "😅", title = "Тренируй слух!";
  if (pct === 100) { emoji = "🏆"; title = "Идеальный диктант! 🎉"; }
  else if (pct >= 80) { emoji = "🌟"; title = "Отличный слух!"; }
  else if (pct >= 60) { emoji = "👍"; title = "Неплохо!"; }

  document.getElementById("resultsEmoji").textContent = emoji;
  document.getElementById("resultsTitle").textContent = title;
  document.getElementById("resultsPct").textContent = pct + "%";
  document.getElementById("resCorrect").textContent = dictantCorrect;
  document.getElementById("resErrors").textContent = dictantErrors;
  document.getElementById("resXp").textContent = dictantXp;
  document.getElementById("resStreak").textContent = "—";
  document.getElementById("resultsVerdict").innerHTML =
    `Правильно: <b>${dictantCorrect} из ${dictantPool.length}</b> (${pct}%)<br>Заработано: <b>+${dictantXp} XP</b>`;
  document.getElementById("dailyRecordBadge").style.display = "none";
  document.getElementById("retryMistakesBtn").style.display = "none";
  showScreen("resultsScreen");
  document.getElementById("dashboardCard").style.display = "none";
  spawnConfetti(25);
  xp += dictantXp; dailyXp += dictantXp; save(); updateMenu();
  trackChStat('dictantDone');
  claimChallengeXp();
  renderActivityCalendar();
}

// ════════════════════════════════════

// ════════════════════════════════════
//  КАЛЕНДАРЬ АКТИВНОСТИ
// ════════════════════════════════════
function renderActivityCalendar() {
  const grid = document.getElementById('activityGrid');
  const monthsEl = document.getElementById('actMonths');
  if (!grid) return;

  const DAYS = 91; // ~3 months
  const today = new Date();
  today.setHours(0,0,0,0);

  // Load stored daily XP history
  let history = {};
  try { history = JSON.parse(localStorage.getItem('etu_act_history')) || {}; } catch(e){}

  // Write today's XP
  const todayKey = today.toDateString();
  history[todayKey] = dailyXp;
  try { localStorage.setItem('etu_act_history', JSON.stringify(history)); } catch(e){}

  // Max for scale
  const vals = Object.values(history).filter(v => v > 0);
  const maxXp = vals.length ? Math.max(...vals) : 1;

  grid.innerHTML = '';
  const monthsSeen = {};
  let monthPositions = [];

  for (let i = DAYS - 1; i >= 0; i--) {
    const d = new Date(today);
    d.setDate(d.getDate() - i);
    const key = d.toDateString();
    const xpVal = history[key] || 0;
    let lvl = 0;
    if (xpVal > 0) {
      const r = xpVal / maxXp;
      lvl = r < .25 ? 1 : r < .5 ? 2 : r < .75 ? 3 : 4;
    }
    const isToday = key === todayKey;
    const cell = document.createElement('div');
    cell.className = `act-cell lvl${lvl}${isToday ? ' today' : ''}`;
    cell.title = `${d.toLocaleDateString('ru-RU', {day:'numeric',month:'short'})}: ${xpVal} XP`;
    grid.appendChild(cell);

    // Track month labels
    const mo = d.toLocaleString('ru-RU', {month:'short'});
    const colIdx = DAYS - 1 - i;
    if (!monthsSeen[mo]) {
      monthsSeen[mo] = true;
      monthPositions.push({ mo, col: colIdx });
    }
  }

  // Month labels — only first of each
  if (monthsEl) {
    const totalCols = 13; // grid columns
    monthsEl.innerHTML = '';
    // Just show first 3 distinct months
    monthPositions.slice(0, 3).forEach(({mo}) => {
      const s = document.createElement('span');
      s.textContent = mo;
      monthsEl.appendChild(s);
    });
  }

  // Streak label
  const streakEl = document.getElementById('actCalStreak');
  if (streakEl) {
    let streak = 0;
    for (let i = 0; i < DAYS; i++) {
      const d = new Date(today);
      d.setDate(d.getDate() - i);
      if ((history[d.toDateString()] || 0) > 0) streak++;
      else break;
    }
    streakEl.textContent = streak > 1 ? `🔥 ${streak} дн. подряд` : '';
  }
}

// ════════════════════════════════════
//  ЕЖЕДНЕВНЫЕ ЧЕЛЛЕНДЖИ
// ════════════════════════════════════
const CHALLENGE_TEMPLATES = [
  { id:'ch_sprint10',  icon:'⚡', name:'Мини-спринт',       desc:'Пройди спринт из 10 слов',          xp:20,  check:()=>{ const s=JSON.parse(localStorage.getItem('etu_ch_stats')||'{}'); return (s.sprints10||0)>=1; } },
  { id:'ch_correct10', icon:'✅', name:'10 правильных',      desc:'Ответь правильно 10 раз подряд',     xp:25,  check:()=>maxStreak>=10 },
  { id:'ch_xp50',      icon:'⭐', name:'50 XP за день',      desc:'Набери 50 XP сегодня',              xp:15,  check:()=>dailyXp>=50 },
  { id:'ch_flash',     icon:'🃏', name:'Флэшкарты',          desc:'Пройди раунд флэшкарт',             xp:15,  check:()=>{ const s=JSON.parse(localStorage.getItem('etu_ch_stats')||'{}'); return (s.flashDone||0)>=1; } },
  { id:'ch_dictant',   icon:'🎧', name:'Диктант',            desc:'Пройди режим диктанта',             xp:20,  check:()=>{ const s=JSON.parse(localStorage.getItem('etu_ch_stats')||'{}'); return (s.dictantDone||0)>=1; } },
  { id:'ch_anagram',   icon:'🧩', name:'Анаграмма',          desc:'Пройди раунд анаграммы',            xp:15,  check:()=>{ const s=JSON.parse(localStorage.getItem('etu_ch_stats')||'{}'); return (s.anagramDone||0)>=1; } },
  { id:'ch_sentence',  icon:'🗣️', name:'Предложение',        desc:'Составь 3 предложения верно',       xp:30,  check:()=>{ const s=JSON.parse(localStorage.getItem('etu_ch_stats')||'{}'); return (s.sentCorrect||0)>=3; } },
  { id:'ch_noerror',   icon:'💎', name:'Без ошибок',         desc:'Пройди спринт без единой ошибки',   xp:35,  check:()=>{ const s=JSON.parse(localStorage.getItem('etu_ch_stats')||'{}'); return (s.perfectSprint||0)>=1; } },
  { id:'ch_wod',       icon:'📖', name:'Слово дня',          desc:'Отметь слово дня как изученное',    xp:10,  check:()=>localStorage.getItem('etu_wod_known_'+new Date().toDateString())==='1' },
  { id:'ch_correct5',  icon:'🔥', name:'Серия 5',            desc:'Набери серию из 5 правильных',      xp:10,  check:()=>maxStreak>=5 },
];

function getTodayChallenges() {
  const todayKey = 'etu_chal_' + new Date().toDateString();
  let saved = null;
  try { saved = JSON.parse(localStorage.getItem(todayKey)); } catch(e){}
  if (saved && Array.isArray(saved)) return saved;
  // Pick 3 random challenges deterministically by date
  // BUG FIX v14: старый алгоритм использовал a.id.length как хеш,
  // из-за чего несколько челленджей с одинаковой длиной id давали одинаковый хеш
  // и порядок был нестабильным / нерандомным. Исправлено: уникальный хеш по символам id.
  const seed = new Date().toDateString().split(' ').reduce((a,c)=>a+c.charCodeAt(0),0);
  const _hashId = (id, s) => id.split('').reduce((h,c,i)=>(h ^ (c.charCodeAt(0) * (i+1) * s)) & 0x7fffffff, s);
  const shuffled = [...CHALLENGE_TEMPLATES].sort((a,b) => _hashId(a.id,seed) - _hashId(b.id,seed));
  const picked = shuffled.slice(0, 3).map(c => c.id);
  try { localStorage.setItem(todayKey, JSON.stringify(picked)); } catch(e){}
  return picked;
}

function renderChallenges() {
  const el = document.getElementById('challengesList');
  const refreshEl = document.getElementById('chalRefreshTime');
  if (!el) return;

  const ids = getTodayChallenges();
  const templates = ids.map(id => CHALLENGE_TEMPLATES.find(c => c.id === id)).filter(Boolean);

  // Time until midnight
  const now = new Date();
  const midnight = new Date(now); midnight.setHours(24,0,0,0);
  const hoursLeft = Math.floor((midnight - now) / 3600000);
  const minsLeft = Math.floor(((midnight - now) % 3600000) / 60000);
  if (refreshEl) refreshEl.textContent = `Обновится через ${hoursLeft}ч ${minsLeft}м`;

  el.innerHTML = templates.map(c => {
    const done = c.check();
    return `<div class="challenge-item${done?' done':''}">
      <span class="ch-icon">${c.icon}</span>
      <div class="ch-info">
        <div class="ch-name">${c.name}</div>
        <div class="ch-desc">${c.desc}</div>
      </div>
      <span class="ch-xp${done?' done-xp':''}">+${c.xp} XP</span>
      <span class="ch-check">${done ? '✅' : '○'}</span>
    </div>`;
  }).join('');
}

function claimChallengeXp() {
  // Called after any game action — check if newly completed
  const ids = getTodayChallenges();
  const claimedKey = 'etu_chal_claimed_' + new Date().toDateString();
  let claimed = [];
  try { claimed = JSON.parse(localStorage.getItem(claimedKey)) || []; } catch(e){}

  ids.forEach(id => {
    if (claimed.includes(id)) return;
    const tmpl = CHALLENGE_TEMPLATES.find(c => c.id === id);
    if (!tmpl) return;
    if (tmpl.check()) {
      claimed.push(id);
      xp += tmpl.xp; dailyXp += tmpl.xp;
      alertPop(`🎯 Челлендж выполнен! +${tmpl.xp} XP`);
    }
  });
  try { localStorage.setItem(claimedKey, JSON.stringify(claimed)); } catch(e){}
  renderChallenges();
  renderChest();
}

function trackChStat(key, val=1) {
  const todayKey = 'etu_ch_stats_' + new Date().toDateString();
  let s = {};
  try { s = JSON.parse(localStorage.getItem(todayKey)) || {}; } catch(e){}
  s[key] = (s[key] || 0) + val;
  try { localStorage.setItem(todayKey, JSON.stringify(s)); } catch(e){}
  // alias so check() finds it — всегда синхронизируем с текущим днём
  try { localStorage.setItem('etu_ch_stats', JSON.stringify(s)); } catch(e){}
}

// BUG FIX v14: сбрасываем 'etu_ch_stats' при старте нового дня,
// чтобы вчерашняя статистика не была видна как выполненные сегодняшние задания.
(function _resetChStatsIfNewDay() {
  const todayKey = 'etu_ch_stats_' + new Date().toDateString();
  const todayStats = localStorage.getItem(todayKey);
  if (!todayStats) {
    // Новый день — обнуляем глобальный ключ
    try { localStorage.setItem('etu_ch_stats', JSON.stringify({})); } catch(e){}
  } else {
    // Синхронизируем с сегодняшними данными
    try { localStorage.setItem('etu_ch_stats', todayStats); } catch(e){}
  }
})();

// ════════════════════════════════════
//  РЕЖИМ «СОСТАВЬ ПРЕДЛОЖЕНИЕ»
// ════════════════════════════════════
const SENTENCES = [
  { ru: 'Я иду на работу каждый день',          en: 'I go to work every day' },
  { ru: 'Она любит читать книги по вечерам',     en: 'She likes to read books in the evening' },
  { ru: 'Мы живём в большом городе',             en: 'We live in a big city' },
  { ru: 'Он учит английский каждое утро',        en: 'He learns English every morning' },
  { ru: 'Дети играют в парке после школы',       en: 'The children play in the park after school' },
  { ru: 'Я не знаю правильного ответа',          en: 'I do not know the right answer' },
  { ru: 'Мы едим вместе каждый вечер',           en: 'We eat together every evening' },
  { ru: 'Она говорит по-английски очень хорошо', en: 'She speaks English very well' },
  { ru: 'Мой друг работает врачом',              en: 'My friend works as a doctor' },
  { ru: 'Я хочу выучить много новых слов',       en: 'I want to learn many new words' },
  { ru: 'Они живут рядом с нашей школой',        en: 'They live near our school' },
  { ru: 'Погода сегодня очень хорошая',          en: 'The weather today is very good' },
  { ru: 'Он не любит вставать рано утром',       en: 'He does not like to wake up early' },
  { ru: 'Мы идём в кино в субботу',             en: 'We are going to the cinema on Saturday' },
  { ru: 'Я читаю новости каждое утро',           en: 'I read the news every morning' },
  { ru: 'Кошка спит на диване',                  en: 'The cat is sleeping on the sofa' },
  { ru: 'Моя сестра работает в офисе',           en: 'My sister works in an office' },
  { ru: 'Они говорят по-русски дома',            en: 'They speak Russian at home' },
  { ru: 'Я хожу в спортзал три раза в неделю',  en: 'I go to the gym three times a week' },
  { ru: 'Он купил новый телефон вчера',          en: 'He bought a new phone yesterday' },
];

const SEN_TOTAL = 8;
let senPool = [], senIdx = 0, senCorrect = 0, senErrors = 0, senXpEarned = 0, senMaxStreak = 0, senStreak = 0;
let senCurrent = null, senAnswerArr = [], senWordsArr = [];

function startSentence() {
  playStart();
  senPool = [...SENTENCES].sort(() => Math.random() - .5).slice(0, SEN_TOTAL);
  senIdx = 0; senCorrect = 0; senErrors = 0; senXpEarned = 0; senMaxStreak = 0; senStreak = 0;
  document.getElementById('dashboardCard').style.display = 'none';
  showScreen('sentenceScreen');
  nextSentence();
}

function nextSentence() {
  if (senIdx >= senPool.length) { showSentenceResult(); return; }
  senCurrent = senPool[senIdx];
  senAnswerArr = [];
  // Shuffle words + add 2 distractors
  const words = senCurrent.en.split(' ');
  const distractors = ['always','never','very','often','really','just','also','only'].sort(()=>Math.random()-.5).slice(0,2);
  senWordsArr = [...words, ...distractors].sort(() => Math.random() - .5);

  document.getElementById('senRu').textContent = senCurrent.ru;
  document.getElementById('senHintLine').textContent = `${words.length} слов`;
  document.getElementById('senCounter').textContent = `Предложение ${senIdx+1} из ${SEN_TOTAL}`;
  document.getElementById('senBar').style.width = (senIdx / SEN_TOTAL * 100) + '%';
  document.getElementById('senXp').textContent = xp;
  document.getElementById('senStreak').textContent = senStreak;
  document.getElementById('senResult').innerHTML = '';
  document.getElementById('senResult').className = 'result';
  renderSenWords();
  renderSenAnswer();
}

function renderSenWords() {
  const el = document.getElementById('senWords');
  el.innerHTML = senWordsArr.map((w, i) =>
    `<div class="sentence-tile${senAnswerArr.some(a=>a.i===i)?' used':''}" data-idx="${i}" onclick="senAddWord(${i})">${w}</div>`
  ).join('');
}

function renderSenAnswer() {
  const el = document.getElementById('senAnswer');
  if (!senAnswerArr.length) {
    el.innerHTML = '<span style="color:var(--text2);font-size:13px;font-style:italic;align-self:center;">Нажимай слова снизу →</span>';
    el.classList.remove('has-words');
  } else {
    el.classList.add('has-words');
    el.innerHTML = senAnswerArr.map((a, i) =>
      `<div class="sentence-answer-tile" data-pos="${i}" onclick="senRemoveWord(${i})">${a.w}</div>`
    ).join('');
  }
}

function senAddWord(idx) {
  if (senAnswerArr.some(a => a.i === idx)) return;
  playClick();
  if (navigator.vibrate) navigator.vibrate(15);
  senAnswerArr.push({ i: idx, w: senWordsArr[idx] });
  renderSenWords();
  renderSenAnswer();
}

function senAnswerClick(e) {
  const tile = e.target.closest('.sentence-answer-tile');
  if (!tile) return;
  const pos = parseInt(tile.dataset.pos);
  senRemoveWord(pos);
}

function senRemoveWord(pos) {
  playClick();
  senAnswerArr.splice(pos, 1);
  renderSenWords();
  renderSenAnswer();
}

function senCheck() {
  unlockAudio();
  if (!senAnswerArr.length) { alertPop('Составь предложение!'); return; }
  const answer = senAnswerArr.map(a => a.w).join(' ').toLowerCase().trim();
  const correct = senCurrent.en.toLowerCase().trim();
  const ok = answer === correct;
  const res = document.getElementById('senResult');

  if (ok) {
    senCorrect++; senStreak++; senMaxStreak = Math.max(senMaxStreak, senStreak);
    const earned = senStreak >= 3 ? 20 : 15;
    senXpEarned += earned; xp += earned; dailyXp += earned;
    res.className = 'result ok-text';
    res.innerHTML = `✅ Верно! +${earned} XP`;
    playCorrect();
    if (navigator.vibrate) navigator.vibrate(30);
    document.getElementById('senXp').textContent = xp;
    document.getElementById('senStreak').textContent = senStreak;
    trackChStat('sentCorrect');
    save();
  } else {
    senErrors++; senStreak = 0;
    res.className = 'result err-text';
    res.innerHTML = `❌ Ошибка.<br><span style="font-size:13px;color:var(--text2);">Правильно: <strong style="color:var(--ok)">${senCurrent.en}</strong></span>`;
    playWrong();
    if (navigator.vibrate) navigator.vibrate([40,30,40]);
    document.getElementById('senStreak').textContent = 0;
  }

  senIdx++;
  // Disable tiles
  document.querySelectorAll('#senWords .sentence-tile, #senAnswer .sentence-answer-tile')
    .forEach(t => t.style.pointerEvents = 'none');
  document.querySelectorAll('.actions button').forEach(b => b.disabled = true);
  claimChallengeXp();
  setTimeout(() => {
    document.querySelectorAll('.actions button').forEach(b => b.disabled = false);
    nextSentence();
  }, ok ? 900 : 1600);
}

function senHint() {
  if (xp < 5) { alertPop('Нужно минимум 5 XP для подсказки!'); return; }
  xp -= 5; playHint(); update(); save();
  // Add next correct word
  const correctWords = senCurrent.en.split(' ');
  const pos = senAnswerArr.length;
  if (pos >= correctWords.length) return;
  const nextWord = correctWords[pos];
  const idx = senWordsArr.findIndex((w, i) => w === nextWord && !senAnswerArr.some(a => a.i === i));
  if (idx !== -1) senAddWord(idx);
}

function senSkip() {
  playClick(); senErrors++; senStreak = 0; senIdx++;
  document.getElementById('senResult').className = 'result err-text';
  document.getElementById('senResult').innerHTML = `⏭ Пропущено. Правильно: <strong>${senCurrent.en}</strong>`;
  setTimeout(nextSentence, 1200);
}

function exitSentence() { playClick(); goToMainMenu(); }

function showSentenceResult() {
  clearInterval(interval); playSprint();
  _lastResultMode = 'sentence';
  const pct = SEN_TOTAL > 0 ? Math.round(senCorrect / SEN_TOTAL * 100) : 0;
  let emoji = '😅', title = 'Продолжай практиковаться!';
  if (pct === 100) { emoji = '🏆'; title = 'Идеально! 🎉'; trackChStat('perfectSprint'); }
  else if (pct >= 80) { emoji = '🌟'; title = 'Отличный результат!'; }
  else if (pct >= 60) { emoji = '👍'; title = 'Хорошая работа!'; }
  document.getElementById('senResEmoji').textContent = emoji;
  document.getElementById('senResTitle').textContent = title;
  document.getElementById('senResPct').textContent = pct + '%';
  document.getElementById('senResCorrect').textContent = senCorrect;
  document.getElementById('senResErrors').textContent = senErrors;
  document.getElementById('senResXp').textContent = senXpEarned;
  document.getElementById('senResStreak').textContent = senMaxStreak;
  document.getElementById('senResVerdict').innerHTML = `Правильно: <b>${senCorrect} из ${SEN_TOTAL}</b><br>Заработано: <b>+${senXpEarned} XP</b>`;
  // BUG FIX v14: XP уже начислен в senCheck() при каждом правильном ответе.
  // Повторное добавление здесь вызывало двойное начисление XP за весь раунд.
  // xp += senXpEarned; dailyXp += senXpEarned; — УДАЛЕНО
  save(); updateMenu(); claimChallengeXp();
  showScreen('sentenceResultScreen');
  document.getElementById('dashboardCard').style.display = 'none';
  spawnConfetti(25);
}

// ════════════════════════════════════
//  СУНДУКИ С НАГРАДАМИ
// ════════════════════════════════════
const CHEST_DAYS = 7;
const CHEST_REWARDS = [
  { xp: 20,  label: '+20 XP' },
  { xp: 25,  label: '+25 XP' },
  { xp: 30,  label: '+30 XP' },
  { xp: 40,  label: '+40 XP' },
  { xp: 50,  label: '+50 XP' },
  { xp: 60,  label: '+60 XP' },
  { xp: 100, label: '🏆 +100 XP' },
];

function getAllChallengesDoneToday() {
  const ids = getTodayChallenges();
  const claimedKey = 'etu_chal_claimed_' + new Date().toDateString();
  let claimed = [];
  try { claimed = JSON.parse(localStorage.getItem(claimedKey)) || []; } catch(e){}
  return ids.length > 0 && ids.every(id => claimed.includes(id));
}

function getChestState() {
  let s = { progress: 0, lastDate: null, claimedToday: false };
  try { s = Object.assign(s, JSON.parse(localStorage.getItem('etu_chest_state')) || {}); } catch(e){}
  return s;
}
function saveChestState(s) {
  try { localStorage.setItem('etu_chest_state', JSON.stringify(s)); } catch(e){}
}

function renderChest() {
  const track = document.getElementById('chestTrack');
  const btn = document.getElementById('chestClaimBtn');
  const sub = document.getElementById('chestSub');
  if (!track) return;

  const todayKey = new Date().toDateString();
  let state = getChestState();

  // Reset progress streak if a day was missed (not today, not yesterday)
  if (state.lastDate && state.lastDate !== todayKey) {
    const last = new Date(state.lastDate);
    const today = new Date(todayKey);
    const diffDays = Math.round((today - last) / 86400000);
    if (diffDays > 1) { state.progress = 0; }
    state.claimedToday = false;
    state.lastDate = todayKey;
    saveChestState(state);
  } else if (!state.lastDate) {
    state.lastDate = todayKey;
    saveChestState(state);
  }

  // If all challenges done today and not yet credited
  const allDone = getAllChallengesDoneToday();
  if (allDone && !state.creditedToday) {
    state.progress = Math.min(state.progress + 1, CHEST_DAYS);
    state.creditedToday = true;
    saveChestState(state);
  }
  // BUG FIX v14: не сбрасываем creditedToday при каждом рендере —
  // только при смене дня (уже сделано выше в блоке lastDate).
  // Старый код сбрасывал флаг при каждом открытии меню, если не все задания выполнены,
  // что позволяло получить прогресс сундука повторно в тот же день.

  // Render track
  track.innerHTML = '';
  for (let i = 0; i < CHEST_DAYS; i++) {
    const filled = i < state.progress;
    const isLast = i === CHEST_DAYS - 1;
    const cell = document.createElement('div');
    cell.className = `chest-day${filled ? ' filled' : ''}`;
    cell.textContent = isLast ? '🎁' : (i+1);
    track.appendChild(cell);
  }

  // Button state
  if (state.progress >= CHEST_DAYS && !state.claimedToday) {
    btn.disabled = false;
    btn.classList.add('ready');
    btn.textContent = '🎁 Открыть!';
    sub.textContent = 'Сундук готов! Открой и получи награду 🎉';
  } else if (state.progress >= CHEST_DAYS && state.claimedToday) {
    btn.disabled = true;
    btn.classList.remove('ready');
    btn.textContent = '✅ Открыт';
    sub.textContent = 'Награда уже получена. Завтра начнётся новый сундук.';
  } else {
    btn.disabled = true;
    btn.classList.remove('ready');
    btn.textContent = `🔒 ${state.progress}/${CHEST_DAYS}`;
    sub.textContent = allDone
      ? '✅ Сегодня челленджи выполнены — приходи завтра!'
      : 'Выполни все 3 челленджа сегодня, чтобы продвинуться';
  }
}

function claimChest() {
  let state = getChestState();
  if (state.progress < CHEST_DAYS || state.claimedToday) return;
  const reward = CHEST_REWARDS[Math.floor(Math.random() * CHEST_REWARDS.length)];
  xp += reward.xp; dailyXp += reward.xp;
  state.progress = 0;
  state.claimedToday = true;
  saveChestState(state);
  save(); update();
  playAchievement();
  spawnConfetti(35);
  if (navigator.vibrate) navigator.vibrate([60,40,60,40,120]);
  showShopToast(`🎁 Сундук открыт!<br><span style="font-size:28px;">${reward.label}</span>`);
  renderChest();
}

function showShopToast(html) {
  const el = document.createElement('div');
  el.className = 'shop-toast';
  el.innerHTML = html;
  document.body.appendChild(el);
  setTimeout(() => el.remove(), 1800);
}

// ════════════════════════════════════
//  МАГАЗИН
// ════════════════════════════════════
const SHOP_FRAMES = [
  {
    id: 'frame_none', name: 'Без рамки', price: 0,
    css: 'none'
  },
  {
    id: 'frame_blue', name: 'Синяя', price: 0,
    css: '3px solid #4f8eff',
    shadow: '0 0 0 2px rgba(79,142,255,0.35)'
  },
  {
    id: 'frame_gold', name: 'Золото', price: 80,
    css: '3px solid #ffc247',
    shadow: '0 0 12px rgba(255,194,71,0.5)',
    gradient: 'linear-gradient(135deg,#ffc247,#ff8c42)'
  },
  {
    id: 'frame_fire', name: 'Огонь', price: 120,
    css: '3px solid #ff4f6d',
    shadow: '0 0 16px rgba(255,79,109,0.6)',
    gradient: 'linear-gradient(135deg,#ff4f6d,#ff8c42)'
  },
  {
    id: 'frame_neon', name: 'Неон', price: 150,
    css: '3px solid #22d88f',
    shadow: '0 0 18px rgba(34,216,143,0.7)',
    gradient: 'linear-gradient(135deg,#22d88f,#4f8eff)'
  },
  {
    id: 'frame_galaxy', name: 'Галактика', price: 200,
    css: '3px solid #a78bfa',
    shadow: '0 0 20px rgba(167,139,250,0.7)',
    gradient: 'linear-gradient(135deg,#7c5cfc,#4f8eff,#22d88f)'
  },
  {
    id: 'frame_rainbow', name: 'Радуга', price: 300,
    css: '3px solid transparent',
    bgImage: 'linear-gradient(135deg,#ff4f6d,#ffc247,#22d88f,#4f8eff,#a78bfa)',
    shadow: '0 0 22px rgba(167,139,250,0.5)',
    animated: true
  },
  {
    id: 'frame_diamond', name: 'Алмаз', price: 400,
    css: '3px solid #e0f2ff',
    shadow: '0 0 24px rgba(224,242,255,0.8), 0 0 8px rgba(79,142,255,0.6)',
    gradient: 'linear-gradient(135deg,#e0f2ff,#a5d8ff,#74c0fc,#e0f2ff)'
  },
];

const SHOP_THEMES = [
  { id:'theme_default', name:'Классика',  accent:'#4f8eff', accent2:'#7c5cfc', price:0 },
  { id:'theme_sunset',  name:'Закат',     accent:'#ff7a59', accent2:'#ffc247', price:80 },
  { id:'theme_forest',  name:'Лес',       accent:'#22d88f', accent2:'#0ea5e9', price:80 },
  { id:'theme_rose',    name:'Розовый',   accent:'#ff5da2', accent2:'#a855f7', price:100 },
  { id:'theme_mint',    name:'Мята',      accent:'#2dd4bf', accent2:'#4f8eff', price:100 },
  { id:'theme_gold',    name:'Золото',    accent:'#ffc247', accent2:'#ff7a18', price:150 },
];

const SHOP_SOUNDS = [
  { id:'default', icon:'🔔', name:'Классика', price:0 },
  { id:'bells',   icon:'🎐', name:'Колокольчик', price:50 },
  { id:'arcade',  icon:'🕹️', name:'Аркада', price:50 },
  { id:'chime',   icon:'✨', name:'Перезвон', price:75 },
];

function getOwned(kind) {
  let o = {};
  try { o = JSON.parse(localStorage.getItem('etu_shop_owned')) || {}; } catch(e){}
  return o[kind] || [];
}
function setOwned(kind, arr) {
  let o = {};
  try { o = JSON.parse(localStorage.getItem('etu_shop_owned')) || {}; } catch(e){}
  o[kind] = arr;
  try { localStorage.setItem('etu_shop_owned', JSON.stringify(o)); } catch(e){}
}

function showShopScreen() {
  playClick();
  document.getElementById('dashboardCard').style.display = 'none';
  showScreen('shopScreen');
  renderShop();
}

function renderShop() {
  document.getElementById('shopXpBalance').textContent = xp + ' XP';

  // Frames
  const ownedFr = getOwned('frames');
  const equippedFrame = profileData.frame || 'frame_blue';
  const frEl = document.getElementById('shopFrames');
  frEl.innerHTML = SHOP_FRAMES.map(f => {
    const owned = f.price === 0 || ownedFr.includes(f.id);
    const equipped = equippedFrame === f.id;
    const cls = equipped ? 'equipped' : owned ? 'owned' : (xp < f.price ? 'locked' : '');
    const priceHtml = equipped
      ? '<span class="shop-item-price equipped-tag">Надето</span>'
      : owned
        ? '<span class="shop-item-price owned-tag">Надеть</span>'
        : `<span class="shop-item-price">${f.price} XP</span>`;

    // Build frame preview
    let ringStyle = '';
    if (f.id === 'frame_none') {
      ringStyle = 'display:none;';
    } else if (f.animated) {
      // handled by class
    } else if (f.gradient) {
      ringStyle = `background:${f.gradient};`;
    } else {
      ringStyle = `background:${f.css.replace('3px solid ','')};`;
    }
    const animClass = f.animated ? ' animated' : '';
    const shadowStyle = f.shadow ? `box-shadow:${f.shadow};` : '';

    return `<div class="shop-item ${cls}" onclick="buyOrEquipFrame('${f.id}')">
      <div class="frame-preview-wrap">
        <div class="frame-preview-ring${animClass}" style="${ringStyle}${shadowStyle}"></div>
        <div class="frame-preview-inner">🦁</div>
      </div>
      <div class="shop-item-name">${f.name}</div>
      ${priceHtml}
    </div>`;
  }).join('');

  // Themes
  const ownedTh = getOwned('themes');
  const equippedTheme = localStorage.getItem('etu_accent_theme') || 'theme_default';
  const thEl = document.getElementById('shopThemes');
  thEl.innerHTML = SHOP_THEMES.map(t => {
    const owned = t.price === 0 || ownedTh.includes(t.id);
    const equipped = equippedTheme === t.id;
    const cls = equipped ? 'equipped' : owned ? 'owned' : (xp < t.price ? 'locked' : '');
    const priceHtml = equipped
      ? '<span class="shop-item-price equipped-tag">Активна</span>'
      : owned
        ? '<span class="shop-item-price owned-tag">Применить</span>'
        : `<span class="shop-item-price">${t.price} XP</span>`;
    return `<div class="shop-item ${cls}" onclick="buyOrEquipTheme('${t.id}')">
      <div class="theme-swatch" style="background:linear-gradient(135deg,${t.accent},${t.accent2});"></div>
      <div class="shop-item-name">${t.name}</div>
      ${priceHtml}
    </div>`;
  }).join('');

  // Sounds
  const ownedSnd = getOwned('sounds');
  const equippedSound = localStorage.getItem('etu_sound_pack') || 'default';
  const sndEl = document.getElementById('shopSounds');
  sndEl.innerHTML = SHOP_SOUNDS.map(s => {
    const owned = s.price === 0 || ownedSnd.includes(s.id);
    const equipped = equippedSound === s.id;
    const cls = equipped ? 'equipped' : owned ? 'owned' : (xp < s.price ? 'locked' : '');
    const priceHtml = equipped
      ? '<span class="shop-item-price equipped-tag">Включён</span>'
      : owned
        ? '<span class="shop-item-price owned-tag">Включить</span>'
        : `<span class="shop-item-price">${s.price} XP</span>`;
    return `<div class="shop-item ${cls}" onclick="buyOrEquipSound('${s.id}')">
      <div class="shop-item-icon">${s.icon}</div>
      <div class="shop-item-name">${s.name}</div>
      ${priceHtml}
    </div>`;
  }).join('');
}

function buyOrEquipFrame(id) {
  const item = SHOP_FRAMES.find(f => f.id === id);
  if (!item) return;
  const owned = item.price === 0 || getOwned('frames').includes(id);
  if (owned) {
    playClick();
    profileData.frame = id;
    localStorage.setItem('etu_profile', JSON.stringify(profileData));
    applyAvatarFrame(id);
    if (typeof updateProfileRing === 'function') updateProfileRing();
    renderShop();
    return;
  }
  if (xp < item.price) { alertPop(`Не хватает XP! Нужно ${item.price} XP`); return; }
  shopConfirm(`Купить рамку «<b>${item.name}</b>» за <b>${item.price} XP</b>?`, () => {
    const ownedFr = getOwned('frames'); ownedFr.push(id); setOwned('frames', ownedFr);
    xp -= item.price;
    profileData.frame = id;
    localStorage.setItem('etu_profile', JSON.stringify(profileData));
    save(); update();
    applyAvatarFrame(id);
    if (typeof updateProfileRing === 'function') updateProfileRing();
    playAchievement(); spawnConfetti(20);
    showShopToast(`✅ Рамка надета!<br><span style="font-size:22px;">🖼️ ${item.name}</span>`);
    renderShop();
  });
}

function applyAvatarFrame(frameId) {
  const frame = SHOP_FRAMES.find(f => f.id === frameId) || SHOP_FRAMES[1];
  const menuAvEl = document.getElementById('menuAvatar');
  if (!menuAvEl) return;

  // Wrap menuAvatar in frame-wrap if not already
  let wrap = document.getElementById('menuAvatarFrameWrap');
  if (!wrap) {
    const parent = menuAvEl.parentNode;
    wrap = document.createElement('div');
    wrap.id = 'menuAvatarFrameWrap';
    wrap.className = 'avatar-frame-wrap';
    parent.insertBefore(wrap, menuAvEl);
    wrap.appendChild(menuAvEl);
  }

  // Remove old ring
  let ring = document.getElementById('menuAvatarFrameRing');
  if (ring) ring.remove();
  // Also reset any inline styles on avatar
  menuAvEl.style.outline = '';
  menuAvEl.style.boxShadow = '';

  if (frameId === 'frame_none') return;

  if (frame.animated || frame.gradient) {
    // Use a background div behind the avatar
    ring = document.createElement('div');
    ring.id = 'menuAvatarFrameRing';
    ring.className = 'avatar-frame-ring' + (frame.animated ? ' animated' : '');
    if (!frame.animated && frame.gradient) {
      ring.style.background = frame.gradient;
    }
    if (frame.shadow) ring.style.boxShadow = frame.shadow;
    wrap.insertBefore(ring, menuAvEl);
  } else {
    // Solid color — use outline on the avatar element itself (no z-index issues)
    const color = frame.css.replace('3px solid ', '');
    menuAvEl.style.outline = `3px solid ${color}`;
    menuAvEl.style.outlineOffset = '2px';
    if (frame.shadow) menuAvEl.style.boxShadow = frame.shadow;
  }
}

function loadAvatarFrame() {
  const frameId = profileData.frame || 'frame_blue';
  // Defer until DOM ready
  setTimeout(() => applyAvatarFrame(frameId), 0);
}

function buyOrEquipTheme(id) {
  const item = SHOP_THEMES.find(t => t.id === id);
  if (!item) return;
  const owned = item.price === 0 || getOwned('themes').includes(id);
  if (owned) {
    playClick();
    applyAccentTheme(id);
    renderShop();
    return;
  }
  if (xp < item.price) { alertPop(`Не хватает XP! Нужно ${item.price} XP`); return; }
  shopConfirm(`Купить тему «<b>${item.name}</b>» за <b>${item.price} XP</b>?`, () => {
    const ownedTh = getOwned('themes'); ownedTh.push(id); setOwned('themes', ownedTh);
    xp -= item.price; save(); update();
    applyAccentTheme(id);
    playAchievement(); spawnConfetti(20);
    showShopToast(`✅ Тема применена!<br><span style="font-size:24px;">${item.name}</span>`);
    renderShop();
  });
}

function buyOrEquipSound(id) {
  const item = SHOP_SOUNDS.find(s => s.id === id);
  if (!item) return;
  const owned = item.price === 0 || getOwned('sounds').includes(id);
  if (owned) {
    playClick();
    localStorage.setItem('etu_sound_pack', id);
    unlockAudio();
    setTimeout(() => playCorrect(), 200);
    renderShop();
    return;
  }
  if (xp < item.price) { alertPop(`Не хватает XP! Нужно ${item.price} XP`); return; }
  shopConfirm(`Купить звук «<b>${item.name}</b>» за <b>${item.price} XP</b>?`, () => {
    const ownedSnd = getOwned('sounds'); ownedSnd.push(id); setOwned('sounds', ownedSnd);
    localStorage.setItem('etu_sound_pack', id);
    xp -= item.price; save(); update();
    unlockAudio();
    setTimeout(() => playCorrect(), 200);
    spawnConfetti(20);
    showShopToast(`✅ Звук применён!<br><span style="font-size:24px;">${item.icon} ${item.name}</span>`);
    renderShop();
  });
}

function applyAccentTheme(id) {
  const item = SHOP_THEMES.find(t => t.id === id) || SHOP_THEMES[0];
  document.documentElement.style.setProperty('--accent', item.accent);
  document.documentElement.style.setProperty('--accent2', item.accent2);
  localStorage.setItem('etu_accent_theme', id);
  _syncThemeColor();
}

function loadAccentTheme() {
  const id = localStorage.getItem('etu_accent_theme');
  if (id) applyAccentTheme(id);
}

// ════════════════════════════════════
loadTheme();
loadAccentTheme();
loadProfile();
renderChest();
document.getElementById("soundBtn").textContent=soundEnabled?"🔊":"🔇";

loadPrefs();
setGameMode(gameMode);
updateMenu();
update();
renderAchievements();
renderMistakes();
renderMyWords();
goToMainMenu();

// NEW v4 init
_lastLevel = getLevel(xp);
updateDayStreak();
renderWeeklyChart();
updateProfileRing();

// v5 init
initOnboarding();
renderHardWords();

// v7 init
initWordOfDay();
checkResumeSprint();

// v8 init
renderActivityCalendar();
renderChallenges();

// v13 init — рамки аватара
loadAvatarFrame();

// onCorrect патч убран — level up встроен напрямую

// ════ Custom shop confirm modal (replaces browser confirm) ════
function shopConfirm(message, onYes, yesLabel) {
  let modal = document.getElementById('shopConfirmModal');
  if (!modal) {
    modal = document.createElement('div');
    modal.id = 'shopConfirmModal';
    modal.style.cssText = 'position:fixed;inset:0;background:rgba(8,12,20,.85);backdrop-filter:blur(12px);z-index:9998;display:flex;align-items:center;justify-content:center;padding:20px;';
    modal.innerHTML = '<div style="background:var(--card);border:1px solid var(--card-border);border-radius:24px;padding:28px 24px;max-width:360px;width:100%;text-align:center;box-shadow:0 30px 80px rgba(0,0,0,.5);"><div id="shopConfirmMsg" style="font-size:17px;font-weight:700;line-height:1.5;margin-bottom:24px;color:var(--text);"></div><div style="display:flex;gap:12px;"><button id="shopConfirmNo" style="flex:1;padding:14px;border-radius:14px;border:1px solid var(--card-border);background:var(--muted);color:var(--text);font-size:15px;font-weight:700;cursor:pointer;">Отмена</button><button id="shopConfirmYes" style="flex:1;padding:14px;border-radius:14px;border:none;background:linear-gradient(135deg,var(--accent),var(--accent2));color:#fff;font-size:15px;font-weight:800;cursor:pointer;">Купить ✓</button></div></div>';
    document.body.appendChild(modal);
  }
  document.getElementById('shopConfirmMsg').innerHTML = message;
  document.getElementById('shopConfirmYes').textContent = yesLabel || 'Купить ✓';
  modal.style.display = 'flex';
  const hide = () => { modal.style.display = 'none'; };
  document.getElementById('shopConfirmYes').onclick = () => { hide(); onYes(); };
  document.getElementById('shopConfirmNo').onclick = hide;
  modal.onclick = (e) => { if (e.target === modal) hide(); };
}

// ════ КРОППЕР ФОТО ════
const _crop = {
  img: null, scale: 1, ox: 0, oy: 0,
  dragging: false, lastX: 0, lastY: 0,
  SIZE: 560
};

function openCropper(dataUrl) {
  const modal = document.getElementById('cropperModal');
  const canvas = document.getElementById('cropperCanvas');
  const ctx = canvas.getContext('2d');
  const zoom = document.getElementById('cropperZoom');

  _crop.img = new Image();
  _crop.img.onload = () => {
    // Fit image to canvas initially
    const s = Math.max(_crop.SIZE / _crop.img.width, _crop.SIZE / _crop.img.height);
    _crop.scale = s;
    _crop.ox = (_crop.SIZE - _crop.img.width * s) / 2;
    _crop.oy = (_crop.SIZE - _crop.img.height * s) / 2;
    zoom.value = 1;
    _cropDraw();
  };
  _crop.img.src = dataUrl;
  modal.classList.add('open');

  // OPTIMIZATION v14: регистрируем обработчики только один раз, а не при каждом открытии
  if (!_crop._handlersAttached) {
    _crop._handlersAttached = true;

    zoom.oninput = () => {
      const z = parseFloat(zoom.value);
      const base = Math.max(_crop.SIZE / _crop.img.width, _crop.SIZE / _crop.img.height);
      const newScale = base * z;
      const cx = _crop.SIZE / 2;
      const cy = _crop.SIZE / 2;
      _crop.ox = cx - (cx - _crop.ox) * (newScale / _crop.scale);
      _crop.oy = cy - (cy - _crop.oy) * (newScale / _crop.scale);
      _crop.scale = newScale;
      _cropClamp();
      _cropDraw();
    };

    const wrap = document.getElementById('cropperWrap');
    wrap.ontouchstart = (e) => {
      if (e.touches.length === 1) {
        _crop.dragging = true;
        _crop.lastX = e.touches[0].clientX;
        _crop.lastY = e.touches[0].clientY;
      }
    };
    wrap.ontouchmove = (e) => {
      e.preventDefault();
      if (!_crop.dragging || e.touches.length !== 1) return;
      const dx = e.touches[0].clientX - _crop.lastX;
      const dy = e.touches[0].clientY - _crop.lastY;
      _crop.lastX = e.touches[0].clientX;
      _crop.lastY = e.touches[0].clientY;
      _crop.ox += dx * 2;
      _crop.oy += dy * 2;
      _cropClamp();
      _cropDraw();
    };
    wrap.ontouchend = () => { _crop.dragging = false; };

    wrap.onmousedown = (e) => { _crop.dragging = true; _crop.lastX = e.clientX; _crop.lastY = e.clientY; };
    wrap.onmousemove = (e) => {
      if (!_crop.dragging) return;
      _crop.ox += (e.clientX - _crop.lastX) * 2;
      _crop.oy += (e.clientY - _crop.lastY) * 2;
      _crop.lastX = e.clientX; _crop.lastY = e.clientY;
      _cropClamp(); _cropDraw();
    };
    wrap.onmouseup = wrap.onmouseleave = () => { _crop.dragging = false; };
  }
}

function _cropClamp() {
  const w = _crop.img.width * _crop.scale;
  const h = _crop.img.height * _crop.scale;
  const S = _crop.SIZE;
  _crop.ox = Math.min(0, Math.max(S - w, _crop.ox));
  _crop.oy = Math.min(0, Math.max(S - h, _crop.oy));
}

function _cropDraw() {
  const canvas = document.getElementById('cropperCanvas');
  // OPTIMIZATION v14: кешируем контекст на canvas-элементе, а не получаем каждый раз
  if (!canvas._ctx) canvas._ctx = canvas.getContext('2d');
  const ctx = canvas._ctx;
  ctx.clearRect(0, 0, _crop.SIZE, _crop.SIZE);
  ctx.drawImage(_crop.img, _crop.ox, _crop.oy, _crop.img.width * _crop.scale, _crop.img.height * _crop.scale);
}

function confirmCrop() {
  const canvas = document.getElementById('cropperCanvas');
  // Export 400x400 final
  const out = document.createElement('canvas');
  out.width = 400; out.height = 400;
  const ctx = out.getContext('2d');
  const ratio = 400 / _crop.SIZE;
  ctx.drawImage(canvas, 0, 0, _crop.SIZE, _crop.SIZE, 0, 0, 400, 400);
  const dataUrl = out.toDataURL('image/jpeg', 0.85);
  closeCropper();
  // Apply as avatar photo
  profileData.avatarPhoto = dataUrl;
  try { localStorage.setItem("etu_avatarPhoto", dataUrl); } catch(err) {
    alertPop("⚠️ Фото слишком большое."); return;
  }
  _applyAvatarToEl(document.getElementById("profileAvatarBig"), profileData.avatar, dataUrl);
  _applyAvatarToEl(document.getElementById("menuAvatar"), profileData.avatar, dataUrl);
  const delBtn = document.getElementById("avatarPhotoDelBtn");
  if (delBtn) delBtn.style.display = "inline-flex";
  document.getElementById("avatarGrid").querySelectorAll("div").forEach(d => {
    d.style.borderColor = "transparent"; d.style.background = "var(--muted2)";
  });
}

function closeCropper() {
  document.getElementById('cropperModal').classList.remove('open');
}

</script>

<!-- ═══ КРОППЕР ФОТО ═══ -->
<div class="cropper-modal" id="cropperModal">
  <div class="cropper-title">✂️ Обрезка фото</div>
  <div class="cropper-canvas-wrap" id="cropperWrap">
    <canvas id="cropperCanvas" width="560" height="560"></canvas>
    <div class="cropper-circle-mask"></div>
    <div class="cropper-overlay"></div>
  </div>
  <div class="cropper-hint">Тяни пальцем чтобы переместить</div>
  <div class="cropper-zoom-row">
    <span>🔍</span>
    <input type="range" id="cropperZoom" min="0.5" max="3" step="0.01" value="1">
    <span>🔎</span>
  </div>
  <div class="cropper-btns">
    <button class="cropper-btn-cancel" onclick="closeCropper()">Отмена</button>
    <button class="cropper-btn-ok" onclick="confirmCrop()">Готово ✓</button>
  </div>
</div>
</body>
</html>
