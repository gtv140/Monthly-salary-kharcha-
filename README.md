<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker - Ultimate Modern</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base ===== */
*{box-sizing:border-box;margin:0;padding:0;font-family:'Roboto',sans-serif;}
body{background:#f5f6fa;color:#2f3640;line-height:1.5;}
a{text-decoration:none;color:inherit;}

/* ===== Header / Hero ===== */
header{position:relative;background:linear-gradient(135deg,#6a11cb,#2575fc);color:#fff;padding:40px 20px;text-align:center;font-size:32px;font-weight:700;text-shadow:1px 1px 6px rgba(0,0,0,0.3);border-bottom-left-radius:15px;border-bottom-right-radius:15px;overflow:hidden;}
header::before{content:"";position:absolute;top:0;left:0;width:100%;height:100%;background:url('https://images.unsplash.com/photo-1612831455545-d1d3f165e06d?auto=format&fit=crop&w=1500&q=80') center/cover no-repeat;opacity:0.2;border-bottom-left-radius:15px;border-bottom-right-radius:15px;}

/* ===== Container ===== */
.container{max-width:1200px;margin:20px auto;padding:15px;}

/* ===== Info Boxes ===== */
.infoBoxes{display:flex;flex-wrap:wrap;gap:15px;margin-bottom:20px;}
.box{flex:1;min-width:180px;background:linear-gradient(145deg,#ff9a9e,#fad0c4);color:#fff;padding:20px;border-radius:15px;box-shadow:0 8px 15px rgba(0,0,0,0.2);transition:0.3s;cursor:pointer;text-align:center;font-weight:700;position:relative;overflow:hidden;}
.box:hover{transform:translateY(-5px);box-shadow:0 12px 20px rgba(0,0,0,0.3);}
.box span{display:block;font-size:24px;margin-top:10px;}

/* Box animations */
.box::after{content:"";position:absolute;top:-50%;left:-50%;width:200%;height:200%;background:rgba(255,255,255,0.1);transform:rotate(45deg);transition:0.5s;}
.box:hover::after{top:0;left:0;}

/* ===== Buttons ===== */
button{padding:10px 20px;border:none;border-radius:10px;background:#2575fc;color:#fff;font-weight:700;cursor:pointer;transition:0.3s;margin:5px;}
button:hover{opacity:0.85;}

/* ===== Table ===== */
table{width:100%;border-collapse:collapse;margin-top:20px;border-radius:10px;overflow:hidden;box-shadow:0 6px 15px rgba(0,0,0,0.1);}
th,td{text-align:center;padding:12px;border-bottom:1px solid #ddd;}
th{background:#2575fc;color:#fff;letter-spacing:1px;}
tr:hover{background:rgba(37,117,252,0.1);}

/* ===== Inputs ===== */
input[type="number"], input[type="text"]{padding:8px 12px;border-radius:10px;border:2px solid #2575fc;outline:none;margin:5px;width:120px;transition:0.3s;}
input:focus{border-color:#6a11cb;box-shadow:0 0 8px #6a11cb;}

/* ===== Tips ===== */
.tipBox{background:linear-gradient(135deg,#ffecd2,#fcb69f);padding:15px;margin:15px 0;border-radius:12px;color:#333;font-weight:500;box-shadow:0 6px 12px rgba(0,0,0,0.15);}
.tipBox strong{color:#2575fc;}

/* ===== Hero Images / Banner Section ===== */
.heroImages{display:flex;overflow-x:auto;gap:15px;margin:20px 0;}
.heroImages img{min-width:300px;height:180px;object-fit:cover;border-radius:15px;box-shadow:0 8px 15px rgba(0,0,0,0.2);transition:0.3s;cursor:pointer;}
.heroImages img:hover{transform:scale(1.05);}

/* ===== Responsive ===== */
@media(max-width:768px){.infoBoxes{flex-direction:column;} .heroImages img{min-width:250px;}}
</style>
</head>
<body>

<header>💼 Pocket Tracker 💼</header>
<div class="container">

<!-- Hero Images / Modern Visuals -->
<div class="heroImages">
<img src="https://images.unsplash.com/photo-1603791440384-56cd371ee9a7?auto=format&fit=crop&w=800&q=80" alt="Budget Planning">
<img src="https://images.unsplash.com/photo-1581093588401-2e4f133ee625?auto=format&fit=crop&w=800&q=80" alt="Expense Tracking">
<img src="https://images.unsplash.com/photo-1600891964599-f61ba0e24092?auto=format&fit=crop&w=800&q=80" alt="Savings">
</div>

<!-- User Info Boxes -->
<div class="infoBoxes">
<div class="box" onclick="updateSalaryPrompt()">💰 Salary <span id="salaryDisplay">0</span> PKR</div>
<div class="box" onclick="updateLoanPrompt()">💳 Loan <span id="loanDisplay">0</span> PKR</div>
<div class="box" onclick="updateBalancePrompt()">💸 Current Balance <span id="balanceDisplay">0</span> PKR</div>
<div class="box">💾 Total Saving <span id="savingDisplay">0</span> PKR</div>
</div>

<!-- Expense Buttons -->
<div class="infoBoxes">
<div class="box" onclick="addExpense('Food')">🍔 Food</div>
<div class="box" onclick="addExpense('Fuel')">⛽ Fuel</div>
<div class="box" onclick="addExpense('Snacks')">🍿 Snacks</div>
<div class="box" onclick="addExpense('Bills')">💡 Bills</div>
<div class="box" onclick="addExpense('Entertainment')">🎮 Fun</div>
<div class="box" onclick="clearExpenses()">🗑️ Clear All</div>
</div>

<!-- Daily Expense Table -->
<h2>Daily Expenses</h2>
<table id="expenseTable">
<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th></tr>
</table>

<!-- Chart -->
<h2>Visual Overview</h2>
<canvas id="expenseChart" height="200"></canvas>

<!-- User Tips -->
<div class="tipBox">
💡 <strong>Tips:</strong> Track every expense daily, keep balance under control, save regularly, and review weekly.
</div>
<div class="tipBox">
📱 Mobile friendly! Tap boxes to update salary, loan, or add expenses. Interactive charts & colourful UI.
</div>
<div class="tipBox">
✨ Hero banners above give modern feel like top web platforms. Scroll horizontally to see visuals.
</div>
</div>

<script>
// ===== Local Storage Data =====
let userData = JSON.parse(localStorage.getItem('pocketUser')||'{"salary":0,"loan":0,"balance":0,"expenses":[],"saving":0}');
updateDisplay();

// ===== Functions =====
function updateDisplay(){
    document.getElementById('salaryDisplay').innerText=userData.salary;
    document.getElementById('loanDisplay').innerText=userData.loan;
    document.getElementById('balanceDisplay').innerText=userData.balance;
    document.getElementById('savingDisplay').innerText=userData.saving;
    updateTable();
    updateChart();
}

function updateSalaryPrompt(){
    let val = parseInt(prompt("Enter Salary:",userData.salary))||0;
    userData.salary = val;
    userData.balance = val - userData.loan - totalExpenses();
    userData.saving = userData.balance;
    saveData();
}

function updateLoanPrompt(){
    let val = parseInt(prompt("Enter Loan Amount:",userData.loan))||0;
    userData.loan = val;
    userData.balance = userData.salary - userData.loan - totalExpenses();
    userData.saving = userData.balance;
    saveData();
}

function updateBalancePrompt(){
    let val = parseInt(prompt("Enter Current Balance:",userData.balance))||0;
    userData.balance = val;
    saveData();
}

function addExpense(type){
    let val = parseInt(prompt(`Enter ${type} expense:`,"0"))||0;
    let today = new Date().toLocaleDateString();
    let dayData = userData.expenses.find(e=>e.date===today);
    if(!dayData){
        dayData = {date:today,Food:0,Fuel:0,Snacks:0,Bills:0,Entertainment:0,Total:0};
        userData.expenses.push(dayData);
    }
    dayData[type] += val;
    dayData.Total = dayData.Food + dayData.Fuel + dayData.Snacks + dayData.Bills + dayData.Entertainment;
    userData.balance = userData.salary - userData.loan - totalExpenses();
    userData.saving = userData.balance;
    saveData();
}

function clearExpenses(){
    if(confirm("Clear all expenses?")){
        userData.expenses = [];
        userData.balance = userData.salary - userData.loan;
        userData.saving = userData.balance;
        saveData();
    }
}

function totalExpenses(){
    return userData.expenses.reduce((acc,d)=>acc+d.Total,0);
}

function updateTable(){
    let table = document.getElementById('expenseTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th></tr>";
    userData.expenses.forEach((d,i)=>{
        let row = table.insertRow();
        row.insertCell(0).innerText=i+1;
        row.insertCell(1).innerText=d.Food;
        row.insertCell(2).innerText=d.Fuel;
        row.insertCell(3).innerText=d.Snacks;
        row.insertCell(4).innerText=d.Bills;
        row.insertCell(5).innerText=d.Entertainment;
        row.insertCell(6).innerText=d.Total;
    });
}

function saveData(){
    localStorage.setItem('pocketUser',JSON.stringify(userData));
    updateDisplay();
}

// ===== Chart =====
let chart;
function updateChart(){
    const ctx = document.getElementById('expenseChart').getContext('2d');
    const labels = userData.expenses.map(d=>d.date);
    const dataFood = userData.expenses.map(d=>d.Food);
    const dataFuel = userData.expenses.map(d=>d.Fuel);
    const dataSnacks = userData.expenses.map(d=>d.Snacks);
    const dataBills = userData.expenses.map(d=>d.Bills);
    const dataFun = userData.expenses.map(d=>d.Entertainment);

    if(chart) chart.destroy();
    chart = new Chart(ctx,{
        type:'bar',
        data:{
            labels:labels,
            datasets:[
                {label:'Food',data:dataFood,backgroundColor:'#ff6b6b'},
                {label:'Fuel',data:dataFuel,backgroundColor:'#4ecdc4'},
                {label:'Snacks',data:dataSnacks,backgroundColor:'#f7b731'},
                {label:'Bills',data:dataBills,backgroundColor:'#45aaf2'},
                {label:'Fun',data:dataFun,backgroundColor:'#a55eea'}
            ]
        },
        options:{
            responsive:true,
            plugins:{legend:{position:'top'}},
            scales:{y:{beginAtZero:true}}
        }
    });
}
</script>
</body>
</html>
