<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Traker</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base Styles ===== */
body {
    margin:0;
    padding:0;
    font-family: 'Roboto', sans-serif;
    background: linear-gradient(135deg, #1e3c72, #2a5298);
    color: #fff;
    transition:0.5s;
}
header{
    background: linear-gradient(135deg,#29b6f6,#00acc1);
    padding: 20px;
    text-align: center;
    font-size: 28px;
    font-weight: 700;
    color: #fff;
    text-shadow: 2px 2px 5px rgba(0,0,0,0.3);
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
}
h2{margin:15px 0; text-align:center;}
#datetime{text-align:center;margin:10px 0;color:#ffeb3b;font-weight:700;}

/* ===== Dashboard Boxes ===== */
.dashboard {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin: 20px;
    gap: 15px;
}
.card {
    background: linear-gradient(145deg,#00acc1,#29b6f6);
    width: 140px;
    padding: 20px;
    border-radius: 15px;
    text-align: center;
    font-weight: 700;
    cursor: pointer;
    box-shadow: 0 8px 20px rgba(0,0,0,0.3);
    transition: 0.4s;
}
.card:hover{transform: scale(1.08); box-shadow:0 10px 30px rgba(0,0,0,0.5);}
input{padding:8px;border-radius:10px;border:2px solid #03a9f4;width:120px;margin:3px;}
input:focus{border-color:#0288d1;box-shadow:0 0 8px #0288d1;outline:none;}

/* ===== Info Boxes ===== */
.infoBox{
    display:flex;
    flex-wrap: wrap;
    justify-content:center;
    margin: 15px;
    gap:15px;
}
.infoBox div{
    background: rgba(255,255,255,0.1);
    padding:20px;
    border-radius:15px;
    min-width:150px;
    text-align:center;
    font-weight:bold;
    box-shadow:0 6px 15px rgba(0,0,0,0.3);
}
.infoBox div span{display:block;margin-top:5px;font-size:18px;color:#ffeb3b;}

/* ===== Table ===== */
table{
    width:100%;
    border-collapse: collapse;
    margin:20px 0;
    background: rgba(255,255,255,0.95);
    color:#333;
    border-radius:12px;
    overflow:hidden;
    box-shadow:0 6px 15px rgba(0,0,0,0.2);
}
th, td{padding:10px;text-align:center;border:1px solid #e0e0e0;}
th{background:#0288d1;color:#fff;}
tr:nth-child(even){background:rgba(0,172,193,0.1);}
tr:hover{background:rgba(0,172,193,0.2);}

/* ===== Footer floating shapes ===== */
.floating{
    position:absolute;
    border-radius:50%;
    opacity:0.3;
    animation: float 12s infinite;
    z-index:0;
}
@keyframes float{
    0%{transform:translateY(0) rotate(0deg);}
    50%{transform:translateY(-50px) rotate(180deg);}
    100%{transform:translateY(0) rotate(360deg);}
}

/* ===== Responsive ===== */
@media(max-width:768px){
    .dashboard{flex-direction:column;align-items:center;}
    .infoBox{flex-direction:column;align-items:center;}
}
</style>
</head>
<body>
<header>💼 Pocket Traker 💼</header>
<div id="datetime"></div>

<h2>User Dashboard</h2>
<div class="infoBox">
    <div>Username<br><span id="username">Guest</span></div>
    <div>Salary<br><input type="number" id="salaryInput" value="0" onchange="updateSalary()"></div>
    <div>Loan<br><input type="number" id="loanInput" value="0" onchange="updateLoan()"></div>
    <div>Goal Saving<br><input type="number" id="goalInput" value="10000" onchange="updateGoal()"></div>
</div>

<h2>Summary</h2>
<div class="infoBox">
    <div>Current Balance<br><span id="currentBalance">0</span></div>
    <div>Remaining Balance<br><span id="remainingBalance">0</span></div>
    <div>Current Saving<br><span id="currentSaving">0</span></div>
    <div>Goal Achieved<br><span id="goalPercent">0%</span></div>
</div>

<h2>Daily Expenses</h2>
<div class="dashboard">
    <div class="card" onclick="addDaily('food')">🍔 Food</div>
    <div class="card" onclick="addDaily('fuel')">⛽ Fuel</div>
    <div class="card" onclick="addDaily('snacks')">🍿 Snacks</div>
    <div class="card" onclick="addDaily('bills')">💡 Bills</div>
    <div class="card" onclick="addDaily('fun')">🎮 Fun</div>
    <div class="card" onclick="clearAll()">🗑️ Clear</div>
</div>

<h2>Daily Table</h2>
<table id="dailyTable">
    <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
</table>

<h2>Overview Chart</h2>
<canvas id="chart" width="400" height="200"></canvas>

<!-- Floating Shapes -->
<div class="floating" style="width:15px;height:15px;background:#ffeb3b;top:50px;left:30px;"></div>
<div class="floating" style="width:12px;height:12px;background:#03a9f4;top:150px;left:120px;"></div>
<div class="floating" style="width:18px;height:18px;background:#4caf50;top:300px;left:200px;"></div>
<div class="floating" style="width:10px;height:10px;background:#ff5722;top:400px;left:50px;"></div>

<script>
// ===== Timer =====
function updateDateTime(){
    const now=new Date();
    const options={weekday:'long',year:'numeric',month:'long',day:'numeric'};
    document.getElementById('datetime').innerText = now.toLocaleDateString('en-US',options)+' | '+now.toLocaleTimeString();
}
setInterval(updateDateTime,1000);
updateDateTime();

// ===== Local Storage =====
let username = prompt("Enter your name:","Guest");
document.getElementById('username').innerText=username;

let dailyData = JSON.parse(localStorage.getItem(username+'_data')||"[]");
let salary = parseInt(localStorage.getItem(username+'_salary')||"0");
let loan = parseInt(localStorage.getItem(username+'_loan')||"0");
let goal = parseInt(localStorage.getItem(username+'_goal')||"10000");

document.getElementById('salaryInput').value=salary;
document.getElementById('loanInput').value=loan;
document.getElementById('goalInput').value=goal;

// ===== Functions =====
function saveData(){
    localStorage.setItem(username+'_data', JSON.stringify(dailyData));
    localStorage.setItem(username+'_salary', salary);
    localStorage.setItem(username+'_loan', loan);
    localStorage.setItem(username+'_goal', goal);
}

function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;calculate();saveData();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;calculate();saveData();}
function updateGoal(){goal=parseInt(document.getElementById('goalInput').value)||10000;calculate();saveData();}

function addDaily(type){
    const value = parseInt(prompt(`Enter ${type} amount:`,"0"))||0;
    const today = new Date().toLocaleDateString();
    let dayData = {food:0,fuel:0,snacks:0,bills:0,fun:0,total:0,date:today};
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today){
        dayData = dailyData[dailyData.length-1];
        dayData[type]+=value;
    } else {
        dayData[type]=value;
        dailyData.push(dayData);
    }
    dayData.total = dayData.food+dayData.fuel+dayData.snacks+dayData.bills+dayData.fun;
    saveData();
    calculate();
}

function clearAll(){if(confirm("Clear all data?")){dailyData=[];saveData();calculate();}}

// ===== Calculate & Update =====
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
        row.insertCell(5).innerText=day.fun;
        row.insertCell(6).innerText=day.total;
        row.insertCell(7).innerText=day.date;
        totalExpense+=day.total;
    });
    const remaining = salary - totalExpense;
    const saving = Math.max(0, remaining);
    document.getElementById('currentBalance').innerText=salary;
    document.getElementById('remainingBalance').innerText=remaining;
    document.getElementById('currentSaving').innerText=saving;
    document.getElementById('goalPercent').innerText=Math.min(Math.round(saving/goal*100),100)+'%';
    
    // Chart
    const ctx=document.getElementById('chart').getContext('2d');
    if(window.myChart) window.myChart.destroy();
    window.myChart = new Chart(ctx,{
        type:'bar',
        data:{
            labels:['Salary','Total Expense','Current Saving'],
            datasets:[{
                label:'PKR',
                data:[salary,totalExpense,saving],
                backgroundColor:['#00acc1','#f39c12','#4caf50']
            }]
        },
        options:{
            responsive:true,
            plugins:{legend:{display:false}}
        }
    });
}

calculate();
</script>
</body>
</html>
