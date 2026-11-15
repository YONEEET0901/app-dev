# app-dev
My first repository
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Look Back (ルックバック) — Tribute</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Courier+Prime:ital,wght@0,400;0,700;1,400&family=Noto+Serif+JP:wght@400;700&family=Permanent+Marker&display=swap" rel="stylesheet">
  <style>
    :root {
      --paper-bg: #f7f4eb;
      --paper-card: #fffdf8;
      --ink-black: #1c1b18;
      --pencil-gray: #5a574f;
      --rain-blue: #4a6b82;
      --sunset-amber: #d97736;
      --border-ink: 2.5px solid #1c1b18;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background-color: var(--paper-bg);
      background-image: radial-gradient(#d8d2c3 1px, transparent 1px);
      background-size: 20px 20px;
      color: var(--ink-black);
      font-family: 'Courier Prime', monospace;
      line-height: 1.6;
      padding: 2rem 1rem;
    }

    .container {
      max-width: 900px;
      margin: 0 auto;
    }

    /* Manga Panel Styling */
    .manga-panel {
      background: var(--paper-card);
      border: var(--border-ink);
      box-shadow: 6px 6px 0px var(--ink-black);
      padding: 2rem;
      margin-bottom: 2.5rem;
      position: relative;
      transition: transform 0.2s ease;
    }

    .manga-panel:hover {
      transform: translate(-2px, -2px);
    }

    /* Header / Hero */
    header.manga-panel {
      text-align: center;
      border-width: 3px;
      background: linear-gradient(180deg, #fffdf8 0%, #f1ece0 100%);
    }

    .jp-title {
      font-family: 'Noto Serif JP', serif;
      font-size: 1.2rem;
      letter-spacing: 0.4rem;
      color: var(--pencil-gray);
      margin-bottom: 0.5rem;
    }

    h1 {
      font-family: 'Permanent Marker', cursive;
      font-size: clamp(2.8rem, 6vw, 4.5rem);
      letter-spacing: 2px;
      text-transform: uppercase;
      line-height: 1.1;
      margin-bottom: 0.75rem;
    }

    .subtitle {
      font-style: italic;
      color: var(--pencil-gray);
      font-size: 1.05rem;
      max-width: 600px;
      margin: 0 auto 1.5rem;
    }

    .meta-tags {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 0.75rem;
    }

    .tag {
      border: 1.5px solid var(--ink-black);
      padding: 0.25rem 0.75rem;
      font-size: 0.85rem;
      font-weight: bold;
      background: var(--paper-bg);
    }

    /* Section Headings */
    h2 {
      font-family: 'Noto Serif JP', serif;
      font-size: 1.6rem;
      border-bottom: 2px solid var(--ink-black);
      padding-bottom: 0.4rem;
      margin-bottom: 1.25rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    h2 span.panel-num {
      font-family: 'Courier Prime', monospace;
      font-size: 0.9rem;
      background: var(--ink-black);
      color: var(--paper-card);
      padding: 0.15rem 0.5rem;
    }

    /* Two-Column Character Grid */
    .character-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 1.5rem;
    }

    .char-card {
      border: 2px solid var(--ink-black);
      padding: 1.5rem;
      background: var(--paper-bg);
      cursor: pointer;
      transition: background 0.2s ease;
    }

    .char-card.active {
      background: #fff;
      border-color: var(--rain-blue);
      box-shadow: 4px 4px 0px var(--rain-blue);
    }

    .char-card h3 {
      font-family: 'Noto Serif JP', serif;
      font-size: 1.35rem;
      margin-bottom: 0.25rem;
    }

    .char-role {
      font-size: 0.85rem;
      color: var(--sunset-amber);
      font-weight: bold;
      text-transform: uppercase;
      margin-bottom: 0.75rem;
      display: block;
    }

    /* Interactive Quote Box */
    .quote-box {
      border-left: 4px solid var(--rain-blue);
      padding: 1rem 1.5rem;
      margin-top: 1.5rem;
      background: #eef3f6;
      font-style: italic;
    }

    /* Timeline / 4-Koma Structure */
    .yonkoma-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 1rem;
    }

    .koma {
      border: 2px solid var(--ink-black);
      padding: 1rem;
      background: #fff;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }

    .koma-header {
      font-weight: bold;
      font-size: 0.85rem;
      color: var(--pencil-gray);
      margin-bottom: 0.5rem;
      border-bottom: 1px dashed var(--pencil-gray);
      padding-bottom: 0.25rem;
    }

    .koma p {
      font-size: 0.92rem;
    }

    /* Rain Toggle Button */
    .btn {
      background: var(--ink-black);
      color: var(--paper-card);
      border: none;
      padding: 0.6rem 1.2rem;
      font-family: 'Courier Prime', monospace;
      font-weight: bold;
      cursor: pointer;
      margin-top: 1rem;
      transition: opacity 0.2s;
    }

    .btn:hover {
      opacity: 0.85;
    }

    /* Rain Canvas Overlay */
    #rain-canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 999;
      opacity: 0;
      transition: opacity 0.6s ease;
    }

    #rain-canvas.raining {
      opacity: 0.55;
    }

    footer {
      text-align: center;
      font-size: 0.85rem;
      color: var(--pencil-gray);
      margin-top: 2rem;
    }
  </style>
</head>
<body>

  <canvas id="rain-canvas"></canvas>

  <div class="container">
    <!-- Hero Panel -->
    <header class="manga-panel">
      <div class="jp-title">ルックバック</div>
      <h1>Look Back</h1>
      <p class="subtitle">"Why do you draw?" — A tribute to Tatsuki Fujimoto & Kiyotaka Oshiyama's masterpiece on art, rivalry, and grief.</p>
      <div class="meta-tags">
        <span class="tag">Released: 2024</span>
        <span class="tag">Director: Kiyotaka Oshiyama</span>
        <span class="tag">Studio Durian</span>
        <span class="tag">58 Minutes</span>
      </div>
      <button class="btn" id="rain-toggle">🌧️ Toggle Rain Scene Atmosphere</button>
    </header>

    <!-- Synopsis Panel -->
    <section class="manga-panel">
      <h2>The Story <span class="panel-num">PAGE 01</span></h2>
      <p>
        Overly confident fourth-grader <strong>Ayumu Fujino</strong> publishes popular four-panel manga strips in her school newspaper—until she is outshined by <strong>Kyomoto</strong>, a reclusive, agoraphobic classmate whose background art exhibits breathtaking, professional-grade detail.
      </p>
      <br>
      <p>
        What begins as bitter jealousy transforms into an inseparable creative partnership. Seated side by side at their desks across seasons and years, the two girls pour their lives into ink and paper, discovering what it truly means to create for someone else.
      </p>
    </section>

    <!-- Characters Panel -->
    <section class="manga-panel">
      <h2>The Creators (Click to Inspect) <span class="panel-num">PAGE 02</span></h2>
      <div class="character-grid">
        <div class="char-card active" onclick="selectCharacter('fujino', this)">
          <h3>Ayumu Fujino (藤野 歩)</h3>
          <span class="char-role">Story & Character Design</span>
          <p>Driven by pride, validation, and relentless stubbornness. Though initially motivated by outdoing her rival, her true spark comes from seeing Kyomoto laugh at her panels.</p>
        </div>
        <div class="char-card" onclick="selectCharacter('kyomoto', this)">
          <h3>Kyomoto (京本)</h3>
          <span class="char-role">Backgrounds & Architecture</span>
          <p>Quiet, observant, and deeply anxious about the outside world. Fujino's manga gives her the courage to step beyond her bedroom door and pursue fine arts.</p>
        </div>
      </div>

      <div class="quote-box" id="character-quote">
        "Fujino-chan... I've been a huge fan of yours for so long!" — Kyomoto, on the day they finally met.
      </div>
    </section>

    <!-- 4-Koma Narrative Structure -->
    <section class="manga-panel">
      <h2>4-Koma (Four-Panel) Arc <span class="panel-num">PAGE 03</span></h2>
      <div class="yonkoma-grid">
        <div class="koma">
          <div class="koma-header">01. KI (Introduction)</div>
          <p>Fujino reigns as the undisputed manga prodigy of her elementary school class until a blank box beside hers is filled by Kyomoto.</p>
        </div>
        <div class="koma">
          <div class="koma-header">02. SHŌ (Development)</div>
          <p>Humiliated by the gap in skill, Fujino locks herself in her room for over a year, filling dozens of sketchbooks with anatomical studies.</p>
        </div>
        <div class="koma">
          <div class="koma-header">03. TEN (Twist)</div>
          <p>Graduation day. Delivering a certificate to Kyomoto's house, Fujino dances in the pouring rain after realizing her rival is actually her biggest fan.</p>
        </div>
        <div class="koma">
          <div class="koma-header">04. KETSU (Conclusion)</div>
          <p>Paths diverge toward serialization and art university. Even through tragedy and alternate timelines, the pencil strokes remain taped to the window.</p>
        </div>
      </div>
    </section>

    <footer>
      <p>Created for GitHub • Inspired by <em>Look Back</em> by Tatsuki Fujimoto</p>
    </footer>
  </div>

  <script>
    // Interactive Character Quote Switcher
    const quotes = {
      fujino: '"Even if I hate drawing, even if it hurts... I pick up the pen again because you were watching my back."',
      kyomoto: '"Fujino-chan... I\'ve been a huge fan of yours for so long!" — Kyomoto, on the day they finally met.'
    };

    function selectCharacter(charKey, element) {
      document.querySelectorAll('.char-card').forEach(card => card.classList.remove('active'));
      element.classList.add('active');
      document.getElementById('character-quote').textContent = quotes[charKey];
    }

    // Rain Scene Canvas Effect
    const canvas = document.getElementById('rain-canvas');
    const ctx = canvas.getContext('2d');
    const rainBtn = document.getElementById('rain-toggle');
    let isRaining = false;
    let drops = [];

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    for (let i = 0; i < 120; i++) {
      drops.push({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        length: Math.random() * 18 + 10,
        speed: Math.random() * 6 + 6
      });
    }

    function drawRain() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.strokeStyle = '#4a6b82';
      ctx.lineWidth = 1.2;
      ctx.beginPath();
      for (let d of drops) {
        ctx.moveTo(d.x, d.y);
        ctx.lineTo(d.x - 2, d.y + d.length);
        d.y += d.speed;
        d.x -= 1;
        if (d.y > canvas.height) {
          d.y = -20;
          d.x = Math.random() * canvas.width;
        }
      }
      ctx.stroke();
      if (isRaining) requestAnimationFrame(drawRain);
    }

    rainBtn.addEventListener('click', () => {
      isRaining = !isRaining;
      canvas.classList.toggle('raining', isRaining);
      rainBtn.textContent = isRaining ? '☀️ Stop Rain Scene Atmosphere' : '🌧️ Toggle Rain Scene Atmosphere';
      if (isRaining) drawRain();
    });
  </script>
</body>
</html>
