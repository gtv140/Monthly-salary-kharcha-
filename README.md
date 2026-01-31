<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker Ultimate</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base ===== */
body{margin:0;font-family:'Roboto',sans-serif;background:#f0f4ff;color:#333;}
h1,h2,h3{margin:0 0 10px 0;font-weight:700;}
p{margin:0 0 10px 0;}
.container{width:95%;max-width:600px;margin:0 auto;padding-bottom:50px;}

/* ===== Header ===== */
header{background:linear-gradient(135deg,#1e3c72,#2a5298);color:#fff;padding:15px 0;text-align:center;border-radius:0 0 12px 12px;box-shadow:0 4px 12px rgba(0,0,0,0.2);}
header .logo{font-size:28px;font-weight:700;}

/* ===== Menu ===== */
nav{display:flex;justify-content:center;margin:15px 0;gap:15px;flex-wrap:wrap;}
nav button{padding:10px 15px;border:none;border-radius:8px;background:#ffd700;color:#2a5298;font-weight:600;cursor:pointer;transition:0.3s;}
nav button:hover{opacity:0.85;}

/* ===== Pages ===== */
.page{display:none;}
.page.active{display:block;}

/* ===== Dashboard Boxes ===== */
.dashboard{display:flex;flex-direction:column;gap:15px;margin-bottom:20px;}
.box{background:#fff;padding:20px;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.1);display:flex;justify-content:space-between;align-items:center;transition:0.3s;}
.box:hover{transform:translateY(-5px);box-shadow:0 10px 20px rgba(0,0,0,0.15);}
.box h3{font-size:16px;color:#2a5298;}
.box span{font-size:20px;font-weight:700;}

/* ===== Table ===== */
table{width:100%;border-collapse:collapse;margin-bottom:20px;background:#fff;border-radius:12px;overflow:hidden;box-shadow:0 6px 15px rgba(0,0,0,0.1);}
th,td{padding:10px;text-align:center;border-bottom:1px solid #eee;}
th{background:#2a5298;color:#fff;font-weight:600;}
tr:hover{background:#f0f8ff;}

/* ===== Cards ===== */
.card-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:20px;}
.card{background:#fff;padding:15px;border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.1);text-align:center;transition:0.3s;cursor:pointer;}
.card i{font-size:28px;color:#2a5298;margin-bottom:8px;}
.card:hover{transform:translateY(-4px);box-shadow:0 8px 20px rgba(0,0,0,0.15);}
.card p{font-size:14px;color:#555;}

/* ===== Inputs ===== */
input[type=text],input[type=number]{padding:8px;border-radius:8px;border:1px solid #2a5298;width:calc(100% - 18px);margin:5px 0;outline:none;}
input[type=text]:focus,input[type=number]:focus{border-color:#ffb400;box-shadow:0 0 6px #ffb400;}

/* ===== Buttons ===== */
button.add-btn{padding:8px 12px;background:#2a5298;color:#fff;border:none;border-radius:8px;font-weight:600;cursor:pointer;transition:0.3s;}
button.add-btn:hover{opacity:0.85;}

/* ===== Footer ===== */
footer{background:#2a5298;color:#fff;text-align:center;padding:15px;margin-top:20px;border-radius:12px;}
footer p{margin:5px;font-size:14px;}
</style>
</head>
<body>

<div class="container">
  <!-- Header -->
  <header>
    <div class="logo">Pocket Tracker Ultimate</div>
  </header>

  <!-- Menu -->
  <nav>
    <button onclick="showPage('dashboard')">Dashboard</button>
    <button onclick="showPage('daily')">Daily Report</button>
    <button onclick="showPage('weekly')">Weekly Report</button>
    <button onclick="showPage('settings')">Settings</button>
  </nav>

  <!-- Pages -->
  <div id="usernamePage" class="page active">
    <h2>Welcome!</h2>
    <p>Enter your username to continue:</p>
    <input type="text" id="usernameInput" placeholder="Your Name">
    <button class="add-btn" onclick="enterUsername()">Continue</button>
  </div>

  <div id="dashboard" class="page">
    <h2 style="text-align:center;color:#2a5298;">Dashboard</h2>
    <div class="dashboard">
      <div class="box"><h3>Username</h3><span id="infoUser"></span></div>
      <div class="box"><h3>Current Balance</h3><span id="remaining">0</span> PKR</div>
      <div class="box"><h3>Current Saving</h3><span id="currentSaving">0</span> PKR</div>
      <div class="box"><h3>Total Expense</h3><span id="totalExpense">0</span> PKR</div>
    </div>

    <h2 style="text-align:center;color:#2a5298;">Quick Add Daily Expense</h2>
    <div class="card-grid">
      <div class="card" onclick="addExpense('food')"><i class="fas fa-burger"></i><p>Food</p></div>
      <div class="card" onclick="addExpense('fuel')"><i class="fas fa-gas-pump"></i><p>Fuel</p></div>
      <div class="card" onclick="addExpense('snacks')"><i class="fas fa-cookie-bite"></i><p>Snacks</p></div>
      <div class="card" onclick="addExpense('bills')"><i class="fas fa-lightbulb"></i><p>Bills</p></div>
      <div class="card" onclick="addExpense('entertainment')"><i class="fas fa-gamepad"></i><p>Entertainment</p></div>
    </div>
  </div>

  <div id="daily" class="page">
    <h2 style="text-align:center;color:#2a5298;">Daily Report</h2>
    <table id="dailyTable">
      <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Date</th></tr>
    </table>
  </div>

  <div id="weekly" class="page">
    <h2 style="text-align:center;color:#2a5298;">Weekly Report</h2>
    <table id="weeklyTable">
      <tr><th>Week</th><th>Total Expense</th><th>Remaining Balance</th><th>Saving</th></tr>
    </table>
  </div>

  <div id="settings" class="page">
    <h2 style="text-align:center;color:#2a5298;">Settings</h2>
    <p>Set your salary, loan, and saving goal:</p>
    <input type="number" id="salaryInput" placeholder="Salary"><br>
    <input type="number" id="loanInput" placeholder="Loan Amount"><br>
    <input type="number" id="goalInput" placeholder="Saving Goal"><br>
    <button class="add-btn" onclick="updateSettings()">Save Settings</button>
  </div>

  <!-- Footer -->
  <footer>
    <p>&copy; 2026 Pocket Tracker</p>
    <p>Contact: support@pockettracker.com</p>
  </footer>
</div>

<script>
// ===== Pages =====
function showPage(id){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

// ===== User Data =====
let username='',salary=50000,loanAmount=10000,goal=10000,dailyData=[];

function enterUsername(){
  const val=document.getElementById('usernameInput').value.trim();
  if(val===''){alert('Enter a username!'); return;}
  username=val;
  localStorage.setItem('username',username);
  showPage('dashboard');
  updateDashboard();
}

function updateSettings(){
  salary=parseInt(document.getElementById('salaryInput').value)||salary;
  loanAmount=parseInt(document.getElementById('loanInput').value)||loanAmount;
  goal=parseInt(document.getElementById('goalInput').value)||goal;
  saveData();
  updateDashboard();
}

// ===== Expense =====
function addExpense(type){
  const val=parseInt(prompt(`Enter ${type} amount in PKR:`,"0"))||0;
  const today=new Date().toLocaleDateString();
  let dayEntry=dailyData.find(d=>d.date===today);
  if(!dayEntry){
    dayEntry={food:0,fuel:0,snacks:0,bills:0,entertainment:0,total:0,date:today};
    dailyData.push(dayEntry);
  }
  dayEntry[type]+=val;
  dayEntry.total=dayEntry.food+dayEntry.fuel+dayEntry.snacks+dayEntry.bills+dayEntry.entertainment;
  saveData();
  updateDashboard();
}

// ===== Dashboard =====
function updateDashboard(){
  document.getElementById('infoUser').innerText=username;
  let totalExpense=0;
  dailyData.forEach(d=>{totalExpense+=d.total;});
  const remaining=salary-totalExpense-loanAmount;
  const currentSaving=Math.max(0,remaining);
  document.getElementById('totalExpense').innerText=totalExpense;
  document.getElementById('remaining')?.innerText=remaining;
  document.getElementById('currentSaving')?.innerText=currentSaving;
  updateDailyTable();
  updateWeeklyTable();
}

// ===== Tables =====
function updateDailyTable(){
  const table=document.getElementById('dailyTable');
  if(!table) return;
  table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Date</th></tr>";
  dailyData.forEach((d,i)=>{
    const row=table.insertRow();
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

function updateWeeklyTable(){
  const table=document.getElementById('weeklyTable');
  if(!table) return;
  table.innerHTML="<tr><th>Week</th><th>Total Expense</th><th>Remaining Balance</th><th>Saving</th></tr>";
  const weekMap={};
  dailyData.forEach(d=>{
    const week=new Date(d.date).getWeek();
    if(!weekMap[week]) weekMap[week]={total:0};
    weekMap[week].total+=d.total;
  });
  Object.keys(weekMap).forEach((w,i)=>{
    const row=table.insertRow();
    row.insertCell(0).innerText=i+1;
    row.insertCell(1).innerText=weekMap[w].total;
    const remaining=salary-weekMap[w].total-loanAmount;
    const saving=Math.max(0,remaining);
    row.insertCell(2).innerText=remaining;
    row.insertCell(3).innerText=saving;
  });
}

// ===== Local Storage =====
function saveData(){
  localStorage.setItem('dailyData',JSON.stringify(dailyData));
  localStorage.setItem('salary',salary);
  localStorage.setItem('loanAmount',loanAmount);
  localStorage.setItem('goal',goal);
}

// ===== Load Data =====
window.onload=function(){
  username=localStorage.getItem('username')||'';
  dailyData=JSON.parse(localStorage.getItem('dailyData')||'[]');
  salary=parseInt(localStorage.getItem('salary'))||50000;
  loanAmount=parseInt(localStorage.getItem('loanAmount'))||10000;
  goal=parseInt(localStorage.getItem('goal'))||10000;
  if(username!==''){showPage('dashboard');updateDashboard();}
}

// ===== Helper =====
Date.prototype.getWeek=function(){
  const d=new Date(this.getTime());
  d.setHours(0,0,0,0);
  d.setDate(d.getDate()+4-(d.getDay()||7));
  const yearStart=new Date(d.getFullYear(),0,1);
  const weekNo=Math.ceil((((d-yearStart)/86400000)+1)/7);
  return weekNo;
}
</script>

</body>
</html>
