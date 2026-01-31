<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker - WebHub Modern</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body{margin:0;font-family:'Roboto',sans-serif;background:#f4f7fc;color:#333;}

/* Hero Section */
.hero{background:url('https://images.unsplash.com/photo-1605902711622-cfb43c443f2d?auto=format&fit=crop&w=1350&q=80') center/cover no-repeat;color:#fff;padding:40px 20px;border-radius:12px;margin:20px 0;text-align:center;box-shadow:0 6px 15px rgba(0,0,0,0.2);}
.hero h1{font-size:28px;margin-bottom:10px;text-shadow:1px 1px 5px rgba(0,0,0,0.6);}
.hero p{font-size:16px;text-shadow:1px 1px 5px rgba(0,0,0,0.6);}
.hero button{padding:12px 25px;border-radius:8px;background:#ffd700;color:#2a5298;border:none;font-weight:700;cursor:pointer;transition:0.3s;margin-top:15px;}
.hero button:hover{opacity:0.9;transform:translateY(-2px);}

/* Container */
.container{width:95%;max-width:650px;margin:0 auto;padding-bottom:50px;}

/* Header */
header{background:#2a5298;color:#fff;padding:15px;text-align:center;box-shadow:0 4px 12px rgba(0,0,0,0.2);}
header .logo{font-size:28px;font-weight:700;}
nav{margin-top:10px;display:flex;flex-wrap:wrap;justify-content:center;}
nav button{margin:5px;cursor:pointer;padding:10px 18px;border:none;border-radius:8px;font-weight:600;background:#ffd700;color:#2a5298;transition:0.3s;box-shadow:0 4px 8px rgba(0,0,0,0.2);}
nav button:hover{transform:translateY(-3px);opacity:0.85;}

/* Pages */
.page{display:none;}
.page.active{display:block;animation:fadeIn 0.5s;}
@keyframes fadeIn{from{opacity:0;}to{opacity:1;}}

/* Dashboard Cards */
.dashboard{display:flex;flex-direction:column;gap:15px;margin-bottom:20px;}
.box{background:#fff;padding:18px;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.15);display:flex;justify-content:space-between;align-items:center;transition:0.3s;}
.box:hover{transform:translateY(-5px);box-shadow:0 10px 20px rgba(0,0,0,0.25);}
.box h3{font-size:16px;color:#2a5298;display:flex;align-items:center;}
.box h3 i{margin-right:8px;}
.box span{font-size:18px;font-weight:700;}

/* Tables */
table{width:100%;border-collapse:collapse;margin-bottom:20px;background:#fff;border-radius:12px;overflow:hidden;box-shadow:0 6px 15px rgba(0,0,0,0.15);}
th,td{padding:10px;text-align:center;border-bottom:1px solid #eee;font-size:13px;}
th{background:#2a5298;color:#fff;font-weight:600;}
tr:hover{background:rgba(42,82,152,0.1);}

/* Cards Grid */
.card-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:20px;}
.card{background:#fff;padding:15px;border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.15);text-align:center;transition:0.3s;cursor:pointer;}
.card img{width:50px;margin-bottom:8px;}
.card:hover{transform:translateY(-4px);box-shadow:0 8px 20px rgba(0,0,0,0.2);}
.card p{font-size:14px;color:#555;}

/* Buttons */
.btn{padding:8px 15px;border-radius:8px;background:#ffd700;color:#2a5298;border:none;cursor:pointer;transition:0.3s;margin-top:5px;}
.btn:hover{opacity:0.9;transform:translateY(-2px);}

/* Footer */
footer{background:#2a5298;color:#fff;text-align:center;padding:15px;margin-top:20px;border-radius:12px;}
footer p{margin:5px;font-size:14px;}

/* Forms */
.form-group{display:flex;justify-content:space-between;margin-bottom:10px;}
.form-group input{width:45%;padding:8px;border-radius:6px;border:1px solid #ccc;}

/* Charts */
canvas{background:#fff;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.15);padding:10px;margin-bottom:20px;}
</style>
</head>
<body>

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
  <button onclick="enterUsername()">Get Started</button>
</section>

<section id="dashboard" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Your Dashboard</h2>
  <div class="dashboard">
    <div class="box"><h3><i class="fas fa-user"></i>Username</h3><span id="infoUser">User</span></div>
    <div class="box"><h3><i class="fas fa-wallet"></i>Current Balance</h3><span id="remaining">0</span> PKR</div>
    <div class="box"><h3><i class="fas fa-piggy-bank"></i>Current Saving</h3><span id="currentSaving">0</span> PKR</div>
    <div class="box"><h3><i class="fas fa-money-bill-wave"></i>Total Expense</h3><span id="totalExpense">0</span> PKR</div>
  </div>
</section>

<section id="daily" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Daily Expenses</h2>
  <div id="dailyForm">
    <div class="form-group"><input type="number" placeholder="Food" id="foodInput"><input type="number" placeholder="Fuel" id="fuelInput"></div>
    <div class="form-group"><input type="number" placeholder="Snacks" id="snacksInput"><input type="number" placeholder="Bills" id="billsInput"></div>
    <div class="form-group"><input type="number" placeholder="Entertainment" id="entertainmentInput"></div>
    <button class="btn" onclick="addDailyEntryForm()">Add Entry</button>
    <button class="btn" onclick="resetDailyData()">Reset All</button>
  </div>
  <table id="dailyTable">
    <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Date</th><th>Action</th></tr>
  </table>
</section>

<section id="weekly" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Weekly Summary</h2>
  <table id="weeklyTable">
    <tr><th>Week</th><th>Total Expense</th><th>Saving</th></tr>
  </table>
</section>

<section id="monthly" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Monthly Summary</h2>
  <table id="monthlyTable">
    <tr><th>Month</th><th>Total Expense</th><th>Saving</th></tr>
  </table>
</section>

<section id="charts" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Expense & Saving Charts</h2>
  <canvas id="expenseChart"></canvas>
</section>

<section id="settings" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Settings</h2>
  <p>Salary: <input type="number" id="salaryInput" value="50000"></p>
  <p>Loan Amount: <input type="number" id="loanInput" value="10000"></p>
  <button class="btn" onclick="updateSettings()">Save Settings</button>
  <button class="btn" onclick="resetAll()">Reset All Data</button>
</section>

<section id="tips" class="page">
  <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Tips & Advice</h2>
  <div class="card-grid">
    <div class="card"><img src="https://img.icons8.com/ios-filled/50/2a5298/bulb.png"/><p>Track daily to save efficiently.</p></div>
    <div class="card"><img src="https://img.icons8.com/ios-filled/50/2a5298/piggy-bank.png"/><p>Set monthly saving goals.</p></div>
    <div class="card"><img src="https://img.icons8.com/ios-filled/50/2a5298/clock.png"/><p>Review weekly expenses.</p></div>
    <div class="card"><img src="https://img.icons8.com/ios-filled/50/2a5298/mobile.png"/><p>Use anywhere on mobile.</p></div>
    <div class="card"><img src="https://img.icons8.com/ios-filled/50/2a5298/combo-chart.png"/><p>Visual charts for all expenses.</p></div>
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
let dailyData = JSON.parse(localStorage.getItem('dailyData')) || [];

function showPage(pageId){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(pageId).classList.add('active');
  if(pageId==='dashboard') updateDashboard();
  if(pageId==='daily') updateDailyTable();
  if(pageId==='weekly') updateWeeklyTable();
  if(pageId==='monthly') updateMonthlyTable();
  if(pageId==='charts') drawChart();
}

function enterUsername(){
  const val = document.getElementById('usernameInput').value.trim();
  if(!val){alert('Enter a username!'); return;}
  username = val;
  localStorage.setItem('username', username);
  showPage('dashboard');
}

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

function updateDailyTable(){
  const table = document.getElementById('dailyTable');
  table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Date</th><th>Action</th></tr>";
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
    const actionCell = row.insertCell(8);
    const delBtn = document.createElement('button');
    delBtn.className='btn';
    delBtn.innerText='Delete';
    delBtn.onclick = ()=>{deleteEntry(i);};
    actionCell.appendChild(delBtn);
  });
}

function deleteEntry(index){
  if(confirm("Are you sure to delete this entry?")){
    dailyData.splice(index,1);
    localStorage.setItem('dailyData', JSON.stringify(dailyData));
    updateDashboard();
    updateDailyTable();
    updateWeeklyTable();
    updateMonthlyTable();
    drawChart();
  }
}

function resetDailyData(){if(confirm("Reset all daily data?")){dailyData=[];localStorage.setItem('dailyData',JSON.stringify(dailyData));updateDailyTable();updateDashboard();updateWeeklyTable();updateMonthlyTable();drawChart();}}
function resetAll(){if(confirm("Reset all data?")){dailyData=[];username='';salary=50000;loanAmount=10000;localStorage.clear();showPage('welcome');}}

function updateWeeklyTable(){
  const table = document.getElementById('weeklyTable');
  table.innerHTML="<tr><th>Week</th><th>Total Expense</th><th>Saving</th></tr>";
  let week=1;
  for(let i=0;i<dailyData.length;i+=7){
    const weekData = dailyData.slice(i,i+7);
    const total = weekData.reduce((a,b)=>a+b.total,0);
    const saving = Math.max(0,salary-total-loanAmount-total);
    const row = table.insertRow();
    row.insertCell(0).innerText=week++;
    row.insertCell(1).innerText=total;
    row.insertCell(2).innerText=saving;
  }
}

function updateMonthlyTable(){
  const table = document.getElementById('monthlyTable');
  table.innerHTML="<tr><th>Month</th><th>Total Expense</th><th>Saving</th></tr>";
  if(dailyData.length===0) return;
  const monthMap={};
  dailyData.forEach(d=>{
    const m=new Date(d.date).getMonth()+1;
    if(!monthMap[m]) monthMap[m]=0;
    monthMap[m]+=d.total;
  });
  Object.keys(monthMap).forEach(m=>{
    const total = monthMap[m];
    const saving = Math.max(0,salary-total-loanAmount-total);
    const row = table.insertRow();
    row.insertCell(0).innerText=m;
    row.insertCell(1).innerText=total;
    row.insertCell(2).innerText=saving;
  });
}

function updateSettings(){
  salary = Number(document.getElementById('salaryInput').value);
  loanAmount = Number(document.getElementById('loanInput').value);
  localStorage.setItem('salary', salary);
  localStorage.setItem('loan', loanAmount);
  alert('Settings updated!');
  updateDashboard();
  updateWeeklyTable();
  updateMonthlyTable();
  drawChart();
}

function addDailyEntryForm(){
  const food = Number(document.getElementById('foodInput').value) || 0;
  const fuel = Number(document.getElementById('fuelInput').value) || 0;
  const snacks = Number(document.getElementById('snacksInput').value) || 0;
  const bills = Number(document.getElementById('billsInput').value) || 0;
  const entertainment = Number(document.getElementById('entertainmentInput').value) || 0;
  const total = food+fuel+snacks+bills+entertainment;
  const date = new Date().toLocaleDateString();
  dailyData.push({food,fuel,snacks,bills,entertainment,total,date});
  localStorage.setItem('dailyData', JSON.stringify(dailyData));
  document.getElementById('foodInput').value='';
  document.getElementById('fuelInput').value='';
  document.getElementById('snacksInput').value='';
  document.getElementById('billsInput').value='';
  document.getElementById('entertainmentInput').value='';
  updateDashboard();
  updateDailyTable();
  updateWeeklyTable();
  updateMonthlyTable();
  drawChart();
}

function drawChart(){
  const ctx = document.getElementById('expenseChart').getContext('2d');
  const labels = dailyData.map((d,i)=>`Day ${i+1}`);
  const expenses = dailyData.map(d=>d.total);
  const savings = dailyData.map(d=>Math.max(0,salary - loanAmount - d.total));
  
  if(window.myChart) window.myChart.destroy(); // Reset chart
  window.myChart = new Chart(ctx, {
    type: 'bar',
    data: {
      labels: labels,
      datasets: [
        {
          label: 'Expense',
          data: expenses,
          backgroundColor: 'rgba(255, 215, 0, 0.7)',
          borderColor: '#ffd700',
          borderWidth: 1
        },
        {
          label: 'Saving',
          data: savings,
          backgroundColor: 'rgba(42, 82, 152, 0.7)',
          borderColor: '#2a5298',
          borderWidth: 1
        }
      ]
    },
    options: {
      responsive: true,
      scales: {
        y: {
          beginAtZero: true
        }
      },
      plugins: {
        legend: {position: 'top'},
        tooltip: {mode: 'index', intersect: false}
      }
    }
  });
}

// Initialize
showPage('welcome');
</script>

</div>
</body>
</html>
