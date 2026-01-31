<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker – Smart Finance</title>

<!-- Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">

<!-- Icons -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

<!-- Chart -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Poppins}
body{background:#0f172a;color:#fff}

/* NAVBAR */
nav{
position:sticky;top:0;z-index:100;
display:flex;justify-content:space-between;align-items:center;
padding:15px 25px;
background:rgba(15,23,42,0.9);
backdrop-filter:blur(10px)
}
nav h1{color:#38bdf8}
nav a{color:#fff;margin-left:15px;text-decoration:none;font-weight:500}
nav a:hover{color:#38bdf8}

/* HERO */
.hero{
height:90vh;
background:url("https://images.unsplash.com/photo-1559526324-593bc073d938") center/cover no-repeat;
display:flex;align-items:center;justify-content:center;
text-align:center;padding:20px
}
.hero div{
background:rgba(0,0,0,.55);
padding:40px;border-radius:20px
}
.hero h2{font-size:40px}
.hero p{margin:15px 0}
.hero button{
padding:12px 25px;border:none;border-radius:30px;
background:#38bdf8;color:#000;font-weight:700;cursor:pointer
}

/* SECTION */
section{padding:60px 20px;max-width:1200px;margin:auto}
section h3{text-align:center;font-size:32px;margin-bottom:30px}

/* CARDS */
.cards{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:20px
}
.card{
background:linear-gradient(135deg,#1e293b,#020617);
padding:25px;border-radius:20px;
box-shadow:0 15px 40px rgba(0,0,0,.5);
transition:.3s
}
.card:hover{transform:translateY(-8px)}
.card i{font-size:35px;color:#38bdf8;margin-bottom:10px}

/* DASHBOARD */
.dashboard input{
width:100%;padding:10px;border-radius:10px;border:none;margin-top:8px
}

/* TABLE */
table{width:100%;border-collapse:collapse;margin-top:20px;background:#fff;color:#000;border-radius:15px;overflow:hidden}
th,td{padding:12px;text-align:center}
th{background:#38bdf8}
tr:nth-child(even){background:#e0f2fe}

/* TIPS */
.tips{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
gap:20px
}
.tip{
background:#020617;padding:20px;border-radius:15px;
border:1px solid #1e293b
}

/* FOOTER */
footer{
background:#020617;
text-align:center;
padding:20px;
margin-top:50px
}

/* MOBILE */
@media(max-width:600px){
.hero h2{font-size:28px}
}
</style>
</head>

<body>

<nav>
<h1>💰 Pocket Tracker</h1>
<div>
<a href="#dashboard">Dashboard</a>
<a href="#expenses">Expenses</a>
<a href="#summary">Summary</a>
<a href="#tips">Tips</a>
</div>
</nav>

<div class="hero">
<div>
<h2>Smart Money, Smart Life</h2>
<p>Track expenses • Save more • Stress less</p>
<button onclick="document.getElementById('dashboard').scrollIntoView()">Get Started</button>
</div>
</div>

<!-- DASHBOARD -->
<section id="dashboard">
<h3>Dashboard</h3>
<div class="cards dashboard">
<div class="card">
<i class="fa-solid fa-user"></i>
<p>Username</p>
<input id="user" value="User">
</div>
<div class="card">
<i class="fa-solid fa-wallet"></i>
<p>Salary</p>
<input type="number" id="salary" value="50000">
</div>
<div class="card">
<i class="fa-solid fa-credit-card"></i>
<p>Loan</p>
<input type="number" id="loan" value="10000">
</div>
<div class="card">
<i class="fa-solid fa-piggy-bank"></i>
<p>Saving Goal</p>
<input type="number" id="goal" value="10000">
</div>
</div>
</section>

<!-- EXPENSES -->
<section id="expenses">
<h3>Daily Expenses</h3>
<div class="cards">
<div class="card" onclick="addExpense('Food')"><i class="fa-solid fa-burger"></i>Food</div>
<div class="card" onclick="addExpense('Fuel')"><i class="fa-solid fa-gas-pump"></i>Fuel</div>
<div class="card" onclick="addExpense('Bills')"><i class="fa-solid fa-file-invoice"></i>Bills</div>
<div class="card" onclick="addExpense('Fun')"><i class="fa-solid fa-face-laugh"></i>Fun</div>
</div>

<table id="table">
<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Bills</th><th>Fun</th><th>Total</th></tr>
</table>

<canvas id="chart" height="120"></canvas>
</section>

<!-- SUMMARY -->
<section id="summary">
<h3>Monthly Summary</h3>
<div class="cards">
<div class="card"><i class="fa-solid fa-scale-balanced"></i>Remaining Balance: <span id="balance">0</span></div>
<div class="card"><i class="fa-solid fa-piggy-bank"></i>Total Saving: <span id="saving">0</span></div>
</div>
</section>

<!-- TIPS -->
<section id="tips">
<h3>Smart Tips</h3>
<div class="tips">
<div class="tip">🍱 Home food = more savings</div>
<div class="tip">💧 Stay hydrated in night shift</div>
<div class="tip">📊 Review expenses weekly</div>
<div class="tip">😴 Proper sleep improves control</div>
</div>
</section>

<footer>
<p>© 2026 Pocket Tracker • Designed with ❤️ Sweetie</p>
</footer>

<script>
let data=[]
let day=0

function addExpense(type){
let amount=+prompt("Enter "+type+" expense")
if(!amount) return
if(!data[day]) data[day]={Food:0,Fuel:0,Bills:0,Fun:0}
data[day][type]+=amount
render()
}

function render(){
let table=document.getElementById("table")
table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Bills</th><th>Fun</th><th>Total</th></tr>"
let total=0
data.forEach((d,i)=>{
let sum=d.Food+d.Fuel+d.Bills+d.Fun
total+=sum
table.innerHTML+=`<tr><td>${i+1}</td><td>${d.Food}</td><td>${d.Fuel}</td><td>${d.Bills}</td><td>${d.Fun}</td><td>${sum}</td></tr>`
})
let salary=+salaryInput.value
let loan=+loanInput.value
document.getElementById("balance").innerText=salary-total-loan
document.getElementById("saving").innerText=Math.max(0,salary-total-loan)
updateChart()
localStorage.setItem("pocketData",JSON.stringify(data))
}

let salaryInput=document.getElementById("salary")
let loanInput=document.getElementById("loan")
salaryInput.onchange=loanInput.onchange=render

let ctx=document.getElementById("chart")
let chart
function updateChart(){
if(chart) chart.destroy()
chart=new Chart(ctx,{
type:"line",
data:{
labels:data.map((_,i)=>"Day "+(i+1)),
datasets:[{
label:"Expenses",
data:data.map(d=>d.Food+d.Fuel+d.Bills+d.Fun),
borderColor:"#38bdf8",
tension:.4
}]
}
})
}

window.onload=()=>{
let saved=localStorage.getItem("pocketData")
if(saved) data=JSON.parse(saved)
render()
}
</script>

</body>
</html>
