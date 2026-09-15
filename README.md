<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Cinemática de tijera — calculadora de mecanismo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;500;600&family=IBM+Plex+Mono:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --paper:#F5F3EC;
    --grid:#E1DCCB;
    --ink:#1C1E22;
    --ink-soft:#5B5A52;
    --panel:#FFFFFF;
    --line:#C9C4B2;
    --teal:#2B6E5E;
    --teal-dark:#1B4B40;
    --red:#B23A2E;
    --red-soft:#D98F84;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    background:
      linear-gradient(var(--grid) 1px, transparent 1px) 0 0/32px 32px,
      linear-gradient(90deg, var(--grid) 1px, transparent 1px) 0 0/32px 32px,
      var(--paper);
    color:var(--ink);
    font-family:'IBM Plex Sans', sans-serif;
    min-height:100vh;
  }
  header{
    max-width:1180px;
    margin:0 auto;
    padding:48px 24px 24px;
  }
  header h1{
    font-size:32px;
    font-weight:600;
    margin:0 0 8px;
    letter-spacing:-0.01em;
  }
  header p{
    font-size:15px;
    color:var(--ink-soft);
    max-width:60ch;
    line-height:1.6;
    margin:0;
  }
  main{
    max-width:1180px;
    margin:0 auto;
    padding:0 24px 64px;
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:24px;
  }
  @media (max-width:900px){
    main{grid-template-columns:1fr;}
  }
  .panel{
    background:var(--panel);
    border:1px solid var(--ink);
    padding:28px;
  }
  .panel h2{
    font-size:16px;
    font-weight:600;
    margin:0 0 4px;
  }
  .panel .sub{
    font-size:13px;
    color:var(--ink-soft);
    margin:0 0 20px;
    line-height:1.5;
  }
  .field-row{
    display:flex;
    align-items:center;
    gap:10px;
    padding:9px 0;
    border-bottom:1px solid var(--line);
  }
  .field-row:last-of-type{border-bottom:none;}
  .field-lbl{
    width:110px;
    font-size:14px;
  }
  .chip{
    font-family:'IBM Plex Mono', monospace;
    font-size:11px;
    padding:5px 10px;
    border:1px solid var(--ink);
    background:transparent;
    color:var(--ink-soft);
    cursor:pointer;
  }
  .chip.on{
    background:var(--teal);
    border-color:var(--teal);
    color:#fff;
  }
  input[type=number]{
    width:100px;
    font-family:'IBM Plex Mono', monospace;
    font-size:14px;
    padding:6px 8px;
    border:1px solid var(--line);
    background:var(--paper);
    color:var(--ink);
  }
  input[type=number]:disabled{
    color:var(--ink-soft);
    background:#EFEDE4;
  }
  input[type=range]{
    flex:1;
    accent-color:var(--teal);
  }
  .unit{
    font-family:'IBM Plex Mono', monospace;
    font-size:12px;
    color:var(--ink-soft);
    width:28px;
  }
  .err{
    font-size:12px;
    color:var(--red);
    min-height:16px;
    margin-top:6px;
    font-family:'IBM Plex Mono', monospace;
  }
  svg{display:block; margin-top:16px;}
  .readout{
    display:flex;
    gap:24px;
    margin-top:12px;
    padding-top:12px;
    border-top:1px solid var(--line);
    flex-wrap:wrap;
  }
  .readout div{font-size:12px; color:var(--ink-soft);}
  .readout span{
    display:block;
    font-family:'IBM Plex Mono', monospace;
    font-size:18px;
    font-weight:500;
    color:var(--ink);
  }
  .range-row{
    display:flex;
    align-items:center;
    gap:10px;
    margin-top:16px;
  }
  footer{
    max-width:1180px;
    margin:0 auto;
    padding:0 24px 56px;
    font-size:13px;
    color:var(--ink-soft);
    line-height:1.7;
    border-top:1px solid var(--line);
    padding-top:24px;
  }
  footer code{
    font-family:'IBM Plex Mono', monospace;
    background:#EFEDE4;
    padding:1px 5px;
  }
</style>
</head>
<body>

<header>
  <h1>Cinemática de un mecanismo de tijera</h1>
  <p>Relación H = L·sinθ, W = L·cosθ para un mecanismo de tijera simétrico. Introduce dos valores en la calculadora general, o define un rango de ángulo físico y observa el recorrido real del mecanismo.</p>
</header>

<main>

  <section class="panel">
    <h2>Calculadora general</h2>
    <p class="sub">Marca 2 datos conocidos. Los otros 2 se calculan solos a partir de H = L·sinθ, W = L·cosθ.</p>

    <div class="field-row">
      <span class="field-lbl">L (brazo)</span>
      <button class="chip" id="chip-L">conocido</button>
      <input type="number" id="in-L">
      <span class="unit">mm</span>
    </div>
    <div class="field-row">
      <span class="field-lbl">&theta; (ángulo)</span>
      <button class="chip" id="chip-t">conocido</button>
      <input type="number" id="in-t">
      <span class="unit">&deg;</span>
    </div>
    <div class="field-row">
      <span class="field-lbl">H (altura)</span>
      <button class="chip" id="chip-H">conocido</button>
      <input type="number" id="in-H">
      <span class="unit">mm</span>
    </div>
    <div class="field-row">
      <span class="field-lbl">W (ancho)</span>
      <button class="chip" id="chip-W">conocido</button>
      <input type="number" id="in-W">
      <span class="unit">mm</span>
    </div>
    <div class="err" id="err-calc"></div>

    <svg id="svg-calc" width="100%" viewBox="0 0 500 300" role="img">
      <title>Diagrama del mecanismo, calculadora general</title>
      <defs><marker id="arrow1" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke" stroke-width="1.5"/></marker></defs>
      <line id="c-arm1" stroke="#2B6E5E" stroke-width="5" stroke-linecap="round"/>
      <line id="c-arm2" stroke="#2B6E5E" stroke-width="5" stroke-linecap="round"/>
      <circle id="c-tl" r="6" fill="#1B4B40"/><circle id="c-tr" r="6" fill="#1B4B40"/>
      <circle id="c-bl" r="6" fill="#1B4B40"/><circle id="c-br" r="6" fill="#1B4B40"/>
      <circle cx="250" cy="150" r="5" fill="#B23A2E"/>
    </svg>

    <div class="readout">
      <div>Longitud L<span id="out-L">—</span></div>
      <div>Ángulo &theta;<span id="out-t">—</span></div>
      <div>Altura H<span id="out-H">—</span></div>
      <div>Ancho W<span id="out-W">—</span></div>
    </div>
  </section>

  <section class="panel">
    <h2>Simulador de recorrido</h2>
    <p class="sub">Define el rango físico de &theta; (el límite real que permiten tus ranuras y varillas) y desplaza el mecanismo dentro de ese rango.</p>

    <div class="field-row">
      <span class="field-lbl">L (brazo)</span>
      <input type="number" id="r-L" value="215.407">
      <span class="unit">mm</span>
    </div>
    <div class="field-row">
      <span class="field-lbl">&theta; mínimo</span>
      <input type="number" id="r-min" value="15">
      <span class="unit">&deg;</span>
    </div>
    <div class="field-row">
      <span class="field-lbl">&theta; máximo</span>
      <input type="number" id="r-max" value="75">
      <span class="unit">&deg;</span>
    </div>

    <div class="range-row">
      <span class="unit" style="width:auto;">&theta;=</span>
      <input type="range" id="r-slider" min="15" max="75" step="0.1" value="45">
      <span class="unit" id="r-theta-val" style="width:auto;">45.0&deg;</span>
    </div>
    <div class="err" id="err-range"></div>

    <svg id="svg-range" width="100%" viewBox="0 0 500 300" role="img">
      <title>Diagrama del mecanismo, simulador de recorrido</title>
      <line id="ghost-arm1" stroke="#C9C4B2" stroke-width="3" stroke-linecap="round"/>
      <line id="ghost-arm2" stroke="#C9C4B2" stroke-width="3" stroke-linecap="round"/>
      <line id="ghost2-arm1" stroke="#C9C4B2" stroke-width="3" stroke-linecap="round"/>
      <line id="ghost2-arm2" stroke="#C9C4B2" stroke-width="3" stroke-linecap="round"/>
      <line id="r-arm1" stroke="#B23A2E" stroke-width="5" stroke-linecap="round"/>
      <line id="r-arm2" stroke="#B23A2E" stroke-width="5" stroke-linecap="round"/>
      <circle id="r-tl" r="6" fill="#1B4B40"/><circle id="r-tr" r="6" fill="#1B4B40"/>
      <circle id="r-bl" r="6" fill="#1B4B40"/><circle id="r-br" r="6" fill="#1B4B40"/>
      <circle cx="250" cy="150" r="5" fill="#B23A2E"/>
    </svg>

    <div class="readout">
      <div>H actual<span id="out-rH">—</span></div>
      <div>W actual<span id="out-rW">—</span></div>
      <div>Recorrido &Delta;H<span id="out-dH">—</span></div>
      <div>Recorrido &Delta;W<span id="out-dW">—</span></div>
    </div>
  </section>

  <section class="panel" style="grid-column:1/-1;">
    <h2>Modelo con pivotes fijos y deslizantes</h2>
    <p class="sub">Para una mesa de tijera real: un pivote fijo abajo y otro fijo arriba, alineados en la misma columna; los otros dos extremos deslizan en ranuras. Usa los mismos L y &theta; de la calculadora general. El origen (0,0) es el pivote fijo inferior.</p>

    <div style="display:grid; grid-template-columns:1fr 1fr; gap:24px;">
      <div>
        <svg id="svg-pivot" width="100%" viewBox="0 0 500 320" role="img">
          <title>Diagrama de coordenadas de los pivotes fijos y deslizantes</title>
          <line id="p-axisx" stroke="#B23A2E" stroke-width="1"/>
          <line id="p-axisy" stroke="#2B6E5E" stroke-width="1"/>
          <line id="p-arm1" stroke="#8A8672" stroke-width="5" stroke-linecap="round"/>
          <line id="p-arm2" stroke="#8A8672" stroke-width="5" stroke-linecap="round"/>
          <circle id="p-brf" r="7" fill="#1B4B40"/>
          <circle id="p-trf" r="7" fill="#1B4B40"/>
          <circle id="p-bls" r="7" fill="none" stroke="#B23A2E" stroke-width="3"/>
          <circle id="p-tls" r="7" fill="none" stroke="#B23A2E" stroke-width="3"/>
          <text id="p-brf-lbl" font-family="IBM Plex Mono, monospace" font-size="11" fill="#1C1E22"></text>
          <text id="p-trf-lbl" font-family="IBM Plex Mono, monospace" font-size="11" fill="#1C1E22"></text>
          <text id="p-bls-lbl" font-family="IBM Plex Mono, monospace" font-size="11" fill="#1C1E22"></text>
          <text id="p-tls-lbl" font-family="IBM Plex Mono, monospace" font-size="11" fill="#1C1E22"></text>
        </svg>
      </div>
      <div>
        <div class="field-row"><span class="field-lbl">Fijo inferior</span><span style="font-family:'IBM Plex Mono',monospace; font-size:13px;" id="pv-brf">(0, 0)</span></div>
        <div class="field-row"><span class="field-lbl">Deslizante inf.</span><span style="font-family:'IBM Plex Mono',monospace; font-size:13px;" id="pv-bls">—</span></div>
        <div class="field-row"><span class="field-lbl">Fijo superior</span><span style="font-family:'IBM Plex Mono',monospace; font-size:13px;" id="pv-trf">—</span></div>
        <div class="field-row"><span class="field-lbl">Deslizante sup.</span><span style="font-family:'IBM Plex Mono',monospace; font-size:13px;" id="pv-tls">—</span></div>
        <div class="readout" style="border-top:none; padding-top:16px;">
          <div>Ángulo de apertura en el cruce<span id="pv-aperture">—</span></div>
          <div>Recorrido del pasador desde cierre total (&theta;=0)<span id="pv-stroke">—</span></div>
          <div>Elevación desde cierre total<span id="pv-lift">—</span></div>
        </div>
      </div>
    </div>
  </section>

</main>

<footer>
  Fórmulas usadas — mecanismo de tijera simétrico, brazos cruzados en su punto medio: <code>H = L·sin&theta;</code>, <code>W = L·cos&theta;</code>, <code>L = &radic;(H&sup2;+W&sup2;)</code>. Con pivotes fijos alineados verticalmente en un lado y ranuras en el otro, ambos pasadores deslizantes se mueven la misma distancia W respecto a la columna fija — el mecanismo sube sin desplazarse lateralmente. El ángulo de apertura en el cruce es <code>180&deg; − 2&theta;</code>. El recorrido &Delta;H y &Delta;W es la diferencia entre las posiciones extremas &theta;<sub>min</sub> y &theta;<sub>max</sub> que definas. Este cálculo no incluye interferencias mecánicas (topes, colisión de piezas) — verifica siempre contra tu CAD.
</footer>

<script>
// ---- Calculadora general ----
let known = ['L','t'];
const vals = { L:215.407, t:21.8, H:80, W:200 };

function round(v){ return Math.round(v*1000)/1000; }

function toggleKnown(key){
  if(known.includes(key)){
    if(known.length<=2) return;
    known = known.filter(k=>k!==key);
  } else {
    known.push(key);
    if(known.length>2) known.shift();
  }
  refreshChips();
  computeCalc();
}
['L','t','H','W'].forEach(k=>{
  document.getElementById('chip-'+k).addEventListener('click', ()=>toggleKnown(k));
  document.getElementById('in-'+k).addEventListener('input', ()=>{
    if(!known.includes(k)) return;
    const v = parseFloat(document.getElementById('in-'+k).value);
    if(!isNaN(v)) vals[k]=v;
    computeCalc();
  });
});

function refreshChips(){
  ['L','t','H','W'].forEach(k=>{
    const chip = document.getElementById('chip-'+k);
    const input = document.getElementById('in-'+k);
    const isKnown = known.includes(k);
    chip.classList.toggle('on', isKnown);
    input.disabled = !isKnown;
    if(isKnown) input.value = round(vals[k]);
  });
}

function setErr(id, msg){ document.getElementById(id).textContent = msg||''; }

function computeCalc(){
  setErr('err-calc','');
  const [a,b] = known;
  const pair = [a,b].sort().join('');
  let {L,t,H,W} = vals;
  const rad = t*Math.PI/180;
  try{
    if(pair==='Lt'){ H=L*Math.sin(rad); W=L*Math.cos(rad); }
    else if(pair==='HL'){ if(H>L) throw 'H no puede ser mayor que L'; t=Math.asin(H/L)*180/Math.PI; W=Math.sqrt(L*L-H*H); }
    else if(pair==='LW'){ if(W>L) throw 'W no puede ser mayor que L'; t=Math.acos(W/L)*180/Math.PI; H=Math.sqrt(L*L-W*W); }
    else if(pair==='Ht'){ L=H/Math.sin(rad); W=L*Math.cos(rad); }
    else if(pair==='Wt'){ L=W/Math.cos(rad); H=L*Math.sin(rad); }
    else if(pair==='HW'){ L=Math.sqrt(H*H+W*W); t=Math.atan2(H,W)*180/Math.PI; }
    if(t<0||t>90) throw 'El ángulo debe estar entre 0 y 90°';
    Object.assign(vals,{L,t,H,W});
  } catch(e){ setErr('err-calc', typeof e==='string'?e:'Datos incompatibles'); }

  ['L','t','H','W'].forEach(k=>{
    if(!known.includes(k)) document.getElementById('in-'+k).value = round(vals[k]);
  });
  document.getElementById('out-L').textContent = round(vals.L)+' mm';
  document.getElementById('out-t').textContent = round(vals.t)+'°';
  document.getElementById('out-H').textContent = round(vals.H)+' mm';
  document.getElementById('out-W').textContent = round(vals.W)+' mm';
  drawCross('c', vals.L, vals.t, 250, 150, 1.35);
  updatePivotPanel();
}

// ---- Modelo con pivotes fijos y deslizantes ----
function updatePivotPanel(){
  const { L, t, H, W } = vals;
  document.getElementById('pv-brf').textContent = '(0, 0)';
  document.getElementById('pv-bls').textContent = '(-'+round(W)+', 0)';
  document.getElementById('pv-trf').textContent = '(0, '+round(H)+')';
  document.getElementById('pv-tls').textContent = '(-'+round(W)+', '+round(H)+')';

  const aperture = 180 - 2*t;
  const stroke = L - W;
  document.getElementById('pv-aperture').textContent = round(aperture)+'°';
  document.getElementById('pv-stroke').textContent = round(stroke)+' mm';
  document.getElementById('pv-lift').textContent = round(H)+' mm';

  // diagram: origin (bottom-right fixed) placed at pixel (400,270), x grows left, y grows up
  const ox=400, oy=270, k=1.35;
  const toPx = (x,y) => [ox - x*k, oy - y*k];
  const [brfx,brfy] = toPx(0,0);
  const [blsx,blsy] = toPx(-W,0);
  const [trfx,trfy] = toPx(0,H);
  const [tlsx,tlsy] = toPx(-W,H);

  document.getElementById('p-axisx').setAttribute('x1', ox+30); document.getElementById('p-axisx').setAttribute('y1', oy);
  document.getElementById('p-axisx').setAttribute('x2', ox-W*k-30); document.getElementById('p-axisx').setAttribute('y2', oy);
  document.getElementById('p-axisy').setAttribute('x1', ox); document.getElementById('p-axisy').setAttribute('y1', oy+20);
  document.getElementById('p-axisy').setAttribute('x2', ox); document.getElementById('p-axisy').setAttribute('y2', oy-H*k-20);

  document.getElementById('p-arm1').setAttribute('x1', blsx); document.getElementById('p-arm1').setAttribute('y1', blsy);
  document.getElementById('p-arm1').setAttribute('x2', trfx); document.getElementById('p-arm1').setAttribute('y2', trfy);
  document.getElementById('p-arm2').setAttribute('x1', brfx); document.getElementById('p-arm2').setAttribute('y1', brfy);
  document.getElementById('p-arm2').setAttribute('x2', tlsx); document.getElementById('p-arm2').setAttribute('y2', tlsy);

  document.getElementById('p-brf').setAttribute('cx',brfx); document.getElementById('p-brf').setAttribute('cy',brfy);
  document.getElementById('p-trf').setAttribute('cx',trfx); document.getElementById('p-trf').setAttribute('cy',trfy);
  document.getElementById('p-bls').setAttribute('cx',blsx); document.getElementById('p-bls').setAttribute('cy',blsy);
  document.getElementById('p-tls').setAttribute('cx',tlsx); document.getElementById('p-tls').setAttribute('cy',tlsy);

  const lbl = (id,x,y,dx,dy,text) => { const el=document.getElementById(id); el.setAttribute('x',x+dx); el.setAttribute('y',y+dy); el.textContent=text; };
  lbl('p-brf-lbl', brfx, brfy, 10, 18, 'fijo inf.');
  lbl('p-trf-lbl', trfx, trfy, 10, -12, 'fijo sup.');
  lbl('p-bls-lbl', blsx, blsy, -10, 18, 'deslizante');
  lbl('p-tls-lbl', tlsx, tlsy, -10, -12, 'deslizante');
}

function drawCross(prefix, L, thetaDeg, cx, cy, k){
  const rad = thetaDeg*Math.PI/180;
  const halfL = L/2;
  const dx = halfL*Math.cos(rad)*k, dy = halfL*Math.sin(rad)*k;
  const tl=[cx-dx,cy-dy], tr=[cx+dx,cy-dy], bl=[cx-dx,cy+dy], br=[cx+dx,cy+dy];
  document.getElementById(prefix+'-arm1').setAttribute('x1',tl[0]); document.getElementById(prefix+'-arm1').setAttribute('y1',tl[1]);
  document.getElementById(prefix+'-arm1').setAttribute('x2',br[0]); document.getElementById(prefix+'-arm1').setAttribute('y2',br[1]);
  document.getElementById(prefix+'-arm2').setAttribute('x1',tr[0]); document.getElementById(prefix+'-arm2').setAttribute('y1',tr[1]);
  document.getElementById(prefix+'-arm2').setAttribute('x2',bl[0]); document.getElementById(prefix+'-arm2').setAttribute('y2',bl[1]);
  document.getElementById(prefix+'-tl').setAttribute('cx',tl[0]); document.getElementById(prefix+'-tl').setAttribute('cy',tl[1]);
  document.getElementById(prefix+'-tr').setAttribute('cx',tr[0]); document.getElementById(prefix+'-tr').setAttribute('cy',tr[1]);
  document.getElementById(prefix+'-bl').setAttribute('cx',bl[0]); document.getElementById(prefix+'-bl').setAttribute('cy',bl[1]);
  document.getElementById(prefix+'-br').setAttribute('cx',br[0]); document.getElementById(prefix+'-br').setAttribute('cy',br[1]);
}

// ---- Simulador de recorrido ----
const rL = document.getElementById('r-L');
const rMin = document.getElementById('r-min');
const rMax = document.getElementById('r-max');
const rSlider = document.getElementById('r-slider');

function computeRange(){
  setErr('err-range','');
  let L = parseFloat(rL.value), min = parseFloat(rMin.value), max = parseFloat(rMax.value);
  if(isNaN(L)||isNaN(min)||isNaN(max)){ return; }
  if(min>=max){ setErr('err-range','θ mínimo debe ser menor que θ máximo'); return; }
  if(min<0||max>90){ setErr('err-range','El rango debe estar entre 0° y 90°'); return; }

  rSlider.min = min; rSlider.max = max;
  let theta = parseFloat(rSlider.value);
  if(theta<min) theta=min;
  if(theta>max) theta=max;
  rSlider.value = theta;

  const rad = theta*Math.PI/180, radMin = min*Math.PI/180, radMax = max*Math.PI/180;
  const H = L*Math.sin(rad), W = L*Math.cos(rad);
  const Hmin = L*Math.sin(radMin), Hmax = L*Math.sin(radMax);
  const Wmin = L*Math.cos(radMin), Wmax = L*Math.cos(radMax);

  document.getElementById('r-theta-val').textContent = round(theta)+'°';
  document.getElementById('out-rH').textContent = round(H)+' mm';
  document.getElementById('out-rW').textContent = round(W)+' mm';
  document.getElementById('out-dH').textContent = round(Math.abs(Hmax-Hmin))+' mm';
  document.getElementById('out-dW').textContent = round(Math.abs(Wmax-Wmin))+' mm';

  drawCross('ghost', L, min, 250, 150, 1.35);
  drawCross('ghost2', L, max, 250, 150, 1.35);
  drawCross('r', L, theta, 250, 150, 1.35);
}

[rL, rMin, rMax].forEach(el=>el.addEventListener('input', computeRange));
rSlider.addEventListener('input', computeRange);

refreshChips();
computeCalc();
computeRange();
</script>

</body>
</html>
