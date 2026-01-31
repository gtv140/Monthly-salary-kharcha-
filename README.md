<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Tracker Ultimate Modern</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body{
    font-family:'Roboto',sans-serif;
    margin:0;padding:0;
    background: linear-gradient(135deg,#1e3c72,#2a5298);
    color:#fff;
    transition:0.5s;
}
header{
    text-align:center;
    font-size:30px;
    font-weight:bold;
    padding:15px;
    background:linear-gradient(135deg,#ff6f61,#de6262);
    border-bottom-left-radius:15px;
    border-bottom-right-radius:15px;
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
}
nav{
    display:flex;justify-content:center;gap:15px;margin:10px 0;
    flex-wrap:wrap;
}
nav button{
    padding:10px 15px;
    border-radius:10px;
    border:none;
    background:rgba(255,255,255,0.2);
    color:#fff;
    font-weight:700;
    cursor:pointer;
    transition:0.3s;
}
nav button:hover{background:rgba(255,255,255,0.4);transform:scale(1.05);}
#themeToggle{
    position:fixed;top:15px;right:15px;padding:10px 15px;background:#0288d1;color:#fff;border-radius:10px;font-weight:bold;cursor:pointer;z-index:1000;
}
.cards{
    display:flex;flex-wrap:wrap;justify-content:center;margin:20px;
}
.card{
    background:rgba(255,255,255,0.1);
    backdrop-filter:blur(10px);
    border-radius:15px;
    margin:10px;
    padding:20px;
    text-align:center;
    width:150px;
    box-shadow:0 8px 20px rgba(0,0,0,0.35);
    transition:0.4s;
    cursor:pointer;
}
.card:hover{transform:scale(1.05);box-shadow:0 10px 30px rgba(0,0,0,0.45);}
.card img{width:50px;height:50px;margin-bottom:10px;}
#dailyTable{
    width:95%;margin:20px auto;border-collapse:collapse;background:rgba(255,255,255,0.95);color:#333;border-radius:12px;overflow:hidden;
}
th,td{border:1px solid #e0e0e0;padding:10px;text-align:center;}
th{background:#0288d1;color:#fff;}
tr:nth-child(even){background:rgba(0,172,193,0.1);}
tr:hover{background:rgba(0,172,193,0.2);}
#chartContainer{max-width:95%;margin:20px auto;padding:10px;background:rgba(255,255,255,0.1);border-radius:15px;backdrop-filter:blur(10px);}
.floating{position:absolute;border-radius:50%;opacity:0.3;animation:float 10s infinite;z-index:0;}
@keyframes float{0%{transform:translateY(0) rotate(0deg);}50%{transform:translateY(-50px) rotate(180deg);}100%{transform:translateY(0) rotate(360deg);}}
@media(max-width:600px){.cards{flex-direction:column;align-items:center;}.card{width:80%;}}
</style>
</head>
<body>
<header>💼 Pocket Tracker Ultimate 💼</header>
<nav>
    <button onclick="showSection('dashboard')">Dashboard</button>
    <button onclick="showSection('add')">Add Expense</button>
    <button onclick="showSection('charts')">Charts</button>
    <button onclick="showSection('tips')">Tips</button>
</nav>
<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<!-- Sections -->
<div id="dashboard">
    <div class="cards">
        <div class="card"><img src="https://img.icons8.com/ios-filled/50/ffffff/user.png"/><br>Username<br><b id="infoUser">User</b></div>
        <div class="card"><img src="https://img.icons8.com/ios-filled/50/ffffff/money.png"/><br>Current Balance<br><b id="currentBalance">0</b></div>
        <div class="card"><img src="https://img.icons8.com/ios-filled/50/ffffff/save.png"/><br>Total Saving<br><b id="totalSaving">0</b></div>
        <div class="card"><img src="https://img.icons8.com/ios-filled/50/ffffff/expenses.png"/><br>Total Expense<br><b id="totalExpense">0</b></div>
    </div>
</div>

<div id="add" style="display:none;">
    <div class="cards">
        <div class="card">Salary<br><input type="number" id="salaryInput" value="0" onchange="updateSalary()"></div>
        <div class="card">Loan<br><input type="number" id="loanInput" value="0" onchange="updateLoan()"></div>
        <div class="card" onclick="addDaily('food')"><img src="https://img.icons8.com/ios-filled/50/ffffff/hamburger.png"/><br>Food</div>
        <div class="card" onclick="addDaily('fuel')"><img src="https://img.icons8.com/ios-filled/50/ffffff/gas-station.png"/><br>Fuel</div>
        <div class="card" onclick="addDaily('snacks')"><img src="https://img.icons8.com/ios-filled/50/ffffff/popcorn.png"/><br>Snacks</div>
        <div class="card" onclick="addDaily('bills')"><img src="https://img.icons8.com/ios-filled/50/ffffff/light-bulb.png"/><br>Bills</div>
        <div class="card" onclick="addDaily('entertainment')"><img src="https://img.icons8.com/ios-filled/50/ffffff/game-controller.png"/><br>Fun</div>
        <div class="card" onclick="clearAll()"><img src="https://img.icons8.com/ios-filled/50/ffffff/delete.png"/><br>Clear All</div>
    </div>
</div>

<div id="charts" style="display:none;">
    <div id="chartContainer">
        <canvas id="chart" width="400" height="200"></canvas>
    </div>
</div>

<div id="tips" style="display:none;padding:10px;text-align:center;">
    <h2>💡 Tips</h2>
    <ul style="list-style:none;padding:0;">
        <li>• Track all small expenses daily.</li>
        <li>• Set monthly goals & salary.</li>
        <li>• Check charts for trends.</li>
        <li>• Use weekends to save & plan.</li>
        <li>• Always check balance before spending.</li>
    </ul>
</div>

<h2 style="text-align:center;">Daily Expenses Table</h2>
<table id="dailyTable">
<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
</table>

<div class="floating" style="width:15px;height:15px;background:#ffeb3b;top:50px;left:30px;"></div>
<div class="floating" style="width:12px;height:12px;background:#03a9f4;top:150px;left:120px;"></div>
<div class="floating" style="width:18px;height:18px;background:#4caf50;top:300px;left:200px;"></div>
<div class="floating" style="width:10px;height:10px;background:#ff5722;top:400px;left:50px;"></div>

<script>
let darkTheme=true;
function toggleTheme(){
    if(darkTheme){document.body.style.background="#fff";document.body.style.color="#333";darkTheme=false;}
    else{document.body.style.background="linear-gradient(135deg,#1e3c72,#2a5298)";document.body.style.color="#fff";darkTheme=true;}
}
function showSection(id){
    ['dashboard','add','charts','tips'].forEach(s=>document.getElementById(s).style.display='none');
    document.getElementById(id).style.display='block';
}
let dailyData=JSON.parse(localStorage.getItem('dailyData')||"[]");
let salary=parseInt(localStorage.getItem('salary'))||0;
let loan=parseInt(localStorage.getItem('loan'))||0;
function updateCards(){
    const totalExpense=dailyData.reduce((a,b)=>a+b.food+b.fuel+b.snacks+b.bills+b.entertainment,0)+loan;
    const currentBalance=Math.max(0,salary-totalExpense);
    const totalSaving=currentBalance;
    document.getElementById('currentBalance').innerText=currentBalance;
    document.getElementById('totalExpense').innerText=totalExpense;
    document.getElementById('totalSaving').innerText=totalSaving;
}
function addDaily(type){
    const value=parseInt(prompt(`Enter ${type} amount:`,"0"))||0;
    const today=new Date().toLocaleDateString();
    let last=dailyData[dailyData.length-1];
    if(last && last.date===today){last[type]=(last[type]||0)+value;}
    else{let newDay={food:0,fuel:0,snacks:0,bills:0,entertainment:0,date:today};newDay[type]=value;dailyData.push(newDay);}
    saveData();renderTable();updateCards();
}
function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;saveData();updateCards();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;saveData();updateCards();}
function clearAll(){if(confirm("Delete all data?")){dailyData=[];saveData();renderTable();updateCards();}}
function saveData(){localStorage.setItem('dailyData',JSON.stringify(dailyData));localStorage.setItem('salary',salary);localStorage.setItem('loan',loan);}
function loadData(){document.getElementById('salaryInput').value=salary;document.getElementById('loanInput').value=loan;updateCards();renderTable();}
function renderTable(){
    const table=document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    dailyData.forEach((day,i)=>{
        const row=table.insertRow();
        row.insertCell(0).innerText=i+1;
        row.insertCell(1).innerText=day.food||0;
        row.insertCell(2).innerText=day.fuel||0;
        row.insertCell(3).innerText=day.snacks||0;
        row.insertCell(4).innerText=day.bills||0;
        row.insertCell(5).innerText=day.entertainment||0;
        row.insertCell(6).innerText=(day.food||0)+(day.fuel||0)+(day.snacks||0)+(day.bills||0)+(day.entertainment||0);
        row.insertCell(7).innerText=day.date;
    });
    renderChart();
}
let chart=null;
function renderChart(){
    const ctx=document.getElementById('chart').getContext('2d');
    const labels=dailyData.map(d=>d.date);
    const data=dailyData.map(d=>(d.food||0)+(d.fuel||0)+(d.snacks||0)+(d.bills||0)+(d.entertainment||0));
    if(chart) chart.destroy();
    chart=new Chart(ctx,{
        type:'bar',
        data:{labels:labels,datasets:[{label:'Daily Expense',data:data,backgroundColor:'rgba(255,99,132,0.6)'}]},
        options:{responsive:true,plugins:{legend:{display:true}}}
    });
}
loadData();
</script>
</body>
</html>
