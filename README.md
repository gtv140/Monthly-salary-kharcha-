<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Traker</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base ===== */
body {
    margin:0; padding:0; font-family: 'Roboto', sans-serif;
    background: linear-gradient(135deg,#1e3c72,#2a5298);
    color:#fff; overflow-x:hidden;
}
header {
    text-align:center; padding:20px;
    font-size:32px; font-weight:700;
    color:#fff; text-shadow:2px 2px 5px rgba(0,0,0,0.3);
    background: linear-gradient(135deg,#ff416c,#ff4b2b);
}
h2 { text-align:center; margin:15px 0; font-weight:700; }
button { cursor:pointer; font-weight:700; border:none; border-radius:10px; padding:10px 15px; transition:0.3s;}
button:hover { opacity:0.85; }
input { padding:8px; border-radius:10px; border:2px solid #03a9f4; outline:none; width:120px; margin:5px;}
input:focus { border-color:#0288d1; box-shadow:0 0 8px #0288d1; }

/* ===== Dashboard ===== */
.container { max-width:1200px; margin:0 auto; padding:10px;}
.info-boxes { display:flex; flex-wrap:wrap; justify-content:space-around; margin:20px 0;}
.info-box { flex:1; min-width:180px; margin:10px; padding:20px; border-radius:15px; text-align:center;
    background: rgba(255,255,255,0.1); backdrop-filter:blur(8px); box-shadow:0 8px 20px rgba(0,0,0,0.25);
    transition:0.3s; font-weight:bold;
}
.info-box:hover { transform:scale(1.05); }
.cards { display:flex; flex-wrap:wrap; justify-content:center; margin:20px 0;}
.card { width:130px; margin:10px; padding:15px; border-radius:15px; text-align:center;
    background: linear-gradient(135deg,#00c6ff,#0072ff); box-shadow:0 8px 20px rgba(0,0,0,0.3);
    cursor:pointer; transition:0.3s;
}
.card img { width:50px; height:50px; margin-bottom:5px;}
.card:hover { transform:scale(1.08); }

/* ===== Table ===== */
table { width:100%; border-collapse:collapse; margin:20px 0; background: rgba(255,255,255,0.95); color:#333; border-radius:10px; overflow:hidden; box-shadow:0 6px 15px rgba(0,0,0,0.2);}
th,td { padding:10px; text-align:center; border:1px solid #e0e0e0; }
th { background:#0288d1; color:#fff; }
tr:nth-child(even){ background:rgba(0,172,193,0.1); }
tr:hover { background:rgba(0,172,193,0.2); }

/* ===== Responsive ===== */
@media(max-width:768px){
    .info-boxes, .cards { flex-direction:column; align-items:center; }
    .info-box, .card { width:80%; }
}
</style>
</head>
<body>
<header>💼 Pocket Traker 💼</header>
<div class="container">
<h2>User Dashboard</h2>

<!-- Info Boxes -->
<div class="info-boxes">
<div class="info-box">Username<br><span id="userName">User1</span></div>
<div class="info-box">Salary<br><input type="number" id="salary" value="50000" onchange="calculate()"></div>
<div class="info-box">Loan<br><input type="number" id="loan" value="10000" onchange="calculate()"></div>
<div class="info-box">Goal Saving<br><input type="number" id="goal" value="10000" onchange="calculate()"></div>
<div class="info-box">Remaining Balance<br><span id="balance">0</span></div>
<div class="info-box">Current Saving<br><span id="saving">0</span></div>
</div>

<!-- Daily Expense Cards -->
<h2>Daily Expenses</h2>
<div class="cards">
<div class="card" onclick="addExpense('Food')">
<img src="https://cdn-icons-png.flaticon.com/512/3075/3075977.png" alt="Food">
<br>Food
</div>
<div class="card" onclick="addExpense('Fuel')">
<img src="https://cdn-icons-png.flaticon.com/512/633/633759.png" alt="Fuel">
<br>Fuel
</div>
<div class="card" onclick="addExpense('Snacks')">
<img src="https://cdn-icons-png.flaticon.com/512/3075/3075971.png" alt="Snacks">
<br>Snacks
</div>
<div class="card" onclick="addExpense('Bills')">
<img src="https://cdn-icons-png.flaticon.com/512/414/414974.png" alt="Bills">
<br>Bills
</div>
<div class="card" onclick="addExpense('Fun')">
<img src="https://cdn-icons-png.flaticon.com/512/833/833314.png" alt="Fun">
<br>Fun
</div>
</div>

<!-- Expense Table -->
<h2>Expense History</h2>
<table id="expenseTable">
<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
</table>

<!-- Chart -->
<h2>Visual Overview</h2>
<canvas id="expenseChart" width="400" height="200"></canvas>
</div>

<script>
// ===== Data =====
let dayCount=0;
let expenses = [];
let salaryInput=document.getElementById('salary');
let loanInput=document.getElementById('loan');
let goalInput=document.getElementById('goal');

// ===== Functions =====
function addExpense(type){
    const val=parseInt(prompt(`Enter ${type} expense in PKR:`,"0"))||0;
    const date=new Date().toLocaleDateString();
    if(expenses.length>0 && expenses[expenses.length-1].date===date){
        expenses[expenses.length-1][type]+=val;
    } else {
        let newDay={Day:++dayCount,Food:0,Fuel:0,Snacks:0,Bills:0,Fun:0,date:date};
        newDay[type]=val;
        expenses.push(newDay);
    }
    updateTable();
    calculate();
}

function updateTable(){
    const table=document.getElementById('expenseTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    expenses.forEach(d=>{
        const row=table.insertRow();
        row.insertCell(0).innerText=d.Day;
        row.insertCell(1).innerText=d.Food;
        row.insertCell(2).innerText=d.Fuel;
        row.insertCell(3).innerText=d.Snacks;
        row.insertCell(4).innerText=d.Bills;
        row.insertCell(5).innerText=d.Fun;
        row.insertCell(6).innerText=d.Food+d.Fuel+d.Snacks+d.Bills+d.Fun;
        row.insertCell(7).innerText=d.date;
    });
}

function calculate(){
    let totalExpense=expenses.reduce((acc,d)=>acc+d.Food+d.Fuel+d.Snacks+d.Bills+d.Fun,0)+parseInt(loanInput.value);
    let remaining=parseInt(salaryInput.value)-totalExpense;
    let saving=Math.max(0,remaining);
    document.getElementById('balance').innerText=remaining;
    document.getElementById('saving').innerText=saving;
    updateChart();
}

// ===== Chart =====
let ctx=document.getElementById('expenseChart').getContext('2d');
let chart;
function updateChart(){
    let labels=expenses.map(d=>"Day "+d.Day);
    let data=expenses.map(d=>d.Food+d.Fuel+d.Snacks+d.Bills+d.Fun);
    if(chart) chart.destroy();
    chart=new Chart(ctx,{
        type:'bar',
        data:{
            labels:labels,
            datasets:[{
                label:'Daily Expense PKR',
                data:data,
                backgroundColor:'rgba(255,99,132,0.6)'
            }]
        },
        options:{ responsive:true, maintainAspectRatio:false }
    });
}

// ===== Local Storage =====
window.onload=function(){
    if(localStorage.getItem('expenses')){ expenses=JSON.parse(localStorage.getItem('expenses')); dayCount=expenses.length; updateTable(); calculate(); }
}

window.onbeforeunload=function(){ localStorage.setItem('expenses',JSON.stringify(expenses)); }
</script>
</body>
</html>
