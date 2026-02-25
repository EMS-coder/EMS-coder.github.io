<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Easy Monday Solutions — Automatización Inteligente</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&display=swap" rel="stylesheet" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0a0d0f;
      --surface: #111518;
      --border: #1e2428;
      --accent: #00e5a0;
      --accent2: #00b8ff;
      --text: #edf2f4;
      --muted: #6b7b85;
      --card: #13181c;
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'DM Sans', sans-serif;
      font-weight: 300;
      overflow-x: hidden;
      cursor: none;
    }

    /* Custom cursor */
    .cursor {
      width: 12px; height: 12px;
      background: var(--accent);
      border-radius: 50%;
      position: fixed;
      top: 0; left: 0;
      pointer-events: none;
      z-index: 9999;
      transition: transform 0.15s ease, background 0.2s;
      mix-blend-mode: difference;
    }
    .cursor-ring {
      width: 36px; height: 36px;
      border: 1px solid rgba(0,229,160,0.4);
      border-radius: 50%;
      position: fixed;
      top: 0; left: 0;
      pointer-events: none;
      z-index: 9998;
      transition: transform 0.35s ease, opacity 0.2s;
    }

    /* Noise overlay */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 1000;
      opacity: 0.4;
    }

    /* ── NAV ── */
    nav {
      position: fixed; top: 0; left: 0; right: 0;
      z-index: 500;
      padding: 1.5rem 3rem;
      display: flex; align-items: center; justify-content: space-between;
      border-bottom: 1px solid transparent;
      transition: border-color 0.3s, background 0.3s;
    }
    nav.scrolled {
      background: rgba(10,13,15,0.85);
      backdrop-filter: blur(16px);
      border-color: var(--border);
    }
    .logo {
      font-family: 'Syne', sans-serif;
      font-weight: 800;
      font-size: 1.15rem;
      letter-spacing: -0.02em;
      display: flex; align-items: center; gap: 0.5rem;
    }
    .logo-dot { width: 8px; height: 8px; background: var(--accent); border-radius: 50%; display: inline-block; }
    nav ul { list-style: none; display: flex; gap: 2.5rem; }
    nav a {
      font-size: 0.85rem;
      font-weight: 400;
      color: var(--muted);
      text-decoration: none;
      letter-spacing: 0.05em;
      text-transform: uppercase;
      transition: color 0.2s;
      cursor: none;
    }
    nav a:hover { color: var(--text); }
    .nav-cta {
      border: 1px solid var(--accent) !important;
      color: var(--accent) !important;
      padding: 0.45rem 1.2rem;
      border-radius: 2px;
      transition: background 0.2s, color 0.2s !important;
    }
    .nav-cta:hover { background: var(--accent); color: var(--bg) !important; }

    /* ── HERO ── */
    #hero {
      min-height: 100vh;
      display: flex; flex-direction: column; justify-content: center;
      padding: 8rem 3rem 4rem;
      position: relative;
      overflow: hidden;
    }
    .hero-grid {
      position: absolute; inset: 0;
      background-image:
        linear-gradient(rgba(0,229,160,0.04) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,229,160,0.04) 1px, transparent 1px);
      background-size: 60px 60px;
      mask-image: radial-gradient(ellipse 80% 80% at 50% 50%, black 20%, transparent 100%);
    }
    .hero-glow {
      position: absolute;
      width: 600px; height: 600px;
      border-radius: 50%;
      background: radial-gradient(circle, rgba(0,229,160,0.12) 0%, transparent 65%);
      top: -100px; right: -100px;
      animation: floatGlow 8s ease-in-out infinite;
    }
    .hero-glow2 {
      position: absolute;
      width: 400px; height: 400px;
      border-radius: 50%;
      background: radial-gradient(circle, rgba(0,184,255,0.08) 0%, transparent 65%);
      bottom: 0; left: 10%;
      animation: floatGlow 10s ease-in-out infinite reverse;
    }
    @keyframes floatGlow {
      0%, 100% { transform: translateY(0) scale(1); }
      50% { transform: translateY(-30px) scale(1.05); }
    }
    .hero-label {
      font-size: 0.75rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 1.5rem;
      display: flex; align-items: center; gap: 0.75rem;
    }
    .hero-label::before {
      content: '';
      display: block; width: 32px; height: 1px;
      background: var(--accent);
    }
    h1 {
      font-family: 'Syne', sans-serif;
      font-weight: 800;
      font-size: clamp(3rem, 7vw, 6.5rem);
      line-height: 0.95;
      letter-spacing: -0.04em;
      max-width: 14ch;
      position: relative;
    }
    h1 em {
      font-style: normal;
      color: transparent;
      -webkit-text-stroke: 1px rgba(0,229,160,0.7);
    }
    .hero-sub {
      margin-top: 2rem;
      font-size: 1.1rem;
      color: var(--muted);
      max-width: 48ch;
      line-height: 1.7;
    }
    .hero-actions {
      margin-top: 3rem;
      display: flex; gap: 1rem; flex-wrap: wrap;
    }
    .btn-primary {
      background: var(--accent);
      color: var(--bg);
      padding: 0.85rem 2rem;
      border-radius: 2px;
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 0.9rem;
      letter-spacing: 0.04em;
      text-decoration: none;
      cursor: none;
      display: inline-flex; align-items: center; gap: 0.5rem;
      transition: transform 0.2s, box-shadow 0.2s;
    }
    .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 30px rgba(0,229,160,0.3); }
    .btn-secondary {
      border: 1px solid var(--border);
      color: var(--text);
      padding: 0.85rem 2rem;
      border-radius: 2px;
      font-family: 'Syne', sans-serif;
      font-weight: 600;
      font-size: 0.9rem;
      letter-spacing: 0.04em;
      text-decoration: none;
      cursor: none;
      transition: border-color 0.2s, background 0.2s;
    }
    .btn-secondary:hover { border-color: var(--accent); background: rgba(0,229,160,0.05); }
    .hero-stats {
      margin-top: 5rem;
      display: flex; gap: 3rem; flex-wrap: wrap;
    }
    .stat { display: flex; flex-direction: column; gap: 0.25rem; }
    .stat-num {
      font-family: 'Syne', sans-serif;
      font-size: 2.5rem;
      font-weight: 800;
      color: var(--accent);
      line-height: 1;
    }
    .stat-label { font-size: 0.8rem; color: var(--muted); letter-spacing: 0.05em; text-transform: uppercase; }

    /* ── MARQUEE ── */
    .marquee-wrap {
      overflow: hidden;
      border-top: 1px solid var(--border);
      border-bottom: 1px solid var(--border);
      padding: 1rem 0;
      background: var(--surface);
    }
    .marquee-track {
      display: flex; gap: 3rem;
      animation: marquee 20s linear infinite;
      white-space: nowrap;
    }
    @keyframes marquee { from { transform: translateX(0); } to { transform: translateX(-50%); } }
    .marquee-item {
      font-family: 'Syne', sans-serif;
      font-size: 0.75rem;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      color: var(--muted);
      display: flex; align-items: center; gap: 1.5rem;
      flex-shrink: 0;
    }
    .marquee-item::after { content: '◆'; color: var(--accent); font-size: 0.5rem; }

    /* ── SECTIONS COMMON ── */
    section { padding: 7rem 3rem; }
    .section-label {
      font-size: 0.7rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--accent);
      margin-bottom: 1rem;
    }
    h2 {
      font-family: 'Syne', sans-serif;
      font-weight: 800;
      font-size: clamp(2rem, 4vw, 3.5rem);
      letter-spacing: -0.03em;
      line-height: 1.05;
      margin-bottom: 1.25rem;
    }
    .section-intro { color: var(--muted); font-size: 1.05rem; line-height: 1.75; max-width: 50ch; }

    /* ── SERVICES ── */
    #services { background: var(--surface); }
    .services-header { display: flex; justify-content: space-between; align-items: flex-end; margin-bottom: 4rem; flex-wrap: wrap; gap: 2rem; }
    .services-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 1px;
      background: var(--border);
    }
    .service-card {
      background: var(--card);
      padding: 2.5rem;
      position: relative;
      overflow: hidden;
      transition: background 0.3s;
    }
    .service-card:hover { background: #161d22; }
    .service-card::after {
      content: '';
      position: absolute;
      bottom: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      transform: scaleX(0);
      transition: transform 0.3s;
      transform-origin: left;
    }
    .service-card:hover::after { transform: scaleX(1); }
    .service-icon {
      width: 48px; height: 48px;
      margin-bottom: 1.5rem;
      color: var(--accent);
    }
    .service-num {
      font-family: 'Syne', sans-serif;
      font-size: 0.7rem;
      letter-spacing: 0.15em;
      color: var(--muted);
      margin-bottom: 0.75rem;
    }
    .service-card h3 {
      font-family: 'Syne', sans-serif;
      font-size: 1.2rem;
      font-weight: 700;
      margin-bottom: 0.75rem;
      letter-spacing: -0.02em;
    }
    .service-card p { font-size: 0.9rem; color: var(--muted); line-height: 1.7; }

    /* ── HOW IT WORKS ── */
    #process { position: relative; }
    .process-steps { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 0; margin-top: 4rem; }
    .step {
      padding: 2rem;
      border-left: 1px solid var(--border);
      position: relative;
    }
    .step:first-child { border-left: none; }
    .step-num {
      font-family: 'Syne', sans-serif;
      font-size: 3.5rem;
      font-weight: 800;
      color: transparent;
      -webkit-text-stroke: 1px var(--border);
      line-height: 1;
      margin-bottom: 1rem;
      transition: -webkit-text-stroke 0.3s;
    }
    .step:hover .step-num { -webkit-text-stroke-color: var(--accent); }
    .step h3 { font-family: 'Syne', sans-serif; font-size: 1rem; font-weight: 700; margin-bottom: 0.5rem; }
    .step p { font-size: 0.85rem; color: var(--muted); line-height: 1.65; }

    /* ── WHY US ── */
    #why { background: var(--surface); }
    .why-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 4rem; align-items: center; margin-top: 3rem; }
    .why-visual {
      position: relative;
      height: 420px;
      display: flex; align-items: center; justify-content: center;
    }
    .ring {
      position: absolute;
      border-radius: 50%;
      border: 1px solid;
      animation: spin linear infinite;
    }
    .ring:nth-child(1) { width: 300px; height: 300px; border-color: rgba(0,229,160,0.15); animation-duration: 20s; }
    .ring:nth-child(2) { width: 220px; height: 220px; border-color: rgba(0,184,255,0.2); animation-duration: 15s; border-style: dashed; }
    .ring:nth-child(3) { width: 140px; height: 140px; border-color: rgba(0,229,160,0.3); animation-duration: 10s; animation-direction: reverse; }
    @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
    .ring-center {
      width: 64px; height: 64px;
      background: var(--accent);
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      position: relative; z-index: 1;
      box-shadow: 0 0 40px rgba(0,229,160,0.4);
    }
    .ring-center svg { width: 28px; height: 28px; color: var(--bg); }
    .why-list { display: flex; flex-direction: column; gap: 1.5rem; }
    .why-item { display: flex; gap: 1rem; align-items: flex-start; }
    .why-check {
      width: 20px; height: 20px; flex-shrink: 0;
      border: 1px solid var(--accent);
      border-radius: 2px;
      display: flex; align-items: center; justify-content: center;
      color: var(--accent);
      margin-top: 0.15rem;
      font-size: 0.75rem;
    }
    .why-item strong { display: block; font-family: 'Syne', sans-serif; font-size: 0.95rem; font-weight: 600; margin-bottom: 0.25rem; }
    .why-item p { font-size: 0.85rem; color: var(--muted); line-height: 1.65; }

    /* ── INDUSTRIES ── */
    .industries-pills { display: flex; flex-wrap: wrap; gap: 0.75rem; margin-top: 2.5rem; }
    .pill {
      padding: 0.6rem 1.25rem;
      border: 1px solid var(--border);
      border-radius: 100px;
      font-size: 0.85rem;
      color: var(--muted);
      cursor: none;
      transition: border-color 0.2s, color 0.2s, background 0.2s;
    }
    .pill:hover { border-color: var(--accent); color: var(--accent); background: rgba(0,229,160,0.05); }

    /* ── TESTIMONIALS ── */
    #testimonials { background: var(--surface); }
    .testi-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 1.5rem; margin-top: 3rem; }
    .testi-card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 2rem;
      transition: border-color 0.3s, transform 0.3s;
    }
    .testi-card:hover { border-color: rgba(0,229,160,0.3); transform: translateY(-4px); }
    .testi-stars { color: var(--accent); font-size: 0.85rem; margin-bottom: 1rem; letter-spacing: 0.1em; }
    .testi-card blockquote { font-size: 0.95rem; line-height: 1.75; color: #c8d4db; margin-bottom: 1.5rem; }
    .testi-author { display: flex; align-items: center; gap: 0.75rem; }
    .testi-avatar {
      width: 36px; height: 36px;
      border-radius: 50%;
      background: linear-gradient(135deg, var(--accent), var(--accent2));
      display: flex; align-items: center; justify-content: center;
      font-family: 'Syne', sans-serif;
      font-weight: 700;
      font-size: 0.75rem;
      color: var(--bg);
    }
    .testi-name { font-family: 'Syne', sans-serif; font-size: 0.85rem; font-weight: 600; }
    .testi-role { font-size: 0.75rem; color: var(--muted); }

    /* ── CTA ── */
    #cta {
      text-align: center;
      position: relative;
      overflow: hidden;
    }
    #cta::before {
      content: '';
      position: absolute;
      inset: 0;
      background: radial-gradient(ellipse 60% 60% at 50% 50%, rgba(0,229,160,0.07) 0%, transparent 70%);
    }
    #cta h2 { font-size: clamp(2.5rem, 5vw, 4.5rem); max-width: 16ch; margin: 0 auto 1.5rem; }
    #cta p { color: var(--muted); margin: 0 auto 2.5rem; max-width: 48ch; }
    .cta-actions { display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap; }

    /* ── FOOTER ── */
    footer {
      border-top: 1px solid var(--border);
      padding: 3rem;
      display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 2rem;
      background: var(--surface);
    }
    .footer-logo { font-family: 'Syne', sans-serif; font-weight: 800; font-size: 1rem; display: flex; align-items: center; gap: 0.5rem; }
    footer nav ul { list-style: none; display: flex; gap: 2rem; flex-wrap: wrap; }
    footer nav a { font-size: 0.8rem; color: var(--muted); text-decoration: none; cursor: none; transition: color 0.2s; }
    footer nav a:hover { color: var(--text); }
    .footer-copy { font-size: 0.75rem; color: var(--muted); }

    /* ── ANIMATIONS ── */
    .reveal {
      opacity: 0;
      transform: translateY(28px);
      transition: opacity 0.7s ease, transform 0.7s ease;
    }
    .reveal.visible { opacity: 1; transform: none; }

    @media (max-width: 768px) {
      nav { padding: 1.25rem 1.5rem; }
      nav ul { display: none; }
      section { padding: 5rem 1.5rem; }
      #hero { padding: 7rem 1.5rem 3rem; }
      .why-grid { grid-template-columns: 1fr; }
      .why-visual { height: 260px; }
      footer { padding: 2rem 1.5rem; }
    }
  </style>
</head>
<body>

  <div class="cursor" id="cursor"></div>
  <div class="cursor-ring" id="cursorRing"></div>

  <!-- NAV -->
  <nav id="navbar">
    <div class="logo">
      <span class="logo-dot"></span>
      Easy Monday Solutions
    </div>
    <ul>
      <li><a href="#services">Servicios</a></li>
      <li><a href="#process">Proceso</a></li>
      <li><a href="#why">Nosotros</a></li>
      <li><a href="#testimonials">Clientes</a></li>
      <li><a href="#cta" class="nav-cta">Contacto</a></li>
    </ul>
  </nav>

  <!-- HERO -->
  <section id="hero">
    <div class="hero-grid"></div>
    <div class="hero-glow"></div>
    <div class="hero-glow2"></div>
    <div style="position:relative;">
      <p class="hero-label reveal">Consultoría de Automatización</p>
      <h1 class="reveal" style="transition-delay:0.1s">El lunes<br/>más <em>fácil</em><br/>de tu empresa</h1>
      <p class="hero-sub reveal" style="transition-delay:0.2s">Automatizamos los procesos que consumen tu tiempo, para que tu equipo se enfoque en lo que realmente importa. Software inteligente, resultados reales.</p>
      <div class="hero-actions reveal" style="transition-delay:0.3s">
        <a href="#cta" class="btn-primary">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
          Empezar ahora
        </a>
        <a href="#process" class="btn-secondary">Ver cómo funciona</a>
      </div>
      <div class="hero-stats reveal" style="transition-delay:0.4s">
        <div class="stat"><span class="stat-num">80%</span><span class="stat-label">Reducción de tareas manuales</span></div>
        <div class="stat"><span class="stat-num">3×</span><span class="stat-label">Velocidad operativa</span></div>
        <div class="stat"><span class="stat-num">100+</span><span class="stat-label">Procesos automatizados</span></div>
      </div>
    </div>
  </section>

  <!-- MARQUEE -->
  <div class="marquee-wrap">
    <div class="marquee-track">
      <span class="marquee-item">Automatización de procesos</span>
      <span class="marquee-item">Integración de sistemas</span>
      <span class="marquee-item">RPA & Bots</span>
      <span class="marquee-item">APIs e integraciones</span>
      <span class="marquee-item">Dashboards inteligentes</span>
      <span class="marquee-item">Flujos de trabajo</span>
      <span class="marquee-item">Notificaciones automáticas</span>
      <span class="marquee-item">Sincronización de datos</span>
      <span class="marquee-item">Automatización de procesos</span>
      <span class="marquee-item">Integración de sistemas</span>
      <span class="marquee-item">RPA & Bots</span>
      <span class="marquee-item">APIs e integraciones</span>
      <span class="marquee-item">Dashboards inteligentes</span>
      <span class="marquee-item">Flujos de trabajo</span>
      <span class="marquee-item">Notificaciones automáticas</span>
      <span class="marquee-item">Sincronización de datos</span>
    </div>
  </div>

  <!-- SERVICES -->
  <section id="services">
    <div class="services-header">
      <div>
        <p class="section-label reveal">Servicios</p>
        <h2 class="reveal">Lo que<br/>hacemos</h2>
      </div>
      <p class="section-intro reveal">Desde la auditoría inicial hasta el despliegue en producción, cubrimos el ciclo completo de automatización de tu empresa.</p>
    </div>
    <div class="services-grid">

      <div class="service-card reveal">
        <p class="service-num">01</p>
        <svg class="service-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M9 3H5a2 2 0 00-2 2v4m6-6h10a2 2 0 012 2v4M9 3v18m0 0h10a2 2 0 002-2V9M9 21H5a2 2 0 01-2-2V9m0 0h18"/></svg>
        <h3>Automatización de Flujos</h3>
        <p>Elimina los cuellos de botella mapeando y digitalizando procesos operativos. Desde aprobaciones hasta reportes automáticos.</p>
      </div>

      <div class="service-card reveal" style="transition-delay:0.1s">
        <p class="service-num">02</p>
        <svg class="service-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="12" cy="12" r="3"/><path d="M19.07 4.93A10 10 0 110 12h2"/><path d="M12 2v2M12 20v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42"/></svg>
        <h3>Integración de Sistemas</h3>
        <p>Conectamos tu ERP, CRM, e-commerce y herramientas SaaS para que hablen entre sí sin intervención humana.</p>
      </div>

      <div class="service-card reveal" style="transition-delay:0.2s">
        <p class="service-num">03</p>
        <svg class="service-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/><path d="M14 17.5h7M17.5 14v7"/></svg>
        <h3>RPA & Bots Inteligentes</h3>
        <p>Robots de software que ejecutan tareas repetitivas, extraen datos y rellenan formularios con precisión 24/7.</p>
      </div>

      <div class="service-card reveal" style="transition-delay:0.3s">
        <p class="service-num">04</p>
        <svg class="service-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M3 3h18v18H3z" rx="2"/><path d="M3 9h18M9 21V9"/></svg>
        <h3>Dashboards & Reportes</h3>
        <p>Centraliza tus KPIs en paneles en tiempo real. Decisiones basadas en datos, no en suposiciones.</p>
      </div>

      <div class="service-card reveal" style="transition-delay:0.1s">
        <p class="service-num">05</p>
        <svg class="service-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>
        <h3>Consultoría & Auditoría</h3>
        <p>Analizamos tus operaciones y diseñamos un roadmap de automatización con ROI medible y plazos claros.</p>
      </div>

      <div class="service-card reveal" style="transition-delay:0.2s">
        <p class="service-num">06</p>
        <svg class="service-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M18 20V10M12 20V4M6 20v-6"/></svg>
        <h3>Analítica con IA</h3>
        <p>Modelos predictivos y análisis inteligente que anticipan problemas antes de que ocurran y optimizan recursos.</p>
      </div>

    </div>
  </section>

  <!-- PROCESS -->
  <section id="process">
    <p class="section-label reveal">Proceso</p>
    <h2 class="reveal">Cómo<br/>trabajamos</h2>
    <div class="process-steps">
      <div class="step reveal">
        <div class="step-num">01</div>
        <h3>Diagnóstico</h3>
        <p>Mapeamos tus procesos actuales e identificamos los puntos de mayor impacto para la automatización.</p>
      </div>
      <div class="step reveal" style="transition-delay:0.1s">
        <div class="step-num">02</div>
        <h3>Diseño</h3>
        <p>Proponemos soluciones técnicas a medida con estimaciones de ahorro de tiempo y costos concretos.</p>
      </div>
      <div class="step reveal" style="transition-delay:0.2s">
        <div class="step-num">03</div>
        <h3>Desarrollo</h3>
        <p>Construimos e integramos las automatizaciones en iteraciones cortas con validaciones continuas.</p>
      </div>
      <div class="step reveal" style="transition-delay:0.3s">
        <div class="step-num">04</div>
        <h3>Despliegue</h3>
        <p>Ponemos en producción con capacitación al equipo y monitoreo activo en las primeras semanas.</p>
      </div>
      <div class="step reveal" style="transition-delay:0.4s">
        <div class="step-num">05</div>
        <h3>Optimización</h3>
        <p>Medimos resultados, ajustamos parámetros y escalamos las soluciones según el crecimiento de tu empresa.</p>
      </div>
    </div>
  </section>

  <!-- WHY -->
  <section id="why">
    <div class="why-grid">
      <div>
        <p class="section-label reveal">Por qué elegirnos</p>
        <h2 class="reveal">Automatización<br/>sin<br/>complicaciones</h2>
        <div class="why-list" style="margin-top:2rem">
          <div class="why-item reveal">
            <div class="why-check">✓</div>
            <div>
              <strong>Enfoque en resultados</strong>
              <p>No vendemos tecnología, vendemos tiempo libre y eficiencia medible con métricas claras.</p>
            </div>
          </div>
          <div class="why-item reveal" style="transition-delay:0.1s">
            <div class="why-check">✓</div>
            <div>
              <strong>Implementación ágil</strong>
              <p>Primeras automatizaciones en producción en menos de 4 semanas. Sin proyectos eternos.</p>
            </div>
          </div>
          <div class="why-item reveal" style="transition-delay:0.2s">
            <div class="why-check">✓</div>
            <div>
              <strong>Soporte continuo</strong>
              <p>Acompañamos la evolución de tu empresa. Las necesidades cambian, las soluciones también.</p>
            </div>
          </div>
          <div class="why-item reveal" style="transition-delay:0.3s">
            <div class="why-check">✓</div>
            <div>
              <strong>Sin dependencia tecnológica</strong>
              <p>Documentamos todo y capacitamos a tu equipo para que seas autónomo si así lo deseas.</p>
            </div>
          </div>
        </div>
      </div>
      <div class="why-visual reveal">
        <div class="ring"></div>
        <div class="ring"></div>
        <div class="ring"></div>
        <div class="ring-center">
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/></svg>
        </div>
      </div>
    </div>
  </section>

  <!-- INDUSTRIES -->
  <section id="industries">
    <p class="section-label reveal">Industrias</p>
    <h2 class="reveal">Trabajamos<br/>con cualquier sector</h2>
    <p class="section-intro reveal">Si tu empresa repite tareas, tiene datos sin conectar o reportes manuales, podemos ayudarte.</p>
    <div class="industries-pills reveal" style="transition-delay:0.2s">
      <span class="pill">🏭 Manufactura</span>
      <span class="pill">🛒 Retail & E-commerce</span>
      <span class="pill">🏥 Salud</span>
      <span class="pill">💰 Finanzas & Contabilidad</span>
      <span class="pill">🏗️ Construcción</span>
      <span class="pill">🚚 Logística</span>
      <span class="pill">🏨 Hospitalidad</span>
      <span class="pill">⚖️ Servicios legales</span>
      <span class="pill">📊 Marketing & Agencias</span>
      <span class="pill">🎓 Educación</span>
      <span class="pill">🌾 Agro</span>
      <span class="pill">🔧 Servicios técnicos</span>
    </div>
  </section>

  <!-- TESTIMONIALS -->
  <section id="testimonials">
    <p class="section-label reveal">Clientes</p>
    <h2 class="reveal">Lo que dicen<br/>nuestros clientes</h2>
    <div class="testi-grid">
      <div class="testi-card reveal">
        <div class="testi-stars">★★★★★</div>
        <blockquote>"Automatizaron todo nuestro proceso de facturación. Lo que antes nos llevaba 3 horas al día ahora ocurre solo en minutos. Increíble."</blockquote>
        <div class="testi-author">
          <div class="testi-avatar">ML</div>
          <div>
            <p class="testi-name">Mariana López</p>
            <p class="testi-role">CFO, Distribuidora Andina</p>
          </div>
        </div>
      </div>
      <div class="testi-card reveal" style="transition-delay:0.1s">
        <div class="testi-stars">★★★★★</div>
        <blockquote>"Conectaron nuestro CRM con el inventario y el e-commerce. Cero errores de stock en 8 meses. La inversión se pagó en 6 semanas."</blockquote>
        <div class="testi-author">
          <div class="testi-avatar">CR</div>
          <div>
            <p class="testi-name">Carlos Ríos</p>
            <p class="testi-role">Director de Operaciones, TechRetail</p>
          </div>
        </div>
      </div>
      <div class="testi-card reveal" style="transition-delay:0.2s">
        <div class="testi-stars">★★★★★</div>
        <blockquote>"El equipo entendió nuestro negocio rápido. En 3 semanas teníamos el primer bot corriendo. Ahora procesamos el doble con el mismo personal."</blockquote>
        <div class="testi-author">
          <div class="testi-avatar">SV</div>
          <div>
            <p class="testi-name">Sofía Vargas</p>
            <p class="testi-role">Gerente General, LogiPack S.A.</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- CTA -->
  <section id="cta">
    <p class="section-label reveal" style="justify-content:center;display:flex">Contacto</p>
    <h2 class="reveal">Tu próximo lunes<br/>puede ser diferente</h2>
    <p class="reveal" style="transition-delay:0.1s">Conversemos sobre tus procesos actuales. La primera consulta es gratuita y sin compromiso.</p>
    <div class="cta-actions reveal" style="transition-delay:0.2s">
      <a href="mailto:hola@easymonday.solutions" class="btn-primary">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
        hola@easymonday.solutions
      </a>
      <a href="https://wa.me/5491100000000" class="btn-secondary">WhatsApp →</a>
    </div>
  </section>

  <!-- FOOTER -->
  <footer>
    <div class="footer-logo">
      <span class="logo-dot"></span>
      Easy Monday Solutions
    </div>
    <nav>
      <ul>
        <li><a href="#services">Servicios</a></li>
        <li><a href="#process">Proceso</a></li>
        <li><a href="#why">Nosotros</a></li>
        <li><a href="#cta">Contacto</a></li>
      </ul>
    </nav>
    <p class="footer-copy">© 2025 Easy Monday Solutions. Todos los derechos reservados.</p>
  </footer>

  <script>
    // Cursor
    const cursor = document.getElementById('cursor');
    const ring = document.getElementById('cursorRing');
    let mx = 0, my = 0, rx = 0, ry = 0;
    document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });
    function animateCursor() {
      cursor.style.transform = `translate(${mx - 6}px, ${my - 6}px)`;
      rx += (mx - rx) * 0.12; ry += (my - ry) * 0.12;
      ring.style.transform = `translate(${rx - 18}px, ${ry - 18}px)`;
      requestAnimationFrame(animateCursor);
    }
    animateCursor();

    // Navbar scroll
    const navbar = document.getElementById('navbar');
    window.addEventListener('scroll', () => {
      navbar.classList.toggle('scrolled', window.scrollY > 50);
    });

    // Reveal on scroll
    const reveals = document.querySelectorAll('.reveal');
    const obs = new IntersectionObserver(entries => {
      entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); obs.unobserve(e.target); } });
    }, { threshold: 0.12 });
    reveals.forEach(el => obs.observe(el));

    // Trigger hero reveals immediately
    setTimeout(() => {
      document.querySelectorAll('#hero .reveal').forEach(el => el.classList.add('visible'));
    }, 100);
  </script>
</body>
</html>
