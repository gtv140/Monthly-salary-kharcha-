<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker - WebHub Modern</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body{margin:0;font-family:'Roboto',sans-serif;background:linear-gradient(135deg,#e0f0ff,#f0f4ff);color:#333;}
h1,h2,h3{margin:0 0 10px 0;font-weight:700;}
p{margin:0 0 10px 0;}
.container{width:95%;max-width:500px;margin:0 auto;padding-bottom:50px;}

/* Header */
header{background:linear-gradient(135deg,#1e3c72,#2a5298);color:#fff;padding:15px 0;text-align:center;box-shadow:0 4px 12px rgba(0,0,0,0.2);}
header .logo{font-size:28px;font-weight:700;}
nav{margin-top:10px;}
nav button{margin:5px;cursor:pointer;padding:8px 15px;border:none;border-radius:8px;font-weight:600;background:#ffd700;color:#2a5298;transition:0.3s;box-shadow:0 4px 8px rgba(0,0,0,0.2);}
nav button:hover{transform:translateY(-3px);opacity:0.85;}

/* Pages */
.page{display:none;}
.page.active{display:block;animation:fadeIn 0.5s;}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}

/* Welcome */
#welcome h1{font-size:26px;text-align:center;margin:15px 0;}
#welcome p{text-align:center;margin-bottom:15px;}
#welcome input{width:80%;padding:10px;border-radius:8px;border:1px solid #ccc;margin-bottom:10px;}
#welcome button{padding:10px 20px;border-radius:8px;background:#2a5298;color:#fff;cursor:pointer;transition:0.3s;}
#welcome button:hover{opacity:0.9;}

/* Dashboard Boxes */
.dashboard{display:flex;flex-direction:column;gap:15px;margin-bottom:20px;}
.box{background:rgba(255,255,255,0.85);padding:15px;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.15);display:flex;justify-content:space-between;align-items:center;transition:0.3s;}
.box:hover{transform:translateY(-5px);box-shadow:0 10px 20px rgba(0,0,0,0.25);}
.box h3{font-size:16px;color:#2a5298;}
.box span{font-size:18px;font-weight:700;}

/* Tables */
table{width:100%;border-collapse:collapse;margin-bottom:20px;background:rgba(255,255,255,0.85);border-radius:12px;overflow:hidden;box-shadow:0 6px 15px rgba(0,0,0,0.15);}
th,td{padding:10px;text-align:center;border-bottom:1px solid #eee;font-size:13px;}
th{background:#2a5298;color:#fff;font-weight:600;}
tr:hover{background:rgba(42,82,152,0.1);}

/* Cards */
.card-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.card{background:rgba(255,255,255,0.85);padding:15px;border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.15);text-align:center;transition:0.3s;cursor:pointer;}
.card i{font-size:24px;color:#2a5298;margin-bottom:8px;}
.card:hover{transform:translateY(-4px);box-shadow:0 8px 20px rgba(0,0,0,0.2);}
.card p{font-size:14px;color:#555;}

/* Charts */
canvas{background:rgba(255,255,255,0.85);border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.15);padding:10px;margin-bottom:20px;}

/* Footer */
footer{background:linear-gradient(135deg,#1e3c72,#2a5298);color:#fff;text-align:center;padding:15px;margin-top:20px;border-radius:12px;}
footer p{margin:5px;font-size:14px;}
</style>
</head>
<body>
<div class="container">

<!-- Header -->
<header>
  <div class="logo">Pocket Tracker</div>
  <nav>
    <button onclick="showPage('dashboard')">Dashboard</button>
    <button onclick="showPage('daily')">Daily</button>
    <button onclick="showPage('weekly')">Weekly</button>
    <button onclick="showPage('charts')">Charts</button>
    <button onclick="showPage('settings')">Settings</button>
    <button onclick="showPage('tips')">Tips</button>
  </nav>
</header>

<!-- Welcome -->
<section id="welcome" class="page active">
  <h1>Welcome to Pocket Tracker!</h1>
  <p>Enter your username to continue:</p>
  <input type="text" id="usernameInput" placeholder="Your Name">
  <br>
  <button onclick="enterUsername()">Continue</button>
</section>

<!-- Dashboard -->
<section id="dashboard" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Your Dashboard</h2>
  <div class="dashboard">
    <div class="box"><h3>Username</h3><span id="infoUser">User</span></div>
    <div class="box"><h3>Current Balance</h3><span id="remaining">0</span> PKR</div>
    <div class="box"><h3>Current Saving</h3><span id="currentSaving">0</span> PKR</div>
    <div class="box"><h3>Total Expense</h3><span id="totalExpense">0</span> PKR</div>
  </div>
</section>

<!-- Daily -->
<section id="daily" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Daily Expenses</h2>
  <table id="dailyTable">
    <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Daily Total</th><th>Date</th></tr>
  </table>
  <button onclick="addDailyEntry()">Add Daily Entry</button>
</section>

<!-- Weekly -->
<section id="weekly" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Weekly Summary</h2>
  <table id="weeklyTable">
    <tr><th>Week</th><th>Total Expense</th><th>Saving</th></tr>
  </table>
</section>

<!-- Charts -->
<section id="charts" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Expense & Saving Charts</h2>
  <canvas id="expenseChart"></canvas>
</section>

<!-- Settings -->
<section id="settings" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Settings</h2>
  <p>Salary: <input type="number" id="salaryInput" value="50000"></p>
  <p>Loan Amount: <input type="number" id="loanInput" value="10000"></p>
  <button onclick="updateSettings()">Save Settings</button>
</section>

<!-- Tips -->
<section id="tips" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Tips & Advice</h2>
  <div class="card-grid">
    <div class="card"><i class="fas fa-lightbulb"></i><p>Track daily to save efficiently.</p></div>
    <div class="card"><i class="fas fa-hand-holding-usd"></i><p>Set monthly saving goals.</p></div>
    <div class="card"><i class="fas fa-clock"></i><p>Review weekly expenses.</p></div>
    <div class="card"><i class="fas fa-mobile-alt"></i><p>Use anywhere on mobile.</p></div>
    <div class="card"><i class="fas fa-chart-line"></i><p>Visual charts for all expenses.</p></div>
  </div>
</section>

<!-- Footer -->
<footer>
  <p>&copy; 2026 Pocket Tracker</p>
  <p>Contact: support@pockettracker.com</p>
</footer>
</div>

<script>
// ===== Global Variables =====
let username = localStorage.getItem('username') || '';
let salary = Number(localStorage.getItem('salary')) || 50000;
let loanAmount = Number(localStorage.getItem('loan')) || 10000;
let dailyData = JSON.parse(localStorage.getItem('dailyData')) || [];

// ===== Page Switch =====
function showPage(pageId){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(pageId).classList.add('active');
  if(pageId==='dashboard') updateDashboard();
  if(pageId==='daily') updateDailyTable();
  if(pageId==='weekly') updateWeeklyTable();
  if(pageId==='charts') drawChart();
}

// ===== Username =====
function enterUsername(){
  const val = document.getElementById('usernameInput').value.trim();
  if(!val){alert('Enter a username!'); return;}
  username = val;
  localStorage.setItem('username', username);
  showPage('dashboard');
}

// ===== Dashboard =====
function updateDashboard(){
  let totalExpense=0;
  dailyData.forEach(d=>totalExpense+=d.total);
  const remaining = salary-totalExpense-loanAmount;
  const currentSaving = Math.max(0,remaining);
  document.getElementById('infoUser').innerText=username;
  document.getElementById('totalExpense').innerText=totalExpense;
  document.getElementById('remaining').innerText=remaining;
  document.getElementById('currentSaving').innerText=currentSaving;
}

// ===== Daily =====
function updateDailyTable(){
  const table = document.getElementById('dailyTable');
  table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Daily Total</th><th>Date</th></tr>";
  dailyData.forEach((d,i)=>{
    const row = table.insertRow();
    row.insertCell(0).innerText=i+1;
    row.insertCell(1).innerText=d.food;
    row.insertCell(2).innerText=d.fuel;
    row.insertCell(3).innerText=d.snacks;
    row.insertCell(4).innerText=d.bills;
    row.insertCell(5).innerText=d.entertainment;
    row.insertCell(6).innerText=d.total;
    row.insertCell(7).innerText=d.date;
  });
}

// ===== Weekly =====
function updateWeeklyTable(){
  const table = document.getElementById('weeklyTable');
  table.innerHTML="<tr><th>Week</th><th>Total Expense</th><th>Saving</th></tr>";
  let week=1;
  for(let i=0;i<dailyData.length;i+=7){
    const weekData = dailyData.slice(i,i+7);
    const total = weekData.reduce((a,b)=>a+b.total,0);
    const saving = Math.max(0,salary-total-loanAmount);
    const row = table.insertRow();
    row.insertCell(0).innerText=week++;
    row.insertCell(1).innerText=total;
    row.insertCell(2).innerText=saving;
  }
}

// ===== Settings =====
function updateSettings(){
  salary = Number(document.getElementById('salaryInput').value);
  loanAmount = Number(document.getElementById('loanInput').value);
  localStorage.setItem('salary', salary);
  localStorage.setItem('loan', loanAmount);
  alert('Settings updated!');
  updateDashboard();
  updateWeeklyTable();
  drawChart();
}

// ===== Add Daily Entry =====
function addDailyEntry(){
  const food = Number(prompt("Food expense:","0"));
  const fuel = Number(prompt("Fuel expense:","0"));
  const snacks = Number(prompt("Snacks expense:","0"));
  const bills = Number(prompt("Bills expense:","0"));
  const entertainment = Number(prompt("Entertainment:","0"));
  const total = food+fuel+snacks+bills+entertainment;
  const date = new Date().toLocaleDateString();
  dailyData.push({food,fuel,snacks,bills,entertainment,total,date});
  localStorage.setItem('dailyData', JSON.stringify(dailyData));
  updateDashboard();
  updateDailyTable();
  updateWeeklyTable();
  drawChart();
}

// ===== Chart =====
function drawChart(){
  const ctx = document.getElementById('expenseChart').getContext('2d');
  const labels = dailyData.map(d=>d.date);
  const expenseData = dailyData.map(d=>d.total);
  const savingData = dailyData.map(d=>salary-d.total-loanAmount);
  if(window.expChart) window.expChart.destroy();
  window.expChart = new Chart(ctx,{
    type:'line',
    data:{
      labels:labels,
      datasets:[
        {label:'Daily Expense',data:expenseData,borderColor:'#2a5298',backgroundColor:'rgba(42,82,152,0.2)',fill:true},
        {label:'Remaining Saving',data:savingData,borderColor:'#ffd700',backgroundColor:'rgba(255,215,0,0.2)',fill:true}
      ]
    },
    options:{responsive:true,plugins:{legend:{position:'top'}}}
  });
}

// ===== Auto Load =====
if(username){showPage('dashboard');}
</script>
</body>
</html>
