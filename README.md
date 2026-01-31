<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Tracker - Modern Dashboard</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Body ===== */
body {
    margin:0; padding:0;
    font-family: 'Roboto', sans-serif;
    background: linear-gradient(135deg, #1e3c72, #2a5298);
    color:#fff;
    transition:0.5s;
}
a {text-decoration:none; color:inherit;}

/* ===== Header / Nav ===== */
header {
    background: linear-gradient(135deg,#29b6f6,#00acc1);
    padding:20px;
    font-size:28px;
    font-weight:bold;
    text-align:center;
    color:#fff;
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
    border-radius:0 0 20px 20px;
    box-shadow:0 5px 20px rgba(0,0,0,0.3);
    position:sticky; top:0; z-index:10;
}
nav {
    display:flex; justify-content:center; gap:20px; margin-top:10px;
}
nav a {
    background: rgba(255,255,255,0.1); padding:10px 15px; border-radius:10px; font-weight:bold;
    transition:0.3s;
}
nav a:hover {background: rgba(255,255,255,0.25); transform:scale(1.05);}

/* ===== Container ===== */
.container {max-width:1200px; margin:auto; padding:15px;}
.flex {display:flex; flex-wrap:wrap; gap:15px; justify-content:center;}

/* ===== Cards ===== */
.card, .expense-card, .feature-card {
    border-radius:15px; padding:20px; text-align:center; box-shadow:0 8px 25px rgba(0,0,0,0.3); transition:0.3s;
}
.card {background: rgba(255,255,255,0.1); backdrop-filter: blur(10px); width:180px;}
.card:hover, .expense-card:hover, .feature-card:hover {transform:scale(1.05);}
.expense-card {background: linear-gradient(145deg,#ff9800,#ffb74d); width:140px; font-weight:bold; cursor:pointer;}
.feature-card {background: rgba(255,255,255,0.15); width:200px; cursor:pointer;}

/* ===== Table ===== */
table {width:100%; border-collapse:collapse; margin-top:20px; background:rgba(255,255,255,0.95); color:#333; border-radius:12px; overflow:hidden;}
th,td {padding:10px; text-align:center; border:1px solid #e0e0e0;}
th {background:#0288d1; color:#fff;}
tr:nth-child(even){background: rgba(0,172,193,0.1);}
tr:hover {background: rgba(0,172,193,0.2);}

/* ===== Chart ===== */
#chart {background:rgba(255,255,255,0.1); border-radius:15px; padding:15px; margin-top:20px;}

/* ===== Icons ===== */
img.icon {width:40px; height:40px; margin-bottom:5px;}

/* ===== Responsive ===== */
@media(max-width:768px){.card, .expense-card {width:45%;} .feature-card{width:45%;}}
@media(max-width:480px){.card, .expense-card {width:90%;} .feature-card{width:90%;}}

/* ===== Hero Section ===== */
.hero {background: linear-gradient(135deg,#00acc1,#29b6f6); border-radius:15px; padding:40px; margin-bottom:20px; text-align:center; box-shadow:0 10px 30px rgba(0,0,0,0.4);}
.hero h1 {font-size:36px; margin-bottom:10px;}
.hero p {font-size:18px; color:#ffeb3b;}
</style>
</head>
<body>

<header>Pocket Tracker 💼
<nav>
<a href="#dashboard">Dashboard</a>
<a href="#expenses">Expenses</a>
<a href="#features">Features</a>
<a href="#analytics">Analytics</a>
</nav>
</header>

<div class="container">
<div class="hero">
<h1>Manage Your Money Smartly</h1>
<p>Track expenses, set goals & save efficiently</p>
</div>

<h2 id="dashboard">Welcome, User!</h2>

<!-- User Info Cards -->
<div class="flex">
    <div class="card"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/money.png"/><br>Current Balance<br><span id="currentBalance">0</span> PKR</div>
    <div class="card"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/savings.png"/><br>Current Saving<br><span id="currentSaving">0</span> PKR</div>
    <div class="card"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/wallet.png"/><br>Salary<br><span id="salaryAmount">0</span> PKR</div>
    <div class="card"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/debt.png"/><br>Loan<br><span id="loanAmount">0</span> PKR</div>
</div>

<!-- Update Inputs -->
<div class="flex">
    <div class="card">
        <img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/wallet.png"/><br>
        Salary: <input type="number" id="salaryInput" value="50000"><br>
        <button onclick="updateSalary()">Update</button>
    </div>
    <div class="card">
        <img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/debt.png"/><br>
        Loan: <input type="number" id="loanInput" value="0"><br>
        <button onclick="updateLoan()">Update</button>
    </div>
    <div class="card">
        <img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/savings.png"/><br>
        Goal Saving: <input type="number" id="goalInput" value="10000"><br>
        <button onclick="updateGoal()">Update</button>
    </div>
</div><h2 id="expenses">Daily Expenses</h2>
<div class="flex">
    <div class="expense-card" onclick="addDaily('food')"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/hamburger.png"/><br>Food</div>
    <div class="expense-card" onclick="addDaily('fuel')"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/gas-pump.png"/><br>Fuel</div>
    <div class="expense-card" onclick="addDaily('snacks')"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/popcorn.png"/><br>Snacks</div>
    <div class="expense-card" onclick="addDaily('bills')"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/light-on.png"/><br>Bills</div>
    <div class="expense-card" onclick="addDaily('fun')"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/controller.png"/><br>Fun</div>
</div>

<h2>Expense History</h2>
<table id="dailyTable">
<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
</table>

<h2 id="features">Modern Features</h2>
<div class="flex">
    <div class="feature-card"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/line-chart.png"/><br>Interactive Charts</div>
    <div class="feature-card"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/cloud.png"/><br>Cloud Backup</div>
    <div class="feature-card"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/export.png"/><br>Export CSV</div>
    <div class="feature-card"><img class="icon" src="https://img.icons8.com/ios-filled/50/ffffff/settings.png"/><br>Settings</div>
</div>

<h2 id="analytics">Analytics</h2>
<canvas id="chart" width="400" height="200"></canvas>

<script>
// ===== Data & LocalStorage =====
let dailyData = JSON.parse(localStorage.getItem('dailyData')||'[]');
let salary = parseInt(localStorage.getItem('salary')||50000);
let loan = parseInt(localStorage.getItem('loan')||0);
let goal = parseInt(localStorage.getItem('goal')||10000);

function saveData(){ localStorage.setItem('dailyData', JSON.stringify(dailyData)); }

function updateDashboard(){
    let totalExpense=0;
    dailyData.forEach(d => totalExpense += (d.food+d.fuel+d.snacks+d.bills+d.fun));
    let currentSaving = Math.max(0, salary - loan - totalExpense);
    document.getElementById('currentBalance').innerText = salary - totalExpense;
    document.getElementById('currentSaving').innerText = currentSaving;
    document.getElementById('salaryAmount').innerText = salary;
    document.getElementById('loanAmount').innerText = loan;

    // Table
    const table = document.getElementById('dailyTable');
    table.innerHTML = "<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    dailyData.forEach((d,i)=>{
        const row = table.insertRow();
        row.insertCell(0).innerText=i+1;
        row.insertCell(1).innerText=d.food;
        row.insertCell(2).innerText=d.fuel;
        row.insertCell(3).innerText=d.snacks;
        row.insertCell(4).innerText=d.bills;
        row.insertCell(5).innerText=d.fun;
        row.insertCell(6).innerText=d.food+d.fuel+d.snacks+d.bills+d.fun;
        row.insertCell(7).innerText=d.date;
    });
    updateChart();
}

function addDaily(type){
    const val = parseInt(prompt(`Enter ${type} amount in PKR:`,"0"))||0;
    const today = new Date().toLocaleDateString();
    let lastDay = dailyData[dailyData.length-1];
    if(lastDay && lastDay.date===today){ lastDay[type] += val; }
    else { let newDay = {food:0,fuel:0,snacks:0,bills:0,fun:0,date:today}; newDay[type]=val; dailyData.push(newDay);}
    saveData(); updateDashboard();
}

function updateSalary(){ salary = parseInt(document.getElementById('salaryInput').value)||50000; localStorage.setItem('salary', salary); updateDashboard();}
function updateLoan(){ loan = parseInt(document.getElementById('loanInput').value)||0; localStorage.setItem('loan', loan); updateDashboard();}
function updateGoal(){ goal = parseInt(document.getElementById('goalInput').value)||10000; localStorage.setItem('goal', goal); updateDashboard();}

// Chart
let barChart;
function updateChart(){
    const ctx = document.getElementById('chart').getContext('2d');
    const totalExpense = dailyData.reduce((acc,d)=>acc+(d.food+d.fuel+d.snacks+d.bills+d.fun),0);
    const currentSaving = Math.max(0, salary - loan - totalExpense);
    if(barChart) barChart.destroy();
    barChart = new Chart(ctx,{
        type:'bar',
        data:{
            labels:['Salary','Loan','Total Expense','Current Saving'],
            datasets:[{
                label:'PKR',
                data:[salary,loan,totalExpense,currentSaving],
                backgroundColor:['#00acc1','#ff5252','#ffb74d','#4caf50']
            }]
        },
        options:{responsive:true,plugins:{legend:{display:false}}}
    });
}

updateDashboard();
</script>
</body>
</html>
