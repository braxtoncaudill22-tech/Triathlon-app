# Triathlon-app
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover" />
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
<meta name="apple-mobile-web-app-title" content="Tri Plan" />
<meta name="theme-color" content="#1565c0" />
<title>Olympic Triathlon Plan</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: -apple-system, 'Inter', 'Helvetica Neue', Arial, sans-serif;
    background: #f0f4f8;
    min-height: 100vh;
    padding-bottom: env(safe-area-inset-bottom, 24px);
    -webkit-tap-highlight-color: transparent;
  }
  button { font-family: inherit; cursor: pointer; }

  /* HEADER */
  #header {
    background: linear-gradient(135deg, #0d47a1 0%, #1565c0 60%, #1976d2 100%);
    color: #fff;
    padding: 48px 20px 20px;
    text-align: center;
  }
  #header .eyebrow { font-size: 11px; letter-spacing: 0.2em; text-transform: uppercase; opacity: 0.7; margin-bottom: 6px; }
  #header h1 { font-size: 24px; font-weight: 800; letter-spacing: -0.5px; line-height: 1.2; margin-bottom: 12px; }
  .distance-pills { display: flex; justify-content: center; gap: 8px; flex-wrap: wrap; margin-bottom: 8px; }
  .distance-pills span { font-size: 12px; background: rgba(255,255,255,0.15); border-radius: 20px; padding: 4px 12px; }
  #header .meta { font-size: 11px; opacity: 0.6; margin-bottom: 14px; }
  #progress-wrap { max-width: 460px; margin: 0 auto; }
  #progress-label { display: flex; justify-content: space-between; font-size: 11px; opacity: 0.75; margin-bottom: 5px; }
  #progress-track { background: rgba(255,255,255,0.2); border-radius: 4px; height: 7px; }
  #progress-bar { background: #fff; height: 100%; border-radius: 4px; transition: width 0.35s ease; width: 0%; }

  /* WEEK TABS */
  #tab-scroll { overflow-x: auto; -webkit-overflow-scrolling: touch; background: #fff; border-bottom: 1px solid #e0e7ef; position: sticky; top: 0; z-index: 20; }
  #tabs { display: flex; gap: 0; padding: 0 12px; min-width: max-content; }
  .tab {
    flex: 0 0 auto; padding: 13px 16px 11px;
    background: none; border: none; border-bottom: 3px solid transparent;
    font-size: 13px; font-weight: 600; color: #94a3b8;
    transition: all 0.15s; white-space: nowrap;
  }
  .tab.active { color: #1565c0; border-bottom-color: #1565c0; }

  /* MAIN */
  #main { max-width: 660px; margin: 0 auto; padding: 20px 14px 32px; }

  /* WEEK HEADER */
  #week-eyebrow { font-size: 11px; text-transform: uppercase; letter-spacing: 0.15em; color: #1565c0; font-weight: 700; }
  #week-title { font-size: 21px; font-weight: 800; color: #1a237e; margin: 3px 0 4px; }
  #week-sub { font-size: 13px; color: #555; margin-bottom: 16px; }

  /* DAY CARDS */
  .day-card {
    background: #fff;
    border-radius: 14px;
    border: 1px solid #e8edf2;
    box-shadow: 0 1px 4px rgba(0,0,0,0.06);
    margin-bottom: 10px;
    overflow: hidden;
    transition: border-color 0.2s, background 0.2s;
  }
  .day-card.done { background: #f0fdf4; border-color: #bbf7d0; }
  .card-row {
    display: flex; align-items: center; gap: 12px;
    padding: 13px 14px; cursor: pointer; user-select: none;
  }
  .day-col { min-width: 38px; text-align: center; flex-shrink: 0; }
  .day-name { font-size: 10px; font-weight: 700; color: #aaa; text-transform: uppercase; }
  .day-icon {
    margin-top: 4px; width: 38px; height: 38px; border-radius: 50%;
    display: flex; align-items: center; justify-content: center; font-size: 19px;
  }
  .card-body { flex: 1; min-width: 0; }
  .tag-row { display: flex; align-items: center; gap: 6px; flex-wrap: wrap; margin-bottom: 5px; }
  .type-tag { font-size: 10px; font-weight: 700; border-radius: 10px; padding: 2px 9px; text-transform: uppercase; letter-spacing: 0.05em; }
  .intensity-pip { display: flex; align-items: center; gap: 4px; font-size: 11px; color: #888; }
  .pip-dot { width: 7px; height: 7px; border-radius: 50%; display: inline-block; flex-shrink: 0; }
  .workout-text { font-size: 13.5px; color: #333; line-height: 1.45; }
  .done .workout-text { color: #16a34a; text-decoration: line-through; }
  .card-actions { display: flex; flex-direction: column; align-items: flex-end; gap: 6px; flex-shrink: 0; }
  .mark-btn {
    border: none; border-radius: 8px; padding: 5px 11px;
    font-size: 11px; font-weight: 700; background: #f1f5f9; color: #64748b;
    transition: all 0.15s;
  }
  .done .mark-btn { background: #16a34a; color: #fff; }
  .chevron { font-size: 10px; color: #ccc; transition: transform 0.2s; }
  .chevron.open { transform: rotate(180deg); }

  /* LEGEND */
  .panel { background: #fff; border-radius: 14px; padding: 14px 16px; box-shadow: 0 1px 4px rgba(0,0,0,0.06); margin-top: 16px; }
  .panel-title { font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.12em; color: #999; margin-bottom: 10px; }
  .legend-row { display: flex; align-items: center; gap: 8px; font-size: 12.5px; color: #444; margin-bottom: 7px; }

  /* TIPS */
  .tips-panel { background: #e3f2fd; border-radius: 14px; padding: 14px 16px; margin-top: 12px; }
  .tips-title { font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.12em; color: #1565c0; margin-bottom: 10px; }
  .tip-row { display: flex; gap: 8px; font-size: 12.5px; color: #1a237e; line-height: 1.5; margin-bottom: 7px; }
  .tip-num { min-width: 16px; font-weight: 700; opacity: 0.5; }

  /* Install banner */
  #install-banner {
    display: none;
    position: fixed; bottom: 0; left: 0; right: 0;
    background: #1565c0; color: #fff;
    padding: 14px 20px calc(14px + env(safe-area-inset-bottom));
    text-align: center; font-size: 13px; z-index: 100;
  }
  #install-banner button { background: rgba(255,255,255,0.2); color: #fff; border: none; border-radius: 8px; padding: 6px 14px; font-size: 12px; font-weight: 700; margin-left: 10px; }
  #install-banner .dismiss { background: none; opacity: 0.6; font-size: 11px; }
</style>
</head>
<body>

<div id="header">
  <div class="eyebrow">6-Week Plan · Sprint → Olympic</div>
  <h1>Olympic Triathlon<br>Training Plan</h1>
  <div class="distance-pills">
    <span>🏊 1,600m Swim</span>
    <span>🚴 24 mi Bike</span>
    <span>🏃 6 mi Run</span>
  </div>
  <div class="meta">31M · 5′9″ · 195 lbs · Sprint time 1:25</div>
  <div id="progress-wrap">
    <div id="progress-label"><span>Overall progress</span><span id="progress-text">0 / 42 sessions</span></div>
    <div id="progress-track"><div id="progress-bar"></div></div>
  </div>
</div>

<div id="tab-scroll"><div id="tabs"></div></div>

<div id="main">
  <div id="week-eyebrow"></div>
  <div id="week-title"></div>
  <div id="week-sub"></div>
  <div id="days"></div>
  <div id="legend" class="panel">
    <div class="panel-title">Intensity Guide</div>
    <div class="legend-row"><span class="pip-dot" style="background:#43a047"></span><strong>Easy</strong> — Can hold a full conversation</div>
    <div class="legend-row"><span class="pip-dot" style="background:#fb8c00"></span><strong>Moderate</strong> — Comfortably uncomfortable</div>
    <div class="legend-row"><span class="pip-dot" style="background:#e53935"></span><strong>Hard</strong> — Can speak in short sentences</div>
    <div class="legend-row"><span class="pip-dot" style="background:#e91e63"></span><strong>Race</strong> — Everything you have</div>
  </div>
  <div class="tips-panel">
    <div class="tips-title">Coach's Notes</div>
    <div class="tip-row"><span class="tip-num">1.</span><span>Never skip two rest days in a row — recovery is training.</span></div>
    <div class="tip-row"><span class="tip-num">2.</span><span>Brick workouts (Bike → Run) are non-negotiable. The leg transition is the hardest skill in triathlon.</span></div>
    <div class="tip-row"><span class="tip-num">3.</span><span>Nutrition: practice race-day fueling every long session. Nothing new on race day.</span></div>
    <div class="tip-row"><span class="tip-num">4.</span><span>Swim drills beat swim volume at this stage. Form beats distance.</span></div>
    <div class="tip-row"><span class="tip-num">5.</span><span>If something hurts (not soreness), take an extra rest day. You have time.</span></div>
  </div>
</div>

<div id="install-banner">
  📲 Add to Home Screen for daily access
  <button onclick="document.getElementById('install-banner').style.display='none'">Got it</button>
  <button class="dismiss" onclick="localStorage.setItem('bannerDismissed','1');document.getElementById('install-banner').style.display='none'">Don't show again</button>
</div>

<script>
const WEEKS = [
  {
    label:"Week 1", theme:"Base Building",
    subtitle:"Establish aerobic base. Low intensity, high consistency.",
    days:[
      {day:"Mon",type:"Swim",intensity:"Easy",    workout:"800m easy continuous swim. Focus on form — bilateral breathing, long strokes, relaxed kick."},
      {day:"Tue",type:"Bike",intensity:"Easy",    workout:"45 min steady ride. Keep HR conversational. Flat route preferred."},
      {day:"Wed",type:"Run", intensity:"Easy",    workout:"2.5 mile easy run. Slow enough to hold a conversation the entire time."},
      {day:"Thu",type:"Swim",intensity:"Moderate",workout:"4×200m with 30 sec rest. Drills: catch-up drill, fingertip drag."},
      {day:"Fri",type:"Rest",intensity:"Rest",    workout:"Full rest or 20 min walk. Prioritize sleep and hydration."},
      {day:"Sat",type:"Brick",intensity:"Moderate",workout:"45 min bike → 15 min run. Practice T2 transition. Keep run effort easy."},
      {day:"Sun",type:"Run", intensity:"Easy",    workout:"3 mile easy long run. Keep heart rate low and steady."},
    ]
  },
  {
    label:"Week 2", theme:"Volume Increase",
    subtitle:"Add 10–15% more volume. Introduce longer bike rides.",
    days:[
      {day:"Mon",type:"Swim",intensity:"Easy",    workout:"1000m continuous. Aim to negative split (swim 2nd half slightly faster)."},
      {day:"Tue",type:"Bike",intensity:"Moderate",workout:"60 min steady. Include 2×10 min slightly harder efforts in the middle."},
      {day:"Wed",type:"Run", intensity:"Easy",    workout:"3 mile easy run + 4×30 sec strides at the end."},
      {day:"Thu",type:"Swim",intensity:"Moderate",workout:"5×200m with 20 sec rest. Focus on maintaining pace across all sets."},
      {day:"Fri",type:"Rest",intensity:"Rest",    workout:"Rest or yoga/stretching. Foam roll quads, calves, lats."},
      {day:"Sat",type:"Brick",intensity:"Moderate",workout:"60 min bike → 20 min run. Aim to hold race pace effort on the run."},
      {day:"Sun",type:"Run", intensity:"Easy",    workout:"4 mile easy long run. Focus on even effort."},
    ]
  },
  {
    label:"Week 3", theme:"Intensity Introduction",
    subtitle:"First hard efforts. Introduce threshold and interval work.",
    days:[
      {day:"Mon",type:"Swim",intensity:"Hard",    workout:"1200m: 400m warm-up + 4×100m hard (15 sec rest) + 400m cool-down."},
      {day:"Tue",type:"Bike",intensity:"Hard",    workout:"70 min: 20 min warm-up + 3×10 min at race pace + 10 min cool-down."},
      {day:"Wed",type:"Run", intensity:"Hard",    workout:"3.5 mile run with middle mile at 10K effort."},
      {day:"Thu",type:"Swim",intensity:"Moderate",workout:"1400m: 200 warm-up + 8×100m (20 sec rest) + 200 cool-down."},
      {day:"Fri",type:"Rest",intensity:"Rest",    workout:"Full rest. Eat well. Legs will be tired — this is normal."},
      {day:"Sat",type:"Brick",intensity:"Hard",   workout:"75 min bike → 25 min run. Target race pace on both legs."},
      {day:"Sun",type:"Run", intensity:"Easy",    workout:"4.5 mile long run. Easy and relaxed."},
    ]
  },
  {
    label:"Week 4", theme:"Race Simulation",
    subtitle:"Simulate race conditions. Build confidence in longer distances.",
    days:[
      {day:"Mon",type:"Swim",intensity:"Moderate",workout:"1600m continuous. This is your race distance — note your time."},
      {day:"Tue",type:"Bike",intensity:"Moderate",workout:"80 min steady at race effort. Dial in nutrition (gel at 40 min)."},
      {day:"Wed",type:"Run", intensity:"Hard",    workout:"4 mile run: 1 easy + 2 at race pace + 1 easy."},
      {day:"Thu",type:"Swim",intensity:"Hard",    workout:"1600m: 400 warm-up + 800m steady + 4×100m hard + 200 cool-down."},
      {day:"Fri",type:"Rest",intensity:"Rest",    workout:"Complete rest. Biggest training day is tomorrow."},
      {day:"Sat",type:"Brick",intensity:"Race",   workout:"90 min bike (≈22 mi) → 30 min run (≈3.5 mi). Race effort throughout."},
      {day:"Sun",type:"Run", intensity:"Easy",    workout:"5 mile easy run. Keep it slow — this is recovery, not training."},
    ]
  },
  {
    label:"Week 5", theme:"Peak + Sharpening",
    subtitle:"Highest load week. Race-specific intensity. Trust the process.",
    days:[
      {day:"Mon",type:"Swim",intensity:"Hard",    workout:"1800m: warm-up + 10×100m (15 sec rest) + cool-down."},
      {day:"Tue",type:"Bike",intensity:"Hard",    workout:"90 min: 30 min warm-up + 40 min at race effort + 20 min cool-down."},
      {day:"Wed",type:"Run", intensity:"Hard",    workout:"5 mile run with miles 2–4 at race pace."},
      {day:"Thu",type:"Swim",intensity:"Moderate",workout:"1600m steady. Goal: match or beat Week 4 continuous time."},
      {day:"Fri",type:"Rest",intensity:"Rest",    workout:"Full rest. Light stretching only. Begin carb-loading tonight."},
      {day:"Sat",type:"Brick",intensity:"Race",   workout:"Full race rehearsal: 1600m swim → T1 → 24 mi bike → T2 → 6 mi run. Proof of concept."},
      {day:"Sun",type:"Run", intensity:"Easy",    workout:"Easy 20 min jog only. Flush legs out. No heroics today."},
    ]
  },
  {
    label:"Week 6", theme:"Taper + Race",
    subtitle:"Cut volume 40–50%. Stay sharp, stay fresh. Race on Saturday.",
    days:[
      {day:"Mon",type:"Swim",intensity:"Easy",    workout:"800m easy with 4×50m fast. Shake out the legs, feel the water."},
      {day:"Tue",type:"Bike",intensity:"Easy",    workout:"40 min easy spin. Include 3×2 min at race cadence to stay sharp."},
      {day:"Wed",type:"Run", intensity:"Easy",    workout:"3 mile easy run with 4×20 sec strides at race pace."},
      {day:"Thu",type:"Swim",intensity:"Easy",    workout:"600m easy. Just feel good. No pushing."},
      {day:"Fri",type:"Rest",intensity:"Rest",    workout:"Complete rest. Lay out gear. Hydrate. Eat familiar foods. Sleep early."},
      {day:"Sat",type:"Race",intensity:"Race",    workout:"🏁 RACE DAY — Olympic Triathlon. 1600m swim · 24 mi bike · 6 mi run. You've done the work."},
      {day:"Sun",type:"Rest",intensity:"Rest",    workout:"🎉 Recovery. Light walk if anything. Celebrate — you're an Olympic triathlete."},
    ]
  },
];

const TYPE_COLORS = {
  Swim:  {bg:"#dbeeff", text:"#1565c0"},
  Bike:  {bg:"#e8f5e9", text:"#1b5e20"},
  Run:   {bg:"#fff3e0", text:"#bf360c"},
  Brick: {bg:"#ede7f6", text:"#4527a0"},
  Race:  {bg:"#fce4ec", text:"#880e4f"},
  Rest:  {bg:"#f5f5f5", text:"#757575"},
};
const INTENSITY_COLOR = {Easy:"#43a047",Moderate:"#fb8c00",Hard:"#e53935",Race:"#e91e63",Rest:"#bdbdbd"};
const TYPE_ICON = {Swim:"🏊",Bike:"🚴",Run:"🏃",Brick:"⚡",Race:"🏁",Rest:"😴"};
const TOTAL = WEEKS.reduce((a,w)=>a+w.days.length,0);

// --- State ---
let activeWeek = 0;
let done = {};

function load() {
  try {
    const d = localStorage.getItem('tri_done');
    if (d) done = JSON.parse(d);
    const w = localStorage.getItem('tri_week');
    if (w !== null) activeWeek = parseInt(w);
  } catch(e) {}
}
function save() {
  try {
    localStorage.setItem('tri_done', JSON.stringify(done));
    localStorage.setItem('tri_week', String(activeWeek));
  } catch(e) {}
}

function toggleDone(key) {
  done[key] = !done[key];
  save();
  render();
}

function setWeek(i) {
  activeWeek = i;
  save();
  render();
  window.scrollTo({top: 0, behavior:'smooth'});
}

function updateProgress() {
  const count = Object.values(done).filter(Boolean).length;
  const pct = Math.round(count/TOTAL*100);
  document.getElementById('progress-bar').style.width = pct+'%';
  document.getElementById('progress-text').textContent = count+' / '+TOTAL+' sessions · '+pct+'%';
}

function render() {
  // Tabs
  const tabsEl = document.getElementById('tabs');
  tabsEl.innerHTML = WEEKS.map((w,i)=>
    `<button class="tab${i===activeWeek?' active':''}" onclick="setWeek(${i})">${w.label}</button>`
  ).join('');

  // Week header
  const wk = WEEKS[activeWeek];
  document.getElementById('week-eyebrow').textContent = wk.label+' of 6';
  document.getElementById('week-title').textContent = wk.theme;
  document.getElementById('week-sub').textContent = wk.subtitle;

  // Days
  const daysEl = document.getElementById('days');
  daysEl.innerHTML = wk.days.map((d,i) => {
    const key = `${activeWeek}-${i}`;
    const isDone = !!done[key];
    const tc = TYPE_COLORS[d.type] || TYPE_COLORS.Rest;
    const pipColor = INTENSITY_COLOR[d.intensity] || '#bdbdbd';
    return `
      <div class="day-card${isDone?' done':''}">
        <div class="card-row" onclick="toggleDone('${key}')">
          <div class="day-col">
            <div class="day-name">${d.day}</div>
            <div class="day-icon" style="background:${tc.bg}">${TYPE_ICON[d.type]||'📋'}</div>
          </div>
          <div class="card-body">
            <div class="tag-row">
              <span class="type-tag" style="background:${tc.bg};color:${tc.text}">${d.type}</span>
              <span class="intensity-pip"><span class="pip-dot" style="background:${pipColor}"></span>${d.intensity}</span>
            </div>
            <div class="workout-text">${d.workout}</div>
          </div>
          <div class="card-actions">
            <button class="mark-btn" onclick="event.stopPropagation();toggleDone('${key}')">${isDone?'✓ Done':'Mark done'}</button>
          </div>
        </div>
      </div>`;
  }).join('');

  updateProgress();
}

// Smart default: jump to today's week based on first open date
function smartDefaultWeek() {
  if (localStorage.getItem('tri_week') !== null) return; // user already navigated
  try {
    let start = localStorage.getItem('tri_start');
    if (!start) { start = new Date().toISOString(); localStorage.setItem('tri_start', start); }
    const days = Math.floor((Date.now() - new Date(start)) / 86400000);
    activeWeek = Math.min(Math.floor(days/7), 5);
  } catch(e) {}
}

// Install banner
function maybeShowBanner() {
  const isIOS = /iphone|ipad|ipod/i.test(navigator.userAgent);
  const isStandalone = window.navigator.standalone;
  const dismissed = localStorage.getItem('bannerDismissed');
  if (isIOS && !isStandalone && !dismissed) {
    setTimeout(()=>{ document.getElementById('install-banner').style.display='block'; }, 1500);
  }
}

load();
smartDefaultWeek();
render();
maybeShowBanner();
</script>
</body>
</html>
