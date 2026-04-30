# Hacks<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GAMEHUB X PRO</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body { background: radial-gradient(circle at top, #120022, #000); }
    .glow { box-shadow: 0 0 25px rgba(168,85,247,.8); }
  </style>
</head>
<body class="text-white min-h-screen"><header class="p-6 text-center text-4xl font-extrabold bg-black/60 backdrop-blur glow">
  🚀 GAMEHUB X PRO
</header><nav class="flex justify-center gap-6 p-4 text-lg">
  <button onclick="show('games')">Games</button>
  <button onclick="show('profile')">Profile</button>
  <button onclick="show('leaderboard')">Leaderboard</button>
  <button onclick="show('shop')">Shop</button>
</nav><main class="p-6"><!-- PROFILE --><div id="profile" class="hidden text-center">
  <h2 class="text-3xl mb-4">👤 Profile</h2>
  <input id="username" placeholder="Enter name" class="text-black p-2 rounded" />
  <button onclick="saveUser()" class="ml-2 bg-purple-600 px-4 py-2 rounded">Save</button>
  <p id="welcome" class="mt-4"></p>
</div><!-- GAMES --><div id="games" class="grid grid-cols-1 md:grid-cols-2 gap-6"><div class="bg-black/40 p-4 rounded-2xl glow text-center">
  <h2 class="text-xl font-bold">Clicker</h2>
  <p id="score" class="text-3xl">0</p>
  <button onclick="clickGame()" class="bg-purple-600 px-6 py-2 rounded-xl">CLICK</button>
</div><div class="bg-black/40 p-4 rounded-2xl glow text-center">
  <h2 class="text-xl font-bold">Reaction Test</h2>
  <button id="reactBtn" onclick="startReaction()" class="bg-red-500 px-6 py-2 rounded">WAIT...</button>
  <p id="reactionTime"></p>
</div></div><!-- LEADERBOARD --><div id="leaderboard" class="hidden text-center">
  <h2 class="text-3xl">🏆 Leaderboard</h2>
  <ul id="board"></ul>
</div><!-- SHOP --><div id="shop" class="hidden text-center">
  <h2 class="text-3xl">🛒 Shop</h2>
  <button onclick="buy()" class="bg-yellow-500 px-6 py-2 rounded">Upgrade (+1)</button>
  <p id="coins">Coins: 0</p>
</div></main><script>
function show(id){['games','profile','leaderboard','shop'].forEach(s=>document.getElementById(s).classList.add('hidden'));document.getElementById(id).classList.remove('hidden');}

// PROFILE
function saveUser(){
  const name=document.getElementById('username').value;
  localStorage.setItem('user',name);
  document.getElementById('welcome').innerText="Welcome "+name;
}

// CLICKER SYSTEM
let score=0, coins=0, multi=1;
function clickGame(){
  score+=multi; coins+=multi;
  document.getElementById('score').innerText=score;
  document.getElementById('coins').innerText="Coins: "+coins;
  saveScore();
}

function buy(){ if(coins>=10){coins-=10;multi++;} document.getElementById('coins').innerText="Coins: "+coins; }

// LEADERBOARD (LOCAL)
function saveScore(){
  let board=JSON.parse(localStorage.getItem('board')||'[]');
  const user=localStorage.getItem('user')||'Anon';
  board.push({user,score});
  board.sort((a,b)=>b.score-a.score);
  board=board.slice(0,5);
  localStorage.setItem('board',JSON.stringify(board));
  renderBoard();
}

function renderBoard(){
  const list=document.getElementById('board');
  list.innerHTML='';
  let board=JSON.parse(localStorage.getItem('board')||'[]');
  board.forEach(p=>{
    let li=document.createElement('li');
    li.innerText=p.user+': '+p.score;
    list.appendChild(li);
  });
}
renderBoard();

// REACTION GAME
let startTime;
function startReaction(){
  const btn=document.getElementById('reactBtn');
  btn.innerText='WAIT...';
  setTimeout(()=>{
    btn.innerText='CLICK!';
    btn.onclick=stopReaction;
    startTime=Date.now();
  },Math.random()*3000+1000);
}

function stopReaction(){
  const time=Date.now()-startTime;
  document.getElementById('reactionTime').innerText=time+' ms';
  document.getElementById('reactBtn').onclick=startReaction;
}
</script></body>
</html>
