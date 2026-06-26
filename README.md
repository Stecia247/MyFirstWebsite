<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Random Fun Facts</title>
  <style>
    :root{
      --bg1:#0f172a;
      --bg2:#071033;
      --card:#0b1220;
      --accent:#7c3aed;
      --muted:#9aa4b2;
      color-scheme: dark;
      --radius:16px;
    }
    html,body{height:100%;margin:0;font-family:Inter,ui-sans-serif,system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;}
    body{
      background: radial-gradient(1200px 600px at 10% 10%, rgba(124,58,237,0.12), transparent),
                  radial-gradient(900px 500px at 90% 90%, rgba(6,95,70,0.06), transparent),
                  linear-gradient(180deg,var(--bg1),var(--bg2));
      display:flex;align-items:center;justify-content:center;padding:28px;
    }
    .wrap{
      width:100%;max-width:920px;background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border:1px solid rgba(255,255,255,0.04);
      padding:28px;border-radius:20px;box-shadow: 0 10px 30px rgba(2,6,23,0.6);
    }
    header{display:flex;align-items:center; gap:16px; margin-bottom:18px;}
    h1{margin:0;font-size:1.4rem;color:#fff;letter-spacing:-0.2px}
    p.lead{margin:0;color:var(--muted);font-size:0.95rem}
    .main{
      display:grid;grid-template-columns:1fr 320px;gap:20px;align-items:start;
    }
    @media (max-width:880px){ .main{grid-template-columns:1fr;}}
    .card{
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      padding:28px;border-radius:var(--radius);border:1px solid rgba(255,255,255,0.03);
    }
    .fact {
      min-height:160px; display:flex;align-items:center;justify-content:center;
      font-size:1.25rem;color:#e6eef8;text-align:center;padding:18px;line-height:1.5;
      position:relative;
    }
    .fact .text{max-width:72ch;opacity:0;transform:translateY(10px);transition:all 360ms cubic-bezier(.2,.9,.3,1);}
    .fact.show .text{opacity:1;transform:none;}
    .meta{display:flex;gap:8px;align-items:center;justify-content:center;margin-top:14px;color:var(--muted);font-size:0.9rem}
    .controls{display:flex;gap:8px;flex-wrap:wrap;justify-content:center;margin-top:18px}
    button, .select, .slider{
      background:transparent;border:1px solid rgba(255,255,255,0.06);color:#fff;padding:8px 12px;border-radius:10px;cursor:pointer;
      transition:all 160ms;border-width:1px;font-weight:600;
    }
    button:hover{transform:translateY(-2px);box-shadow:0 6px 18px rgba(124,58,237,0.08)}
    .accent{background:linear-gradient(90deg,var(--accent),#0891b2);border:0;color:white}
    .small{font-size:0.88rem;padding:6px 10px}
    .right-panel{display:flex;flex-direction:column;gap:12px}
    label{font-size:0.86rem;color:var(--muted)}
    .history{max-height:320px;overflow:auto;padding:8px;border-radius:10px;background:rgba(255,255,255,0.01);border:1px dashed rgba(255,255,255,0.02)}
    .history-item{padding:10px;border-radius:8px;margin-bottom:8px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);color:#e9f2ff;font-size:0.95rem}
    .footer{margin-top:16px;color:var(--muted);font-size:0.9rem;text-align:center}
    .row{display:flex;gap:8px;align-items:center}
    .toggle{display:inline-flex;align-items:center;gap:8px}
    input[type="range"]{width:100%}
    .muted{color:var(--muted);font-weight:500}
    .tag{display:inline-block;background:rgba(255,255,255,0.03);padding:6px 8px;border-radius:999px;font-size:0.8rem;color:var(--muted)}
  </style>
</head>
<body>
  <main class="wrap" role="main">
    <header>
      <div>
        <h1>Random Fun Facts</h1>
        <p class="lead">Unique, interesting facts — no repeats until you see them all. Click "New Fact" or enable Auto-play.</p>
      </div>
    </header>

    <section class="main" aria-live="polite">
      <section class="card" aria-labelledby="fact-heading">
        <div id="fact-heading" class="muted" style="text-align:center">Here's a fun fact for you</div>

        <div class="fact" id="factCard" role="article" aria-atomic="true" aria-live="polite">
          <div class="text" id="factText">Loading facts…</div>
        </div>

        <div class="meta" id="factSource"><span class="tag" id="factTag"></span></div>

        <div class="controls" style="justify-content:center">
          <button id="newBtn" class="accent">New Fact</button>
          <button id="copyBtn" class="small">Copy</button>
          <button id="tweetBtn" class="small">Tweet</button>
          <button id="resetBtn" class="small">Reset Seen</button>
        </div>

        <div class="footer">Tip: facts won't repeat until you've seen each one. You can add your own facts in the script.</div>
      </section>

      <aside class="right-panel">
        <div class="card">
          <label for="autoplay" class="row"><input type="checkbox" id="autoplay" /> Enable Auto-play</label>
          <label for="speed" style="margin-top:8px">Speed (seconds): <span id="speedVal">6</span></label>
          <input id="speed" type="range" min="2" max="12" value="6" />

          <div style="margin-top:12px">
            <label for="filter">Filter by tag</label>
            <select id="filter" class="select" aria-label="Filter facts by tag">
              <option value="all">All</option>
            </select>
          </div>
        </div>

        <div class="card">
          <label>Recently shown</label>
          <div class="history" id="history" aria-live="polite"></div>
        </div>

        <div class="card" style="text-align:center">
          <button id="addSample" class="small">Add 5 more sample facts</button>
          <div style="margin-top:10px;color:var(--muted);font-size:0.86rem">Want customization? I can add categories, animations, or load facts from an API.</div>
        </div>
      </aside>
    </section>
  </main>

  <script>
    // Facts array. Each fact has text, optional source, tag(s).
    const FACTS = [
      {text: "Honey never spoils — archaeologists found edible 3,000-year-old honey in Egyptian tombs.", tag: "history"},
      {text: "There are more trees on Earth than stars in the Milky Way galaxy (estimated ~3 trillion trees).", tag: "science"},
      {text: "Octopuses have three hearts and blue blood.", tag: "animals"},
      {text: "Bananas are berries, but strawberries are not.", tag: "food"},
      {text: "A day on Venus is longer than a year on Venus.", tag: "space"},
      {text: "Wombat poop is cube-shaped; it helps them mark territory without it rolling away.", tag: "animals"},
      {text: "Oxford University is older than the Aztec Empire: teaching there started in the 11th century.", tag: "history"},
      {text: "Cleopatra lived closer in time to the Moon landing than to the building of the Great Pyramid.", tag: "history"},
      {text: "Hot water can freeze faster than cold water under certain conditions — it's called the Mpemba effect.", tag: "science"},
      {text: "The Eiffel Tower can be 15 cm taller during hot days due to thermal expansion.", tag: "engineering"},
      {text: "There are 'zombie' cicadas that emerge every 17 years in synchronized broods.", tag: "animals"},
      {text: "Venus rotates clockwise — it's the only planet in the solar system that does so (a 'retrograde' rotation).", tag: "space"},
      {text: "A 'jiffy' is an actual unit of time — in physics it can mean the time light takes to travel one fermi.", tag: "science"},
      {text: "Sharks existed before trees — sharks have been around for about 400 million years.", tag: "animals"},
      {text: "The inventor of the Pringles can is buried in one.", tag: "food"},
      {text: "The shortest war in history lasted 38–45 minutes (Anglo-Zanzibar War, 1896).", tag: "history"},
      {text: "Oxford comma disputes can't topple grammar — but they can change legal meanings.", tag: "language"},
      {text: "A single bolt of lightning contains enough energy to toast 100,000 slices of bread.", tag: "science"},
      {text: "You can start a fire with ice by shaping it into a lens and focusing sunlight.", tag: "survival"},
      {text: "The microwave oven was invented after a researcher walked by a radar tube and a chocolate bar melted in his pocket.", tag: "technology"},
      {text: "Scotland's national animal is the unicorn.", tag: "culture"},
      {text: "The world's oldest known 'your mom' joke is 3,500 years old (Babylonian).", tag: "history"},
      {text: "Sloths only poop about once a week and do a dangerous trip to the ground to do it.", tag: "animals"},
      {text: "A group of flamingos is called a 'flamboyance'.", tag: "animals"},
      {text: "There's a species of fungus, Ophiocordyceps, that can hijack ant behavior and make them climb before dying — 'zombie ants'.", tag: "biology"},
      {text: "The word 'robot' comes from a Czech word meaning 'forced labor'.", tag: "language"},
      {text: "Jellyfish have existed for over 500 million years and some are biologically immortal.", tag: "animals"},
      {text: "A teaspoon of a neutron star would weigh about 6 billion tons on Earth.", tag: "space"},
      {text: "The first computer bug was an actual bug — a moth trapped in a relay of the Harvard Mark II.", tag: "technology"}
    ];

    // State
    let facts = [...FACTS]; // mutable copy for additions
    let seenIndices = new Set();
    let history = []; // recent shown facts (latest first)
    const MAX_HISTORY = 12;

    // Elements
    const factText = document.getElementById('factText');
    const factTag = document.getElementById('factTag');
    const factCard = document.getElementById('factCard');
    const newBtn = document.getElementById('newBtn');
    const copyBtn = document.getElementById('copyBtn');
    const tweetBtn = document.getElementById('tweetBtn');
    const resetBtn = document.getElementById('resetBtn');
    const historyEl = document.getElementById('history');
    const autoplay = document.getElementById('autoplay');
    const speedRange = document.getElementById('speed');
    const speedVal = document.getElementById('speedVal');
    const filter = document.getElementById('filter');
    const addSample = document.getElementById('addSample');

    let autoTimer = null;

    // Helpers
    function getEligibleIndices() {
      const selTag = filter.value;
      const eligible = [];
      facts.forEach((f, i) => {
        if (selTag === 'all' || f.tag === selTag) {
          if (!seenIndices.has(i)) eligible.push(i);
        }
      });
      return eligible;
    }

    function pickRandomIndex() {
      const eligible = getEligibleIndices();
      if (eligible.length === 0) return -1;
      const idx = eligible[Math.floor(Math.random() * eligible.length)];
      return idx;
    }

    function showFactByIndex(i, skipAnimation=false) {
      const f = facts[i];
      if (!f) return;
      // Accessibility live region update
      factText.classList.remove('show');
      // slight delay to let exit animation occur
      setTimeout(()=> {
        factText.textContent = f.text;
        factTag.textContent = f.tag || '';
        factCard.classList.add('show');
        factText.classList.add('show');
      }, 60);

      seenIndices.add(i);
      history.unshift({i, text: f.text});
      if (history.length > MAX_HISTORY) history.pop();
      renderHistory();

      // If all seen for current filter, show a short message on the next click
      if (getEligibleIndices().length === 0) {
        // do nothing extra here; UI Reset button gives control
      }
    }

    function showRandomFact() {
      const idx = pickRandomIndex();
      if (idx === -1) {
        // all seen for this filter
        factText.textContent = "You've seen all facts in this filter — click Reset Seen to start again (or change the filter).";
        factTag.textContent = '';
        return;
      }
      showFactByIndex(idx);
    }

    function renderHistory() {
      historyEl.innerHTML = '';
      if (history.length === 0) {
        historyEl.innerHTML = '<div class="muted" style="padding:8px">No facts shown yet.</div>';
        return;
      }
      history.forEach(entry => {
        const d = document.createElement('div');
        d.className = 'history-item';
        d.textContent = entry.text.length > 160 ? entry.text.slice(0,160)+'…' : entry.text;
        historyEl.appendChild(d);
      });
    }

    function populateFilter() {
      const tags = new Set(facts.map(f=>f.tag || 'misc'));
      const current = filter.value || 'all';
      filter.innerHTML = '<option value="all">All</option>';
      Array.from(tags).sort().forEach(t => {
        const opt = document.createElement('option');
        opt.value = t;
        opt.textContent = t[0].toUpperCase() + t.slice(1);
        filter.appendChild(opt);
      });
      // restore if possible
      if ([...filter.options].some(o=>o.value===current)) filter.value = current;
    }

    // Buttons
    newBtn.addEventListener('click', ()=> {
      showRandomFact();
      restartAutoIfRunning();
    });

    copyBtn.addEventListener('click', async ()=>{
      try {
        await navigator.clipboard.writeText(factText.textContent || '');
        copyBtn.textContent = 'Copied!';
        setTimeout(()=> copyBtn.textContent = 'Copy', 1200);
      } catch (e) {
        copyBtn.textContent = 'Copy (failed)';
        setTimeout(()=> copyBtn.textContent = 'Copy', 1200);
      }
    });

    tweetBtn.addEventListener('click', ()=>{
      const text = encodeURIComponent(factText.textContent + " — via Random Fun Facts");
      const url = `https://twitter.com/intent/tweet?text=${text}`;
      window.open(url, '_blank', 'noopener');
    });

    resetBtn.addEventListener('click', ()=>{
      seenIndices.clear();
      history = [];
      renderHistory();
      factText.textContent = "Seen state reset. Click 'New Fact' to get a fresh random fact.";
      factTag.textContent = '';
      restartAutoIfRunning();
    });

    filter.addEventListener('change', ()=> {
      // If there are unseen facts in the new filter, show one; otherwise show warning.
      showRandomFact();
      populateFilter(); // keep options current when tags change
      restartAutoIfRunning();
    });

    speedRange.addEventListener('input', ()=> {
      speedVal.textContent = speedRange.value;
      restartAutoIfRunning();
    });

    autoplay.addEventListener('change', ()=> {
      if (autoplay.checked) startAuto();
      else stopAuto();
    });

    function startAuto(){
      stopAuto();
      const seconds = Number(speedRange.value) || 6;
      autoTimer = setInterval(()=>{
        showRandomFact();
        // if no eligible left, stop autoplay to avoid spam
        if (pickRandomIndex() === -1) {
          stopAuto();
          autoplay.checked = false;
        }
      }, seconds * 1000);
    }

    function stopAuto(){
      if (autoTimer) { clearInterval(autoTimer); autoTimer = null; }
    }

    function restartAutoIfRunning(){
      if (autoplay.checked) startAuto();
    }

    addSample.addEventListener('click', ()=>{
      const extras = [
        {text: "A small moonquake on the Moon can last for over 10 minutes—longer than most quakes on Earth.", tag:"space"},
        {text: "The fastest recorded raindrop fell at around 18 mph — air resistance limits droplet speed.", tag:"science"},
        {text: "Some bamboo species can grow up to 91 cm (35 inches) in 24 hours.", tag:"plants"},
        {text: "Pineapples take about two years to grow to full size.", tag:"food"},
        {text: "The inventor of the Frisbee was turned into a Frisbee after he died — his ashes were molded into Frisbees.", tag:"culture"}
      ];
      facts.push(...extras);
      populateFilter();
      addSample.textContent = 'Added!';
      setTimeout(()=> addSample.textContent = 'Add 5 more sample facts', 1200);
    });

    // Initialize
    function init(){
      populateFilter();
      renderHistory();
      // initial pick
      setTimeout(()=> showRandomFact(), 180);
    }
    init();

    // Make sure animations/visibility classes applied on load
    document.addEventListener('DOMContentLoaded', ()=> factCard.classList.add('show'));

    // Keyboard accessibility: space or Enter to get new fact when focused on body
    document.body.addEventListener('keydown', (e)=>{
      if (e.code === 'Space' || e.code === 'Enter') {
        // ignore when typing in a form control
        const tag = document.activeElement && document.activeElement.tagName;
        if (tag === 'INPUT' || tag === 'TEXTAREA' || tag === 'SELECT' || document.activeElement.isContentEditable) return;
        e.preventDefault();
        showRandomFact();
      }
    });

    // Expose some functions for debugging in console
    window._funFacts = {
      facts, seenIndices, reset: ()=>{ seenIndices.clear(); history=[]; renderHistory(); }
    };
  </script>
</body>
</html>
