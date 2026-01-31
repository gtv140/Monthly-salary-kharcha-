<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
body{
    font-family:'Roboto',sans-serif;
    margin:0;
    padding:0;
    background:linear-gradient(135deg,#1e3c72,#2a5298);
    color:#fff;
}
header{
    text-align:center;
    padding:20px;
    font-size:28px;
    font-weight:700;
    color:#fff;
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
}
.container{
    max-width:1200px;
    margin:20px auto;
    padding:10px;
}
.infoBox{
    display:flex;
    flex-wrap:wrap;
    justify-content:space-around;
    margin-bottom:20px;
}
.infoBox div{
    flex:1;
    min-width:150px;
    background:linear-gradient(135deg,#29b6f6,#00acc1);
    margin:10px;
    padding:20px;
    border-radius:15px;
    text-align:center;
    font-weight:bold;
    box-shadow:0 8px 20px rgba(0,0,0,0.3);
    transition:0.4s;
}
.infoBox div:hover{
    transform:scale(1.05);
}
.dashboard{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    margin-bottom:20px;
}
.card{
    width:120px;
    margin:10px;
    padding:20px;
    text-align:center;
    border-radius:15px;
    background:linear-gradient(135deg,#f39c12,#f1c40f);
    font-weight:bold;
    cursor:pointer;
    box-shadow:0 6px 15px rgba(0,0,0,0.3);
    transition:0.4s;
}
.card:hover{
    transform:scale(1.08);
}
table{
    width:100%;
    border-collapse:collapse;
    background:rgba(255,255,255,0.95);
    color:#333;
    border-radius:12px;
    overflow:hidden;
    margin-bottom:20px;
}
th,td{
    padding:10px;
    border:1px solid #e0e0e0;
    text-align:center;
}
th{
    background:#0288d1;
    color:#fff;
}
tr:nth-child(even){background:rgba(0,172,193,0.1);}
tr:hover{background:rgba(0,172,193,0.2);}
@media(max-width:768px){
    .infoBox{flex-direction:column;}
    .card{width:45%;}
}
</style>
</head>
<body>
<header>💼 Pocket Tracker 💼</header>
<div class="container">
    <div class="infoBox">
        <div>Username<br><span id="username">User</span></div>
        <div>Salary<br><input type="number" id="salaryInput" value="0" onchange="updateSalary()"></div>
        <div>Goal Saving<br><input type="number" id="goalInput" value="10000" onchange="updateGoal()"></div>
        <div>Loan<br><input type="number" id="loanInput" value="0" onchange="updateLoan()"></div>
    </div>

    <h2 style="text-align:center;">Dashboard Summary</h2>
    <div class="infoBox">
        <div>Total Expense<br><span id="totalExpense">0</span></div>
        <div>Remaining Balance<br><span id="remaining">0</span></div>
        <div>Current Saving<br><span id="currentSaving">0</span></div>
        <div>Goal Achieved<br><span id="goalStatus">0%</span></div>
    </div>

    <h2 style="text-align:center;">Add Daily Expenses</h2>
    <div class="dashboard" id="expenseCards">
        <div class="card" onclick="addDaily('food')">🍔 Food</div>
        <div class="card" onclick="addDaily('fuel')">⛽ Fuel</div>
        <div class="card" onclick="addDaily('snacks')">🍿 Snacks</div>
        <div class="card" onclick="addDaily('bills')">💡 Bills</div>
        <div class="card" onclick="addDaily('fun')">🎮 Fun</div>
        <div class="card" onclick="clearAll()">🗑️ Clear</div>
    </div>

    <h2 style="text-align:center;">Daily Expenses Table</h2>
    <table id="dailyTable">
        <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Daily Total</th><th>Date</th></tr>
    </table>

    <h2 style="text-align:center;">Visual Overview</h2>
    <canvas id="chart" width="400" height="200"></canvas>
</div>

<script>
let username="User";
let dailyData=[],salary=0,goal=10000,loan=0;

window.onload=function(){updateDashboard();};

function addDaily(type){
    let value=parseInt(prompt(`Enter ${type} amount:`,"0"))||0;
    let today={food:0,fuel:0,snacks:0,bills:0,fun:0,total:0,date:new Date().toLocaleDateString()};
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today.date){
        let last=dailyData[dailyData.length-1];
        last[type]+=value;
        last.total=last.food+last.fuel+last.snacks+last.bills+last.fun;
    } else {
        today[type]=value;
        today.total=today.food+today.fuel+today.snacks+today.bills+today.fun;
        dailyData.push(today);
    }
    saveData();
    updateDashboard();
}

function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;saveData();updateDashboard();}
function updateGoal(){goal=parseInt(document.getElementById('goalInput').value)||10000;saveData();updateDashboard();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;saveData();updateDashboard();}

function saveData(){
    let data={username,dailyData,salary,goal,loan};
    localStorage.setItem('pocketTrackerData',JSON.stringify(data));
}

function loadData(){
    let data=JSON.parse(localStorage.getItem('pocketTrackerData'));
    if(data){
        username=data.username;
        dailyData=data.dailyData;
        salary=data.salary;
        goal=data.goal;
        loan=data.loan;
        document.getElementById('salaryInput').value=salary;
        document.getElementById('goalInput').value=goal;
        document.getElementById('loanInput').value=loan;
    }
}

function updateDashboard(){
    loadData();
    document.getElementById('username').innerText=username;
    let totalExpense=loan;
    let table=document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Daily Total</th><th>Date</th></tr>";
    dailyData.forEach((day,index)=>{
        let row=table.insertRow();
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
    let remaining=salary-totalExpense;
    let currentSaving=Math.max(0,remaining);
    let goalPercent=Math.min(Math.round((currentSaving/goal)*100),100);
    document.getElementById('totalExpense').innerText=totalExpense;
    document.getElementById('remaining').innerText=remaining;
    document.getElementById('currentSaving').innerText=currentSaving;
    document.getElementById('goalStatus').innerText=goalPercent+"%";
    drawChart(salary,totalExpense,currentSaving);
}

function drawChart(salary,totalExpense,currentSaving){
    const ctx=document.getElementById('chart').getContext('2d');
    if(window.barChart) window.barChart.destroy();
    window.barChart=new Chart(ctx,{
        type:'bar',
        data:{
            labels:['Salary','Total Expense','Current Saving'],
            datasets:[{
                label:'PKR',
                data:[salary,totalExpense,currentSaving],
                backgroundColor:['#00acc1','#f39c12','#4caf50']
            }]
        },
        options:{responsive:true,plugins:{legend:{display:false}}}
    });
}

function clearAll(){if(confirm("Delete all data?")){dailyData=[];saveData();updateDashboard();}}
</script>
</body>
</html>
