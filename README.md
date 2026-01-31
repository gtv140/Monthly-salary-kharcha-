<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base Styles ===== */
body {
  font-family: 'Roboto', sans-serif;
  margin: 0; padding: 0;
  background: linear-gradient(135deg,#1e3c72,#2a5298);
  color: #fff;
  transition: 0.5s;
}
h1,h2,h3{text-align:center;margin:10px;font-weight:700;}
button{cursor:pointer;font-weight:700;transition:0.3s;}
button:hover{opacity:0.85;}
input{padding:8px;border-radius:10px;border:none;margin:3px;width:120px;}

/* ===== Header ===== */
header {
  background: linear-gradient(135deg,#29b6f6,#00acc1);
  padding: 20px;
  text-align: center;
  font-size: 28px;
  font-weight: bold;
  color: #fff;
  text-shadow: 2px 2px 5px rgba(0,0,0,0.3);
  box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
}

/* ===== Theme Toggle ===== */
#themeToggle {
  position: fixed; top: 15px; right: 15px;
  padding: 10px 15px;
  background: #0288d1;
  color: #fff;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  font-weight: bold;
}

/* ===== Dashboard Layout ===== */
.dashboard-container {
  max-width: 1200px;
  margin: 20px auto;
  padding: 0 15px;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit,minmax(200px,1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.card {
  background: linear-gradient(145deg,#00acc1,#29b6f6);
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0,0,0,0.35);
  text-align: center;
  transition: 0.4s;
}
.card:hover { transform: scale(1.05); box-shadow:0 10px 30px rgba(0,0,0,0.45); }
.card i { font-size: 32px; margin-bottom: 10px; }

/* ===== Table ===== */
table {
  width: 100%;
  border-collapse: collapse;
  background: rgba(255,255,255,0.95);
  color: #333;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 6px 15px rgba(0,0,0,0.2);
  margin-bottom: 30px;
}
th, td { padding: 10px; text-align: center; }
th { background: #0288d1; color: #fff; letter-spacing: 1px; }
tr:nth-child(even){background: rgba(0,172,193,0.1);}
tr:hover { background: rgba(0,172,193,0.2); }

/* ===== Footer Floating Lights ===== */
.floating {
  position: absolute;
  border-radius: 50%;
  opacity: 0.3;
  animation: float 10s infinite;
  z-index:0;
}
@keyframes float {
  0%{transform:translateY(0) rotate(0deg);}
  50%{transform:translateY(-50px) rotate(180deg);}
  100%{transform:translateY(0) rotate(360deg);}
}

/* ===== Responsive ===== */
@media(max-width:600px){
  header { font-size: 22px; padding: 15px;}
  #themeToggle { padding: 8px 10px; font-size:14px; }
}
</style>
</head>
<body>

<header>Pocket Tracker 💼</header>
<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<div class="dashboard-container">

  <!-- User Info Cards -->
  <div class="card-grid">
    <div class="card"><i>👤</i><h3 id="username">User</h3><p>Username</p></div>
    <div class="card"><i>💰</i><h3 id="currentBalance">0</h3><p>Current Balance</p></div>
    <div class="card"><i>💵</i><h3 id="totalSaving">0</h3><p>Total Saving</p></div>
    <div class="card"><i>📊</i><h3 id="totalExpense">0</h3><p>Total Expense</p></div>
  </div>

  <!-- Controls -->
  <div class="card-grid">
    <div class="card" onclick="addSalary()"><i>💵</i><p>Add Salary</p></div>
    <div class="card" onclick="addLoan()"><i>💳</i><p>Add Loan</p></div>
    <div class="card" onclick="addExpense()"><i>🛒</i><p>Add Expense</p></div>
    <div class="card" onclick="clearAll()"><i>🗑️</i><p>Clear All</p></div>
  </div>

  <!-- Daily Expenses Table -->
  <h2>Daily Expenses</h2>
  <table id="dailyTable">
    <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Daily Total</th><th>Date</th></tr>
  </table>

  <!-- Charts -->
  <h2>Overview</h2>
  <canvas id="chart" width="400" height="200"></canvas>

</div>

<!-- Floating Lights -->
<div class="floating" style="width:15px;height:15px;background:#ffeb3b;top:50px;left:30px;"></div>
<div class="floating" style="width:12px;height:12px;background:#03a9f4;top:150px;left:120px;"></div>
<div class="floating" style="width:18px;height:18px;background:#4caf50;top:300px;left:200px;"></div>
<div class="floating" style="width:10px;height:10px;background:#ff5722;top:400px;left:50px;"></div>

<script>
// ===== Theme Toggle =====
let darkTheme=true;
function toggleTheme(){
  if(darkTheme){
    document.body.style.background="#fff";
    document.body.style.color="#333";
    darkTheme=false;
  }else{
    document.body.style.background="linear-gradient(135deg,#1e3c72,#2a5298)";
    document.body.style.color="#fff";
    darkTheme=true;
  }
}

// ===== Local Storage Data =====
let username = localStorage.getItem('username') || prompt("Enter your name") || "User";
let salary = parseInt(localStorage.getItem('salary')||0);
let loan = parseInt(localStorage.getItem('loan')||0);
let dailyData = JSON.parse(localStorage.getItem('dailyData')||"[]");

document.getElementById('username').innerText = username;

function saveData(){
  localStorage.setItem('salary', salary);
  localStorage.setItem('loan', loan);
  localStorage.setItem('dailyData', JSON.stringify(dailyData));
}

// ===== Add Functions =====
function addSalary(){
  let val = parseInt(prompt("Enter salary:","0"))||0;
  salary += val;
  updateDashboard();
  saveData();
}

function addLoan(){
  let val = parseInt(prompt("Enter loan:","0"))||0;
  loan += val;
  updateDashboard();
  saveData();
}

function addExpense(){
  let food = parseInt(prompt("Food expense:","0"))||0;
  let fuel = parseInt(prompt("Fuel expense:","0"))||0;
  let snacks = parseInt(prompt("Snacks:","0"))||0;
  let bills = parseInt(prompt("Bills:","0"))||0;
  let fun = parseInt(prompt("Fun expense:","0"))||0;
  let date = new Date().toLocaleDateString();
  let daily = {food,fuel,snacks,bills,fun,date};
  daily.total = food+fuel+snacks+bills+fun;
  dailyData.push(daily);
  updateDashboard();
  saveData();
}

function clearAll(){
  if(confirm("Clear all data?")){
    dailyData = [];
    salary = 0;
    loan = 0;
    updateDashboard();
    saveData();
  }
}

// ===== Update Dashboard =====
function updateDashboard(){
  let totalExpense = dailyData.reduce((a,b)=>a+b.total,0)+loan;
  let totalSaving = Math.max(0,salary-totalExpense);
  let currentBalance = salary-totalExpense;
  document.getElementById('currentBalance').innerText = currentBalance;
  document.getElementById('totalSaving').innerText = totalSaving;
  document.getElementById('totalExpense').innerText = totalExpense;

  // Table
  const table = document.getElementById('dailyTable');
  table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Daily Total</th><th>Date</th></tr>";
  dailyData.forEach((d,i)=>{
    const row = table.insertRow();
    row.insertCell(0).innerText = i+1;
    row.insertCell(1).innerText = d.food;
    row.insertCell(2).innerText = d.fuel;
    row.insertCell(3).innerText = d.snacks;
    row.insertCell(4).innerText = d.bills;
    row.insertCell(5).innerText = d.fun;
    row.insertCell(6).innerText = d.total;
    row.insertCell(7).innerText = d.date;
  });

  // Chart
  const ctx = document.getElementById('chart').getContext('2d');
  if(window.barChart) window.barChart.destroy();
  window.barChart = new Chart(ctx,{
    type:'bar',
    data:{
      labels:['Salary','Total Expense','Total Saving'],
      datasets:[{
        label:'PKR',
        data:[salary,totalExpense,totalSaving],
        backgroundColor:['#4caf50','#f44336','#2196f3']
      }]
    },
    options:{responsive:true,plugins:{legend:{display:false}}}
  });
}

updateDashboard();
</script>

</body>
</html>
