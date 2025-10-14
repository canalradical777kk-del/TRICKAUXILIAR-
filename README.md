<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Auxílio 1.01.2V — RzX Vip</title>
  <style>
    :root{
      --bg:#060608;
      --card:#0f0f11;
      --muted:#bfc1c6;
      --accent1:#8f56ff;
      --accent2:#c89bff;
      --glass: rgba(255,255,255,0.02);
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    html,body{height:100%;}
    body{
      margin:0;
      background:radial-gradient(ellipse at top left, rgba(143,86,255,0.04), transparent 10%),
                 radial-gradient(ellipse at bottom right, rgba(200,155,255,0.02), transparent 10%),
                 var(--bg);
      -webkit-font-smoothing:antialiased;
      color:#fff;
      overflow-y:auto;
    }

    /* particle background canvas covers whole screen */
    #particle-canvas{
      position:fixed;
      inset:0;
      z-index:0;
      pointer-events:none;
    }

    .center-wrap{
      min-height:100vh;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:24px;
      z-index:1;
      position:relative;
    }

    /* panel smaller like you asked */
    .card{
      width: min(360px, 92vw);
      background: linear-gradient(180deg, rgba(255,100,170,0.12), rgba(255,150,200,0.1));
      border-radius:18px;
      padding:22px;
      box-shadow: 0 12px 40px rgba(140,80,255,0.08), inset 0 1px 0 rgba(255,255,255,0.02);
      border: 1px solid rgba(140,80,255,0.06);
      backdrop-filter: blur(6px);
    }

    .title{
      text-align:center;
      font-size:22px;
      font-weight:700;
      margin-bottom:14px;
    }

    .section{
      background: rgba(255,255,255,0.02);
      border-radius:12px;
      padding:12px 14px;
      margin-bottom:12px;
      border:1px solid rgba(255,255,255,0.02);
    }

    .accordion-head{
      display:flex;
      align-items:center;
      justify-content:space-between;
      font-weight:600;
      cursor:pointer;
      user-select:none;
    }

    .chev{
      width:28px;height:28px;border-radius:8px;
      display:grid;place-items:center;background:transparent;
      border:1px solid rgba(255,255,255,0.02);
      transition: transform .18s ease;
    }

    .accordion-body{ margin-top:10px; display:none; }
    .accordion-body.active{ display:block; }

    .checkbox-row{
      display:flex;
      gap:12px;
      align-items:center;
      margin:12px 0;
    }
    .checkbox{
      width:22px; height:22px; border-radius:6px; display:grid;place-items:center;
      background:#111; border:1px solid rgba(255,255,255,0.04); cursor:pointer;
    }
    .checkbox input{display:none;}
    .checkbox svg{opacity:0; transform:scale(.8); transition: all .12s ease; }
    .checkbox.checked{ background: linear-gradient(90deg,var(--accent1),var(--accent2)); border: none; }
    .checkbox.checked svg{ opacity:1; transform:scale(1); filter: drop-shadow(0 2px 6px rgba(140,80,255,.24)); }

    .option-label{ flex:1; color:var(--muted); font-weight:600; }

    .fov-row{ display:flex; align-items:center; gap:12px; margin-top:8px; color:var(--muted); }
    .fov-row input[type=range]{ flex:1; -webkit-appearance: none; height:8px; border-radius:8px; background: linear-gradient(90deg,var(--accent1),var(--accent2)); outline:none; }
    .fov-row input[type=range]::-webkit-slider-thumb{ -webkit-appearance:none; width:18px;height:18px;border-radius:50%; background:#fff; box-shadow:0 2px 8px rgba(0,0,0,0.5); }

    .buttons{ display:flex; flex-direction:column; gap:12px; margin-top:8px; }
    .btn{
      padding:12px 14px; border-radius:12px; font-weight:700; border:none; cursor:pointer;
      background: linear-gradient(90deg,var(--accent1),var(--accent2));
      color:#fff; box-shadow: 0 6px 20px rgba(143,86,255,0.18);
    }
    .btn.active{
      outline: 2px solid rgba(255,255,255,0.06);
      transform: translateY(-1px);
      box-shadow: 0 10px 30px rgba(143,86,255,0.22);
    }
    .btn.secondary{
      background: linear-gradient(90deg, rgba(255,255,255,0.03), rgba(255,255,255,0.01));
      border:1px solid rgba(255,255,255,0.03);
      color:var(--muted); font-weight:700;
    }

    .small-note{
      font-size:12px;
      color:var(--muted);
      text-align:center;
      margin-top:6px;
    }

    /* toast */
    .toast{
      position:fixed;
      left:50%;
      transform:translateX(-50%);
      bottom:36px;
      background: linear-gradient(90deg,var(--accent1),var(--accent2));
      color:#fff; padding:10px 18px; border-radius:12px;
      box-shadow:0 8px 30px rgba(140,80,255,0.18); z-index:200;
      opacity:0; pointer-events:none; transition:all .25s ease;
    }
    .toast.show{ opacity:1; transform: translateX(-50%) translateY(0); }

    /* small screens */
    @media (max-width:420px){
      .card{ padding:18px; border-radius:14px; }
      .title{ font-size:20px; }
    }

  </style>
</head>
<body>
<!-- RzzX: particle canvas + login overlay (injetado sem modificar o painel) -->
<canvas id="rzzx-particle-canvas" aria-hidden="true"></canvas>

<style id="rzzx-styles">
  /* Canvas (fundo animado) */
  #rzzx-particle-canvas{
    position:fixed;
    top:0; left:0;
    width:100%; height:100%;
    z-index:0;
    pointer-events:none;
    background:transparent;
  }

  /* Login overlay - bem isolado por IDs para não interferir com estilos do painel */
  #rzzx-loginOverlay{
    position:fixed;
    inset:0;
    display:flex;
    align-items:center;
    justify-content:center;
    z-index:99999; /* acima de tudo */
    background: rgba(0,0,0,0.55);
    -webkit-backdrop-filter: blur(4px);
    backdrop-filter: blur(4px);
  }
  #rzzx-login-card{
    background: linear-gradient(180deg, rgba(255,80,140,0.25), rgba(255,120,180,0.2));
    color: #fff;
    padding:18px;
    border-radius:12px;
    box-shadow: 0 10px 40px rgba(255,100,170,0.3);
    min-width: 320px;
    max-width:92vw;
    text-align:center;
    border: 1px solid rgba(255,120,180,0.25);
  }
  #rzzx-login-card h2{ margin:0 0 8px; font-size:18px; }
  #rzzx-login-card p{ margin:0 0 12px; color:#cfcfd3; font-size:13px; }

  #rzzx-login-field{ display:flex; gap:8px; align-items:center; justify-content:center; }
  #rzzx-login-field input{
    flex:1;
    padding:10px 12px;
    border-radius:8px;
    border:1px solid rgba(255,255,255,0.06);
    background: rgba(255,255,255,0.02);
    color:#fff;
    outline:none;
    min-width:120px;
    font-weight:600;
  }
  #rzzx-login-field button{
    padding:10px 12px;
    border-radius:8px;
    border:none;
    cursor:pointer;
    background: linear-gradient(90deg,#ff4f9c,#ff94d9);
    color:#fff;
    font-weight:700;
  }

  #rzzx-errorMsg{ color:#ff6b6b; margin-top:10px; display:none; font-weight:700; }

  /* Garantir que painel e elementos internos fiquem acima do canvas */
  body > *:not(#rzzx-particle-canvas):not(#rzzx-styles):not(#rzzx-loginOverlay){
    /* não alteramos visual; esta regra é leve e apenas assegura posição base */
  }
</style>

<div id="rzzx-loginOverlay" role="dialog" aria-modal="true">
  <div id="rzzx-login-card" role="document" aria-labelledby="rzzx-login-title">
    <h2 id="rzzx-login-title">Login — key de acesso</h2>
    <p>Digite sua <strong>key de acesso</strong> para entrar no painel.</p>
    <div id="rzzx-login-field">
      <input id="rzzx-keyInput" type="text" placeholder="key de acesso" aria-label="key de acesso">
      <button id="rzzx-keySubmit" type="button">Entrar</button>
    </div>
    <div id="rzzx-errorMsg">Key inválida!</div>
  </div>
</div>

<script id="rzzx-script">
(function(){
  // Namespace local
  const CORRECT_KEY = 'key-71271608X'; // chave válida (não exibida ao usuário)
  const overlay = document.getElementById('rzzx-loginOverlay');
  const keyInput = document.getElementById('rzzx-keyInput');
  const keySubmit = document.getElementById('rzzx-keySubmit');
  const errorMsg = document.getElementById('rzzx-errorMsg');

  // Small toast helper (non-intrusive)
  function showToast(msg){
    let t = document.getElementById('rzzx-toast');
    if(!t){
      t = document.createElement('div');
      t.id = 'rzzx-toast';
      Object.assign(t.style, {position:'fixed', left:'50%', transform:'translateX(-50%)', bottom:'28px', padding:'8px 14px', borderRadius:'10px', background:'linear-gradient(90deg,#ff4f9c,#ff94d9)', color:'#fff', zIndex:100000, fontWeight:'700', opacity:0, transition:'opacity .18s ease'});
      document.body.appendChild(t);
    }
    t.textContent = msg;
    t.style.opacity = '1';
    setTimeout(()=>{ t.style.opacity = '0'; }, 2000);
  }

  function unlockIfCorrect(){
    const v = keyInput.value.trim();
    if(v === CORRECT_KEY){
      overlay.style.display = 'none';
      // remove aria-hidden if present
      try{ document.getElementById('mainContent')?.removeAttribute('aria-hidden'); }catch(e){}
      showToast('Bem-vindo — acesso liberado');
    } else {
      // mostrar erro visível e toast
      errorMsg.style.display = 'block';
      // pequeno "shake"
      const card = document.getElementById('rzzx-login-card');
      card.classList.remove('rzzx-shake');
      // restart animation
      void card.offsetWidth;
      card.classList.add('rzzx-shake');
      showToast('Key inválida!');
    }
  }
  keySubmit.addEventListener('click', unlockIfCorrect);
  keyInput.addEventListener('keydown', function(e){ if(e.key === 'Enter') unlockIfCorrect(); });

  // focus inicial
  setTimeout(()=> keyInput.focus(), 200);

  // minimal CSS for shake added dynamically to avoid conflicts
  const style = document.createElement('style');
  style.textContent = `
    @keyframes rzzx-shake { 0%{transform:translateX(0)} 20%{transform:translateX(-8px)} 40%{transform:translateX(8px)} 60%{transform:translateX(-6px)} 80%{transform:translateX(6px)} 100%{transform:translateX(0)} }
    .rzzx-shake{ animation: rzzx-shake .42s ease; }
  `;
  document.head.appendChild(style);

  // --- Particle background (canvas) ---
  const canvas = document.getElementById('rzzx-particle-canvas');
  const ctx = canvas && canvas.getContext ? canvas.getContext('2d') : null;
  if(!ctx) return;

  let w = canvas.width = window.innerWidth;
  let h = canvas.height = window.innerHeight;
  let stars = [];
  const STAR_COUNT = Math.min(400, Math.floor((w*h)/20000)); // density adjusted by viewport area

  function rand(min,max){ return Math.random()*(max-min)+min; }
  function initStars(){
    stars = [];
    for(let i=0;i<STAR_COUNT;i++){
      stars.push({
        x: rand(0,w), y: rand(0,h),
        vx: rand(-0.4,0.4), vy: rand(-0.4,0.4),
        r: rand(0.4,1.8),
        phase: Math.random()*Math.PI*2
      });
    }
  }
  function resize(){
    w = canvas.width = window.innerWidth;
    h = canvas.height = window.innerHeight;
    initStars();
  }
  window.addEventListener('resize', resize);
  initStars();

  function frame(){
    ctx.clearRect(0,0,w,h);
    // subtle gradient background to enhance visibility
    const g = ctx.createLinearGradient(0,0,w,h);
    g.addColorStop(0,'rgba(0,0,0,0)');
    g.addColorStop(1,'rgba(0,0,0,0)');
    ctx.fillStyle = g;
    ctx.fillRect(0,0,w,h);

    for(let i=0;i<stars.length;i++){
      const s = stars[i];
      s.phase += 0.02;
      const alpha = 0.5 + Math.sin(s.phase)*0.5;
      s.x += s.vx; s.y += s.vy;
      if(s.x < -10) s.x = w + 10;
      if(s.x > w + 10) s.x = -10;
      if(s.y < -10) s.y = h + 10;
      if(s.y > h + 10) s.y = -10;

      ctx.beginPath();
      ctx.arc(s.x, s.y, Math.max(0.35, s.r), 0, Math.PI*2);
      ctx.fillStyle = 'rgba(255,200,230,' + (0.6*alpha) + ')';
      ctx.fill();

      // small streak
      ctx.beginPath();
      ctx.moveTo(s.x - s.vx*4, s.y - s.vy*4);
      ctx.lineTo(s.x + s.vx*2, s.y + s.vy*2);
      ctx.strokeStyle = 'rgba(255,255,255,' + (0.07*alpha) + ')';
      ctx.lineWidth = Math.max(0.2, s.r/2);
      ctx.stroke();

      // connect nearby stars
      for(let j=i+1;j<stars.length;j++){
        const s2 = stars[j];
        const dx = s.x - s2.x, dy = s.y - s2.y;
        const d2 = dx*dx + dy*dy;
        if(d2 < 6500){
          const a = 0.00012*(6500 - d2);
          ctx.beginPath();
          ctx.strokeStyle = 'rgba(255,120,200,' + Math.min(0.15, a) + ')';
          ctx.lineWidth = 0.5;
          ctx.moveTo(s.x,s.y);
          ctx.lineTo(s2.x,s2.y);
          ctx.stroke();
        }
      }
    }
    requestAnimationFrame(frame);
  }
  requestAnimationFrame(frame);
})();
</script>
<!-- /RzzX injection -->

  <canvas id="particle-canvas"></canvas>

  <div class="center-wrap">
    <div class="card" role="main" aria-labelledby="pageTitle">
      <div id="pageTitle" class="title">Auxílio RzzX</div>

      <!-- Sistema Avançado (collapsed by default) -->
      <div class="section">
        <div class="accordion-head" data-accordion="sistema">
          <div>▶ Sistema Avançado</div>
          <div class="chev" aria-hidden>▸</div>
        </div>
        <div class="accordion-body" id="sistema-body">
          <div style="margin-top:10px; display:flex;align-items:center; gap:10px;">
            <label style="display:flex;gap:10px;align-items:center;">
              <span class="checkbox" data-option="120fps" tabindex="0">
                <input type="checkbox" />
                <svg width="12" height="9" viewBox="0 0 12 9" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M10.6 1L4.3 7.5 1.4 4.6" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
              </span>
              <span class="option-label">AIMBOT 90%</span>
            </label>
          </div>

          <div style="margin-top:10px; display:flex;align-items:center; gap:10px;">
            <label style="display:flex;gap:10px;align-items:center;">
              <span class="checkbox" data-option="norecoil" tabindex="0">
                <input type="checkbox" />
                <svg width="12" height="9" viewBox="0 0 12 9" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M10.6 1L4.3 7.5 1.4 4.6" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
              </span>
              <span class="option-label">AIM LOCK</span>
            </label>
          </div>
        </div>
      </div>

      <!-- Ajustes de Mira (open by default) -->
      <div class="section">
        <div class="accordion-head" data-accordion="mira">
          <div>▼ Ajustes de Mira</div>
          <div class="chev" aria-hidden>▾</div>
        </div>
        <div class="accordion-body active" id="mira-body">
          <div class="checkbox-row">
            <label style="display:flex;gap:10px;align-items:center;width:100%;">
              <span class="checkbox" data-option="aimbot" tabindex="0">
                <input type="checkbox" />
                <svg width="12" height="9" viewBox="0 0 12 9" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M10.6 1L4.3 7.5 1.4 4.6" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
              </span>
              <span class="option-label">No Recoil</span>
            </label>
          </div>

          <div class="checkbox-row">
            <label style="display:flex;gap:10px;align-items:center;width:100%;">
              <span class="checkbox" data-option="headtrick" tabindex="0">
                <input type="checkbox" />
                <svg width="12" height="9" viewBox="0 0 12 9" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M10.6 1L4.3 7.5 1.4 4.6" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
              </span>
              <span class="option-label">Head Trick</span>
            </label>
          </div>

          <div class="checkbox-row">
            <label style="display:flex;gap:10px;align-items:center;width:100%;">
              <span class="checkbox" data-option="neural" tabindex="0">
                <input type="checkbox" />
                <svg width="12" height="9" viewBox="0 0 12 9" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M10.6 1L4.3 7.5 1.4 4.6" stroke="#fff" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/></svg>
              </span>
              <span class="option-label">120 FPS</span>
            </label>
          </div>

          <div class="fov-row" style="margin-top:6px;">
            <div style="font-weight:700;color:var(--muted);min-width:40px;">FOV</div>
            <input id="fovRange" type="range" min="0" max="100" value="40" />
            <div id="fovValue" style="width:26px;text-align:right;font-weight:700;color:var(--muted)">40</div>
          </div>
        </div>
      </div>

      <!-- Replace the three gradient buttons (removed DNS ones) -->
      <div class="buttons">
        <button id="injectorBtn" class="btn" data-toggle="false">Injector</button>
        <button id="resBtn" class="btn" data-toggle="false">Ativar Resolução</button>
        <button id="applyBtn" class="btn" data-toggle="false">Aplicar pasta</button>
      </div>
    </div>
  </div>

  <div id="toast" class="toast" role="status" aria-live="polite"></div>

  <script>
    /***********************
     * Particle background  *
     ***********************/
    const canvas = document.getElementById('particle-canvas');
    const ctx = canvas.getContext('2d');
    let w = canvas.width = innerWidth;
    let h = canvas.height = innerHeight;
    window.addEventListener('resize', () => { w = canvas.width = innerWidth; h = canvas.height = innerHeight; initParticles(); });

    const particles = [];
    const PARTICLE_COUNT = Math.min(80, Math.floor((w*h)/60000)); // scale with viewport

    function rand(min,max){ return Math.random()*(max-min)+min; }

    function initParticles(){
      particles.length = 0;
      for(let i=0;i<PARTICLE_COUNT;i++){
        particles.push({
          x: rand(0,w),
          y: rand(0,h),
          vx: rand(-0.3,0.3),
          vy: rand(-0.3,0.3),
          r: rand(0.8,1.8)
        });
      }
    }
    initParticles();

    function step(){
      ctx.clearRect(0,0,w,h);
      // draw lines
      for(let i=0;i<particles.length;i++){
        let p = particles[i];
        p.x += p.vx; p.y += p.vy;
        if(p.x<0) p.x = w;
        if(p.x> w) p.x = 0;
        if(p.y<0) p.y = h;
        if(p.y> h) p.y = 0;
        // dot
        ctx.beginPath();
        ctx.fillStyle = 'rgba(255,255,255,0.9)';
        ctx.globalAlpha = 0.85;
        ctx.arc(p.x, p.y, p.r, 0, Math.PI*2);
        ctx.fill();
        // lines to nearby
        for(let j=i+1;j<particles.length;j++){
          let q = particles[j];
          let dx = p.x - q.x, dy = p.y - q.y;
          let d2 = dx*dx + dy*dy;
          if(d2 < 9000){ // threshold
            let alpha = 0.0016*(9000 - d2);
            ctx.beginPath();
            ctx.strokeStyle = `rgba(140,86,255,${alpha})`;
            ctx.lineWidth = 0.8;
            ctx.moveTo(p.x,p.y);
            ctx.lineTo(q.x,q.y);
            ctx.stroke();
          }
        }
      }
      ctx.globalAlpha = 1;
      requestAnimationFrame(step);
    }
    requestAnimationFrame(step);

    /*************************
     * UI Interactions       *
     *************************/
    // accordions
    document.querySelectorAll('.accordion-head').forEach(head=>{
      head.addEventListener('click', ()=>{
        const key = head.getAttribute('data-accordion');
        const body = document.getElementById(key+'-body');
        const chev = head.querySelector('.chev');
        if(body.classList.contains('active')){
          body.classList.remove('active'); if(chev) chev.style.transform = 'rotate(0deg)';
        } else {
          body.classList.add('active'); if(chev) chev.style.transform = 'rotate(90deg)';
        }
      });
    });

    // Toast
    const toastEl = document.getElementById('toast');
    let toastTimer = null;
    function showToast(msg){
      if(toastTimer) { clearTimeout(toastTimer); toastTimer = null; }
      toastEl.textContent = msg;
      toastEl.classList.add('show');
      toastTimer = setTimeout(()=>{ toastEl.classList.remove('show'); toastTimer = null; }, 3000);
    }

    // checkbox toggles
    document.querySelectorAll('.checkbox').forEach(cb=>{
      const input = cb.querySelector('input[type=checkbox]');
      if(cb.classList.contains('checked')) input.checked = true;
      cb.addEventListener('click', ()=>{
        input.checked = !input.checked;
        cb.classList.toggle('checked', input.checked);
        const name = cb.getAttribute('data-option') || 'opção';
        const pretty = mapPrettyName(name);
        showToast(pretty + (input.checked ? ' ativado' : ' desativado'));
      });
      cb.addEventListener('keydown', (ev)=>{
        if(ev.key === 'Enter' || ev.key === ' '){
          ev.preventDefault();
          cb.click();
        }
      });
    });

    function mapPrettyName(key){
      switch(key){
        case '120fps': return 'Neural aim';
        case 'norecoil': return 'No Recoil';
        case 'aimbot': return 'Aimbot 90%';
        case 'headtrick': return 'Head Trick';
        case 'neural': return 'Neural Aim';
        default: return key;
      }
    }

    // fov range
    const fovRange = document.getElementById('fovRange');
    const fovValue = document.getElementById('fovValue');
    if(fovRange){
      fovRange.addEventListener('input', ()=> fovValue.textContent = fovRange.value);
    }

    // buttons: act as toggles and show ativado/desativado
    function setupToggleButton(btnId, prettyName){
      const btn = document.getElementById(btnId);
      btn.addEventListener('click', ()=>{
        const current = btn.getAttribute('data-toggle') === 'true';
        const next = !current;
        btn.setAttribute('data-toggle', next ? 'true' : 'false');
        btn.classList.toggle('active', next);
        showToast(prettyName + (next ? ' ativado' : ' desativado'));
      });
    }

    // botão INJECTOR abre o Free Fire
const injectorBtn = document.getElementById("injectorBtn");
injectorBtn.addEventListener("click", () => {
  const current = injectorBtn.getAttribute("data-toggle") === "true";
  const next = !current;
  injectorBtn.setAttribute("data-toggle", next ? "true" : "false");
  injectorBtn.classList.toggle("active", next);
  showToast("Injector " + (next ? "ativado" : "desativado"));

  if (next) {
    const packageName = 'com.dts.freefireth';
    const intentURI = 'intent://#Intent;package=' + packageName + ';end';

    console.log('Tentando abrir Free Fire:', intentURI);
    showToast('Abrindo Free Fire...');

    try {
      window.location.href = intentURI;
    } catch (e) {
      console.error('Erro ao tentar abrir o Free Fire', e);
      showToast('Erro ao abrir o Free Fire');
    }
  }
});setupToggleButton('resBtn','Ativar Resolução');
    setupToggleButton('applyBtn','Aplicar pasta');

// --- Permissão da pasta no botão "Aplicar pasta" ---
const applyBtnPerm = document.getElementById("applyBtn");

if (applyBtnPerm) {
  applyBtnPerm.addEventListener("click", async () => {
    try {
      // abre seletor pedindo uma pasta
      const dirHandle = await window.showDirectoryPicker();

      if (dirHandle) {
        showToast("Pasta selecionada com sucesso!");
        console.log("Pasta escolhida:", dirHandle);
      }
    } catch (err) {
      console.error("Erro ao acessar pasta:", err);
      showToast("Permissão da pasta cancelada!");
    }
  });
}


    // accessibility
    document.querySelectorAll('.checkbox').forEach(el=> el.setAttribute('tabindex','0'));

    // initialize particle count on load/resize
    setTimeout(()=>{ w = canvas.width = innerWidth; h = canvas.height = innerHeight; initParticles(); }, 200);
  </script>

<script id="reset-funcoes">
window.addEventListener("load", () => {
  function resetFuncoes() {
    try {
      document.querySelectorAll('input[type="checkbox"]').forEach(cb => cb.checked = false);
      document.querySelectorAll('[data-toggle]').forEach(btn => {
        btn.setAttribute('data-toggle','false');
        btn.classList.remove('active');
      });
    } catch(e) { console.error("Erro ao resetar funções:", e); }
  }

  // roda uma vez no início
  resetFuncoes();

  // tenta resetar de novo algumas vezes (~6s)
  let tentativas = 0;
  const interval = setInterval(() => {
    resetFuncoes();
    tentativas++;
    if (tentativas > 20) clearInterval(interval);
  }, 300);
});
</script>


<style>
/* Fundo escuro para destacar as estrelas */
body {
  background: black;
  overflow: hidden;
}

/* Estrelas */
.star {
  position: absolute;
  width: 2px;
  height: 2px;
  background: white;
  border-radius: 50%;
  animation: twinkle 3s infinite alternate;
}

@keyframes twinkle {
  from { opacity: 0.3; }
  to { opacity: 1; }
}

/* Linhas */
.line {
  position: absolute;
  width: 1px;
  height: 100vh;
  background: rgba(255,255,255,0.2);
  animation: moveLine 10s linear infinite;
}

@keyframes moveLine {
  from { transform: translateY(-100vh); }
  to { transform: translateY(100vh); }
}
</style>

<script>
// Gerar estrelas
for (let i = 0; i < 150; i++) {
  let star = document.createElement("div");
  star.className = "star";
  star.style.top = Math.random() * window.innerHeight + "px";
  star.style.left = Math.random() * window.innerWidth + "px";
  document.body.appendChild(star);
}

// Gerar linhas
for (let i = 0; i < 30; i++) {
  let line = document.createElement("div");
  line.className = "line";
  line.style.left = Math.random() * window.innerWidth + "px";
  line.style.animationDuration = (5 + Math.random() * 10) + "s";
  document.body.appendChild(line);
}
</script>

</body>
</html>
