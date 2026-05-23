<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DecoBot — Rule-Based AI Chatbot</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0f;
    --surface: #111118;
    --surface2: #1a1a24;
    --border: #2a2a38;
    --accent: #00e5ff;
    --accent2: #ff6b35;
    --accent3: #7c3aed;
    --text: #e8e8f0;
    --muted: #6b6b80;
    --green: #22c55e;
    --mono: 'Space Mono', monospace;
    --sans: 'Syne', sans-serif;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--sans);
    line-height: 1.7;
    overflow-x: hidden;
  }

  /* ── NOISE TEXTURE ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.5;
  }

  /* ── HERO ── */
  .hero {
    position: relative;
    padding: 80px 40px 60px;
    max-width: 900px;
    margin: 0 auto;
    overflow: hidden;
  }

  .hero-glow {
    position: absolute;
    top: -100px; left: -100px;
    width: 500px; height: 500px;
    background: radial-gradient(circle, rgba(0,229,255,0.08) 0%, transparent 70%);
    pointer-events: none;
  }

  .badge-row {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 32px;
    animation: fadeUp 0.6s ease both;
  }

  .badge {
    font-family: var(--mono);
    font-size: 11px;
    padding: 4px 10px;
    border-radius: 4px;
    border: 1px solid;
    letter-spacing: 0.05em;
  }
  .badge-blue  { color: var(--accent);  border-color: rgba(0,229,255,0.3);  background: rgba(0,229,255,0.05); }
  .badge-green { color: var(--green);   border-color: rgba(34,197,94,0.3);   background: rgba(34,197,94,0.05); }
  .badge-orange{ color: var(--accent2); border-color: rgba(255,107,53,0.3);  background: rgba(255,107,53,0.05); }
  .badge-purple{ color: #a78bfa;        border-color: rgba(124,58,237,0.3);  background: rgba(124,58,237,0.05); }

  .hero h1 {
    font-family: var(--sans);
    font-size: clamp(36px, 6vw, 64px);
    font-weight: 800;
    line-height: 1.05;
    letter-spacing: -0.03em;
    animation: fadeUp 0.6s 0.1s ease both;
  }

  .hero h1 .accent { color: var(--accent); }
  .hero h1 .dim    { color: var(--muted); }

  .hero-sub {
    font-family: var(--mono);
    font-size: 13px;
    color: var(--muted);
    margin-top: 16px;
    letter-spacing: 0.08em;
    animation: fadeUp 0.6s 0.2s ease both;
  }

  .hero-sub span { color: var(--accent2); }

  /* ── MAIN LAYOUT ── */
  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 40px 80px;
    position: relative;
    z-index: 1;
  }

  /* ── DIVIDER ── */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--border), transparent);
    margin: 48px 0;
  }

  /* ── SECTION HEADERS ── */
  .section-label {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--accent);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }

  h2 {
    font-size: 24px;
    font-weight: 700;
    letter-spacing: -0.02em;
    margin-bottom: 20px;
    color: var(--text);
  }

  p { color: #b0b0c0; margin-bottom: 16px; }

  /* ── OVERVIEW CARD ── */
  .overview-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 28px;
    margin-bottom: 16px;
    position: relative;
    overflow: hidden;
  }
  .overview-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--accent), var(--accent3), var(--accent2));
  }

  /* ── GOAL QUOTE ── */
  .goal-quote {
    font-family: var(--mono);
    font-size: 13px;
    color: var(--accent);
    background: rgba(0,229,255,0.05);
    border-left: 3px solid var(--accent);
    padding: 16px 20px;
    border-radius: 0 8px 8px 0;
    margin: 24px 0;
    line-height: 1.6;
  }

  /* ── CHECKLIST ── */
  .checklist {
    list-style: none;
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin: 0;
  }
  .checklist li {
    display: flex;
    align-items: center;
    gap: 12px;
    font-size: 14px;
    color: #c0c0d0;
    padding: 10px 16px;
    background: var(--surface2);
    border-radius: 8px;
    border: 1px solid var(--border);
    transition: border-color 0.2s;
  }
  .checklist li:hover { border-color: rgba(0,229,255,0.2); }
  .check-icon {
    width: 20px; height: 20px;
    background: rgba(34,197,94,0.15);
    border: 1px solid rgba(34,197,94,0.4);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
    font-size: 11px;
    color: var(--green);
  }

  /* ── TWO COLUMN GRID ── */
  .two-col { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
  @media(max-width: 600px) { .two-col { grid-template-columns: 1fr; } }

  /* ── TABLE ── */
  .styled-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
    font-family: var(--mono);
  }
  .styled-table th {
    text-align: left;
    padding: 10px 16px;
    background: var(--surface2);
    color: var(--accent);
    font-size: 11px;
    letter-spacing: 0.1em;
    border-bottom: 1px solid var(--border);
  }
  .styled-table td {
    padding: 10px 16px;
    border-bottom: 1px solid var(--border);
    color: #b0b0c0;
    vertical-align: top;
  }
  .styled-table tr:last-child td { border-bottom: none; }
  .styled-table tr:hover td { background: rgba(255,255,255,0.02); }
  .styled-table .highlight { color: var(--accent2); font-weight: 700; }
  .styled-table .good      { color: var(--green); }
  .styled-table .bad       { color: #f87171; }

  /* ── CODE BLOCK ── */
  .code-block {
    background: #080810;
    border: 1px solid var(--border);
    border-radius: 10px;
    overflow: hidden;
    margin: 16px 0;
  }
  .code-header {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 10px 16px;
    background: var(--surface2);
    border-bottom: 1px solid var(--border);
  }
  .code-dot {
    width: 10px; height: 10px;
    border-radius: 50%;
  }
  .dot-red    { background: #ff5f57; }
  .dot-yellow { background: #febc2e; }
  .dot-green  { background: #28c840; }
  .code-title {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--muted);
    margin-left: 8px;
    letter-spacing: 0.05em;
  }
  .code-block pre {
    padding: 20px;
    font-family: var(--mono);
    font-size: 12.5px;
    line-height: 1.7;
    overflow-x: auto;
    color: #c8c8d8;
  }
  .kw  { color: #c084fc; }
  .fn  { color: #60a5fa; }
  .str { color: #86efac; }
  .cmt { color: #4b5563; font-style: italic; }
  .var { color: var(--accent); }
  .num { color: var(--accent2); }

  /* ── ARCHITECTURE FLOW ── */
  .ipo-flow {
    display: flex;
    align-items: center;
    gap: 0;
    margin: 20px 0;
    flex-wrap: wrap;
  }
  .ipo-box {
    flex: 1;
    min-width: 140px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 20px 16px;
    text-align: center;
    position: relative;
  }
  .ipo-box.active { border-color: rgba(0,229,255,0.4); background: rgba(0,229,255,0.04); }
  .ipo-label {
    font-family: var(--mono);
    font-size: 10px;
    color: var(--muted);
    letter-spacing: 0.15em;
    text-transform: uppercase;
    margin-bottom: 6px;
  }
  .ipo-title {
    font-size: 15px;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 8px;
  }
  .ipo-detail {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--accent);
    line-height: 1.5;
  }
  .ipo-arrow {
    font-size: 22px;
    color: var(--accent2);
    padding: 0 8px;
    flex-shrink: 0;
  }
  @media(max-width: 600px) { .ipo-arrow { display: none; } .ipo-box { min-width: 100%; } }

  /* ── INTENT TABLE CARDS ── */
  .intent-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 12px;
    margin: 16px 0;
  }
  .intent-card {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 14px;
    transition: border-color 0.2s, transform 0.2s;
    cursor: default;
  }
  .intent-card:hover {
    border-color: rgba(0,229,255,0.3);
    transform: translateY(-2px);
  }
  .intent-cat {
    font-family: var(--mono);
    font-size: 10px;
    color: var(--accent2);
    letter-spacing: 0.1em;
    margin-bottom: 8px;
    text-transform: uppercase;
  }
  .intent-examples {
    font-family: var(--mono);
    font-size: 11.5px;
    color: #9090a8;
    line-height: 1.7;
  }
  .intent-examples span {
    display: inline-block;
    background: rgba(0,229,255,0.07);
    border-radius: 3px;
    padding: 1px 5px;
    margin: 2px 2px 2px 0;
    color: #b0d0e0;
  }

  /* ── TERMINAL OUTPUT ── */
  .terminal {
    background: #050509;
    border: 1px solid #1e1e2e;
    border-radius: 10px;
    overflow: hidden;
    font-family: var(--mono);
    font-size: 13px;
  }
  .terminal-bar {
    background: #1a1a28;
    padding: 10px 16px;
    display: flex;
    align-items: center;
    gap: 8px;
    border-bottom: 1px solid #1e1e2e;
  }
  .terminal-title {
    font-size: 11px;
    color: var(--muted);
    letter-spacing: 0.05em;
    margin-left: 8px;
  }
  .terminal-body { padding: 20px; line-height: 1.8; }
  .t-sys  { color: #4b5563; }
  .t-user { color: var(--accent2); }
  .t-user::before { content: 'You: '; }
  .t-bot  { color: #e0e0f0; }
  .t-bot::before { content: 'DecoBot: '; color: var(--accent); }
  .t-line { display: block; }

  /* ── HOW TO RUN ── */
  .run-steps {
    display: flex;
    flex-direction: column;
    gap: 12px;
    counter-reset: step;
  }
  .run-step {
    display: flex;
    gap: 16px;
    align-items: flex-start;
    padding: 16px;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
  }
  .step-num {
    width: 28px; height: 28px;
    background: rgba(0,229,255,0.1);
    border: 1px solid rgba(0,229,255,0.3);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    font-family: var(--mono);
    font-size: 12px;
    color: var(--accent);
    flex-shrink: 0;
    margin-top: 2px;
  }
  .step-content { flex: 1; }
  .step-title { font-weight: 600; font-size: 14px; margin-bottom: 6px; }
  .step-cmd {
    font-family: var(--mono);
    font-size: 12px;
    color: var(--accent);
    background: rgba(0,229,255,0.06);
    padding: 6px 12px;
    border-radius: 4px;
    display: inline-block;
    margin-top: 4px;
  }

  /* ── CONCEPTS ── */
  .concept-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
    gap: 12px;
  }
  .concept-card {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 16px;
  }
  .concept-icon { font-size: 22px; margin-bottom: 8px; }
  .concept-title { font-size: 13px; font-weight: 700; margin-bottom: 6px; color: var(--text); }
  .concept-desc  { font-size: 12px; color: var(--muted); line-height: 1.5; }

  /* ── FOOTER ── */
  .footer {
    border-top: 1px solid var(--border);
    padding: 32px 40px;
    max-width: 900px;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 16px;
  }
  .footer-left { font-size: 13px; color: var(--muted); }
  .footer-left strong { color: var(--text); }
  .footer-links { display: flex; gap: 20px; }
  .footer-links a {
    font-family: var(--mono);
    font-size: 11px;
    color: var(--muted);
    text-decoration: none;
    letter-spacing: 0.05em;
    transition: color 0.2s;
  }
  .footer-links a:hover { color: var(--accent); }

  /* ── ANIMATIONS ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(20px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .fade-in {
    animation: fadeUp 0.6s ease both;
  }
  .delay-1 { animation-delay: 0.1s; }
  .delay-2 { animation-delay: 0.2s; }
  .delay-3 { animation-delay: 0.3s; }
  .delay-4 { animation-delay: 0.4s; }
</style>
</head>
<body>

<!-- ── HERO ──────────────────────────────────────────────── -->
<section class="hero">
  <div class="hero-glow"></div>

  <div class="badge-row">
    <span class="badge badge-blue">Python 3.8+</span>
    <span class="badge badge-green">Status: Complete ✓</span>
    <span class="badge badge-orange">Rule-Based AI</span>
    <span class="badge badge-purple">DecodeLabs 2026</span>
  </div>

  <h1>
    DecoBot<span class="dim"> /</span><br>
    <span class="accent">Rule-Based</span><br>
    AI Chatbot
  </h1>

  <p class="hero-sub">
    Project 1 &nbsp;·&nbsp; <span>DecodeLabs Industrial Training Kit</span> &nbsp;·&nbsp; Batch 2026
  </p>
</section>

<!-- ── MAIN ───────────────────────────────────────────────── -->
<div class="container">

  <!-- OVERVIEW -->
  <div class="section-label">01 — Overview</div>
  <div class="overview-card fade-in">
    <p><strong>DecoBot</strong> is a Rule-Based AI Chatbot built as <strong>Project 1</strong> of the DecodeLabs AI Industrial Training Program. It simulates human conversation using <strong>pure Python control flow</strong> — no machine learning, no external libraries, just dictionaries, loops, and logic.</p>
    <p style="margin-bottom:0">This project demonstrates the <strong style="color:var(--accent)">foundation of all AI systems</strong>: the deterministic logic layer that sits beneath complex probabilistic models.</p>
  </div>

  <div class="goal-quote">
    "Build a chatbot that responds to predefined user inputs using if-else logic running in a continuous loop."
  </div>

  <div class="divider"></div>

  <!-- REQUIREMENTS -->
  <div class="section-label">02 — Requirements</div>
  <h2>Project Checklist</h2>
  <ul class="checklist fade-in delay-1">
    <li><span class="check-icon">✓</span> Handle greetings and exit commands</li>
    <li><span class="check-icon">✓</span> Dictionary-based response logic — O(1) constant-time lookup</li>
    <li><span class="check-icon">✓</span> Continuous <code style="color:var(--accent);font-family:var(--mono);font-size:12px">while True</code> input loop</li>
    <li><span class="check-icon">✓</span> Input sanitization — <code style="color:var(--accent);font-family:var(--mono);font-size:12px">.lower().strip()</code> normalization</li>
    <li><span class="check-icon">✓</span> Fallback response for unknown inputs</li>
    <li><span class="check-icon">✓</span> Clean exit strategy — <code style="color:var(--accent);font-family:var(--mono);font-size:12px">break</code> on kill command</li>
  </ul>

  <div class="divider"></div>

  <!-- ARCHITECTURE -->
  <div class="section-label">03 — Architecture</div>
  <h2>IPO Model</h2>

  <div class="ipo-flow fade-in delay-2">
    <div class="ipo-box">
      <div class="ipo-label">Phase 1</div>
      <div class="ipo-title">INPUT</div>
      <div class="ipo-detail">raw_input()<br>.lower().strip()<br>→ clean_input</div>
    </div>
    <div class="ipo-arrow">→</div>
    <div class="ipo-box active">
      <div class="ipo-label">Phase 2</div>
      <div class="ipo-title">PROCESS</div>
      <div class="ipo-detail">responses.get()<br>O(1) lookup<br>+ fallback</div>
    </div>
    <div class="ipo-arrow">→</div>
    <div class="ipo-box">
      <div class="ipo-label">Phase 3</div>
      <div class="ipo-title">OUTPUT</div>
      <div class="ipo-detail">print(reply)<br>or break<br>on EXIT</div>
    </div>
  </div>

  <br>
  <h2>Why Dictionary over if-elif?</h2>
  <table class="styled-table">
    <thead>
      <tr><th>Approach</th><th>Complexity</th><th>Scalability</th><th>Status</th></tr>
    </thead>
    <tbody>
      <tr>
        <td>if-elif ladder</td>
        <td>O(n) — checks every rule</td>
        <td>Breaks at scale</td>
        <td class="bad">❌ Anti-Pattern</td>
      </tr>
      <tr>
        <td class="highlight">dict.get()</td>
        <td class="good">O(1) — instant lookup</td>
        <td class="good">Scales to 10,000+ rules</td>
        <td class="good">✅ Professional</td>
      </tr>
    </tbody>
  </table>

  <div class="divider"></div>

  <!-- CODE -->
  <div class="section-label">04 — Implementation</div>
  <h2>Core Logic</h2>

  <div class="code-block fade-in delay-2">
    <div class="code-header">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="code-title">rule_based_chatbot.py</span>
    </div>
    <pre><span class="cmt"># ── KNOWLEDGE BASE: Dictionary (O(1) lookup) ──</span>
<span class="var">responses</span> = {
    <span class="str">"hello"</span>        : <span class="str">"Hey there! 👋 I'm DecoBot."</span>,
    <span class="str">"what is ai"</span>   : <span class="str">"AI is the simulation of human intelligence..."</span>,
    <span class="str">"tell me a joke"</span>: <span class="str">"Why do programmers prefer dark mode? Light attracts bugs! 🐛"</span>,
    <span class="cmt"># ... 25+ more intents</span>
}

<span class="cmt"># ── RESPONSE ENGINE ──</span>
<span class="kw">def</span> <span class="fn">get_response</span>(user_input):
    clean = user_input.<span class="fn">lower</span>().<span class="fn">strip</span>()          <span class="cmt"># sanitize</span>
    <span class="kw">if</span> clean <span class="kw">in</span> (<span class="str">"exit"</span>, <span class="str">"quit"</span>, <span class="str">"q"</span>):
        <span class="kw">return</span> <span class="str">"__EXIT__"</span>                         <span class="cmt"># kill command</span>
    <span class="kw">return</span> responses.<span class="fn">get</span>(clean, <span class="str">"🤔 I don't understand."</span>)

<span class="cmt"># ── HEARTBEAT: Infinite Loop ──</span>
<span class="kw">while</span> <span class="num">True</span>:
    raw   = <span class="fn">input</span>(<span class="str">"You: "</span>)
    reply = <span class="fn">get_response</span>(raw)
    <span class="kw">if</span> reply == <span class="str">"__EXIT__"</span>:
        <span class="fn">print</span>(<span class="str">"DecoBot: Goodbye! 👋"</span>)
        <span class="kw">break</span>                                      <span class="cmt"># clean exit</span>
    <span class="fn">print</span>(<span class="fn">f</span><span class="str">"DecoBot: {reply}"</span>)</pre>
  </div>

  <div class="divider"></div>

  <!-- INTENTS -->
  <div class="section-label">05 — Supported Intents</div>
  <h2>Knowledge Base</h2>
  <div class="intent-grid fade-in delay-3">
    <div class="intent-card">
      <div class="intent-cat">Greetings</div>
      <div class="intent-examples">
        <span>hello</span><span>hi</span><span>hey</span>
        <span>good morning</span><span>good evening</span>
      </div>
    </div>
    <div class="intent-card">
      <div class="intent-cat">Identity</div>
      <div class="intent-examples">
        <span>who are you</span><span>what is your name</span><span>what can you do</span>
      </div>
    </div>
    <div class="intent-card">
      <div class="intent-cat">DecodeLabs</div>
      <div class="intent-examples">
        <span>what is decodelabs</span><span>internship</span><span>about decodelabs</span>
      </div>
    </div>
    <div class="intent-card">
      <div class="intent-cat">AI Concepts</div>
      <div class="intent-examples">
        <span>what is ai</span><span>what is ml</span><span>deep learning</span>
      </div>
    </div>
    <div class="intent-card">
      <div class="intent-cat">Programming</div>
      <div class="intent-examples">
        <span>what is python</span><span>what is a loop</span>
      </div>
    </div>
    <div class="intent-card">
      <div class="intent-cat">Small Talk</div>
      <div class="intent-examples">
        <span>how are you</span><span>tell me a joke</span><span>thank you</span>
      </div>
    </div>
    <div class="intent-card">
      <div class="intent-cat">Farewells</div>
      <div class="intent-examples">
        <span>bye</span><span>goodbye</span><span>see you</span>
      </div>
    </div>
    <div class="intent-card">
      <div class="intent-cat">Exit Commands</div>
      <div class="intent-examples">
        <span>exit</span><span>quit</span><span>q</span><span>stop</span><span>close</span>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- SAMPLE OUTPUT -->
  <div class="section-label">06 — Sample Output</div>
  <h2>Live Session</h2>

  <div class="terminal fade-in delay-3">
    <div class="terminal-bar">
      <span class="code-dot dot-red"></span>
      <span class="code-dot dot-yellow"></span>
      <span class="code-dot dot-green"></span>
      <span class="terminal-title">bash — python rule_based_chatbot.py</span>
    </div>
    <div class="terminal-body">
      <span class="t-line t-sys">  ——————————— Rule-Based AI Chatbot ———————————</span>
      <span class="t-line t-sys">  Type 'exit' or 'quit' to stop.</span>
      <span class="t-line" style="display:block;height:8px"></span>
      <span class="t-line t-user">hello</span>
      <span class="t-line t-bot">Hey there! 👋 I'm DecoBot. How can I help you today?</span>
      <span class="t-line" style="display:block;height:8px"></span>
      <span class="t-line t-user">what is machine learning</span>
      <span class="t-line t-bot">Machine Learning is a type of AI where machines learn patterns from data instead of following hard-coded rules.</span>
      <span class="t-line" style="display:block;height:8px"></span>
      <span class="t-line t-user">tell me a joke</span>
      <span class="t-line t-bot">Why do programmers prefer dark mode? Because light attracts bugs! 🐛😂</span>
      <span class="t-line" style="display:block;height:8px"></span>
      <span class="t-line t-user">exit</span>
      <span class="t-line t-bot">Goodbye! Thanks for chatting. 👋</span>
    </div>
  </div>

  <div class="divider"></div>

  <!-- HOW TO RUN -->
  <div class="section-label">07 — Setup</div>
  <h2>How to Run</h2>

  <div class="run-steps fade-in delay-4">
    <div class="run-step">
      <div class="step-num">1</div>
      <div class="step-content">
        <div class="step-title">No installation needed</div>
        <p style="font-size:13px;margin:0;color:var(--muted)">Uses only Python's built-in libraries. No pip install required.</p>
      </div>
    </div>
    <div class="run-step">
      <div class="step-num">2</div>
      <div class="step-content">
        <div class="step-title">Run via Terminal</div>
        <span class="step-cmd">python rule_based_chatbot.py</span>
      </div>
    </div>
    <div class="run-step">
      <div class="step-num">3</div>
      <div class="step-content">
        <div class="step-title">Run in Jupyter / Google Colab</div>
        <p style="font-size:13px;margin:4px 0 0;color:var(--muted)">Paste the full code into a cell and run it. Type in the input box that appears below the cell.</p>
      </div>
    </div>
  </div>

  <div class="divider"></div>

  <!-- KEY CONCEPTS -->
  <div class="section-label">08 — Concepts</div>
  <h2>Key Skills Demonstrated</h2>
  <div class="concept-grid fade-in delay-4">
    <div class="concept-card">
      <div class="concept-icon">🔄</div>
      <div class="concept-title">Control Flow</div>
      <div class="concept-desc">while True loop with break as the kill command</div>
    </div>
    <div class="concept-card">
      <div class="concept-icon">🧹</div>
      <div class="concept-title">Sanitization</div>
      <div class="concept-desc">.lower().strip() normalizes all user input before matching</div>
    </div>
    <div class="concept-card">
      <div class="concept-icon">⚡</div>
      <div class="concept-title">Hash Map Lookup</div>
      <div class="concept-desc">dict.get() gives O(1) constant-time response matching</div>
    </div>
    <div class="concept-card">
      <div class="concept-icon">🛡️</div>
      <div class="concept-title">Fallback Logic</div>
      <div class="concept-desc">Default response prevents crashes on unknown input</div>
    </div>
    <div class="concept-card">
      <div class="concept-icon">📐</div>
      <div class="concept-title">IPO Model</div>
      <div class="concept-desc">Input → Process → Output architecture for traceability</div>
    </div>
    <div class="concept-card">
      <div class="concept-icon">🤖</div>
      <div class="concept-title">Rule-Based AI</div>
      <div class="concept-desc">Deterministic guardrails — zero hallucination risk</div>
    </div>
  </div>

</div><!-- /container -->

<!-- ── FOOTER ─────────────────────────────────────────────── -->
<footer class="footer">
  <div class="footer-left">
    <strong>DecodeLabs Industrial Training Kit</strong><br>
    Batch 2026 · Project 1 · Rule-Based AI Chatbot
  </div>
  <div class="footer-links">
    <a href="http://www.decodelabs.tech">www.decodelabs.tech</a>
    <a href="mailto:decodelabs.tech@gmail.com">Email</a>
    <a href="#">Greater Lucknow, India</a>
  </div>
</footer>

</body>
</html>
