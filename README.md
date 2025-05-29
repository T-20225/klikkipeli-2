
<html lang="fi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Monipeli Klikkerit - Pro Grafiikat</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
    color: #e0e0e0;
    margin: 0; padding: 20px;
    user-select: none;
  }
  h1, h2 {
    text-shadow: 0 2px 6px rgba(0,0,0,0.7);
  }
  button {
    background: linear-gradient(145deg, #1f3a93, #3f5fc2);
    border: none;
    border-radius: 12px;
    color: #fff;
    font-size: 1.2rem;
    padding: 15px 30px;
    cursor: pointer;
    box-shadow: 0 8px 15px rgba(31, 58, 147, 0.3);
    transition: all 0.3s ease;
    text-shadow: 0 1px 3px rgba(0,0,0,0.4);
    position: relative;
    overflow: hidden;
  }
  button:hover {
    background: linear-gradient(145deg, #4c6ef5, #2a4df5);
    box-shadow: 0 12px 20px rgba(46, 97, 255, 0.6);
    transform: translateY(-3px);
  }
  button:active {
    transform: translateY(1px);
    box-shadow: 0 6px 10px rgba(31, 58, 147, 0.2);
  }
  .hidden { display: none; }
  .game-screen { margin-top: 2em; position: relative; min-height: 320px; }
  .moving {
    position: absolute;
    transition: top 0.4s ease, left 0.4s ease;
    box-shadow: 0 15px 30px rgba(255, 255, 255, 0.3);
    border: 2px solid #fff;
    border-radius: 12px;
  }
  #number-grid {
    display: grid;
    grid-template-columns: repeat(5, 70px);
    grid-gap: 15px;
    justify-content: center;
    margin-top: 15px;
  }
  #number-grid button {
    background: linear-gradient(145deg, #22c1c3, #fdbb2d);
    color: #222;
    font-weight: 700;
    font-size: 1.3rem;
    box-shadow: 0 6px 10px rgba(253, 187, 45, 0.4);
    border-radius: 15px;
    padding: 10px 0;
    transition: background 0.3s ease, color 0.3s ease;
    user-select: none;
  }
  #number-grid button:disabled {
    background: linear-gradient(145deg, #a1c4fd, #c2e9fb);
    color: #555;
    box-shadow: none;
    cursor: default;
  }
  #profile-form {
    margin-bottom: 2em;
  }
  #welcome-msg {
    margin-top: 0.5em;
    font-weight: 600;
    font-size: 1.2rem;
    color: #9fc9ff;
    text-shadow: 0 0 8px #2a4df5;
  }
  #result {
    margin-top: 1em;
    font-size: 1.3rem;
    font-weight: 700;
    color: #ffd200;
    text-shadow: 0 0 6px #ffd200;
  }
  #scoreboard {
    background: rgba(0,0,0,0.5);
    padding: 15px 20px;
    margin-top: 20px;
    border-radius: 15px;
    box-shadow: 0 8px 25px rgba(0, 0, 0, 0.7);
    max-width: 320px;
    margin-left: auto;
    margin-right: auto;
  }
  #score-list {
    list-style: none;
    padding-left: 0;
    max-height: 180px;
    overflow-y: auto;
  }
  #score-list li {
    padding: 6px 10px;
    margin-bottom: 6px;
    background: linear-gradient(145deg, #667eea, #764ba2);
    border-radius: 12px;
    box-shadow: inset 0 2px 6px rgba(255, 255, 255, 0.2);
    color: #fff;
    user-select: none;
  }
  #score-list li.highlight {
    background: linear-gradient(135deg, #f7971e, #ffd200);
    color: #222;
    font-weight: 700;
    box-shadow: 0 0 12px #ffd200;
  }
  .reactive-btn {
    background: linear-gradient(135deg, #ff6a00, #ee0979);
    box-shadow: 0 8px 20px rgba(238, 9, 121, 0.6);
    color: #fff;
    font-weight: 700;
  }
  .reactive-btn:hover {
    background: linear-gradient(135deg, #ee0979, #ff6a00);
    box-shadow: 0 12px 30px rgba(255, 106, 0, 0.8);
    transform: translateY(-4px);
  }
  .arrow {
    font-size: 1.3rem;
    position: absolute;
    right: 12px;
    top: 12px;
    animation: bounce 1s infinite alternate;
    color: #fff;
    text-shadow: 0 0 6px #4caf50;
    user-select: none;
  }
  @keyframes bounce {
    0% { transform: translateY(0); }
    100% { transform: translateY(-10px); }
  }
</style>
</head>
<body>

<h1>Monipeli Klikkerit</h1>

<div id="profile-form">
  <label for="player-name">Nimesi: </label>
  <input type="text" id="player-name" autocomplete="off"/>
  <button onclick="saveProfile()">Tallenna profiili</button>
  <p id="welcome-msg"></p>
</div>

<div id="menu" class="hidden">
  <h2>Valitse peli:</h2>
  <button onclick="startGame(1)">1. Perusklikki</button>
  <button onclick="startGame(2)">2. Reaktiopeli</button>
  <button onclick="startGame(3)">3. Haasteklikki</button>
  <button onclick="startGame(4)">4. Liikkuva nappi</button>
  <button onclick="startGame(5)">5. Värihaaste</button>
  <button onclick="startGame(6)">6. Numeroklikki</button>
</div>

<div id="game" class="game-screen hidden">
  <h2 id="game-title"></h2>
  <p id="info"></p>
  <button id="click-btn" class="click-btn" onclick="handleClick()">Klikkaa!</button>
  <div id="number-grid" class="hidden"></div>
  <p id="result"></p>
  <button class="next-btn" onclick="goBack()">Takaisin valikkoon</button>
</div>

<div id="scoreboard" class="hidden">
  <h3>Parhaat tulokset</h3>
  <ul id="score-list"></ul>
</div>

<script>
  let game = 0, count = 0, time, interval, canClick = false, target = 20, startTime = null, number = 1;

  window.onload = function () {
    const savedName = localStorage.getItem("playerName");
    if (savedName) {
      document.getElementById("player-name").value = savedName;
      document.getElementById("welcome-msg").textContent = `Tervetuloa takaisin, ${savedName}!`;
      document.getElementById("profile-form").classList.add("hidden");
      document.getElementById("menu").classList.remove("hidden");
      showScores();
    }
  };

  function saveProfile() {
    const name = document.getElementById("player-name").value.trim();
    if (name) {
      localStorage.setItem("playerName", name);
      document.getElementById("welcome-msg").textContent = `Tervetuloa, ${name}!`;
      document.getElementById("profile-form").classList.add("hidden");
      document.getElementById("menu").classList.remove("hidden");
      showScores();
    }
  }

  function startGame(mode) {
    game = mode;
    count = 0;
    number = 1;
    startTime = null;
    clearTimeout(interval);
    clearInterval(interval);

    document.getElementById("result").textContent = "";
    document.getElementById("scoreboard").classList.add("hidden");
    document.getElementById("menu").classList.add("hidden");
    document.getElementById("game").classList.remove("hidden");

    const btn = document.getElementById("click-btn");
    const grid = document.getElementById("number-grid");
    btn.style.position = "static";
    btn.disabled = false;
    btn.className = "click-btn";
    btn.style.background = "";
    btn.style.color = "";
    btn.style.boxShadow = "";

    grid.innerHTML = "";
    grid.classList.add("hidden");

    switch (game) {
      case 1:
        document.getElementById("game-title").textContent = "Perusklikki";
        document.getElementById("info").textContent = "Klikkaa nappia 20 kertaa mahdollisimman nopeasti.";
        canClick = true;
        startTime = Date.now();
        break;
      case 2:
        document.getElementById("game-title").textContent = "Reaktiopeli";
        document.getElementById("info").textContent = "Klikkaa nappia heti, kun se vaihtaa väriä.";
        canClick = false;
        btn.style.background = "linear-gradient(135deg, #ff6a00, #ee0979)";
        setTimeout(() => {
          canClick = true;
          btn.classList.add("reactive-btn");
          btn.style.background = "";
          document.getElementById("info").textContent = "Nyt! Klikkaa heti!";
          startTime = Date.now();
        }, Math.random() * 3000 + 2000);
        break;
      case 3:
        document.getElementById("game-title").textContent = "Haasteklikki";
        document.getElementById("info").textContent = "Klikkaa nappia 20 kertaa, mutta se piiloutuu välillä.";
        canClick = true;
        startTime = Date.now();
        interval = setInterval(() => {
          const visible = btn.style.display !== "none";
          btn.style.display = visible ? "none" : "inline-block";
        }, 800);
        break;
      case 4:
        document.getElementById("game-title").textContent = "Liikkuva nappi";
        document.getElementById("info").textContent = "Klikkaa nappia 20 kertaa, nappi liikkuu satunnaisesti.";
        canClick = true;
        btn.style.position = "absolute";
        moveButtonRandom();
        startTime = Date.now();
        break;
      case 5:
        document.getElementById("game-title").textContent = "Värihaaste";
        document.getElementById("info").textContent = "Klikkaa nappia vain, kun väri on oranssi.";
        canClick = false;
        btn.style.background = "grey";
        interval = setInterval(() => {
          const orange = Math.random() < 0.5;
          btn.style.background = orange ? "orange" : "grey";
          canClick = orange;
        }, 800);
        startTime = Date.now();
        break;
      case 6:
        document.getElementById("game-title").textContent = "Numeroklikki";
        document.getElementById("info").textContent = "Klikkaa numeronappia oikeassa järjestyksessä 1-20.";
        canClick = true;
        grid.classList.remove("hidden");
        createNumberGrid();
        btn.classList.add("hidden");
        break;
    }
  }

  function handleClick(e) {
    if (!canClick) return;

    count++;
    if (game === 2 && count === 1) {
      // Reaction time measured at first click
      const reactionTime = Date.now() - startTime;
      finishGame(reactionTime);
      return;
    }

    if (count >= target) {
      const elapsed = Date.now() - startTime;
      finishGame(elapsed);
      return;
    }

    if (game === 4) {
      moveButtonRandom();
    }
  }

  function moveButtonRandom() {
    const btn = document.getElementById("click-btn");
    const parent = btn.parentElement;
    const maxX = parent.clientWidth - btn.offsetWidth;
    const maxY = parent.clientHeight - btn.offsetHeight;
    const left = Math.floor(Math.random() * maxX);
    const top = Math.floor(Math.random() * maxY);
    btn.style.left = left + "px";
    btn.style.top = top + "px";
  }

  function createNumberGrid() {
    const grid = document.getElementById("number-grid");
    grid.innerHTML = "";
    const nums = [];
    for (let i = 1; i <= 20; i++) nums.push(i);
    // Sekoita numerot sekaisin
    for (let i = nums.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [nums[i], nums[j]] = [nums[j], nums[i]];
    }
    nums.forEach(num => {
      const btn = document.createElement("button");
      btn.textContent = num;
      btn.onclick = () => {
        if (num === number) {
          number++;
          btn.disabled = true;
          if (number > 20) {
            finishGame(Date.now() - startTime);
          }
        }
      };
      grid.appendChild(btn);
    });
    startTime = Date.now();
  }

  function finishGame(timeTaken) {
    clearInterval(interval);
    canClick = false;
    const btn = document.getElementById("click-btn");
    btn.disabled = true;

    let timeSec = (timeTaken / 1000).toFixed(3);

    document.getElementById("result").textContent = `Aika: ${timeSec} sekuntia!`;
    saveScore(timeSec);

    document.getElementById("scoreboard").classList.remove("hidden");
    showScores();
  }

  function saveScore(time) {
    const name = localStorage.getItem("playerName") || "Anonyymi";
    const scores = JSON.parse(localStorage.getItem("scores") || "[]");
    scores.push({ name, time: parseFloat(time) });
    scores.sort((a,b) => a.time - b.time);
    if (scores.length > 10) scores.length = 10;
    localStorage.setItem("scores", JSON.stringify(scores));
  }

  function showScores() {
    const scores = JSON.parse(localStorage.getItem("scores") || "[]");
    const list = document.getElementById("score-list");
    list.innerHTML = "";
    const name = localStorage.getItem("playerName");
    scores.forEach(s => {
      const li = document.createElement("li");
      li.textContent = `${s.name}: ${s.time.toFixed(3)} s`;
      if (s.name === name) li.classList.add("highlight");
      list.appendChild(li);
    });
    if(scores.length > 0) {
      document.getElementById("scoreboard").classList.remove("hidden");
    }
  }

  function goBack() {
    clearInterval(interval);
    canClick = false;
    document.getElementById("game").classList.add("hidden");
    document.getElementById("menu").classList.remove("hidden");
    document.getElementById("click-btn").disabled = false;
    document.getElementById("click-btn").className = "click-btn";
    document.getElementById("click-btn").style.position = "static";
    document.getElementById("click-btn").style.left = "";
    document.getElementById("click-btn").style.top = "";
    document.getElementById("click-btn").style.background = "";
    document.getElementById("click-btn").style.color = "";
    document.getElementById("click-btn").style.boxShadow = "";
    document.getElementById("click-btn").classList.remove("reactive-btn");
    document.getElementById("click-btn").classList.remove("hidden");
    document.getElementById("number-grid").classList.add("hidden");
    document.getElementById("result").textContent = "";
    showScores();
  }
</script>

</body>
</html>
