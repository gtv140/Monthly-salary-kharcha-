<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker - WebHub Modern</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
:root{
--main-color:#2a5298;
--accent-color:#ffd700;
--bg-color:#f4f7fc;
--text-color:#333;
}
body{margin:0;font-family:'Roboto',sans-serif;background:var(--bg-color);color:var(--text-color);transition:0.3s;}
body.dark{--main-color:#222;color:#eee;--bg-color:#121212;--accent-color:#ffbb00;}
header{background:var(--main-color);color:#fff;padding:15px;text-align:center;box-shadow:0 4px 12px rgba(0,0,0,0.2);}
header .logo{font-size:28px;font-weight:700;}
nav{margin-top:10px;display:flex;flex-wrap:wrap;justify-content:center;}
nav button{margin:5px;cursor:pointer;padding:10px 18px;border:none;border-radius:8px;font-weight:600;background:var(--accent-color);color:var(--main-color);transition:0.3s;box-shadow:0 4px 8px rgba(0,0,0,0.2);}
nav button:hover{transform:translateY(-3px);opacity:0.85;}
.hero{background:var(--main-color) url('https://cdn-icons-png.flaticon.com/512/3135/3135715.png') center/100px no-repeat;color:#fff;padding:60px 20px;border-radius:12px;margin:20px 0;text-align:center;box-shadow:0 6px 15px rgba(0,0,0,0.2);}
.hero h1{font-size:28px;margin-bottom:10px;}
.hero p{font-size:16px;margin-bottom:15px;}
.hero input, .hero input[type=number]{padding:10px;border-radius:8px;border:none;margin-bottom:10px;width:70%;max-width:300px;}
.hero button{padding:12px 25px;border-radius:8px;background:var(--accent-color);color:var(--main-color);border:none;font-weight:700;cursor:pointer;transition:0.3s;}
.hero button:hover{opacity:0.9;transform:translateY(-2px);}
.container{width:95%;max-width:650px;margin:0 auto;padding-bottom:50px;}
.page{display:none;}
.page.active{display:block;animation:fadeIn 0.5s;}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}
.dashboard{display:flex;flex-direction:column;gap:15px;margin-bottom:20px;}
.box{background:#fff;padding:18px;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.15);display:flex;justify-content:space-between;align-items:center;transition:0.3s;cursor:pointer;}
.box:hover{transform:translateY(-5px);box-shadow:0 10px 20px rgba(0,0,0,0.25);}
.box h3{font-size:16px;color:var(--main-color);display:flex;align-items:center;}
.box h3 i{margin-right:8px;}
.box span{font-size:18px;font-weight:700;}
table{width:100%;border-collapse:collapse;margin-bottom:20px;background:#fff;border-radius:12px;overflow:hidden;box-shadow:0 6px 15px rgba(0,0,0,0.15);}
th,td{padding:10px;text-align:center;border-bottom:1px solid #eee;font-size:13px;}
th{background:var(--main-color);color:#fff;font-weight:600;}
tr:hover{background:rgba(42,82,152,0.1);}
.card-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:20px;}
.card{background:#fff;padding:15px;border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.15);text-align:center;transition:0.3s;cursor:pointer;}
.card img{width:50px;margin-bottom:8px;}
.card:hover{transform:translateY(-4px);box-shadow:0 8px 20px rgba(0,0,0,0.2);}
.card p{font-size:14px;color:#555;}
.btn{padding:8px 15px;border-radius:8px;background:var(--accent-color);color:var(--main-color);border:none;cursor:pointer;transition:0.3s;margin-top:5px;}
.btn:hover{opacity:0.9;transform:translateY(-2px);}
footer{background:var(--main-color);color:#fff;text-align:center;padding:15px;margin-top:20px;border-radius:12px;}
footer p{margin:5px;font-size:14px;}
.form-group{display:flex;justify-content:space-between;margin-bottom:10px;}
.form-group input{width:45%;padding:8px;border-radius:6px;border:1px solid #ccc;}
canvas{background:#fff;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.15);padding:10px;margin-bottom:20px;}
.toggle-mode{position:fixed;top:10px;right:10px;padding:10px 15px;background:var(--accent-color);color:var(--main-color);border:none;border-radius:8px;cursor:pointer;z-index:1000;}
</style>
</head>
<body>
<button class="toggle-mode" onclick="toggleMode()">Toggle Light/Dark</button>
<div class="container">
<header>
  <div class="logo">Pocket Tracker</div>
  <nav>
    <button onclick="showPage('dashboard')">Dashboard</button>
    <button onclick="showPage('daily')">Daily</button>
    <button onclick="showPage('weekly')">Weekly</button>
    <button onclick="showPage('monthly')">Monthly</button>
    <button onclick="showPage('charts')">Charts</button>
    <button onclick="showPage('settings')">Settings</button>
    <button onclick="showPage('tips')">Tips</button>
  </nav>
</header>

<section class="hero page active" id="welcome">
  <h1>Track & Save Smartly!</h1>
  <p>Keep your expenses, savings and financial goals on track.</p>
  <input type="text" id="usernameInput" placeholder="Enter your name"><br>
  <input type="number" id="salaryInputHero" placeholder="Enter Salary" value="50000"><br>
  <input type="number" id="loanInputHero" placeholder="Enter Loan Amount" value="10000"><br>
  <input type="number" id="dailyBudgetHero" placeholder="Daily Budget" value="500"><br>
  <button class="btn" onclick="enterUsername()">Get Started</button>
</section>

<section id="dashboard" class="page">
<h2 style="text-align:center;color:var(--main-color);margin-bottom:15px;">Your Dashboard</h2>
<div class="dashboard">
  <div class="box" onclick="showPage('daily')"><h3><i class="fas fa-user"></i>Username</h3><span id="infoUser">User</span></div>
  <div class="box" onclick="showPage('daily')"><h3><i class="fas fa-wallet"></i>Current Balance</h3><span id="remaining">0</span> PKR</div>
  <div class="box" onclick="showPage('daily')"><h3><i class="fas fa-piggy-bank"></i>Current Saving</h3><span id="currentSaving">0</span> PKR</div>
  <div class="box" onclick="showPage('daily')"><h3><i class="fas fa-money-bill-wave"></i>Total Expense</h3><span id="totalExpense">0</span> PKR</div>
  <div class="box"><h3><i class="fas fa-calendar-alt"></i>Date & Time</h3><span id="dateTime">--</span></div>
</div>
</section><section id="daily" class="page">
<h2 style="text-align:center;color:var(--main-color);margin-bottom:15px;">Daily Expenses</h2>
<div id="dailyForm">
  <div class="form-group"><input type="number" placeholder="Food" id="foodInput"><input type="number" placeholder="Fuel" id="fuelInput"></div>
  <div class="form-group"><input type="number" placeholder="Snacks" id="snacksInput"><input type="number" placeholder="Bills" id="billsInput"></div>
  <div class="form-group"><input type="number" placeholder="Entertainment" id="entertainmentInput"></div>
  <button class="btn" onclick="addDailyEntryForm()">Add Entry</button>
  <button class="btn" onclick="resetDailyData()">Reset All</button>
</div>
<table id="dailyTable"><tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Date</th><th>Action</th></tr></table>
</section>

<section id="weekly" class="page">
<h2 style="text-align:center;color:var(--main-color);margin-bottom:15px;">Weekly Summary</h2>
<table id="weeklyTable"><tr><th>Week</th><th>Total Expense</th><th>Saving</th></tr></table>
</section>

<section id="monthly" class="page">
<h2 style="text-align:center;color:var(--main-color);margin-bottom:15px;">Monthly Summary</h2>
<table id="monthlyTable"><tr><th>Month</th><th>Total Expense</th><th>Saving</th></tr></table>
</section>

<section id="charts" class="page">
<h2 style="text-align:center;color:var(--main-color);margin-bottom:15px;">Expense & Saving Charts</h2>
<canvas id="expenseChart"></canvas>
</section>

<section id="settings" class="page">
<h2 style="text-align:center;color:var(--main-color);margin-bottom:15px;">Settings</h2>
<p>Salary: <input type="number" id="salaryInput" value="50000"></p>
<p>Loan Amount: <input type="number" id="loanInput" value="10000"></p>
<p>Daily Budget: <input type="number" id="dailyBudget" value="500"></p>
<button class="btn" onclick="updateSettings()">Save Settings</button>
<button class="btn" onclick="resetAll()">Reset All Data</button>
</section>

<section id="tips" class="page">
<h2 style="text-align:center;color:var(--main-color);margin-bottom:15px;">Tips & Advice</h2>
<div class="card-grid">
<div class="card" onclick="alert('Track daily to save efficiently!')"><img src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png"/><p>Track daily to save efficiently.</p></div>
<div class="card" onclick="alert('Set monthly saving goals!')"><img src="https://cdn-icons-png.flaticon.com/512/833/833314.png"/><p>Set monthly saving goals.</p></div>
<div class="card" onclick="alert('Review weekly expenses!')"><img src="https://cdn-icons-png.flaticon.com/512/1828/1828640.png"/><p>Review weekly expenses.</p></div>
<div class="card" onclick="alert('Use anywhere on mobile!')"><img src="https://cdn-icons-png.flaticon.com/512/2910/2910769.png"/><p>Use anywhere on mobile.</p></div>
<div class="card" onclick="alert('Visual charts for all expenses!')"><img src="https://cdn-icons-png.flaticon.com/512/1055/1055687.png"/><p>Visual charts for all expenses.</p></div>
</div>
</section>

<footer>
<p>&copy; 2026 Pocket Tracker</p>
<p>Contact: support@pockettracker.com</p>
</footer>

<script>
let username = localStorage.getItem('username') || '';
let salary = Number(localStorage.getItem('salary')) || 50000;
let loanAmount = Number(localStorage.getItem('loan')) || 10000;
let dailyBudget = Number(localStorage.getItem('dailyBudget')) || 500;
let dailyData = JSON.parse(localStorage.getItem('dailyData')) || [];
let darkMode = localStorage.getItem('darkMode')==='true';
if(darkMode) document.body.classList.add('dark');

function toggleMode(){
  document.body.classList.toggle('dark');
  darkMode = document.body.classList.contains('dark');
  localStorage.setItem('darkMode', darkMode);
}

function showPage(pageId){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(pageId).classList.add('active');
  if(pageId==='dashboard') updateDashboard();
  if(pageId==='daily') updateDailyTable();
  if(pageId==='weekly') updateWeeklyTable();
  if(pageId==='monthly') updateMonthlyTable();
  if(pageId==='charts') drawChart();
  if(pageId==='tips') updateDateTime();
}

function updateDateTime(){
  const now = new Date();
  document.getElementById('dateTime').innerText = now.toLocaleString();
}

setInterval(updateDateTime, 1000);

function enterUsername(){
  const val = document.getElementById('usernameInput').value.trim();
  if(!val){alert('Enter username!'); return;}
  username = val;
  salary = Number(document.getElementById('salaryInputHero').value)||50000;
  loanAmount = Number(document.getElementById('loanInputHero').value)||10000;
  dailyBudget = Number(document.getElementById('dailyBudgetHero').value)||500;
  localStorage.setItem('username', username);
  localStorage.setItem('salary', salary);
  localStorage.setItem('loan', loanAmount);
  localStorage.setItem('dailyBudget', dailyBudget);
  updateDashboard();
  showPage('dashboard');
}

function updateDashboard(){
  let totalExpense = dailyData.reduce((a,b)=>a+b.total,0);
  let remaining = salary - totalExpense - loanAmount;
  let currentSaving = Math.max(0, remaining);
  document.getElementById('infoUser').innerText = username||'User';
  document.getElementById('totalExpense').innerText = totalExpense;
  document.getElementById('remaining').innerText = remaining;
  document.getElementById('currentSaving').innerText = currentSaving;
  localStorage.setItem('dailyData', JSON.stringify(dailyData));
}

function addDailyEntryForm(){
  const food = Number(document.getElementById('foodInput').value)||0;
  const fuel = Number(document.getElementById('fuelInput').value)||0;
  const snacks = Number(document.getElementById('snacksInput').value)||0;
  const bills = Number(document.getElementById('billsInput').value)||0;
  const entertainment = Number(document.getElementById('entertainmentInput').value)||0;
  const total = food + fuel + snacks + bills + entertainment;
  const today = new Date().toLocaleDateString();
  dailyData.push({food,fuel,snacks,bills,entertainment,total,date:today});
  updateDailyTable();updateDashboard();updateWeeklyTable();updateMonthlyTable();drawChart();
}

function updateDailyTable(){
  const table = document.getElementById('dailyTable');
  table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Date</th><th>Action</th></tr>";
  dailyData.forEach((d,i)=>{
    const row = table.insertRow();
    row.insertCell(0).innerText = i+1;
    row.insertCell(1).innerText = d.food;
    row.insertCell(2).innerText = d.fuel;
    row.insertCell(3).innerText = d.snacks;
    row.insertCell(4).innerText = d.bills;
    row.insertCell(5).innerText = d.entertainment;
    row.insertCell(6).innerText = d.total;
    row.insertCell(7).innerText = d.date;
    const actionCell = row.insertCell(8);
    const delBtn = document.createElement('button');
    delBtn.className = 'btn';
    delBtn.innerText = 'Delete';
    delBtn.onclick = ()=>{deleteEntry(i);};
    actionCell.appendChild(delBtn);
  });
  localStorage.setItem('dailyData', JSON.stringify(dailyData));
}

function deleteEntry(index){
  if(confirm("Delete this entry?")){
    dailyData.splice(index,1);
    updateDailyTable();updateDashboard();updateWeeklyTable();updateMonthlyTable();drawChart();
  }
}

function resetDailyData(){
  if(confirm("Reset all daily data?")){
    dailyData=[];updateDailyTable();updateDashboard();updateWeeklyTable();updateMonthlyTable();drawChart();
  }
}

function resetAll(){
  if(confirm("Reset ALL data?")){
    dailyData=[];username='';salary=50000;loanAmount=10000;dailyBudget=500;
    localStorage.clear();
    showPage('welcome');
  }
}

function updateWeeklyTable(){
  const table = document.getElementById('weeklyTable');
  table.innerHTML="<tr><th>Week</th><th>Total Expense</th><th>Saving</th></tr>";
  let week=1;
  for(let i=0;i<dailyData.length;i+=7){
    const weekData=dailyData.slice(i,i+7);
    const total=weekData.reduce((a,b)=>a+b.total,0);
    const saving=Math.max(0,salary-total-loanAmount-total);
    const row = table.insertRow();
    row.insertCell(0).innerText = week++;
    row.insertCell(1).innerText = total;
    row.insertCell(2).innerText = saving;
  }
}

function updateMonthlyTable(){
  const table = document.getElementById('monthlyTable');
  table.innerHTML="<tr><th>Month</th><th>Total Expense</th><th>Saving</th></tr>";
  if(dailyData.length===0) return;
  const monthMap={};
  dailyData.forEach(d=>{
    const m = new Date(d.date).getMonth()+1;
    if(!monthMap[m]) monthMap[m]=0;
    monthMap[m]+=d.total;
  });
  Object.keys(monthMap).forEach(m=>{
    const total=monthMap[m];
    const saving=Math.max(0,salary-total-loanAmount-total);
    const row=table.insertRow();
    row.insertCell(0).innerText=m;
    row.insertCell(1).innerText=total;
    row.insertCell(2).innerText=saving;
  });
}

function updateSettings(){
  salary=Number(document.getElementById('salaryInput').value)||50000;
  loanAmount=Number(document.getElementById('loanInput').value)||10000;
  dailyBudget=Number(document.getElementById('dailyBudget').value)||500;
  localStorage.setItem('salary',salary);
  localStorage.setItem('loan',loanAmount);
  localStorage.setItem('dailyBudget',dailyBudget);
  alert('Settings updated!');
  updateDashboard();updateWeeklyTable();updateMonthlyTable();drawChart();
}

function drawChart(){
  const ctx = document.getElementById('expenseChart').getContext('2d');
  const labels = dailyData.map((d,i)=>i+1);
  const expenses = dailyData.map(d=>d.total);
  const savings = dailyData.map(d=>Math.max(0,salary-d.total-loanAmount));
  if(window.myChart) window.myChart.destroy();
  window.myChart = new Chart(ctx,{
    type:'bar',
    data:{
      labels:labels,
      datasets:[
        {label:'Expenses',data:expenses,backgroundColor:'rgba(255,99,132,0.6)'},
        {label:'Savings',data:savings,backgroundColor:'rgba(54,162,235,0.6)'}
      ]
    },
    options:{responsive:true,plugins:{legend:{position:'top'}}}
  });
}
</script>
</body>
</html>
