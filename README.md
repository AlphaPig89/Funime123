
<!DOCTYPE html>
<html lang="he" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FUNIME ULTRA | Anime Hub</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">

<style>
:root{
--bg:#050505;
--glass:rgba(20,20,25,0.85);
--accent:#f47521;
--neon:#ff9f43;
--text:#fff;
--muted:#aaa;
--radius:20px;
--blur:blur(35px);
--smooth:all .5s cubic-bezier(.23,1,.32,1);
}

*{box-sizing:border-box;margin:0;padding:0;font-family:Poppins,sans-serif}

body{
background:var(--bg);
color:var(--text);
overflow-x:hidden;
display:flex;
flex-direction:column;
min-height:100vh;
}

.bg-glow{
position:fixed;inset:0;
background:
radial-gradient(circle at 20% 30%, rgba(244,117,33,.2), transparent 40%),
radial-gradient(circle at 80% 70%, rgba(255,159,67,.15), transparent 40%);
filter:blur(120px);
z-index:-1;
}

/* NAVBAR / SIDEBAR */
header{
padding:20px 40px;
display:flex;
justify-content:space-between;
align-items:center;
backdrop-filter:var(--blur);
background:var(--glass);
border-bottom:1px solid rgba(255,255,255,.05);
position:sticky;
top:0;
z-index:100;
}

.logo-main{font-size:28px;font-weight:900;letter-spacing:-1px;color:white;text-decoration:none}
.nav-buttons button{
all:unset;
padding:10px 18px;
margin-inline:5px;
border-radius:15px;
cursor:pointer;
transition:var(--smooth);
color:var(--muted);
font-weight:600;
}
.nav-buttons button:hover,.nav-buttons button.active{
background:var(--accent);
color:#000;
}

/* HERO CINEMATIC */
.hero{
display:flex;
justify-content:space-between;
align-items:center;
padding:60px 40px;
gap:40px;
flex-wrap:wrap;
}
.hero .hero-text{
backdrop-filter:var(--blur);
background:var(--glass);
border-radius:25px;
padding:40px;
animation:float 6s ease-in-out infinite;
}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-15px)}}
.hero h1{font-size:50px;margin-bottom:20px}
.hero p{color:#ccc;margin-bottom:20px;font-weight:500}
.hero button{
padding:14px 28px;border-radius:999px;
border:none;background:var(--accent);
font-weight:800;cursor:pointer;
transition:transform .3s;
}
.hero button:hover{transform:scale(1.05);}

/* MOVIE ROW */
.section-title{
padding:30px 40px;font-size:22px;font-weight:800;
}
.row{
display:flex;
gap:20px;
padding:0 40px 30px;
overflow-x:auto;
scroll-behavior:smooth;
}
.card{
min-width:220px;
border-radius:20px;
overflow:hidden;
cursor:pointer;
position:relative;
transition:var(--smooth);
backdrop-filter:var(--blur);
background:rgba(255,255,255,0.05);
border:1px solid transparent;
}
.card:hover{transform:translateY(-10px);border-color:var(--accent);}
.card img{width:100%;height:320px;object-fit:cover}
.card .info{
position:absolute;bottom:0;inset-inline:0;
padding:12px;
background:linear-gradient(transparent,rgba(0,0,0,.9));
font-weight:700;
}

/* MODAL */
.modal{
position:fixed;inset:0;
background:rgba(0,0,0,.75);
display:flex;
justify-content:center;align-items:center;
opacity:0;
pointer-events:none;
transition:.3s;
z-index:200;
}
.modal.show{
opacity:1;
pointer-events:auto;
}
.modal-box{
background:var(--glass);
backdrop-filter:var(--blur);
border-radius:25px;
width:90%;
max-width:900px;
display:grid;
grid-template-columns:1fr 1fr;
overflow:hidden;
}
.modal-box img{width:100%;height:100%;object-fit:cover}
.modal-content{padding:30px;display:flex;flex-direction:column;gap:15px;}
.modal-content button{
padding:12px;border:none;border-radius:15px;cursor:pointer;font-weight:700;
}
.play{background:var(--accent)}
.fav{background:#fff}

/* PLAYER */
.player{
position:fixed;inset:0;
background:#000;
display:flex;flex-direction:column;
opacity:0;pointer-events:none;
transition:.3s;
z-index:300;
}
.player.show{opacity:1;pointer-events:auto}
.player-top{
padding:15px;background:#000;display:flex;justify-content:space-between;
}
.player iframe{flex:1;border:none}

/* MOBILE */
@media(max-width:900px){
.hero{flex-direction:column;text-align:center}
}
</style>
</head>

<body>
<div class="bg-glow"></div>

<header>
<a href="#" class="logo-main">FUNIME ULTRA</a>
<div class="nav-buttons">
<button class="active" onclick="setTab('home',this)">Home</button>
<button onclick="setTab('movies',this)">Movies</button>
<button onclick="setTab('fav',this)">Favorites</button>
</div>
</header>

<main class="main">
<div id="homeTab">
<div class="hero">
<div class="hero-text">
<h1 id="heroTitle">Chainsaw Man: Reze Arc</h1>
<p id="heroDesc">Denji meets Reze. Love, bombs and chaos.</p>
<button onclick="openModal(movies[0])">▶ Watch Now</button>
</div>
<img src="https://upload.wikimedia.org/wikipedia/en/9/95/Chainsaw_Man_Reze_Arc_movie_poster.jpg" style="border-radius:25px;max-width:400px;">
</div>
</div>

<div class="section-title">🔥 Popular Anime Movies</div>
<div class="row" id="row"></div>

</main>

<!-- MODAL -->
<div class="modal" id="modal">
<div class="modal-box">
<img id="mImg">
<div class="modal-content">
<h2 id="mTitle"></h2>
<p id="mDesc"></p>
<button class="play" onclick="playMovie()">▶ Play</button>
<button class="fav" onclick="toggleFav()">⭐ Favorite</button>
<button onclick="closeModal()">Close</button>
</div>
</div>
</div>

<!-- PLAYER -->
<div class="player" id="player">
<div class="player-top">
<button onclick="closePlayer()">⬅ Back</button>
<div id="pTitle"></div>
</div>
<iframe id="frame" allowfullscreen></iframe>
</div>

<script>
const movies=[
{title:"Chainsaw Man: Reze Arc",img:"https://upload.wikimedia.org/wikipedia/en/9/95/Chainsaw_Man_Reze_Arc_movie_poster.jpg",desc:"Denji meets Reze. Love, bombs and chaos.",url:"https://drive.google.com/file/d/1bLmCakD2rkkpEUWHciKhlaSbhjRX2IcZ/preview"},
{title:"Demon Slayer: Infinity Castle",img:"https://upload.wikimedia.org/wikipedia/en/a/ae/Kimetsu_No_Yaiba_Mugen_Jyo-hen_theatrical_poster.jpg",desc:"Final battle against Muzan begins.",url:"https://drive.google.com/file/d/1bc2jGSPuaaMpwT-7T2WOkSQ5nfdDIP2e/preview"},
{title:"Coming soon",img:"https://i.imgur.com/8rJZQpB.jpg",desc:"Yuuta’s journey begins.",url:"https://www.youtube.com/embed/UPRqnFnnrr8"},
{title:"Coming soon",img:"https://i.imgur.com/kE9f6zP.jpg",desc:"A story of fate and time.",url:"https://www.youtube.com/embed/xU47nhruN-Q"},
{title:"Coming soon",img:"https://i.imgur.com/qfF5vQp.jpg",desc:"A magical journey into the spirit world.",url:"https://www.youtube.com/embed/ByXuk9QqQkk"}
];

let current=null;
let tab="home";
let fav=JSON.parse(localStorage.getItem("fav")||"[]");

const row=document.getElementById("row");
const heroTitle=document.getElementById("heroTitle");
const heroDesc=document.getElementById("heroDesc");
const heroImg=document.querySelector(".hero img");

function render(){
row.innerHTML="";
let list=movies.filter(m=>m.title.toLowerCase().includes((document.getElementById("heroTitle").innerText).toLowerCase()));
if(tab==="fav") list=list.filter(m=>fav.includes(m.title));
movies.forEach(m=>{
const c=document.createElement("div");
c.className="card";
c.innerHTML=`<img src="${m.img}"><div class="info">${m.title}</div>`;
c.onclick=()=>openModal(m);
row.appendChild(c);
});
}
render();

function setTab(t,btn){
tab=t;
document.querySelectorAll(".nav-buttons button").forEach(b=>b.classList.remove("active"));
btn.classList.add("active");
render();
}

function openModal(m){
current=m;
document.getElementById("modal").classList.add("show");
document.getElementById("mImg").src=m.img;
document.getElementById("mTitle").innerText=m.title;
document.getElementById("mDesc").innerText=m.desc;
}

function closeModal(){document.getElementById("modal").classList.remove("show")}

function playMovie(){
document.getElementById("player").classList.add("show");
document.getElementById("frame").src=current.url+"?autoplay=1";
document.getElementById("pTitle").innerText=current.title;
closeModal();
}

function closePlayer(){
document.getElementById("player").classList.remove("show");
document.getElementById("frame").src="";
}

function toggleFav(){
if(fav.includes(current.title)) fav=fav.filter(f=>f!==current.title);
else fav.push(current.title);
localStorage.setItem("fav",JSON.stringify(fav));
alert("⭐ Favorites updated!");
}
</script>
</body>
</html>
