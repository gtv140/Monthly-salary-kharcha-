<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Traker</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body {
    font-family:'Roboto',sans-serif;
    margin:0; padding:0;
    background: linear-gradient(135deg,#1e3c72,#2a5298);
    color:#fff;
}
header{
    background: linear-gradient(135deg,#29b6f6,#00acc1);
    padding:15px; text-align:center;
    font-size:28px; font-weight:bold;
    color:#fff; text-shadow:2px 2px 5px rgba(0,0,0,0.3);
    border-bottom-left-radius:15px; border-bottom-right-radius:15px;
}
button{cursor:pointer;font-weight:bold;transition:0.3s;margin:3px;}
button:hover{opacity:0.85;}
input{padding:8px;border-radius:10px;border:2px solid #03a9f4;outline:none;margin:5px;width:150px;}
input:focus{border-color:#0288d1;box-shadow:0 0 8px #0288d1;}
#themeToggle{position:fixed;top:15px;right:15px;padding:8px 12px;background:#0288d1;color:#fff;border-radius:8px;}
#usernameScreen{display:flex;flex-direction:column;align-items:center;margin-top:50px;}
#dashboard{display:none;padding:20px;}
.dashboardGrid{
    display:grid;
    grid-template-columns: repeat(auto-fit,minmax(150px,1fr));
    gap:15px;
    margin:15px 0;
}
.card{
    background: rgba(255,255,255,0.1); backdrop-filter: blur(8px);
    border-radius:15px; padding:15px; text-align:center; font-weight:bold;
    transition:0.3s;
}
.card:hover{transform:scale(1.05);box-shadow:0 8px 20px rgba(0,0,0,0.4);}
.progressBar{width:100%;background:rgba(255,255,255,0.2);border-radius:10px;overflow:hidden;margin-top:10px;}
.progressBar div{height:15px;background:#ffeb3b;width:0%;transition:width 0.5s;}
table{border-collapse: collapse;width:100%;background: rgba(255,255,255,0.95);color:#333;border-radius:12px;overflow:hidden;margin-top:15px;}
th,td{border:1px solid #e0e0e0;padding:8px;text-align:center;}
th{background:#0288d1;color:#fff;}
tr:nth-child(even){background:rgba(0,172,193,0.1);}
tr:hover{background:rgba(0,172,193,0.2);}
@media(max-width:600px){
    .dashboardGrid{grid-template-columns:1fr;}
    input{width:100px;}
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
// ===== Theme Toggle =====
function toggleTheme(){
    document.body.style.background = document.body.style.background.includes('#fff') ? "linear-gradient(135deg,#1e3c72,#2a5298)" : "#fff";
    document.body.style.color = document.body.style.color=="#333" ? "#fff" : "#333";
}

// ===== Local Storage Variables =====
let username="", dailyData=[], salary=0, loan=0, goal=10000;

// ===== Load from LocalStorage =====
window.onload=function(){
    if(localStorage.getItem('username')){
        username=localStorage.getItem('username');
        dailyData=JSON.parse(localStorage.getItem('dailyData')||"[]");
        salary=parseInt(localStorage.getItem('salary')||0);
        loan=parseInt(localStorage.getItem('loan')||0);
        goal=parseInt(localStorage.getItem('goal')||10000);
        startDashboard();
    }
}

// ===== Start Dashboard =====
function startDashboard(){
    username=document.getElementById('usernameInput').value.trim() || username || "User";
    localStorage.setItem('username',username);
    document.getElementById('welcomeUser').innerText=`Welcome, ${username}!`;
    document.getElementById('usernameScreen').style.display='none';
    document.getElementById('dashboard').style.display='block';
    document.getElementById('salaryInput').value=salary;
    document.getElementById('loanInput').value=loan;
    document.getElementById('goalInput').value=goal;
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
    saveData();
    calculate();
}

// ===== Update Salary, Loan, Goal =====
function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;saveData();calculate();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;saveData();calculate();}
function updateGoal(){goal=parseInt(document.getElementById('goalInput').value)||10000;saveData();calculate();}

// ===== Save to LocalStorage =====
function saveData(){
    localStorage.setItem('dailyData',JSON.stringify(dailyData));
    localStorage.setItem('salary',salary);
    localStorage.setItem('loan',loan);
    localStorage.setItem('goal',goal);
}

// ===== Clear All =====
function clearAll(){if(confirm("Delete all data?")){dailyData=[];saveData();calculate();}}

// ===== Calculate Dashboard =====
function calculate(){
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
