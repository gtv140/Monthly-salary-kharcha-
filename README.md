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
    margin: 0;
    padding: 0;
    background: linear-gradient(135deg, #1e3c72, #2a5298);
    color: #fff;
}
header {
    text-align: center;
    padding: 20px;
    font-size: 32px;
    font-weight: 700;
    color: #ffeb3b;
    text-shadow: 2px 2px 5px rgba(0,0,0,0.3);
    background: linear-gradient(135deg, #29b6f6, #00acc1);
}
.container {
    max-width: 1200px;
    margin: 20px auto;
    padding: 10px;
}
.info-boxes {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    margin-bottom: 20px;
}
.info-box {
    background: rgba(255,255,255,0.1);
    backdrop-filter: blur(6px);
    flex: 1;
    min-width: 180px;
    margin: 10px;
    padding: 20px;
    border-radius: 15px;
    text-align: center;
    font-weight: bold;
    cursor: pointer;
    transition: 0.3s;
}
.info-box:hover {
    transform: scale(1.05);
    box-shadow: 0 10px 20px rgba(0,0,0,0.3);
}
input {
    padding: 8px;
    border-radius: 10px;
    border: 2px solid #03a9f4;
    outline: none;
    width: 120px;
    margin-top: 5px;
}
button {
    padding: 8px 15px;
    border: none;
    border-radius: 10px;
    background: #ffeb3b;
    color: #000;
    font-weight: bold;
    cursor: pointer;
    margin-top: 5px;
}
button:hover {
    opacity: 0.85;
}
.dashboard {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}
.card {
    background: rgba(255,255,255,0.1);
    backdrop-filter: blur(5px);
    width: 130px;
    height: 130px;
    margin: 10px;
    border-radius: 15px;
    text-align: center;
    padding: 10px;
    font-weight: bold;
    transition: 0.3s;
    cursor: pointer;
}
.card:hover {
    transform: scale(1.1);
    box-shadow: 0 10px 20px rgba(0,0,0,0.3);
}
.card img {
    width: 50px;
    height: 50px;
    margin-bottom: 5px;
}
table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
    background: rgba(255,255,255,0.9);
    color: #333;
    border-radius: 12px;
    overflow: hidden;
}
th, td {
    padding: 10px;
    text-align: center;
    border: 1px solid #e0e0e0;
}
th {
    background: #0288d1;
    color: #fff;
}
tr:nth-child(even) {
    background: rgba(0,172,193,0.1);
}
tr:hover {
    background: rgba(0,172,193,0.2);
}
#datetime {
    text-align: center;
    margin: 15px 0;
    font-weight: bold;
    font-size: 18px;
    color: #ffeb3b;
}
</style>
</head>
<body>

<header>Pocket Tracker</header>
<div id="datetime"></div>

<div class="container">
    <div class="info-boxes">
        <div class="info-box">Username<br><span id="usernameDisplay">User</span></div>
        <div class="info-box">Salary<br>PKR <input type="number" id="salary" value="0" onchange="calculate()"></div>
        <div class="info-box">Goal Saving<br>PKR <input type="number" id="goal" value="10000" onchange="calculate()"></div>
        <div class="info-box">Loan<br>PKR <input type="number" id="loan" value="0" onchange="calculate()"></div>
    </div>

    <div class="dashboard">
        <div class="card" onclick="addExpense('food')"><img src="https://img.icons8.com/emoji/48/000000/hamburger-emoji.png"/>Food</div>
        <div class="card" onclick="addExpense('fuel')"><img src="https://img.icons8.com/emoji/48/000000/fuel-pump.png"/>Fuel</div>
        <div class="card" onclick="addExpense('snacks')"><img src="https://img.icons8.com/emoji/48/000000/popcorn.png"/>Snacks</div>
        <div class="card" onclick="addExpense('bills')"><img src="https://img.icons8.com/emoji/48/000000/light-bulb.png"/>Bills</div>
        <div class="card" onclick="addExpense('fun')"><img src="https://img.icons8.com/emoji/48/000000/video-game.png"/>Fun</div>
        <div class="card" onclick="clearAll()"><img src="https://img.icons8.com/emoji/48/000000/wastebasket-emoji.png"/>Clear</div>
    </div>

    <h2>Daily Expenses Table</h2>
    <table id="expenseTable">
        <tr>
            <th>Day</th>
            <th>Food</th>
            <th>Fuel</th>
            <th>Snacks</th>
            <th>Bills</th>
            <th>Fun</th>
            <th>Total</th>
            <th>Date</th>
        </tr>
    </table>

    <h2>Overview</h2>
    <div class="info-boxes">
        <div class="info-box">Total Expense<br>PKR <span id="totalExpense">0</span></div>
        <div class="info-box">Remaining Balance<br>PKR <span id="remainingBalance">0</span></div>
        <div class="info-box">Current Saving<br>PKR <span id="currentSaving">0</span></div>
        <div class="info-box">Goal Achieved<br><span id="goalPercent">0%</span></div>
    </div>
</div>

<script>
// Timer
function updateDateTime(){
    const now=new Date();
    document.getElementById('datetime').innerText=now.toLocaleDateString() + ' | ' + now.toLocaleTimeString();
}
setInterval(updateDateTime,1000);
updateDateTime();

// Data
let dailyExpenses = JSON.parse(localStorage.getItem('dailyExpenses')||'[]');
let salary = parseInt(localStorage.getItem('salary'))||0;
let goal = parseInt(localStorage.getItem('goal'))||10000;
let loan = parseInt(localStorage.getItem('loan'))||0;

// Display salary, goal, loan
document.getElementById('salary').value = salary;
document.getElementById('goal').value = goal;
document.getElementById('loan').value = loan;

// Add Expense
function addExpense(type){
    const value = parseInt(prompt('Enter '+type+' expense PKR:','0'))||0;
    const today = new Date().toLocaleDateString();
    let lastEntry = dailyExpenses.length>0? dailyExpenses[dailyExpenses.length-1]: null;
    if(lastEntry && lastEntry.date===today){
        lastEntry[type] += value;
        lastEntry.total = lastEntry.food+lastEntry.fuel+lastEntry.snacks+lastEntry.bills+lastEntry.fun;
    } else {
        const entry = {food:0,fuel:0,snacks:0,bills:0,fun:0,total:0,date:today};
        entry[type]=value;
        entry.total = entry.food+entry.fuel+entry.snacks+entry.bills+entry.fun;
        dailyExpenses.push(entry);
    }
    saveData();
    calculate();
}

// Save Data
function saveData(){
    localStorage.setItem('dailyExpenses', JSON.stringify(dailyExpenses));
    localStorage.setItem('salary', salary);
    localStorage.setItem('goal', goal);
    localStorage.setItem('loan', loan);
}

// Clear All
function clearAll(){
    if(confirm('Delete all data?')){
        dailyExpenses=[];
        saveData();
        calculate();
    }
}

// Calculate Dashboard
function calculate(){
    salary = parseInt(document.getElementById('salary').value)||0;
    goal = parseInt(document.getElementById('goal').value)||10000;
    loan = parseInt(document.getElementById('loan').value)||0;
    localStorage.setItem('salary', salary);
    localStorage.setItem('goal', goal);
    localStorage.setItem('loan', loan);

    const table = document.getElementById('expenseTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";

    let totalExpense = loan;
    dailyExpenses.forEach((d,i)=>{
        const row = table.insertRow();
        row.insertCell(0).innerText = i+1;
        row.insertCell(1).innerText = d.food;
        row.insertCell(2).innerText = d.fuel;
        row.insertCell(3).innerText = d.snacks;
        row.insertCell(4).innerText = d.bills;
        row.insertCell(5).innerText = d.fun;
        row.insertCell(6).innerText = d.total + Math.round(loan/dailyExpenses.length||0);
        row.insertCell(7).innerText = d.date;
        totalExpense += d.total;
    });

    const remaining = salary-totalExpense;
    const currentSaving = Math.max(0, remaining);
    const goalPercent = Math.min(Math.round((currentSaving/goal)*100),100);

    document.getElementById('totalExpense').innerText = totalExpense;
    document.getElementById('remainingBalance').innerText = remaining;
    document.getElementById('currentSaving').innerText = currentSaving;
    document.getElementById('goalPercent').innerText = goalPercent+'%';
}

// Initial calculation
calculate();
</script>

</body>
</html>
