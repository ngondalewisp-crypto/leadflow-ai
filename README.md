<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LeadFlow AI — Lewis</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:ital,wght@0,300;0,400;1,300&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0a;
    --surface: #111111;
    --border: #1e1e1e;
    --accent: #00ff88;
    --accent2: #00cfff;
    --text: #f0f0f0;
    --muted: #555;
    --dim: #888;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  .cursor {
    position: fixed;
    width: 10px;
    height: 10px;
    background: var(--accent);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9999;
    transition: transform 0.15s ease;
    mix-blend-mode: screen;
  }
  .cursor-ring {
    position: fixed;
    width: 36px;
    height: 36px;
    border: 1px solid var(--accent);
    border-radius: 50%;
    pointer-events: none;
    z-index: 9998;
    transition: all 0.3s ease;
    opacity: 0.5;
    mix-blend-mode: screen;
  }

  /* Noise overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 1000;
    opacity: 0.4;
  }

  /* Grid background */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,255,136,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,255,136,0.03) 1px, transparent 1px);
    background-size: 60px 60px;
    pointer-events: none;
    z-index: 0;
  }

  /* NAV */
  nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 100;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 40px;
    border-bottom: 1px solid var(--border);
    background: rgba(10,10,10,0.8);
    backdrop-filter: blur(20px);
  }

  .logo {
    font-family: 'DM Mono', monospace;
    font-size: 13px;
    color: var(--accent);
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  .nav-tag {
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 0.15em;
    text-transform: uppercase;
  }

  /* HERO */
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 120px 40px 80px;
    z-index: 1;
    overflow: hidden;
  }

  .hero-label {
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    letter-spacing: 0.3em;
    text-transform: uppercase;
    margin-bottom: 32px;
    display: flex;
    align-items: center;
    gap: 12px;
    opacity: 0;
    animation: fadeUp 0.8s ease forwards 0.2s;
  }

  .hero-label::before {
    content: '';
    width: 32px;
    height: 1px;
    background: var(--accent);
  }

  h1 {
    font-size: clamp(48px, 9vw, 96px);
    font-weight: 800;
    line-height: 0.95;
    letter-spacing: -0.03em;
    margin-bottom: 32px;
    opacity: 0;
    animation: fadeUp 0.8s ease forwards 0.4s;
  }

  h1 .highlight {
    color: var(--accent);
    display: block;
  }

  h1 .dim-word {
    color: var(--muted);
  }

  .hero-sub {
    font-size: 16px;
    color: var(--dim);
    max-width: 480px;
    line-height: 1.7;
    margin-bottom: 52px;
    font-weight: 400;
    opacity: 0;
    animation: fadeUp 0.8s ease forwards 0.6s;
  }

  .hero-cta {
    display: flex;
    gap: 16px;
    flex-wrap: wrap;
    opacity: 0;
    animation: fadeUp 0.8s ease forwards 0.8s;
  }

  .btn-primary {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    padding: 16px 32px;
    background: var(--accent);
    color: #000;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 14px;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    text-decoration: none;
    border: none;
    cursor: none;
    transition: all 0.2s ease;
    position: relative;
    overflow: hidden;
  }

  .btn-primary::after {
    content: '';
    position: absolute;
    inset: 0;
    background: rgba(0,0,0,0.1);
    opacity: 0;
    transition: opacity 0.2s;
  }

  .btn-primary:hover::after { opacity: 1; }
  .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 12px 40px rgba(0,255,136,0.3); }

  .btn-secondary {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    padding: 16px 32px;
    background: transparent;
    color: var(--text);
    font-family: 'Syne', sans-serif;
    font-weight: 600;
    font-size: 14px;
    letter-spacing: 0.05em;
    text-transform: uppercase;
    text-decoration: none;
    border: 1px solid var(--border);
    cursor: none;
    transition: all 0.2s ease;
  }

  .btn-secondary:hover { border-color: var(--accent); color: var(--accent); }

  /* Glowing orb */
  .orb {
    position: absolute;
    width: 600px;
    height: 600px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(0,255,136,0.08) 0%, transparent 70%);
    top: 50%;
    right: -100px;
    transform: translateY(-50%);
    pointer-events: none;
    animation: pulse 4s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 0.5; transform: translateY(-50%) scale(1); }
    50% { opacity: 1; transform: translateY(-50%) scale(1.1); }
  }

  /* STATS */
  .stats-bar {
    position: relative;
    z-index: 1;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
  }

  .stat-item {
    padding: 48px 40px;
    border-right: 1px solid var(--border);
    opacity: 0;
    animation: fadeUp 0.6s ease forwards;
  }

  .stat-item:last-child { border-right: none; }
  .stat-item:nth-child(1) { animation-delay: 0.9s; }
  .stat-item:nth-child(2) { animation-delay: 1.1s; }
  .stat-item:nth-child(3) { animation-delay: 1.3s; }

  .stat-number {
    font-size: 48px;
    font-weight: 800;
    color: var(--accent);
    letter-spacing: -0.03em;
    line-height: 1;
    margin-bottom: 8px;
  }

  .stat-label {
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    color: var(--dim);
    text-transform: uppercase;
    letter-spacing: 0.15em;
  }

  /* SECTION */
  section {
    position: relative;
    z-index: 1;
    padding: 100px 40px;
  }

  .section-tag {
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    letter-spacing: 0.3em;
    text-transform: uppercase;
    margin-bottom: 24px;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .section-tag::before {
    content: '';
    width: 24px;
    height: 1px;
    background: var(--accent);
  }

  h2 {
    font-size: clamp(32px, 5vw, 56px);
    font-weight: 800;
    letter-spacing: -0.03em;
    line-height: 1.05;
    margin-bottom: 60px;
  }

  /* PROBLEM */
  .problem-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2px;
    background: var(--border);
    border: 1px solid var(--border);
  }

  .problem-card {
    background: var(--surface);
    padding: 40px;
    transition: background 0.2s;
  }

  .problem-card:hover { background: #161616; }

  .problem-icon {
    font-size: 28px;
    margin-bottom: 20px;
  }

  .problem-card h3 {
    font-size: 18px;
    font-weight: 700;
    margin-bottom: 12px;
    color: var(--text);
  }

  .problem-card p {
    font-size: 14px;
    color: var(--dim);
    line-height: 1.7;
    font-weight: 400;
  }

  /* WHAT YOU GET */
  .deliverables {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 24px;
  }

  .deliverable-card {
    border: 1px solid var(--border);
    padding: 36px;
    position: relative;
    transition: border-color 0.2s, transform 0.2s;
    background: var(--surface);
  }

  .deliverable-card:hover {
    border-color: var(--accent);
    transform: translateY(-4px);
  }

  .deliverable-number {
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    color: var(--accent);
    letter-spacing: 0.2em;
    margin-bottom: 20px;
  }

  .deliverable-card h3 {
    font-size: 20px;
    font-weight: 700;
    margin-bottom: 12px;
  }

  .deliverable-card p {
    font-size: 14px;
    color: var(--dim);
    line-height: 1.7;
    font-weight: 400;
  }

  /* OFFER */
  .offer-box {
    border: 1px solid var(--accent);
    padding: 60px;
    position: relative;
    background: linear-gradient(135deg, rgba(0,255,136,0.04) 0%, transparent 60%);
  }

  .offer-box::before {
    content: 'OFFRE';
    position: absolute;
    top: -1px;
    left: 40px;
    background: var(--accent);
    color: #000;
    font-family: 'DM Mono', monospace;
    font-size: 10px;
    font-weight: 400;
    letter-spacing: 0.2em;
    padding: 4px 12px;
  }

  .offer-price {
    font-size: 80px;
    font-weight: 800;
    color: var(--accent);
    letter-spacing: -0.04em;
    line-height: 1;
    margin-bottom: 8px;
  }

  .offer-price-label {
    font-family: 'DM Mono', monospace;
    font-size: 12px;
    color: var(--dim);
    letter-spacing: 0.15em;
    margin-bottom: 48px;
  }

  .offer-items {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 48px;
  }

  .offer-item {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    font-size: 15px;
    color: var(--dim);
    line-height: 1.5;
  }

  .offer-item::before {
    content: '✓';
    color: var(--accent);
    font-weight: 700;
    flex-shrink: 0;
    margin-top: 2px;
  }

  .guarantee {
    border-top: 1px solid var(--border);
    padding-top: 32px;
    font-family: 'DM Mono', monospace;
    font-size: 12px;
    color: var(--dim);
    font-style: italic;
    line-height: 1.7;
  }

  /* PROCESS */
  .process-steps {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 0;
    position: relative;
  }

  .process-steps::before {
    content: '';
    position: absolute;
    top: 24px;
    left: 10%;
    right: 10%;
    height: 1px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    opacity: 0.3;
  }

  .process-step {
    padding: 0 24px;
    text-align: center;
  }

  .step-dot {
    width: 48px;
    height: 48px;
    border: 1px solid var(--accent);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 auto 24px;
    font-family: 'DM Mono', monospace;
    font-size: 13px;
    color: var(--accent);
    background: var(--bg);
    position: relative;
    z-index: 1;
  }

  .process-step h3 {
    font-size: 16px;
    font-weight: 700;
    margin-bottom: 10px;
  }

  .process-step p {
    font-size: 13px;
    color: var(--dim);
    line-height: 1.6;
  }

  /* FOOTER CTA */
  .footer-cta {
    border-top: 1px solid var(--border);
    padding: 100px 40px;
    text-align: center;
    position: relative;
    z-index: 1;
  }

  .footer-cta h2 {
    margin-bottom: 16px;
  }

  .footer-cta p {
    color: var(--dim);
    font-size: 16px;
    margin-bottom: 48px;
    font-weight: 400;
  }

  .footer-sig {
    margin-top: 80px;
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 0.2em;
  }

  /* ANIMATIONS */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(24px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .reveal {
    opacity: 0;
    transform: translateY(32px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }

  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* RESPONSIVE */
  @media (max-width: 768px) {
    nav { padding: 16px 20px; }
    .hero { padding: 100px 20px 60px; }
    h1 { font-size: 44px; }
    .stats-bar { grid-template-columns: 1fr; }
    .stat-item { border-right: none; border-bottom: 1px solid var(--border); }
    section { padding: 60px 20px; }
    .problem-grid { grid-template-columns: 1fr; }
    .deliverables { grid-template-columns: 1fr; }
    .offer-box { padding: 40px 24px; }
    .offer-price { font-size: 56px; }
    .offer-items { grid-template-columns: 1fr; }
    .process-steps { grid-template-columns: 1fr 1fr; gap: 32px; }
    .process-steps::before { display: none; }
    .footer-cta { padding: 60px 20px; }
  }
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>

<!-- NAV -->
<nav>
  <div class="logo">LeadFlow AI</div>
  <div class="nav-tag">by Lewis</div>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="orb"></div>
  <div class="hero-label">Automatisation IA — Coaches & Infopreneurs</div>
  <h1>
    Tes prospects<br>
    <span class="highlight">closés automatiquement.</span>
    <span class="dim-word">Pendant que tu dors.</span>
  </h1>
  <p class="hero-sub">
    J'installe un système IA qui qualifie tes leads, relance tes prospects et génère des clients — sans que tu touches à rien. En 48h.
  </p>
  <div class="hero-cta">
    <a href="#audit" class="btn-primary">→ Réserver mon audit gratuit</a>
    <a href="#offre" class="btn-secondary">Voir l'offre</a>
  </div>
</section>

<!-- STATS -->
<div class="stats-bar">
  <div class="stat-item">
    <div class="stat-number">+3x</div>
    <div class="stat-label">Leads qualifiés en moyenne</div>
  </div>
  <div class="stat-item">
    <div class="stat-number">48h</div>
    <div class="stat-label">Délai d'installation</div>
  </div>
  <div class="stat-item">
    <div class="stat-number">0€</div>
    <div class="stat-label">Coût mensuel (outils gratuits)</div>
  </div>
</div>

<!-- PROBLÈME -->
<section>
  <div class="section-tag reveal">Le problème</div>
  <h2 class="reveal">Tu perds de l'argent<br>chaque semaine.</h2>
  <div class="problem-grid reveal">
    <div class="problem-card">
      <div class="problem-icon">⏱</div>
      <h3>Réponses trop lentes</h3>
      <p>Un prospect qui attend plus de 2h sans réponse passe à la concurrence. Tu laisses des milliers d'euros sur la table chaque mois.</p>
    </div>
    <div class="problem-card">
      <div class="problem-icon">🔁</div>
      <h3>Pas de relance systématique</h3>
      <p>80% des ventes se font après la 5e relance. La plupart des coaches abandonnent après la première. C'est de l'argent perdu.</p>
    </div>
    <div class="problem-card">
      <div class="problem-icon">🤯</div>
      <h3>Qualification manuelle</h3>
      <p>Tu passes des heures à trier des leads non qualifiés au lieu de te concentrer sur ce qui compte vraiment : créer et closer.</p>
    </div>
    <div class="problem-card">
      <div class="problem-icon">📉</div>
      <h3>Revenus imprévisibles</h3>
      <p>Sans système, tes revenus dépendent de ton énergie. Quand tu décroches, les ventes s'arrêtent. C'est pas un business, c'est un job.</p>
    </div>
  </div>
</section>

<!-- CE QUE TU REÇOIS -->
<section style="background: var(--surface); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border);">
  <div class="section-tag reveal">La solution</div>
  <h2 class="reveal">Ce que j'installe<br>pour toi.</h2>
  <div class="deliverables">
    <div class="deliverable-card reveal">
      <div class="deliverable-number">01</div>
      <h3>Bot de qualification IA</h3>
      <p>Dès qu'un prospect envoie un DM ou remplit ton formulaire, l'IA engage la conversation, pose les bonnes questions et filtre automatiquement les leads chauds.</p>
    </div>
    <div class="deliverable-card reveal">
      <div class="deliverable-number">02</div>
      <h3>Séquence de relance auto</h3>
      <p>3 messages personnalisés envoyés automatiquement sur 5 jours. Naturels, humains, efficaces. Tes prospects reviennent sans que tu lèves le petit doigt.</p>
    </div>
    <div class="deliverable-card reveal">
      <div class="deliverable-number">03</div>
      <h3>Alertes leads chauds</h3>
      <p>Tu reçois une notification uniquement quand un prospect est qualifié et prêt à acheter. Tu n'interviens qu'au moment où c'est rentable.</p>
    </div>
  </div>
</section>

<!-- PROCESSUS -->
<section>
  <div class="section-tag reveal">Comment ça marche</div>
  <h2 class="reveal">4 étapes.<br>72h maximum.</h2>
  <div class="process-steps reveal">
    <div class="process-step">
      <div class="step-dot">01</div>
      <h3>Audit gratuit</h3>
      <p>15 min pour analyser ton tunnel actuel et identifier les fuites.</p>
    </div>
    <div class="process-step">
      <div class="step-dot">02</div>
      <h3>Configuration</h3>
      <p>J'installe et connecte tous les outils selon ton workflow.</p>
    </div>
    <div class="process-step">
      <div class="step-dot">03</div>
      <h3>Tests & validation</h3>
      <p>On teste en live ensemble pour s'assurer que tout tourne.</p>
    </div>
    <div class="process-step">
      <div class="step-dot">04</div>
      <h3>Déploiement</h3>
      <p>Le système est live. Tu reçois une formation courte et je reste dispo 2 semaines.</p>
    </div>
  </div>
</section>

<!-- OFFRE -->
<section id="offre">
  <div class="section-tag reveal">L'investissement</div>
  <div class="offer-box reveal">
    <div class="offer-price">800€</div>
    <div class="offer-price-label">Forfait unique — tout inclus</div>
    <div class="offer-items">
      <div class="offer-item">Audit complet de ton tunnel (gratuit)</div>
      <div class="offer-item">Installation complète du système IA</div>
      <div class="offer-item">Bot de qualification sur mesure</div>
      <div class="offer-item">Séquence de relance automatique</div>
      <div class="offer-item">Alertes leads qualifiés en temps réel</div>
      <div class="offer-item">Formation vidéo pour ton équipe</div>
      <div class="offer-item">Support prioritaire 2 semaines</div>
      <div class="offer-item">Outils 100% gratuits (0€/mois)</div>
    </div>
    <div class="guarantee">
      🛡 Garantie résultats — Si le système ne génère pas au moins 3 leads qualifiés supplémentaires dans les 14 premiers jours, je te rembourse intégralement. Aucune question posée.
    </div>
  </div>
</section>

<!-- FOOTER CTA -->
<div class="footer-cta" id="audit">
  <div class="section-tag" style="justify-content: center; margin-bottom: 32px;">
    <span>Prêt à automatiser ?</span>
  </div>
  <h2>Réserve ton audit<br><span style="color: var(--accent)">gratuit maintenant.</span></h2>
  <p>15 minutes. Zéro engagement. On analyse ensemble ce que tu perds chaque semaine.</p>
  <a href="https://instagram.com/" class="btn-primary" style="display: inline-flex;">→ Envoyer un message à Lewis</a>
  <div class="footer-sig">LEADFLOW AI — by Lewis · 2025</div>
</div>

<script>
  // Custom cursor
  const cursor = document.getElementById('cursor');
  const ring = document.getElementById('cursorRing');
  let mouseX = 0, mouseY = 0, ringX = 0, ringY = 0;

  document.addEventListener('mousemove', e => {
    mouseX = e.clientX;
    mouseY = e.clientY;
    cursor.style.left = mouseX - 5 + 'px';
    cursor.style.top = mouseY - 5 + 'px';
  });

  function animateRing() {
    ringX += (mouseX - ringX - 18) * 0.12;
    ringY += (mouseY - ringY - 18) * 0.12;
    ring.style.left = ringX + 'px';
    ring.style.top = ringY + 'px';
    requestAnimationFrame(animateRing);
  }
  animateRing();

  document.querySelectorAll('a, button').forEach(el => {
    el.addEventListener('mouseenter', () => {
      cursor.style.transform = 'scale(3)';
      ring.style.opacity = '0';
    });
    el.addEventListener('mouseleave', () => {
      cursor.style.transform = 'scale(1)';
      ring.style.opacity = '0.5';
    });
  });

  // Scroll reveal
  const observer = new IntersectionObserver(entries => {
    entries.forEach((entry, i) => {
      if (entry.isIntersecting) {
        setTimeout(() => entry.target.classList.add('visible'), i * 80);
      }
    });
  }, { threshold: 0.1 });

  document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
</script>
</body>
</html>
