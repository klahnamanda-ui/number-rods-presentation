#index.html<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Number Rods – The Maths Hive</title>
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700;800;900&family=Fredoka+One&display=swap" rel="stylesheet">
<style>
:root{--navy:#1A3057;--navy2:#0F1B28;--gold:#E09A10;--unit:52px;--rod-h:44px;}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:'Nunito',sans-serif;background:#E8EBF0;width:100vw;height:100vh;overflow:hidden;display:flex;flex-direction:column;}

/* TOP BAR */
.topbar{background:#fff;border-bottom:2px solid #E2E5EA;height:52px;flex-shrink:0;display:flex;align-items:center;justify-content:space-between;padding:0 18px;box-shadow:0 2px 8px rgba(0,0,0,0.06);}
.tb-brand{font-family:'Fredoka One',cursive;color:var(--navy);font-size:1rem;display:flex;align-items:center;gap:6px;}
.tb-brand span{color:var(--gold);}
.tb-center{display:flex;align-items:center;gap:8px;}
.slide-dots{display:flex;gap:6px;align-items:center;}
.dot{width:10px;height:10px;border-radius:50%;background:#D0D4DC;cursor:pointer;transition:background 0.2s,transform 0.2s;}
.dot.active{background:var(--navy);transform:scale(1.25);}
.slide-label{font-weight:800;font-size:0.78rem;color:#8892A0;margin-left:4px;}
.tb-right{display:flex;align-items:center;gap:8px;}
.tb-btn{font-family:'Nunito',sans-serif;font-weight:800;font-size:0.74rem;padding:5px 12px;border-radius:7px;border:2px solid #E2E5EA;background:#fff;color:var(--navy);cursor:pointer;transition:background 0.15s;}
.tb-btn:hover{background:#F5F6F8;}
.nav-btn{width:32px;height:32px;border-radius:8px;border:2px solid #E2E5EA;background:#fff;color:var(--navy);cursor:pointer;font-size:1rem;display:flex;align-items:center;justify-content:center;font-weight:900;transition:background 0.15s;}
.nav-btn:hover{background:#F0F2F5;}
.nav-btn:disabled{opacity:0.3;cursor:default;}

/* MAIN */
.main{flex:1;display:flex;overflow:hidden;}

/* SIDEBAR — full-width rods showing actual proportional length */
.sidebar{
  width:580px;flex-shrink:0;background:#fff;border-right:2px solid #E2E5EA;
  display:flex;flex-direction:column;padding:12px 16px;gap:6px;
  overflow-y:auto;overflow-x:hidden;
}
.sidebar::-webkit-scrollbar{width:3px;}
.sidebar::-webkit-scrollbar-thumb{background:#D0D4DC;border-radius:2px;}
.sidebar-label{font-size:0.56rem;font-weight:900;letter-spacing:2px;text-transform:uppercase;color:#A0A8B4;margin-bottom:2px;}
.p-rod{
  height:var(--rod-h);border-radius:5px;
  display:flex;align-items:center;justify-content:center;
  cursor:pointer;position:relative;overflow:hidden;
  transition:filter 0.12s,transform 0.12s;flex-shrink:0;
}
.p-rod:hover{filter:brightness(1.1);transform:translateX(4px);}
.p-rod:active{transform:scale(0.97);}
.p-rod .plus{position:absolute;top:3px;right:7px;font-family:'Fredoka One',cursive;font-size:0.7rem;pointer-events:none;z-index:2;opacity:0.65;}

/* SLIDE AREA */
.slide-area{flex:1;display:flex;flex-direction:column;overflow:hidden;padding:12px 14px;}

/* INSTRUCTION + TOGGLE ROW */
.inst-toggle-row{flex-shrink:0;display:flex;align-items:stretch;gap:10px;margin-bottom:10px;}
.instruction-box{flex:1;background:#fff;border:2px solid #E2E5EA;border-radius:12px;padding:10px 16px;display:flex;align-items:flex-start;gap:10px;box-shadow:0 1px 4px rgba(0,0,0,0.05);position:relative;}
.inst-icon{font-size:1.3rem;flex-shrink:0;margin-top:2px;}
.inst-content{flex:1;}
.inst-tag{font-size:0.56rem;font-weight:900;letter-spacing:2px;text-transform:uppercase;color:#A0A8B4;margin-bottom:3px;}
.inst-text{font-size:1.25rem;font-weight:800;color:var(--navy);border:none;outline:none;width:100%;resize:none;font-family:'Nunito',sans-serif;background:transparent;line-height:1.4;}
.inst-edit-hint{font-size:0.58rem;color:#C8CDD6;font-weight:600;position:absolute;bottom:7px;right:10px;}

/* SHOW ADDITION TOGGLE CARD */
.add-toggle-card{flex-shrink:0;background:#fff;border:2px solid #E2E5EA;border-radius:12px;padding:9px 14px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:6px;box-shadow:0 1px 4px rgba(0,0,0,0.05);min-width:120px;}
.add-toggle-label{font-size:0.62rem;font-weight:900;letter-spacing:1.5px;text-transform:uppercase;color:#A0A8B4;text-align:center;line-height:1.2;}
.toggle-wrap{position:relative;width:42px;height:22px;}
.toggle-wrap input{opacity:0;width:0;height:0;}
.toggle-slider{position:absolute;inset:0;background:#D0D4DC;border-radius:20px;cursor:pointer;transition:background 0.2s;}
.toggle-slider::before{content:'';position:absolute;left:3px;top:3px;width:16px;height:16px;border-radius:50%;background:white;transition:transform 0.2s;box-shadow:0 1px 3px rgba(0,0,0,0.2);}
.toggle-wrap input:checked+.toggle-slider{background:#22C55E;}
.toggle-wrap input:checked+.toggle-slider::before{transform:translateX(20px);}

/* ADDITION REVEAL */
.addition-reveal{
  flex-shrink:0;margin-bottom:8px;
  background:linear-gradient(135deg,#EFF6FF 0%,#F0FDF4 100%);
  border:2px solid #BBF7D0;border-radius:12px;
  display:flex;align-items:center;justify-content:center;
  overflow:hidden;
  max-height:0;opacity:0;
  transition:max-height 0.4s cubic-bezier(.4,0,.2,1),opacity 0.3s ease,padding 0.3s ease,margin 0.3s ease;
  padding-top:0;padding-bottom:0;margin-bottom:0;
}
.addition-reveal.visible{max-height:80px;opacity:1;padding:10px 20px;margin-bottom:8px;}
.addition-text{font-family:'Fredoka One',cursive;font-size:2rem;color:var(--navy);letter-spacing:2px;}
.addition-text .num-col{display:inline-block;transition:transform 0.4s cubic-bezier(.18,.89,.32,1.28),opacity 0.3s ease;}
.addition-text .num-col.hidden{transform:scale(0.4);opacity:0;}

/* WORKSPACE — no grid */
.ws-card{flex:1;background:#fff;border:2px solid #E2E5EA;border-radius:12px;overflow:hidden;position:relative;box-shadow:0 1px 4px rgba(0,0,0,0.05);}
.workspace{width:100%;height:100%;position:relative;overflow:hidden;background:#FAFBFC;}
#snapGhost{position:absolute;height:var(--rod-h);border-radius:5px;pointer-events:none;display:none;z-index:9998;border:2px dashed rgba(0,0,0,0.25);opacity:0.35;}

/* RODS */
.rod{position:absolute;height:var(--rod-h);border-radius:5px;display:flex;align-items:center;justify-content:center;cursor:grab;touch-action:none;overflow:hidden;}
.rod.is-dragging{cursor:grabbing;z-index:9999!important;opacity:0.88;}
.rod-num{font-family:'Fredoka One',cursive;font-size:1rem;pointer-events:none;z-index:2;line-height:1;position:absolute;}
.rod-svg{position:absolute;inset:0;width:100%;height:100%;pointer-events:none;}

/* BOTTOM BAR */
.bottombar{background:#fff;border-top:2px solid #E2E5EA;padding:5px 18px;display:flex;align-items:center;justify-content:space-between;flex-shrink:0;}
.bb-brand{font-family:'Fredoka One',cursive;color:var(--navy);font-size:0.78rem;}
.bb-brand span{color:var(--gold);}
.bb-hint{font-size:0.65rem;color:#A0A8B4;font-weight:600;}
.bb-controls{display:flex;align-items:center;gap:12px;}
.tog-group{display:flex;align-items:center;gap:5px;}
.tog-label{font-size:0.7rem;font-weight:700;color:#6B7280;}
.tog-sm{width:34px;height:18px;}
.tog-sm .toggle-slider::before{width:12px;height:12px;}
.toggle-wrap.tog-sm input:checked+.toggle-slider::before{transform:translateX(16px);}
.toggle-wrap.tog-sm input:checked+.toggle-slider{background:var(--navy);}
.mode-pill{display:flex;align-items:center;background:#F0F2F5;border-radius:16px;overflow:hidden;height:26px;border:1px solid #E2E5EA;}
.mode-btn{padding:0 9px;height:100%;font-family:'Nunito',sans-serif;font-weight:800;font-size:0.67rem;color:#8892A0;cursor:pointer;border:none;background:transparent;transition:background 0.15s,color 0.15s;display:flex;align-items:center;}
.mode-btn.active{background:var(--navy);color:#fff;border-radius:14px;}
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
  <div class="tb-brand">🐝 <span>The Maths Hive</span> — Number Rods</div>
  <div class="tb-center">
    <button class="nav-btn" id="prevBtn" onclick="goSlide(-1)" disabled>‹</button>
    <div class="slide-dots" id="slideDots"></div>
    <span class="slide-label" id="slideLabel">Slide 1 of 3</span>
    <button class="nav-btn" id="nextBtn" onclick="goSlide(1)">›</button>
  </div>
  <div class="tb-right">
    <button class="tb-btn" onclick="undoLast()">⟵ Undo</button>
    <button class="tb-btn" onclick="clearUserRods()">↺ Clear</button>
  </div>
</div>

<!-- MAIN -->
<div class="main">
  <div class="sidebar" id="sidebar">
    <div class="sidebar-label">Click to place</div>
  </div>
  <div class="slide-area">
    <div class="inst-toggle-row">
      <div class="instruction-box">
        <div class="inst-icon">👩‍🏫</div>
        <div class="inst-content">
          <div class="inst-tag">Teacher Instruction</div>
          <textarea class="inst-text" id="instText" rows="2" spellcheck="false"></textarea>
        </div>
        <div class="inst-edit-hint">click to edit</div>
      </div>
      <div class="add-toggle-card">
        <div class="add-toggle-label">Show<br>Addition</div>
        <label class="toggle-wrap">
          <input type="checkbox" id="addToggle" onchange="onAddToggle(this.checked)">
          <span class="toggle-slider"></span>
        </label>
      </div>
    </div>

    <div class="addition-reveal" id="addReveal">
      <div class="addition-text" id="addText"></div>
    </div>

    <div class="ws-card">
      <div class="workspace" id="workspace">
        <div id="snapGhost"></div>
      </div>
    </div>
  </div>
</div>

<!-- BOTTOM BAR -->
<div class="bottombar">
  <div class="bb-brand">🐝 <span>The Maths Hive</span></div>
  <div class="bb-hint">Click a rod to place · Drag to move</div>
  <div class="bb-controls">
    <div class="tog-group">
      <span class="tog-label">Snap</span>
      <label class="toggle-wrap tog-sm">
        <input type="checkbox" id="snapToggle" checked>
        <span class="toggle-slider"></span>
      </label>
    </div>
    <div class="mode-pill">
      <button class="mode-btn active" id="modeNum"  onclick="setMode('number')">123</button>
      <button class="mode-btn"        id="modeDot"  onclick="setMode('dots')">⬤⬤</button>
      <button class="mode-btn"        id="modeNone" onclick="setMode('none')">—</button>
    </div>
  </div>
</div>

<script>
const DEFS=[
  {n:1, color:'#E0E0E0',textCol:'#444',  dotCol:'rgba(0,0,0,0.35)'},
  {n:2, color:'#D42A20',textCol:'#fff',  dotCol:'rgba(255,255,255,0.85)'},
  {n:3, color:'#5CB82A',textCol:'#fff',  dotCol:'rgba(255,255,255,0.85)'},
  {n:4, color:'#B02898',textCol:'#fff',  dotCol:'rgba(255,255,255,0.85)'},
  {n:5, color:'#E8C000',textCol:'#fff',  dotCol:'rgba(255,255,255,0.85)'},
  {n:6, color:'#207828',textCol:'#fff',  dotCol:'rgba(255,255,255,0.85)'},
  {n:7, color:'#1A1A1A',textCol:'#fff',  dotCol:'rgba(255,255,255,0.75)'},
  {n:8, color:'#7A3A10',textCol:'#fff',  dotCol:'rgba(255,255,255,0.85)'},
  {n:9, color:'#1858C0',textCol:'#fff',  dotCol:'rgba(255,255,255,0.85)'},
  {n:10,color:'#D84010',textCol:'#fff',  dotCol:'rgba(255,255,255,0.85)'},
];
const DEF_BY_N=n=>DEFS.find(d=>d.n===n);

const UNIT=52, ROD_H=44;
const NUM_SLIDES=3;
const SNAP=()=>document.getElementById('snapToggle').checked;

const SLIDE_CONFIG=[
  {instruction:"What number is missing?", preset:[{n:10,row:0},{n:3,row:1}],  addition:"3 + 7 = 10"},
  {instruction:"What number is missing?", preset:[{n:10,row:0},{n:5,row:1}],  addition:"5 + 5 = 10"},
  {instruction:"What number is missing?", preset:[{n:10,row:0},{n:9,row:1}],  addition:"9 + 1 = 10"},
];

const slideInstructions=SLIDE_CONFIG.map(s=>s.instruction);
const slideUserRods=[[],[],[]];

let currentSlide=0, labelMode='number', userPlaced=[], presetEls=[], zTop=10;

const workspace=document.getElementById('workspace');
const ghost=document.getElementById('snapGhost');
const instText=document.getElementById('instText');
const addReveal=document.getElementById('addReveal');
const addText=document.getElementById('addText');
const addToggle=document.getElementById('addToggle');

// ── ADDITION ──
function onAddToggle(on){
  const cfg=SLIDE_CONFIG[currentSlide];
  if(on&&cfg.addition){addReveal.classList.add('visible');renderAdditionText(cfg.addition);}
  else addReveal.classList.remove('visible');
}
function renderAdditionText(str){
  addText.innerHTML='';
  str.split(/(\d+)/).forEach((p,i)=>{
    const sp=document.createElement('span');
    sp.className='num-col hidden';
    const isNum=/^\d+$/.test(p);
    if(isNum){const def=DEF_BY_N(parseInt(p));sp.style.color=def?def.color:'var(--navy)';sp.style.textShadow=def?`0 2px 6px ${def.color}55`:''}
    else sp.style.color='#64748B';
    sp.textContent=p;
    addText.appendChild(sp);
    setTimeout(()=>sp.classList.remove('hidden'),i*60+50);
  });
}

// ── SLIDES ──
function buildDots(){
  const c=document.getElementById('slideDots');c.innerHTML='';
  for(let i=0;i<NUM_SLIDES;i++){
    const d=document.createElement('div');
    d.className='dot'+(i===currentSlide?' active':'');
    d.onclick=()=>jumpSlide(i);
    c.appendChild(d);
  }
}
function jumpSlide(i){saveSlide();currentSlide=i;loadSlide();}
function goSlide(dir){saveSlide();currentSlide=Math.max(0,Math.min(NUM_SLIDES-1,currentSlide+dir));loadSlide();}

function saveSlide(){
  slideInstructions[currentSlide]=instText.value;
  slideUserRods[currentSlide]=userPlaced.map(d=>({n:d.n,x:d.x,y:d.y}));
  userPlaced.forEach(d=>d.el.remove());userPlaced=[];
  presetEls.forEach(e=>e.remove());presetEls=[];
}
function loadSlide(){
  document.getElementById('prevBtn').disabled=currentSlide===0;
  document.getElementById('nextBtn').disabled=currentSlide===NUM_SLIDES-1;
  document.getElementById('slideLabel').textContent=`Slide ${currentSlide+1} of ${NUM_SLIDES}`;
  buildDots();
  instText.value=slideInstructions[currentSlide];
  autoResizeTextarea();
  addToggle.checked=false;
  addReveal.classList.remove('visible');
  renderPresets();
  (slideUserRods[currentSlide]||[]).forEach(s=>placeUserRod(DEF_BY_N(s.n),s.x,s.y,false));
}

function renderPresets(){
  requestAnimationFrame(()=>{
    const wsH=workspace.offsetHeight;
    const third=Math.floor(wsH/3);
    // snap both rows to nearest rod-height grid within upper third
    const row1Y=Math.round((third - ROD_H*2 - 10) / ROD_H) * ROD_H;
    const row0Y=row1Y + ROD_H;
    const ROW_Y={0:row0Y, 1:row1Y};
    SLIDE_CONFIG[currentSlide].preset.forEach(p=>{
      const def=DEF_BY_N(p.n), rodW=def.n*UNIT;
      const el=document.createElement('div');
      el.className='rod';
      el.style.cssText=`width:${rodW-2}px;background:${def.color};left:${UNIT}px;top:${ROW_Y[p.row]}px;z-index:${++zTop};cursor:grab;`;
      el.innerHTML=buildRodSVG(def,rodW-2,ROD_H,labelMode)+buildNumSpan(def,labelMode);
      workspace.appendChild(el);
      presetEls.push(el);
      attachDragPreset(el,def);
    });
  });
}

function autoResizeTextarea(){instText.style.height='auto';instText.style.height=instText.scrollHeight+'px';}
instText.addEventListener('input',autoResizeTextarea);

// ── SIDEBAR — full actual-length rods ──
function buildSidebar(){
  const sb=document.getElementById('sidebar');
  [...sb.querySelectorAll('.p-rod')].forEach(e=>e.remove());
  // sidebar inner width ≈ 580 - 32px padding = 548px; unit=52 so rod-10=520px fits
  DEFS.forEach(def=>{
    const rodW=def.n*UNIT-2;
    const el=document.createElement('div');
    el.className='p-rod';
    el.style.width=rodW+'px';
    el.style.background=def.color;
    el.innerHTML=buildRodSVG(def,rodW,ROD_H,labelMode)+buildNumSpan(def,labelMode);
    const plus=document.createElement('span');
    plus.className='plus';plus.textContent='+';plus.style.color=def.textCol;
    el.appendChild(plus);
    el.addEventListener('click',()=>spawnUserRod(def));
    sb.appendChild(el);
  });
}
function refreshSidebar(){
  document.querySelectorAll('.p-rod').forEach((el,i)=>{
    const def=DEFS[i], rodW=def.n*UNIT-2;
    el.innerHTML=buildRodSVG(def,rodW,ROD_H,labelMode)+buildNumSpan(def,labelMode);
    const plus=document.createElement('span');
    plus.className='plus';plus.textContent='+';plus.style.color=def.textCol;
    el.appendChild(plus);
  });
}

// ── SVG ──
function buildRodSVG(def,w,h,mode){
  const n=def.n;let lines='';
  if(mode!=='number'){for(let i=1;i<n;i++){const x=(w/n)*i;lines+=`<line x1="${x}" y1="3" x2="${x}" y2="${h-3}" stroke="rgba(255,255,255,0.22)" stroke-width="1.5"/>`;}}
  let dots='';
  if(mode==='dots'){const r=Math.min(7,h*0.28);for(let i=0;i<n;i++){const cx=(w/n)*i+(w/n)/2,cy=h/2;dots+=`<circle cx="${cx}" cy="${cy}" r="${r}" fill="${def.dotCol}"/>`;}}
  return`<svg class="rod-svg" xmlns="http://www.w3.org/2000/svg">${lines}${dots}</svg>`;
}
function buildNumSpan(def,mode){
  return mode==='number'?`<span class="rod-num" style="color:${def.textCol}">${def.n}</span>`:'';
}

// ── PLACE ──
function spawnUserRod(def){
  const s=userPlaced.length%8;
  placeUserRod(def,56+s*18,20+s*12,true);
}
function placeUserRod(def,x,y,animate){
  const rodW=def.n*UNIT-2;
  const el=document.createElement('div');
  el.className='rod';
  el.style.cssText=`width:${rodW}px;background:${def.color};left:${x}px;top:${y}px;z-index:${++zTop};`;
  el.innerHTML=buildRodSVG(def,rodW,ROD_H,labelMode)+buildNumSpan(def,labelMode);
  if(animate){
    el.style.transform='scale(0.4)';el.style.opacity='0';
    el.style.transition='transform 0.18s cubic-bezier(.18,.89,.32,1.28),opacity 0.12s ease';
    requestAnimationFrame(()=>requestAnimationFrame(()=>{el.style.transform='';el.style.opacity='1';setTimeout(()=>el.style.transition='',220);}));
  }
  const data={el,n:def.n,x,y};
  userPlaced.push(data);workspace.appendChild(el);
  attachDragUser(el,data);
}
function refreshAllLabels(){
  userPlaced.forEach(d=>{const def=DEF_BY_N(d.n),rodW=def.n*UNIT-2;d.el.innerHTML=buildRodSVG(def,rodW,ROD_H,labelMode)+buildNumSpan(def,labelMode);});
  presetEls.forEach(el=>{const w=parseInt(el.style.width)+2,n=Math.round(w/UNIT),def=DEF_BY_N(n);if(def)el.innerHTML=buildRodSVG(def,w-2,ROD_H,labelMode)+buildNumSpan(def,labelMode);});
}

// ── DRAG ──
function snapPos(x,y,rodW){
  const r=workspace.getBoundingClientRect();
  const sx=clamp(Math.round(x/UNIT)*UNIT,       0, r.width -rodW);
  const sy=clamp(Math.round(y/ROD_H)*ROD_H,     0, r.height-ROD_H);
  return{sx,sy};
}
function makeDragger(el,getW,onEnd){
  let px0,py0,ex0,ey0,active=false;
  el.addEventListener('pointerdown',e=>{
    e.preventDefault();active=true;
    px0=e.clientX;py0=e.clientY;
    ex0=parseFloat(el.style.left);ey0=parseFloat(el.style.top);
    el.setPointerCapture(e.pointerId);
    el.classList.add('is-dragging');el.style.zIndex=++zTop;
  });
  el.addEventListener('pointermove',e=>{
    if(!active)return;
    const r=workspace.getBoundingClientRect();
    const rodW=getW();
    const rx=clamp(ex0+e.clientX-px0,0,r.width -rodW);
    const ry=clamp(ey0+e.clientY-py0,0,r.height-ROD_H);
    if(SNAP()){
      const{sx,sy}=snapPos(rx,ry,rodW);
      el.style.left=sx+'px';el.style.top=sy+'px';
    } else {
      el.style.left=rx+'px';el.style.top=ry+'px';
    }
  });
  el.addEventListener('pointerup',e=>{
    if(!active)return;active=false;
    el.classList.remove('is-dragging');ghost.style.display='none';
    if(onEnd){
      const fx=parseFloat(el.style.left), fy=parseFloat(el.style.top);
      onEnd(fx,fy);
    }
  });
}
function attachDragUser(el,data){
  makeDragger(el,()=>DEF_BY_N(data.n).n*UNIT-2,(fx,fy)=>{data.x=fx;data.y=fy;});
}
function attachDragPreset(el,def){
  makeDragger(el,()=>def.n*UNIT-2,null);
}

function showGhost(def,x,y){
  if(!SNAP()){ghost.style.display='none';return;}
  const sx=Math.round(x/UNIT)*UNIT,sy=Math.round(y/ROD_H)*ROD_H;
  ghost.style.cssText=`display:block;width:${def.n*UNIT-2}px;left:${sx}px;top:${sy}px;background:${def.color}55;border-color:${def.color};height:${ROD_H}px;border-radius:5px;position:absolute;pointer-events:none;z-index:9998;border:2px dashed rgba(0,0,0,0.25);opacity:0.35;`;
}
function clamp(v,lo,hi){return Math.max(lo,Math.min(hi,v));}

// ── CONTROLS ──
function undoLast(){
  if(!userPlaced.length)return;
  const r=userPlaced.pop();
  r.el.style.transition='transform 0.15s ease,opacity 0.13s ease';
  r.el.style.transform='scale(0.3)';r.el.style.opacity='0';
  setTimeout(()=>r.el.remove(),165);
}
function clearUserRods(){
  [...userPlaced].reverse().forEach((r,i)=>{
    setTimeout(()=>{r.el.style.transition='transform 0.15s ease,opacity 0.13s ease';r.el.style.transform='scale(0.3)';r.el.style.opacity='0';setTimeout(()=>r.el.remove(),160);},i*20);
  });
  userPlaced=[];
}
function setMode(m){
  labelMode=m;
  ['Num','Dot','None'].forEach(k=>document.getElementById('mode'+k).classList.toggle('active',m===k.toLowerCase()));
  refreshAllLabels();refreshSidebar();
}

// ── INIT ──
buildSidebar();
loadSlide();
</script>
</body>
</html>
