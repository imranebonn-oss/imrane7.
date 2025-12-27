<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Redeem Code</title>

<style>
*{box-sizing:border-box}
body{
  margin:0;
  height:100vh;
  font-family:Arial;
  background:linear-gradient(135deg,#00c6ff,#0072ff);
  display:flex;
  justify-content:center;
  align-items:center;
}
.card{
  background:#fff;
  padding:25px;
  width:360px;
  border-radius:15px;
  box-shadow:0 15px 35px rgba(0,0,0,.3);
  text-align:center;
}
h2{margin-bottom:10px;color:#0b3c5d}
input{
  width:100%;
  padding:10px;
  margin:8px 0;
  border-radius:8px;
  border:1px solid #ccc;
}
button{
  width:100%;
  padding:10px;
  margin-top:8px;
  border:none;
  border-radius:8px;
  cursor:pointer;
  font-weight:bold;
}
.btn-main{background:#0072ff;color:white}
.btn-admin{background:#222;color:white}
#result{margin-top:10px;font-weight:bold}
.stats{
  display:flex;
  justify-content:space-between;
  margin-top:10px;
  font-weight:bold;
}
ul{
  list-style:none;
  padding:0;
  margin-top:10px;
  display:none;
}
ul li{
  background:#eaf4ff;
  padding:6px;
  border-radius:6px;
  margin-bottom:5px;
}
.modal{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.4);
  display:none;
  justify-content:center;
  align-items:center;
}
.modal-box{
  background:white;
  padding:20px;
  border-radius:10px;
  width:280px;
}
</style>
</head>

<body>

<div class="card">
  <h2>🎁 Redeem Code</h2>

  <input id="codeInput" placeholder="Entrer le code">
  <button class="btn-main" onclick="redeem()">Valider</button>

  <div id="result"></div>

  <div class="stats">
    <span>⭐ <span id="points">0</span></span>
    <span>📦 <span id="count">0</span></span>
  </div>

  <button class="btn-admin" onclick="openAdmin()">🔐 Admin</button>

  <ul id="codesList"></ul>
</div>

<!-- ADMIN -->
<div class="modal" id="adminModal">
  <div class="modal-box">
    <p>Mot de passe admin</p>
    <input type="password" id="adminPass">
    <button class="btn-main" onclick="checkAdmin()">OK</button>
  </div>
</div>

<script>
const adminPassword = "imrane2005@";
const value = 10;

const codeInput = document.getElementById("codeInput");
const result = document.getElementById("result");
const pointsSpan = document.getElementById("points");
const countSpan = document.getElementById("count");
const codesList = document.getElementById("codesList");
const adminModal = document.getElementById("adminModal");
const adminPass = document.getElementById("adminPass");

let points = Number(localStorage.getItem("points")) || 0;
let codes = JSON.parse(localStorage.getItem("codes")) || [
  "E56TT","TTTT","TTGF","BHHHHH","HUJJ"
];

update();

function redeem(){
  const c = codeInput.value.trim().toUpperCase();
  if(!c) return;

  if(codes.includes(c)){
    points += value;
    codes = codes.filter(x => x !== c); // يُستعمل مرة واحدة
    result.style.color = "green";
    result.textContent = "✅ Code valide +10 points";
  }else{
    result.style.color = "red";
    result.textContent = "❌ Code invalide";
  }
  save();
  update();
  codeInput.value = "";
}

function update(){
  pointsSpan.textContent = points;
  countSpan.textContent = codes.length;
}

function save(){
  localStorage.setItem("points", points);
  localStorage.setItem("codes", JSON.stringify(codes));
}

function openAdmin(){
  adminModal.style.display = "flex";
  adminPass.value = "";
}

function checkAdmin(){
  if(adminPass.value === adminPassword){
    codesList.style.display = "block";
    codesList.innerHTML = "";
    codes.forEach(c=>{
      const li = document.createElement("li");
      li.textContent = c;
      codesList.appendChild(li);
    });
    adminModal.style.display = "none";
  }else{
    alert("Mot de passe incorrect");
  }
}
</script>

</body>
</html>

