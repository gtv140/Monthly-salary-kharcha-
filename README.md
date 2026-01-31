<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Tracker Ultimate</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
/* ===== Base ===== */
body{font-family:'Roboto',sans-serif;margin:0;padding:0; background:linear-gradient(135deg,#1e3c72,#2a5298); color:#fff; overflow-x:hidden; transition:0.5s;}
a{text-decoration:none; color:inherit;}
h1,h2{text-align:center; margin:10px; font-weight:700;}
button{cursor:pointer; transition:0.3s; font-weight:700; border:none; border-radius:10px; padding:10px 20px;}
button:hover{opacity:0.85;}

/* ===== Header & Nav ===== */
header{background:linear-gradient(135deg,#29b6f6,#00acc1); padding:15px 20px; display:flex; justify-content:space-between; align-items:center; box-shadow:0 4px 10px rgba(0,0,0,0.3);}
header h1{font-size:28px; text-shadow:2px 2px 5px rgba(0,0,0,0.3);}
nav a{margin:0 15px; font-weight:bold; transition:0.3s;}
nav a:hover{color:#ffeb3b;}

/* ===== Hero Section ===== */
.hero{text-align:center; padding:60px 20px; background:url('https://images.unsplash.com/photo-1581091215360-620f34e7cfef?auto=format&fit=crop&w=1400&q=80') center/cover no-repeat; color:#fff; position:relative;}
.hero h2{font-size:36px; margin-bottom:20px; text-shadow:2px 2px 6px rgba(0,0,0,0.6);}
.hero p{font-size:20px; margin-bottom:30px;}
.hero button{background:#ffeb3b; color:#1e3c72; font-weight:bold;}

/* ===== Dashboard Cards ===== */
.section{padding:40px 20px; background:#f0f4f8; color:#333; border-radius:15px; max-width:1200px; margin:30px auto;}
.info-section{display:flex; flex-wrap:wrap; justify-content:center;}
.info-box{background:linear-gradient(145deg,#00acc1,#29b6f6); box-shadow:0 8px 20px rgba(0,0,0,0.35); border-radius:15px; margin:10px; padding:20px; text-align:center; width:150px; cursor:pointer; transition:0.4s; font-weight:bold; color:#fff;}
.info-box:hover{transform:scale(1.08); box-shadow:0 10px 30px rgba(0,0,0,0.45);}
.info-box img{width:50px; height:50px; margin-bottom:10px;}

/* ===== Charts ===== */
canvas{background:#fff; border-radius:15px; padding:10px; max-width:100%; display:block; margin:auto;}

/* ===== Floating Button ===== */
#themeToggle{position:fixed; top:15px; right:15px; padding:10px 15px; background:#0288d1; color:#fff; border-radius:50px; font-weight:bold; box-shadow:0 4px 10px rgba(0,0,0,0.3); z-index:1000;}
#themeToggle:hover{opacity:0.8;}

/* ===== Footer ===== */
footer{background:#0288d1; color:#fff; text-align:center; padding:20px; margin-top:40px; border-radius:10px;}

/* ===== Responsive ===== */
@media(max-width:768px){
    .info-section{flex-direction:column; align-items:center;}
    .info-box{width:80%;}
    .hero h2{font-size:28px;}
    .hero p{font-size:16px;}
}
</style>
</head>
<body>

<header>
<h1>💼 Pocket Tracker Ultimate 💼</h1>
<nav>
<a href="#dashboard">Dashboard</a>
<a href="#chart-section">Overview</a>
<a href="#footer">Contact</a>
</nav>
</header>

<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<section class="hero">
<h2>Track Your Expenses Like a Pro</h2>
<p>Manage daily, weekly, and monthly expenses with ease!</p>
<button onclick="document.getElementById('dashboard').scrollIntoView({behavior:'smooth'})">Go to Dashboard</button>
</section>

<section class="section" id="dashboard">
<h2>💰 Dashboard</h2>
<div class="info-section">
<div class="info-box" onclick="addExpense('Food')">
<img src="https://cdn-icons-png.flaticon.com/512/1046/1046784.png" alt="Food">
<h3>Food</h3>
<span id="foodTotal">0 PKR</span>
</div>
<div class="info-box" onclick="addExpense('Fuel')">
<img src="https://cdn-icons-png.flaticon.com/512/743/743131.png" alt="Fuel">
<h3>Fuel</h3>
<span id="fuelTotal">0 PKR</span>
</div>
<div class="info-box" onclick="addExpense('Snacks')">
<img src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png" alt="Snacks">
<h3>Snacks</h3>
<span id="snacksTotal">0 PKR</span>
</div>
<div class="info-box" onclick="addExpense('Bills')">
<img src="https://cdn-icons-png.flaticon.com/512/1681/1681089.png" alt="Bills">
<h3>Bills</h3>
<span id="billsTotal">0 PKR</span>
</div>
<div class="info-box" onclick="addExpense('Entertainment')">
<img src="https://cdn-icons-png.flaticon.com/512/1046/1046785.png" alt="Fun">
<h3>Fun</h3>
<span id="funTotal">0 PKR</span>
</div>
</div>
</section>

<section class="section" id="chart-section">
<h2>📊 Expense Overview</h2>
<canvas id="expenseChart"></canvas>
</section>

<footer id="footer">
&copy; 2026 Pocket Tracker Ultimate | Made with ❤️ by Sweetie
</footer>

<script>
// ===== Theme Toggle =====
let darkTheme=true;
function toggleTheme(){
if(darkTheme){document.body.style.background="#fff"; document.body.style.color="#333"; darkTheme=false;}
else{document.body.style.background="linear-gradient(135deg,#1e3c72,#2a5298)"; document.body.style.color="#fff"; darkTheme=true;}
}

// ===== Local Storage =====
let expenses = JSON.parse(localStorage.getItem('expenses')) || {Food:0, Fuel:0, Snacks:0, Bills:0, Entertainment:0};

// ===== Update Cards =====
function updateCards(){
document.getElementById('foodTotal').innerText=expenses.Food+" PKR";
document.getElementById('fuelTotal').innerText=expenses.Fuel+" PKR";
document.getElementById('snacksTotal').innerText=expenses.Snacks+" PKR";
document.getElementById('billsTotal').innerText=expenses.Bills+" PKR";
document.getElementById('funTotal').innerText=expenses.Entertainment+" PKR";
drawChart();
}

// ===== Add Expense =====
function addExpense(type){
let val=parseInt(prompt(`Enter ${type} expense in PKR:`,"0")) || 0;
expenses[type]+=val;
localStorage.setItem('expenses', JSON.stringify(expenses));
updateCards();
}

// ===== Chart =====
let ctx=document.getElementById('expenseChart').getContext('2d');
let expenseChart;
function drawChart(){
if(expenseChart) expenseChart.destroy();
expenseChart=new Chart(ctx,{
type:'bar',
data:{labels:Object.keys(expenses), datasets:[{label:'Expenses in PKR', data:Object.values(expenses), backgroundColor:['#ff6384','#36a2eb','#ffcd56','#4bc0c0','#9966ff']}]},
options:{responsive:true, plugins:{legend:{display:false}, tooltip:{enabled:true}}, scales:{y:{beginAtZero:true}}}
});
}

// ===== Initialize =====
updateCards();
</script>

</body>
</html><section class="section" id="tracker-section">
<h2>📅 Expense Tracker</h2>
<div style="text-align:center; margin-bottom:20px;">
<button onclick="setView('daily')">Daily</button>
<button onclick="setView('weekly')">Weekly</button>
<button onclick="setView('monthly')">Monthly</button>
</div>

<table id="trackerTable" style="width:100%; border-collapse:collapse; background:#fff; color:#333; border-radius:12px; overflow:hidden;">
<tr style="background:#0288d1; color:#fff;">
<th>Date</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Action</th>
</tr>
</table>
</section>

<script>
// ===== Tracker Data =====
let tracker = JSON.parse(localStorage.getItem('tracker')) || [];

// ===== Add Expense Modal =====
function addTrackerExpense(){
const today = new Date().toLocaleDateString();
let food = parseInt(prompt("Food expense:","0")) || 0;
let fuel = parseInt(prompt("Fuel expense:","0")) || 0;
let snacks = parseInt(prompt("Snacks expense:","0")) || 0;
let bills = parseInt(prompt("Bills expense:","0")) || 0;
let fun = parseInt(prompt("Fun / Entertainment:","0")) || 0;
tracker.push({date:today, Food:food, Fuel:fuel, Snacks:snacks, Bills:bills, Entertainment:fun});
localStorage.setItem('tracker', JSON.stringify(tracker));
updateTrackerTable();
updateChart();
}

// ===== Update Table =====
function updateTrackerTable(){
const table = document.getElementById('trackerTable');
table.innerHTML=`<tr style="background:#0288d1; color:#fff;">
<th>Date</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Action</th>
</tr>`;
tracker.forEach((t, index)=>{
const row = table.insertRow();
row.insertCell(0).innerText = t.date;
row.insertCell(1).innerText = t.Food;
row.insertCell(2).innerText = t.Fuel;
row.insertCell(3).innerText = t.Snacks;
row.insertCell(4).innerText = t.Bills;
row.insertCell(5).innerText = t.Entertainment;
const total = t.Food + t.Fuel + t.Snacks + t.Bills + t.Entertainment;
row.insertCell(6).innerText = total;
const actionCell = row.insertCell(7);
actionCell.innerHTML = `<button onclick="editTracker(${index})" style="margin-right:5px;">✏️</button>
<button onclick="deleteTracker(${index})">🗑️</button>`;
});
}

// ===== Edit/Delete =====
function editTracker(index){
let t = tracker[index];
t.Food = parseInt(prompt("Food expense:", t.Food)) || 0;
t.Fuel = parseInt(prompt("Fuel expense:", t.Fuel)) || 0;
t.Snacks = parseInt(prompt("Snacks expense:", t.Snacks)) || 0;
t.Bills = parseInt(prompt("Bills expense:", t.Bills)) || 0;
t.Entertainment = parseInt(prompt("Fun:", t.Entertainment)) || 0;
tracker[index] = t;
localStorage.setItem('tracker', JSON.stringify(tracker));
updateTrackerTable();
updateChart();
}

function deleteTracker(index){
if(confirm("Delete this entry?")){
tracker.splice(index,1);
localStorage.setItem('tracker', JSON.stringify(tracker));
updateTrackerTable();
updateChart();
}
}

// ===== View Toggle =====
let currentView='daily';
function setView(view){
currentView=view;
// For now simple: could filter weekly/monthly
updateTrackerTable();
}

// ===== Update Chart =====
function updateChart(){
let sum={Food:0, Fuel:0, Snacks:0, Bills:0, Entertainment:0};
tracker.forEach(t=>{
sum.Food += t.Food;
sum.Fuel += t.Fuel;
sum.Snacks += t.Snacks;
sum.Bills += t.Bills;
sum.Entertainment += t.Entertainment;
});
expenses=sum;
updateCards();
}

// ===== Floating Icons =====
const floatIconsData=[
{color:'#ffeb3b', size:15, top:50, left:30},
{color:'#03a9f4', size:12, top:150, left:120},
{color:'#4caf50', size:18, top:300, left:200},
{color:'#ff5722', size:10, top:400, left:50},
{color:'#9c27b0', size:14, top:220, left:300}
];
floatIconsData.forEach(f=>{
let div=document.createElement('div');
div.style.width=f.size+'px';
div.style.height=f.size+'px';
div.style.background=f.color;
div.style.position='absolute';
div.style.top=f.top+'px';
div.style.left=f.left+'px';
div.style.borderRadius='50%';
div.style.opacity=0.3;
div.style.animation='float 10s infinite';
document.body.appendChild(div);
});
</script>

<style>
@keyframes float{
0%{transform:translateY(0) rotate(0deg);}
50%{transform:translateY(-50px) rotate(180deg);}
100%{transform:translateY(0) rotate(360deg);}
}
</style>

<div style="text-align:center; margin:20px;">
<button onclick="addTrackerExpense()" style="background:#ffeb3b; color:#1e3c72; font-weight:bold;">➕ Add Daily Expense</button>
</div>
