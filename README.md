<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body {
    font-family: 'Roboto', sans-serif;
    margin: 0;
    padding: 0;
    background: linear-gradient(135deg,#1e3c72,#2a5298);
    color: #fff;
}
header {
    background: linear-gradient(135deg,#29b6f6,#00acc1);
    text-align: center;
    padding: 20px;
    font-size: 28px;
    font-weight: bold;
    border-bottom-left-radius: 15px;
    border-bottom-right-radius: 15px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.3);
}
.container {
    max-width: 900px;
    margin: 20px auto;
    padding: 10px;
}
.boxes {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    margin-bottom: 20px;
}
.box {
    background: rgba(255,255,255,0.15);
    backdrop-filter: blur(8px);
    padding: 20px;
    margin: 10px;
    border-radius: 15px;
    flex: 1 1 200px;
    text-align: center;
    font-weight: bold;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}
input {
    padding: 8px;
    border-radius: 10px;
    border: none;
    text-align: center;
    width: 100px;
}
button {
    padding: 8px 12px;
    border: none;
    border-radius: 8px;
    font-weight: bold;
    cursor: pointer;
    margin-top: 5px;
    background-color: #03a9f4;
    color: #fff;
}
button:hover {
    opacity: 0.85;
}
table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
    background: rgba(255,255,255,0.95);
    color: #333;
    border-radius: 10px;
    overflow: hidden;
}
th, td {
    padding: 10px;
    text-align: center;
    border-bottom: 1px solid #ccc;
}
th {
    background: #0288d1;
    color: #fff;
}
@media(max-width:600px){
    .boxes {
        flex-direction: column;
        align-items: center;
    }
}
</style>
</head>
<body>

<header>💼 Pocket Tracker 💼</header>

<div class="container">
    <div class="boxes">
        <div class="box">Username<br><span id="username">User</span></div>
        <div class="box">Salary<br><input type="number" id="salary" value="0" onchange="updateBalance()"></div>
        <div class="box">Loan<br><input type="number" id="loan" value="0" onchange="updateBalance()"></div>
        <div class="box">Goal Saving<br><input type="number" id="goal" value="10000" onchange="updateBalance()"></div>
        <div class="box">Current Balance<br><span id="balance">0</span></div>
        <div class="box">Current Saving<br><span id="saving">0</span></div>
    </div>

    <h3>Daily Expenses</h3>
    <div class="boxes">
        <div class="box" onclick="addExpense('Food')">🍔 Food</div>
        <div class="box" onclick="addExpense('Fuel')">⛽ Fuel</div>
        <div class="box" onclick="addExpense('Snacks')">🍿 Snacks</div>
        <div class="box" onclick="addExpense('Bills')">💡 Bills</div>
        <div class="box" onclick="addExpense('Entertainment')">🎮 Fun</div>
    </div>

    <h3>Daily Expenses Table</h3>
    <table id="expenseTable">
        <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
    </table>

    <h3>Visual Overview</h3>
    <canvas id="chart" width="400" height="200"></canvas>
</div>

<script>
let username = 'User';
let salary = 0;
let loan = 0;
let goal = 10000;
let expenses = [];
let balance = 0;
let saving = 0;

// Load saved data
if(localStorage.getItem('pocketData')){
    let data = JSON.parse(localStorage.getItem('pocketData'));
    username = data.username;
    salary = data.salary;
    loan = data.loan;
    goal = data.goal;
    expenses = data.expenses || [];
}
document.getElementById('username').innerText = username;
document.getElementById('salary').value = salary;
document.getElementById('loan').value = loan;
document.getElementById('goal').value = goal;

function updateBalance(){
    salary = parseInt(document.getElementById('salary').value) || 0;
    loan = parseInt(document.getElementById('loan').value) || 0;
    goal = parseInt(document.getElementById('goal').value) || 10000;
    calculate();
    saveData();
}

function addExpense(type){
    let value = parseInt(prompt(`Enter ${type} amount:`)) || 0;
    let today = new Date().toLocaleDateString();
    let day = expenses.length+1;
    let entry = {day, Food:0,Fuel:0,Snacks:0,Bills:0,Entertainment:0,Date:today};
    entry[type] = value;
    expenses.push(entry);
    calculate();
    saveData();
}

function calculate(){
    balance = salary - loan;
    saving = 0;
    let table = document.getElementById('expenseTable');
    table.innerHTML = "<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    expenses.forEach(e=>{
        let total = e.Food+e.Fuel+e.Snacks+e.Bills+e.Entertainment;
        saving += total;
        balance -= total;
        let row = table.insertRow();
        row.insertCell(0).innerText=e.day;
        row.insertCell(1).innerText=e.Food;
        row.insertCell(2).innerText=e.Fuel;
        row.insertCell(3).innerText=e.Snacks;
        row.insertCell(4).innerText=e.Bills;
        row.insertCell(5).innerText=e.Entertainment;
        row.insertCell(6).innerText=total;
        row.insertCell(7).innerText=e.Date;
    });
    document.getElementById('balance').innerText=balance;
    document.getElementById('saving').innerText=saving;
    drawChart();
}

function saveData(){
    let data = {username,salary,loan,goal,expenses};
    localStorage.setItem('pocketData', JSON.stringify(data));
}

function drawChart(){
    let ctx = document.getElementById('chart').getContext('2d');
    if(window.barChart) window.barChart.destroy();
    window.barChart = new Chart(ctx,{
        type:'bar',
        data:{
            labels:['Salary','Loan','Total Expenses','Balance'],
            datasets:[{
                label:'PKR',
                data:[salary,loan,saving,balance],
                backgroundColor:['#00acc1','#f44336','#ffeb3b','#4caf50']
            }]
        },
        options:{responsive:true,maintainAspectRatio:false}
    });
}

// Initialize
calculate();
</script>

</body>
</html>
