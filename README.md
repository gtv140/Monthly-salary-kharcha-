<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Traker</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base ===== */
body {
    font-family: 'Roboto', sans-serif;
    margin:0; padding:0;
    background: linear-gradient(135deg,#1e3c72,#2a5298);
    color: #fff;
    transition: 0.5s;
}
h1,h2{text-align:center;margin:10px;}
button{cursor:pointer;font-weight:bold;transition:0.3s;}
button:hover{opacity:0.85;}
input{padding:8px;border-radius:10px;border:2px solid #03a9f4;outline:none;margin:5px;width:150px;}
input:focus{border-color:#0288d1;box-shadow:0 0 8px #0288d1;}

/* ===== Header ===== */
header{
    background: linear-gradient(135deg,#29b6f6,#00acc1);
    padding:15px;
    text-align:center;
    font-size:28px;
    font-weight:bold;
    color:#fff;
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
    border-bottom-left-radius:15px;
    border-bottom-right-radius:15px;
}

/* ===== Username Screen ===== */
#usernameScreen{display:flex;flex-direction:column;align-items:center;margin-top:100px;}
#usernameScreen input{width:200px;text-align:center;}

/* ===== Dashboard ===== */
#dashboard{display:none;padding:20px;}
.dashboardGrid{
    display:grid;
    grid-template-columns: repeat(auto-fit,minmax(180px,1fr));
    gap:20px;
    margin:20px 0;
}
.card{
    background: rgba(255,255,255,0.1);
    backdrop-filter: blur(8px);
    border-radius:15px;
    padding:20px;
    text-align:center;
    font-weight:bold;
    transition:0.3s;
}
.card:hover{transform:scale(1.05);box-shadow:0 8px 20px rgba(0,0,0,0.4);}
.progressBar{
    width: 100%;
    background: rgba(255,255,255,0.2);
    border-radius:10px;
    overflow:hidden;
    margin-top:10px;
}
.progressBar div{
    height:15px;
    background:#ffeb3b;
    width:0%;
    transition: width 0.5s;
}

/* ===== Expense Table ===== */
table{
    border-collapse: collapse;
    width: 100%;
    background: rgba(255,255,255,0.95);
    color:#333;
    border-radius:12px;
    overflow:hidden;
    margin-top:20px;
}
th,td{border:1px solid #e0e0e0;padding:10px;text-align:center;}
th{background:#0288d1;color:#fff;}
tr:nth-child(even){background:rgba(0,172,193,0.1);}
tr:hover{background:rgba(0,172,193,0.2);}

/* ===== Theme Toggle ===== */
#themeToggle{position:fixed;top:15px;right:15px;padding:8px 12px;background:#0288d1;color:#fff;border-radius:8px;box-shadow:0 4px 10px rgba(0,0,0,0.3);}

/* ===== Responsive ===== */
@media(max-width:600px){
    .dashboardGrid{grid-template-columns:1fr;}
}
</style>
</head>
<body>

<header>Pocket Traker</header>
<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<!-- Username Input -->
<div id="usernameScreen">
    <h2>Enter Your Name</h2>
    <input type="text" id="usernameInput" placeholder="Your Name">
    <button onclick="startDashboard()">Start</button>
</div>

<!-- Dashboard -->
<div id="dashboard">
    <h2 id="welcomeUser">Welcome!</h2>
    
    <div class="dashboardGrid">
        <div class="card">Salary<br>PKR <input type="number" id="salaryInput" value="0" onchange="updateSalary()"></div>
        <div class="card">Loan<br>PKR <input type="number" id="loanInput" value="0" onchange="updateLoan()"></div>
        <div class="card">Goal Saving<br>PKR <input type="number" id="goalInput" value="10000" onchange="updateGoal()"></div>
        <div class="card">Remaining Balance<br><span id="remaining">0</span></div>
        <div class="card">Current Saving<br><span id="currentSaving">0</span>
            <div class="progressBar"><div id="goalBar"></div></div>
        </div>
    </div>
    
    <h2>Daily Expenses</h2>
    <div class="dashboardGrid" id="expenseCards">
        <div class="card" onclick="addDaily('food')">🍔 Food</div>
        <div class="card" onclick="addDaily('fuel')">⛽ Fuel</div>
        <div class="card" onclick="addDaily('snacks')">🍿 Snacks</div>
        <div class="card" onclick="addDaily('bills')">💡 Bills</div>
        <div class="card" onclick="addDaily('entertainment')">🎮 Fun</div>
        <div class="card" onclick="clearAll()">🗑️ Clear All</div>
    </div>
    
    <h2>Expenses Table</h2>
    <table id="dailyTable">
        <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
    </table>
    
    <h2>Overview Chart</h2>
    <canvas id="chart" width="400" height="200"></canvas>
</div>

<script>
// ===== Timer & Theme =====
function toggleTheme(){
    document.body.style.background = document.body.style.background.includes('#fff') ? "linear-gradient(135deg,#1e3c72,#2a5298)" : "#fff";
    document.body.style.color = document.body.style.color=="#333" ? "#fff" : "#333";
}

// ===== Username Dashboard =====
let username="";
let dailyData=[],salary=0,loan=0,goal=10000;

function startDashboard(){
    username=document.getElementById('usernameInput').value.trim() || "User";
    document.getElementById('welcomeUser').innerText=`Welcome, ${username}!`;
    document.getElementById('usernameScreen').style.display='none';
    document.getElementById('dashboard').style.display='block';
    calculate();
}

// ===== Add Daily Expense =====
function addDaily(type){
    const value=parseInt(prompt(`Enter ${type} expense in PKR:`,"0"))||0;
    const today={food:0,fuel:0,snacks:0,bills:0,entertainment:0,total:0,date:new Date().toLocaleDateString()};
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today.date){
        let last=dailyData[dailyData.length-1];
        last[type]+=value;
        last.total=last.food+last.fuel+last.snacks+last.bills+last.entertainment;
    } else {
        today[type]=value;
        today.total=today.food+today.fuel+today.snacks+today.bills+today.entertainment;
        dailyData.push(today);
    }
    calculate();
}

// ===== Update Salary, Goal, Loan =====
function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;calculate();}
function updateGoal(){goal=parseInt(document.getElementById('goalInput').value)||10000;calculate();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;calculate();}

// ===== Clear All =====
function clearAll(){if(confirm("Delete all data?")){dailyData=[];calculate();}}

// ===== Calculate & Update =====
function calculate(){
    // Table
    const table=document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    let totalExpense=loan;
    dailyData.forEach((day,index)=>{
        const row=table.insertRow();
        row.insertCell(0).innerText=index+1;
        row.insertCell(1).innerText=day.food;
        row.insertCell(2).innerText=day.fuel;
        row.insertCell(3).innerText=day.snacks;
        row.insertCell(4).innerText=day.bills;
        row.insertCell(5).innerText=day.entertainment;
        row.insertCell(6).innerText=day.total;
        row.insertCell(7).innerText=day.date;
        totalExpense+=day.total;
    });

    const remaining=salary-totalExpense;
    const currentSaving=Math.max(0,remaining);
    document.getElementById('remaining').innerText=remaining;
    document.getElementById('currentSaving').innerText=currentSaving;
    
    const goalPercent=Math.min(Math.round((currentSaving/goal)*100),100);
    document.getElementById('goalBar').style.width=goalPercent+'%';

    // Chart
    const ctx=document.getElementById('chart').getContext('2d');
    const chartData=[salary,totalExpense,currentSaving];
    if(window.barChart) window.barChart.destroy();
    window.barChart=new Chart(ctx,{type:'bar',data:{labels:['Salary','Expense','Saving'],datasets:[{label:'PKR',data:chartData,backgroundColor:['#00acc1','#f39c12','#4caf50']}]},options:{responsive:true}});
}
</script>
</body>
</html>
