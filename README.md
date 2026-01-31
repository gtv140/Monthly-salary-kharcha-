<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Traker</title>
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
    font-size:32px;
    font-weight:bold;
    color:#ffeb3b;
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
}
#datetime{
    text-align:center;
    margin-bottom:15px;
    font-size:18px;
    color:#ffeb3b;
}
input,button{
    padding:8px 12px;
    border-radius:8px;
    border:none;
    outline:none;
    margin:5px;
}
button{
    font-weight:bold;
    cursor:pointer;
    transition:0.3s;
}
button:hover{
    opacity:0.85;
}

/* ===== Dashboard Boxes ===== */
.container{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    margin:20px;
}
.box{
    background:rgba(255,255,255,0.1);
    backdrop-filter:blur(8px);
    padding:20px;
    margin:10px;
    border-radius:15px;
    width:180px;
    text-align:center;
    box-shadow:0 8px 20px rgba(0,0,0,0.3);
    transition:0.4s;
}
.box:hover{
    transform:scale(1.05);
}

/* ===== Progress Bars ===== */
.progress-container{
    background:rgba(255,255,255,0.1);
    border-radius:10px;
    margin-top:5px;
    width:100%;
    height:15px;
    overflow:hidden;
}
.progress-bar{
    height:100%;
    background:#ffeb3b;
    width:0%;
    transition:0.5s;
}

/* ===== Expense Cards ===== */
.cards{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    margin:20px 0;
}
.card{
    background:linear-gradient(145deg,#00acc1,#29b6f6);
    padding:20px;
    margin:10px;
    border-radius:15px;
    text-align:center;
    width:130px;
    cursor:pointer;
    transition:0.4s;
}
.card:hover{
    transform:scale(1.08);
}

/* ===== Table ===== */
table{
    width:90%;
    margin:20px auto;
    border-collapse: collapse;
    background:rgba(255,255,255,0.9);
    color:#333;
    border-radius:10px;
    overflow:hidden;
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
</style>
</head>
<body>

<header>Pocket Traker</header>
<div id="datetime"></div>

<!-- Username Input -->
<div style="text-align:center;margin-bottom:20px;">
    Username: <input type="text" id="usernameInput" placeholder="Your Name">
    <button onclick="setUsername()">Enter</button>
</div>

<!-- Dashboard -->
<div id="dashboard" style="display:none;">
    <div class="container">
        <div class="box">Username<br><span id="userDisplay">-</span></div>
        <div class="box">Salary (PKR)<br><input type="number" id="salaryInput" value="0" onchange="updateData()"></div>
        <div class="box">Loan (PKR)<br><input type="number" id="loanInput" value="0" onchange="updateData()"></div>
        <div class="box">Goal Saving (PKR)<br><input type="number" id="goalInput" value="10000" onchange="updateData()"></div>
    </div>

    <div class="container">
        <div class="box">Current Balance<br><span id="balance">0</span></div>
        <div class="box">Current Saving<br><span id="saving">0</span></div>
        <div class="box">Goal Progress<br>
            <div class="progress-container">
                <div class="progress-bar" id="goalBar"></div>
            </div>
        </div>
    </div>

    <h2 style="text-align:center;">Add Daily Expense</h2>
    <div class="cards">
        <div class="card" onclick="addExpense('Food')">🍔 Food</div>
        <div class="card" onclick="addExpense('Fuel')">⛽ Fuel</div>
        <div class="card" onclick="addExpense('Snacks')">🍿 Snacks</div>
        <div class="card" onclick="addExpense('Bills')">💡 Bills</div>
        <div class="card" onclick="addExpense('Fun')">🎮 Fun</div>
        <div class="card" onclick="clearData()">🗑️ Clear</div>
    </div>

    <h2 style="text-align:center;">Daily Expenses Table</h2>
    <table id="expenseTable">
        <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
    </table>
</div>

<script>
// Timer
function updateDateTime(){
    const now=new Date();
    document.getElementById('datetime').innerText=now.toLocaleDateString() + " | " + now.toLocaleTimeString();
}
setInterval(updateDateTime,1000);

// Local Storage Variables
let username="", salary=0, loan=0, goal=10000;
let expenses=[];

// Set Username & Show Dashboard
function setUsername(){
    username=document.getElementById('usernameInput').value.trim();
    if(username=="") return alert("Enter a username");
    localStorage.setItem('username',username);
    document.getElementById('userDisplay').innerText=username;
    document.getElementById('dashboard').style.display='block';
    loadData();
}

// Load Data from Local Storage
function loadData(){
    salary=Number(localStorage.getItem('salary')||0);
    loan=Number(localStorage.getItem('loan')||0);
    goal=Number(localStorage.getItem('goal')||10000);
    expenses=JSON.parse(localStorage.getItem('expenses')||"[]");
    document.getElementById('salaryInput').value=salary;
    document.getElementById('loanInput').value=loan;
    document.getElementById('goalInput').value=goal;
    updateData();
}

// Update Dashboard
function updateData(){
    salary=Number(document.getElementById('salaryInput').value)||0;
    loan=Number(document.getElementById('loanInput').value)||0;
    goal=Number(document.getElementById('goalInput').value)||10000;
    localStorage.setItem('salary',salary);
    localStorage.setItem('loan',loan);
    localStorage.setItem('goal',goal);
    calculate();
}

// Add Expense
function addExpense(type){
    let value=Number(prompt(`Enter ${type} amount in PKR:`)||0);
    const today=new Date().toLocaleDateString();
    let dayData={Food:0,Fuel:0,Snacks:0,Bills:0,Fun:0,Total:0,Date:today};
    if(expenses.length>0 && expenses[expenses.length-1].Date===today){
        expenses[expenses.length-1][type]+=value;
    } else {
        dayData[type]=value;
        expenses.push(dayData);
    }
    saveExpenses();
    calculate();
}

// Save Expenses
function saveExpenses(){
    expenses.forEach(e=>{
        e.Total=e.Food+e.Fuel+e.Snacks+e.Bills+e.Fun;
    });
    localStorage.setItem('expenses',JSON.stringify(expenses));
}

// Clear Data
function clearData(){
    if(confirm("Delete all data?")){
        expenses=[];
        localStorage.setItem('expenses',JSON.stringify(expenses));
        calculate();
    }
}

// Calculate & Update Dashboard
function calculate(){
    let totalExpense=expenses.reduce((a,b)=>a+b.Total,0)+loan;
    let balance=salary-totalExpense;
    let saving=Math.max(0,balance);
    let goalPercent=Math.min(Math.round((saving/goal)*100),100);
    document.getElementById('balance').innerText=balance;
    document.getElementById('saving').innerText=saving;
    document.getElementById('goalBar').style.width=goalPercent+"%";

    // Table
    const table=document.getElementById('expenseTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    expenses.forEach((day,index)=>{
        let row=table.insertRow();
        row.insertCell(0).innerText=index+1;
        row.insertCell(1).innerText=day.Food;
        row.insertCell(2).innerText=day.Fuel;
        row.insertCell(3).innerText=day.Snacks;
        row.insertCell(4).innerText=day.Bills;
        row.insertCell(5).innerText=day.Fun;
        row.insertCell(6).innerText=day.Total;
        row.insertCell(7).innerText=day.Date;
    });
}
</script>

</body>
</html>
