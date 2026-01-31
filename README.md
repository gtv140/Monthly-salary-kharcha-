<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker - Modern Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body {
    font-family: 'Roboto', sans-serif;
    margin:0; padding:0;
    background: linear-gradient(135deg,#1e3c72,#2a5298);
    color:#fff;
}
header {
    background: linear-gradient(135deg,#29b6f6,#00acc1);
    text-align:center;
    padding:20px;
    font-size:28px;
    font-weight:bold;
    border-bottom-left-radius:15px;
    border-bottom-right-radius:15px;
    box-shadow:0 4px 15px rgba(0,0,0,0.3);
}
.container {
    padding:20px;
    max-width:1200px;
    margin:auto;
}
input, button {
    padding:8px;
    border-radius:10px;
    border:none;
    outline:none;
}
button {
    background:#03a9f4;
    color:#fff;
    font-weight:bold;
    cursor:pointer;
    margin:5px;
}
button:hover {
    opacity:0.85;
}
.cards {
    display:flex;
    flex-wrap:wrap;
    gap:20px;
    justify-content:center;
    margin-top:20px;
}
.card {
    flex:1 1 200px;
    background:linear-gradient(145deg,#00acc1,#29b6f6);
    border-radius:15px;
    padding:20px;
    text-align:center;
    box-shadow:0 8px 20px rgba(0,0,0,0.35);
    transition:0.4s;
    cursor:pointer;
}
.card:hover {
    transform:scale(1.05);
}
.card img {
    width:50px;
    height:50px;
    margin-bottom:10px;
}
.info-boxes {
    display:flex;
    flex-wrap:wrap;
    gap:20px;
    justify-content:center;
    margin-top:20px;
}
.info-box {
    flex:1 1 200px;
    background:rgba(255,255,255,0.1);
    padding:20px;
    border-radius:15px;
    box-shadow:0 8px 20px rgba(0,0,0,0.25);
    text-align:center;
    transition:0.4s;
}
.info-box:hover {
    transform:scale(1.05);
}
table {
    width:100%;
    border-collapse:collapse;
    margin-top:20px;
    background:rgba(255,255,255,0.95);
    color:#333;
    border-radius:12px;
    overflow:hidden;
}
th, td {
    padding:10px;
    text-align:center;
    border:1px solid #e0e0e0;
}
th {
    background:#0288d1;
    color:#fff;
}
tr:nth-child(even) {
    background:rgba(0,172,193,0.1);
}
tr:hover {
    background:rgba(0,172,193,0.2);
}
#usernameInput {
    width:200px;
    margin-right:10px;
}
@media (max-width:768px){
    .cards, .info-boxes {
        flex-direction:column;
        align-items:center;
    }
}
</style>
</head>
<body>

<header>💼 Pocket Tracker 💼</header>
<div class="container">

<!-- Username -->
<div>
    Username: <input type="text" id="usernameInput" placeholder="Enter your name">
    <button onclick="setUsername()">Start</button>
</div>

<!-- Info Boxes -->
<div class="info-boxes">
    <div class="info-box">
        <h3>Current Balance</h3>
        <p id="balance">0 PKR</p>
    </div>
    <div class="info-box">
        <h3>Salary</h3>
        <input type="number" id="salaryInput" value="0" onchange="updateSalary()">
    </div>
    <div class="info-box">
        <h3>Loan</h3>
        <input type="number" id="loanInput" value="0" onchange="updateLoan()">
    </div>
    <div class="info-box">
        <h3>Goal Saving</h3>
        <input type="number" id="goalInput" value="10000" onchange="updateGoal()">
    </div>
</div>

<!-- Cards -->
<div class="cards">
    <div class="card" onclick="addExpense('Food')">
        <img src="https://img.icons8.com/emoji/48/000000/hamburger-emoji.png"/>
        <p>Food</p>
    </div>
    <div class="card" onclick="addExpense('Fuel')">
        <img src="https://img.icons8.com/color/48/000000/gas-station.png"/>
        <p>Fuel</p>
    </div>
    <div class="card" onclick="addExpense('Snacks')">
        <img src="https://img.icons8.com/color/48/000000/snack.png"/>
        <p>Snacks</p>
    </div>
    <div class="card" onclick="addExpense('Bills')">
        <img src="https://img.icons8.com/color/48/000000/light-on.png"/>
        <p>Bills</p>
    </div>
    <div class="card" onclick="addExpense('Entertainment')">
        <img src="https://img.icons8.com/color/48/000000/controller.png"/>
        <p>Fun</p>
    </div>
</div>

<!-- Table -->
<h2>Daily Expenses</h2>
<table id="expenseTable">
    <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
</table>

<!-- Chart -->
<h2>Overview</h2>
<canvas id="expenseChart" width="400" height="200"></canvas>

</div>

<script>
let username = '';
let salary = 0;
let loan = 0;
let goal = 10000;
let dailyExpenses = JSON.parse(localStorage.getItem('dailyExpenses')||"[]");

function setUsername(){
    const input = document.getElementById('usernameInput').value.trim();
    if(input){
        username = input;
        alert(`Welcome ${username}! Start tracking your expenses.`);
        document.getElementById('usernameInput').value = username;
        updateBalance();
        renderTable();
        renderChart();
    }
}

function updateSalary(){
    salary = parseInt(document.getElementById('salaryInput').value)||0;
    updateBalance();
}
function updateLoan(){
    loan = parseInt(document.getElementById('loanInput').value)||0;
    updateBalance();
}
function updateGoal(){
    goal = parseInt(document.getElementById('goalInput').value)||10000;
}

function addExpense(type){
    const amount = parseInt(prompt(`Enter ${type} expense:`,"0"))||0;
    const today = new Date().toLocaleDateString();
    let lastDay = dailyExpenses.length && dailyExpenses[dailyExpenses.length-1].date === today ? dailyExpenses[dailyExpenses.length-1] : null;
    if(lastDay){
        lastDay[type] = (lastDay[type]||0) + amount;
        lastDay.total = calculateTotal(lastDay);
    } else {
        const newDay = {date:today, Food:0,Fuel:0,Snacks:0,Bills:0,Entertainment:0,total:0};
        newDay[type] = amount;
        newDay.total = calculateTotal(newDay);
        dailyExpenses.push(newDay);
    }
    localStorage.setItem('dailyExpenses', JSON.stringify(dailyExpenses));
    updateBalance();
    renderTable();
    renderChart();
}

function calculateTotal(day){
    return (day.Food||0) + (day.Fuel||0) + (day.Snacks||0) + (day.Bills||0) + (day.Entertainment||0);
}

function updateBalance(){
    let totalExpense = dailyExpenses.reduce((sum,d)=>sum+d.total,0) + loan;
    let balance = salary - totalExpense;
    document.getElementById('balance').innerText = balance + " PKR";
}

function renderTable(){
    const table = document.getElementById('expenseTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    dailyExpenses.forEach((d,i)=>{
        const row = table.insertRow();
        row.insertCell(0).innerText = i+1;
        row.insertCell(1).innerText = d.Food||0;
        row.insertCell(2).innerText = d.Fuel||0;
        row.insertCell(3).innerText = d.Snacks||0;
        row.insertCell(4).innerText = d.Bills||0;
        row.insertCell(5).innerText = d.Entertainment||0;
        row.insertCell(6).innerText = d.total;
        row.insertCell(7).innerText = d.date;
    });
}

function renderChart(){
    const ctx = document.getElementById('expenseChart').getContext('2d');
    const lastDay = dailyExpenses[dailyExpenses.length-1]||{Food:0,Fuel:0,Snacks:0,Bills:0,Entertainment:0};
    if(window.myChart) window.myChart.destroy();
    window.myChart = new Chart(ctx,{
        type:'bar',
        data:{
            labels:['Food','Fuel','Snacks','Bills','Fun'],
            datasets:[{
                label:'PKR',
                data:[lastDay.Food||0,lastDay.Fuel||0,lastDay.Snacks||0,lastDay.Bills||0,lastDay.Entertainment||0],
                backgroundColor:['#ff6384','#36a2eb','#ffce56','#4caf50','#ff5722']
            }]
        },
        options:{
            responsive:true,
            plugins:{legend:{display:false}},
            scales:{y:{beginAtZero:true}}
        }
    });
}

updateBalance();
renderTable();
renderChart();
</script>

</body>
</html>
