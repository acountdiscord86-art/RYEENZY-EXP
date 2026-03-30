<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
  <title>RYEENZY | HUB • Roblox Script Hub</title>
  <!-- Google Fonts & Font Awesome -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,600;14..32,700;14..32,800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Inter', sans-serif;
      background: #05070f;
      color: #f0f3ff;
      overflow-x: hidden;
      line-height: 1.5;
    }

    /* Animated Gradient Background */
    .animated-bg {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -2;
      background: radial-gradient(circle at 20% 30%, #0b1120, #020409);
    }

    /* Particle Canvas */
    #particle-canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      pointer-events: none;
      opacity: 0.4;
    }

    /* Container */
    .container {
      max-width: 1300px;
      margin: 0 auto;
      padding: 1.5rem 2rem;
      position: relative;
      z-index: 2;
    }

    /* Navbar */
    .navbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 1rem;
      padding: 1rem 0;
      margin-bottom: 2rem;
      border-bottom: 1px solid rgba(100, 120, 255, 0.3);
      backdrop-filter: blur(10px);
      border-radius: 0 0 30px 30px;
      animation: slideDown 0.6s ease-out;
    }

    @keyframes slideDown {
      from {
        opacity: 0;
        transform: translateY(-40px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .logo h1 {
      font-size: 2rem;
      font-weight: 800;
      background: linear-gradient(135deg, #ffffff, #8a9aff, #5f72ff);
      background-size: 200% auto;
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      animation: shimmer 4s ease infinite;
    }

    @keyframes shimmer {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    .logo p {
      font-size: 0.7rem;
      letter-spacing: 2px;
      color: #8e9eff;
    }

    .nav-links {
      display: flex;
      gap: 2rem;
      align-items: center;
    }

    .nav-links a {
      color: #cddcff;
      text-decoration: none;
      font-weight: 600;
      font-size: 1rem;
      transition: 0.3s;
      display: flex;
      align-items: center;
      gap: 8px;
      position: relative;
    }

    .nav-links a::after {
      content: '';
      position: absolute;
      bottom: -8px;
      left: 0;
      width: 0;
      height: 2px;
      background: linear-gradient(90deg, #6d7eff, #a77eff);
      transition: width 0.3s;
      border-radius: 2px;
    }

    .nav-links a:hover::after,
    .nav-links a.active::after {
      width: 100%;
    }

    .discord-btn {
      background: linear-gradient(135deg, #5865F2, #4752c4);
      padding: 8px 20px;
      border-radius: 40px;
      font-weight: 600;
      transition: 0.3s;
      box-shadow: 0 4px 15px rgba(88, 101, 242, 0.4);
      animation: pulseGlow 2s infinite;
    }

    @keyframes pulseGlow {
      0% { box-shadow: 0 0 0 0 rgba(88, 101, 242, 0.6); }
      70% { box-shadow: 0 0 0 8px rgba(88, 101, 242, 0); }
      100% { box-shadow: 0 0 0 0 rgba(88, 101, 242, 0); }
    }

    .discord-btn:hover {
      transform: scale(1.05);
      background: linear-gradient(135deg, #6c7aff, #5a68e0);
    }

    /* Sections */
    .section {
      display: none;
      animation: fadeUp 0.5s cubic-bezier(0.2, 0.9, 0.4, 1.1);
    }

    .active-section {
      display: block;
    }

    @keyframes fadeUp {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    /* Glass Card */
    .glass-card {
      background: rgba(15, 22, 40, 0.6);
      backdrop-filter: blur(12px);
      border: 1px solid rgba(100, 120, 255, 0.3);
      border-radius: 1.8rem;
      padding: 1.8rem;
      transition: all 0.4s;
    }

    .glass-card:hover {
      transform: translateY(-6px);
      border-color: #8a9aff;
      box-shadow: 0 20px 30px -12px rgba(0, 0, 0, 0.5);
    }

    /* Stats */
    .stats-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 1.5rem;
      margin: 2rem 0;
    }

    .stat-item {
      text-align: center;
    }

    .stat-number {
      font-size: 2.5rem;
      font-weight: 800;
      background: linear-gradient(135deg, #fff, #8a9aff);
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    /* Executor Lists */
    .executor-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 2rem;
      margin: 2rem 0;
    }

    .executor-category {
      background: rgba(0, 0, 0, 0.4);
      border-radius: 1.5rem;
      padding: 1.2rem;
      transition: 0.3s;
    }

    .executor-category:hover {
      background: rgba(20, 30, 55, 0.6);
    }

    .executor-list {
      display: flex;
      flex-wrap: wrap;
      gap: 0.8rem;
      margin-top: 1rem;
    }

    .executor-badge {
      background: #1f2a4e;
      padding: 0.5rem 1rem;
      border-radius: 40px;
      font-size: 0.8rem;
      font-weight: 600;
      display: inline-flex;
      align-items: center;
      gap: 8px;
      transition: 0.2s;
    }

    .executor-badge i {
      font-size: 1rem;
    }

    .executor-badge:hover {
      background: #3b4bff;
      transform: scale(1.02);
    }

    /* Script Cards */
    .scripts-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
      gap: 1.8rem;
      margin-top: 2rem;
    }

    .script-card {
      background: rgba(10, 16, 30, 0.7);
      backdrop-filter: blur(10px);
      border-radius: 1.5rem;
      padding: 1.5rem;
      border: 1px solid rgba(100, 120, 255, 0.3);
      transition: 0.3s;
      opacity: 0;
      transform: translateY(30px);
    }

    .script-card.reveal {
      opacity: 1;
      transform: translateY(0);
    }

    .script-card:hover {
      transform: translateY(-8px);
      border-color: #6d7eff;
    }

    .script-name {
      font-size: 1.5rem;
      font-weight: 700;
      margin: 0.5rem 0;
      display: flex;
      align-items: center;
      gap: 10px;
      flex-wrap: wrap;
    }

    .free-icon {
      background: linear-gradient(135deg, #2ecc71, #27ae60);
      padding: 4px 10px;
      border-radius: 30px;
      font-size: 0.7rem;
      font-weight: 600;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      letter-spacing: 0.5px;
    }

    .script-code {
      background: #0b1022;
      padding: 0.8rem;
      border-radius: 1rem;
      font-family: monospace;
      font-size: 0.7rem;
      word-break: break-all;
      margin: 1rem 0;
      border-left: 3px solid #5865F2;
    }

    .copy-btn {
      background: linear-gradient(135deg, #1f2a4e, #151e3a);
      border: none;
      padding: 0.6rem 1.2rem;
      border-radius: 2rem;
      font-weight: 600;
      color: white;
      cursor: pointer;
      transition: 0.2s;
      display: inline-flex;
      align-items: center;
      gap: 8px;
    }

    .copy-btn:hover {
      background: #5865F2;
      transform: scale(1.02);
    }

    /* Toast */
    .toast {
      position: fixed;
      bottom: 25px;
      left: 50%;
      transform: translateX(-50%);
      background: #1e293b;
      backdrop-filter: blur(12px);
      padding: 0.8rem 2rem;
      border-radius: 60px;
      font-weight: 500;
      z-index: 1000;
      opacity: 0;
      transition: 0.2s;
      pointer-events: none;
      border-left: 4px solid #5865F2;
    }

    footer {
      text-align: center;
      margin-top: 4rem;
      padding-top: 2rem;
      border-top: 1px solid #202848;
      font-size: 0.8rem;
      opacity: 0.7;
    }

    @media (max-width: 700px) {
      .container {
        padding: 1rem;
      }
      .navbar {
        flex-direction: column;
      }
      .nav-links {
        flex-wrap: wrap;
        justify-content: center;
      }
      .script-code {
        font-size: 0.6rem;
      }
      .script-name {
        font-size: 1.2rem;
      }
    }
  </style>
</head>
<body>
  <div class="animated-bg"></div>
  <canvas id="particle-canvas"></canvas>

  <div class="container">
    <div class="navbar">
      <div class="logo">
        <h1>RYEENZY | HUB</h1>
        <p>PREMIUM SCRIPT HUB</p>
      </div>
      <div class="nav-links">
        <a href="#" class="nav-link" data-section="dashboard"><i class="fas fa-tachometer-alt"></i> Dashboard</a>
        <a href="#" class="nav-link" data-section="scripts"><i class="fas fa-code"></i> Script</a>
        <a href="https://discord.gg/pvCJtCt37" target="_blank" class="discord-btn"><i class="fab fa-discord"></i> Discord</a>
      </div>
    </div>

    <!-- DASHBOARD SECTION -->
    <div id="dashboard-section" class="section active-section">
      <div class="glass-card">
        <span style="background:#3b4bff40; padding:0.3rem 1rem; border-radius:40px; display:inline-block;"><i class="fas fa-crown"></i> RYEENZY HUB</span>
        <h2 style="font-size: 2rem; margin: 1rem 0;">Welcome to Script Hub</h2>
        <p>Koleksi script Roblox gratis terbaik dengan update rutin. Support semua executor modern.</p>
        <div class="stats-grid">
          <div class="stat-item"><div class="stat-number">3+</div><p>Free Scripts</p></div>
          <div class="stat-item"><div class="stat-number">20+</div><p>Executor Support</p></div>
          <div class="stat-item"><div class="stat-number">24/7</div><p>Community</p></div>
        </div>
      </div>

      <!-- Executor Lists -->
      <div class="executor-grid">
        <div class="executor-category">
          <h3><i class="fab fa-android"></i> Android Executor</h3>
          <div class="executor-list">
            <span class="executor-badge"><i class="fas fa-bolt"></i> Arceus X</span>
            <span class="executor-badge"><i class="fas fa-mobile-alt"></i> Hydrogen</span>
            <span class="executor-badge"><i class="fas fa-rocket"></i> Fluxus</span>
            <span class="executor-badge"><i class="fas fa-code"></i> Delta Executor</span>
            <span class="executor-badge"><i class="fas fa-cog"></i> Vega X</span>
            <span class="executor-badge"><i class="fas fa-tachometer-alt"></i> Evon</span>
          </div>
        </div>
        <div class="executor-category">
          <h3><i class="fas fa-desktop"></i> PC Executor</h3>
          <div class="executor-list">
            <span class="executor-badge"><i class="fas fa-dragon"></i> Synapse X</span>
            <span class="executor-badge"><i class="fas fa-crown"></i> ScriptWare</span>
            <span class="executor-badge"><i class="fas fa-wind"></i> Krnl</span>
            <span class="executor-badge"><i class="fas fa-fire"></i> Electron</span>
            <span class="executor-badge"><i class="fas fa-skull"></i> Sentinel</span>
            <span class="executor-badge"><i class="fas fa-cloud"></i> Comet</span>
          </div>
        </div>
      </div>

      <div class="glass-card">
        <h3><i class="fab fa-discord"></i> Join Discord</h3>
        <p>Dapatkan akses script eksklusif, update terbaru, dan support 24/7.</p>
        <a href="https://discord.gg/pvCJtCt37" target="_blank" style="display:inline-block; margin-top:1rem; background:#5865F2; padding:0.6rem 1.8rem; border-radius:40px; text-decoration:none; color:white;">Join Discord →</a>
      </div>
    </div>

    <!-- SCRIP SECTION -->
    <div id="scripts-section" class="section">
      <div class="glass-card">
        <h2><i class="fas fa-gift"></i> Free Scripts</h2>
        <p>Semua script gratis, klik tombol copy untuk mengambil script, lalu paste di executor favoritmu.</p>
      </div>
      <div class="scripts-grid" id="scripts-container"></div>
      <div style="text-align:center; margin-top:2rem;">
        <a href="https://discord.gg/pvCJtCt37" target="_blank" style="background: linear-gradient(135deg,#5865F2,#3b4bff); padding:0.8rem 2rem; border-radius:60px; text-decoration:none; color:white; font-weight:bold; display:inline-flex; gap:10px;"><i class="fab fa-discord"></i> More Scripts on Discord</a>
      </div>
    </div>

    <footer>
      <i class="fas fa-laptop-code"></i> RYEENZY © 2025 — Free Script Hub for Roblox
    </footer>
  </div>

  <div id="toast" class="toast">Script copied to clipboard</div>

  <script>
    // ========== PARTICLE BACKGROUND ==========
    const canvas = document.getElementById('particle-canvas');
    const ctx = canvas.getContext('2d');
    let width, height;
    let particles = [];

    function resizeCanvas() {
      width = window.innerWidth;
      height = window.innerHeight;
      canvas.width = width;
      canvas.height = height;
    }

    function initParticles() {
      particles = [];
      const count = Math.min(80, Math.floor(width * height / 8000));
      for (let i = 0; i < count; i++) {
        particles.push({
          x: Math.random() * width,
          y: Math.random() * height,
          radius: Math.random() * 2 + 1,
          vx: (Math.random() - 0.5) * 0.4,
          vy: (Math.random() - 0.5) * 0.2 + 0.2,
          alpha: Math.random() * 0.5 + 0.2
        });
      }
    }

    function drawParticles() {
      if (!ctx) return;
      ctx.clearRect(0, 0, width, height);
      for (let p of particles) {
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(100, 120, 255, ${p.alpha})`;
        ctx.fill();
        p.x += p.vx;
        p.y += p.vy;
        if (p.x < 0) p.x = width;
        if (p.x > width) p.x = 0;
        if (p.y < 0) p.y = height;
        if (p.y > height) p.y = 0;
      }
      requestAnimationFrame(drawParticles);
    }

    window.addEventListener('resize', () => {
      resizeCanvas();
      initParticles();
    });
    resizeCanvas();
    initParticles();
    drawParticles();

    // ========== SCRIPT DATA (gratis, dengan ikon pengganti tulisan FREE) ==========
    const scripts = [
      {
        name: "AUTOWALK",
        desc: "Auto walk / auto run untuk farming dan eksplorasi map secara otomatis.",
        code: 'loadstring(game:HttpGet("https://api.jnkie.com/api/v1/luascripts/public/ba7d718c738e51ad1c1a37ae93a954fc895de7489f12ebd29e18a286d1426c4a/download"))()'
      },
      {
        name: "SAWAH INDO",
        desc: "Script khusus game Sawah Indo, auto panen, auto tanam, dan fitur lainnya.",
        code: 'loadstring(game:HttpGet("https://api.jnkie.com/api/v1/luascripts/public/8054ee4c436545776587b3dfaaf8e2bad14668a97c0cf49069f512121212d4e7/download"))()'
      },
      {
        name: "FREECAM",
        desc: "Free camera mode untuk melihat seluruh map dengan bebas, tanpa batasan.",
        code: 'loadstring(game:HttpGet("https://api.jnkie.com/api/v1/luascripts/public/3e88cfdeff1a93b822e2c3d12ecf06e4a59d4058aae6bc284131a0c38ce09380/download"))()'
      }
    ];

    function renderScripts() {
      const container = document.getElementById('scripts-container');
      if (!container) return;
      container.innerHTML = '';
      scripts.forEach((script, idx) => {
        const card = document.createElement('div');
        card.className = 'script-card';
        card.setAttribute('data-idx', idx);
        card.innerHTML = `
          <div class="script-name">
            ${escapeHtml(script.name)}
            <span class="free-icon"><i class="fas fa-gem"></i> GRATIS</span>
          </div>
          <div class="script-desc" style="color:#b0c4ff;">${escapeHtml(script.desc)}</div>
          <div class="script-code"><i class="fas fa-code"></i> ${escapeHtml(script.code.substring(0, 80))}...</div>
          <button class="copy-btn" data-code="${escapeHtml(script.code)}"><i class="fas fa-copy"></i> Copy Script</button>
        `;
        container.appendChild(card);
      });

      // Attach copy events
      document.querySelectorAll('.copy-btn').forEach(btn => {
        btn.addEventListener('click', (e) => {
          e.stopPropagation();
          const code = btn.getAttribute('data-code');
          if (code) {
            navigator.clipboard.writeText(code).then(() => {
              showToast('Script copied to clipboard');
            }).catch(() => showToast('Failed to copy, try manually'));
          }
        });
      });

      // Intersection Observer for reveal animation
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            entry.target.classList.add('reveal');
            observer.unobserve(entry.target);
          }
        });
      }, { threshold: 0.2 });
      document.querySelectorAll('.script-card').forEach(card => observer.observe(card));
    }

    function escapeHtml(str) {
      return str.replace(/[&<>]/g, function(m) {
        if (m === '&') return '&amp;';
        if (m === '<') return '&lt;';
        if (m === '>') return '&gt;';
        return m;
      });
    }

    // Toast
    let toastTimeout;
    function showToast(msg) {
      const toast = document.getElementById('toast');
      if (!toast) return;
      toast.style.opacity = '1';
      toast.innerText = msg;
      clearTimeout(toastTimeout);
      toastTimeout = setTimeout(() => {
        toast.style.opacity = '0';
      }, 2500);
    }

    // Navigation
    const dashboardDiv = document.getElementById('dashboard-section');
    const scriptsDiv = document.getElementById('scripts-section');
    const navLinks = document.querySelectorAll('.nav-link');

    function setActiveSection(sectionId) {
      if (sectionId === 'dashboard') {
        dashboardDiv.classList.add('active-section');
        scriptsDiv.classList.remove('active-section');
      } else {
        dashboardDiv.classList.remove('active-section');
        scriptsDiv.classList.add('active-section');
      }
      navLinks.forEach(link => {
        const linkSection = link.getAttribute('data-section');
        if (linkSection === sectionId) {
          link.classList.add('active');
        } else {
          link.classList.remove('active');
        }
      });
    }

    navLinks.forEach(link => {
      link.addEventListener('click', (e) => {
        e.preventDefault();
        const section = link.getAttribute('data-section');
        if (section === 'dashboard' || section === 'scripts') {
          setActiveSection(section);
          window.location.hash = section;
        }
      });
    });

    function handleHash() {
      const hash = window.location.hash.slice(1);
      if (hash === 'dashboard' || hash === 'scripts') {
        setActiveSection(hash);
      } else {
        setActiveSection('dashboard');
      }
    }

    window.addEventListener('hashchange', handleHash);
    handleHash();

    renderScripts();
  </script>
</body>
</html>
