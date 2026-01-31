<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Traker - Ultimate Modern Dashboard</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
body{
    font-family:'Roboto',sans-serif;
    margin:0;padding:0;
    background:linear-gradient(135deg,#1e3c72,#2a5298);
    color:#fff;
    transition:0.5s;
}
header{
    background:linear-gradient(135deg,#29b6f6,#00acc1);
    padding:20px;
    text-align:center;
    font-size:30px;
    font-weight:bold;
    color:#fff;
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
}
.infoBox{
    display:flex;
    flex-wrap:wrap;
    justify-content:space-around;
    margin:20px;
    padding:15px;
}
.infoBox div{
    text-align:center;
    background:rgba(255,255,255,0.1);
    backdrop-filter:blur(8px);
    color:#fff;
    padding:20px;
    margin:10px;
    border-radius:15px;
    flex:1;
    min-width:150px;
    box-shadow:0 8px 20px rgba(0,0,0,0.25);
    transition:0.4s;
    font-weight:bold;
}
.infoBox div:hover{transform:scale(1.05);}
.dashboard{
    display:flex;flex-wrap:wrap;justify-content:center;margin:20px 0;
}
.card{
    background:linear-gradient(145deg,#00acc1,#29b6f6);
    box-shadow:0 8px 20px rgba(0,0,0,0.35);
    border-radius:15px;
    margin:10px;
    padding:20px;
    text-align:center;
    width:160px;
    cursor:pointer;
    transition:0.4s;
    font-weight:bold;
    color:#fff;
}
.card:hover{transform:scale(1.08);box-shadow:0 10px 30px rgba(0,0,0,0.45);}
button{cursor:pointer;border:none;border-radius:8px;padding:8px 12px;font-weight:bold;}
button.primary{background:#ffeb3b;color:#1e3c72;}
button.secondary{background:#4caf50;color:#fff;}
table{
    border-collapse: collapse;
    width:95%;
    margin:20px auto;
    background:rgba(255,255,255,0.95);
    color:#333;
    border-radius:12px;
    overflow:hidden;
    box-shadow:0 6px 15px rgba(0,0,0,0.2);
}
th,td{border:1px solid #e0e0e0;padding:10px;text-align:center;}
th{background:#0288d1;color:#fff;}
tr:nth-child(even){background:rgba(0,172,193,0.1);}
tr:hover{background:rgba(0,172,193,0.2);}
.floating{
    position:absolute;border-radius:50%;opacity:0.3;animation:float 10s infinite;z-index:0;
}
@keyframes float{
    0%{transform:translateY(0) rotate(0deg);}
    50%{transform:translateY(-50px) rotate(180deg);}
    100%{transform:translateY(0) rotate(360deg);}
}
@media(max-width:768px){
    .infoBox{flex-direction:column;align-items:center;}
    .dashboard{flex-direction:column;align-items:center;}
}
</style>
</head>
<body>

<header>💼 Pocket Traker 💼</header>

<div class="infoBox">
    <div>Username<br><span id="username">User1</span></div>
    <div>Salary<br><input type="number" id="salary" value="50000" onchange="calculateBalance()"></div>
    <div>Loan<br><input type="number" id="loan" value="10000" onchange="calculateBalance()"></div>
    <div>Current Balance<br><span id="currentBalance">0</span></div>
    <div>Saving<br><span id="saving">0</span></div>
</div>

<div class="dashboard">
    <div class="card" onclick="addExpense('Food')">🍔 Food</div>
    <div class="card" onclick="addExpense('Fuel')">⛽ Fuel</div>
    <div class="card" onclick="addExpense('Snacks')">🍿 Snacks</div>
    <div class="card" onclick="addExpense('Bills')">💡 Bills</div>
    <div class="card" onclick="addExpense('Entertainment')">🎮 Fun</div>
    <div class="card" onclick="clearAll()">🗑️ Clear All</div>
    <div class="card" onclick="exportCSV()">📊 Export CSV</div>
    <div class="card" onclick="backupJSON()">💾 Backup</div>
    <div class="card" onclick="restoreJSON()">📂 Restore</div>
</div>

<h2 style="text-align:center;">📊 Expenses Table</h2>
<table id="expenseTable">
<tr><th>Date</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Action</th></tr>
</table>

<h2 style="text-align:center;">📈 Expenses Chart</h2>
<canvas id="chart" width="400" height="200" style="max-width:95%; margin:auto; display:block;"></canvas>

<div class="floating" style="width:15px;height:15px;background:#ffeb3b;top:50px;left:30px;"></div>
<div class="floating" style="width:12px;height:12px;background:#03a9f4;top:150px;left:120px;"></div>
<div class="floating" style="width:18px;height:18px;background:#4caf50;top:300px;left:200px;"></div>
<div class="floating" style="width:10px;height:10px;background:#ff5722;top:400px;left:50px;"></div>
<div class="floating" style="width:14px;height:14px;background:#9c27b0;top:220px;left:300px;"></div>

<script>
// ===== Data =====
let expenses = JSON.parse(localStorage.getItem('expenses')) || [];
let salary = parseInt(document.getElementById('salary').value) || 50000;
let loan = parseInt(document.getElementById('loan').value) || 10000;

// ===== Update Balance =====
function calculateBalance(){
salary = parseInt(document.getElementById('salary').value) || 0;
loan = parseInt(document.getElementById('loan').value) || 0;
let totalExp = expenses.reduce((a,b)=>a.total + a.Food + a.Fuel + a.Snacks + a.Bills + a.Entertainment,0);
let balance = salary - loan - totalExp;
document.getElementById('currentBalance').innerText = balance;
document.getElementById('saving').innerText = balance;
updateTable();
updateChart();
if(balance < 5000){alert("⚠️ Low balance!")}
}

// ===== Add Expense =====
function addExpense(type){
let val = parseInt(prompt(`Enter ${type} expense:`,0)) || 0;
let today = new Date().toLocaleDateString();
let expenseObj = {date:today, Food:0, Fuel:0, Snacks:0, Bills:0, Entertainment:0, total:0};
expenseObj[type] = val;
expenseObj.total = val;
expenses.push(expenseObj);
localStorage.setItem('expenses',JSON.stringify(expenses));
calculateBalance();
}

// ===== Clear All =====
function clearAll(){
if(confirm("Clear all expenses?")){
expenses=[];
localStorage.setItem('expenses',JSON.stringify(expenses));
calculateBalance();
}
}

// ===== Export CSV =====
function exportCSV(){
let csv = "Date,Food,Fuel,Snacks,Bills,Fun,Total\n";
expenses.forEach(e=>{
csv += `${e.date},${e.Food},${e.Fuel},${e.Snacks},${e.Bills},${e.Entertainment},${e.total}\n`;
});
let blob = new Blob([csv],{type:'text/csv'});
let link = document.createElement('a');
link.href = URL.createObjectURL(blob);
link.download = 'PocketTraker.csv';
link.click();
}

// ===== Backup / Restore =====
function backupJSON(){
const blob=new Blob([JSON.stringify(expenses)],{type:'application/json'});
const link=document.createElement('a');
link.href=URL.createObjectURL(blob);
link.download='PocketTrakerBackup.json';
link.click();
}
function restoreJSON(){
const fileInput=document.createElement('input');
fileInput.type='file';fileInput.accept='.json';
fileInput.onchange=e=>{
const file=e.target.files[0];
const reader=new FileReader();
reader.onload=ev=>{
expenses=JSON.parse(ev.target.result);
localStorage.setItem('expenses',JSON.stringify(expenses));
alert('Backup restored! Refresh page.');
calculateBalance();
};
reader.readAsText(file);
};
fileInput.click();
}

// ===== Update Table =====
function updateTable(){
const table = document.getElementById('expenseTable');
table.innerHTML="<tr><th>Date</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Action</th></tr>";
expenses.forEach((e,index)=>{
let row = table.insertRow();
row.insertCell(0).innerText = e.date;
row.insertCell(1).innerText = e.Food;
row.insertCell(2).innerText = e.Fuel;
row.insertCell(3).innerText = e.Snacks;
row.insertCell(4).innerText = e.Bills;
row.insertCell(5).innerText = e.Entertainment;
row.insertCell(6).innerText = e.total;
row.insertCell(7).innerHTML = `<button onclick="deleteExpense(${index})">🗑️</button>`;
});
}

// ===== Delete Expense =====
function deleteExpense(index){
expenses.splice(index,1);
localStorage.setItem('expenses',JSON.stringify(expenses));
calculateBalance();
}

// ===== Chart =====
let ctx = document.getElementById('chart').getContext('2d');
let barChart;
function updateChart(){
let totalFood = expenses.reduce((a,b)=>a.Food,0);
let totalFuel = expenses.reduce((a,b)=>a.Fuel,0);
let totalSnacks = expenses.reduce((a,b)=>a.Snacks,0);
let totalBills = expenses.reduce((a,b)=>a.Bills,0);
let totalFun = expenses.reduce((a,b)=>a.Entertainment,0);

if(barChart) barChart.destroy();
barChart = new Chart(ctx,{
type:'bar',
data:{
labels:['Food','Fuel','Snacks','Bills','Fun'],
datasets:[{label:'Expenses PKR',data:[totalFood,totalFuel,totalSnacks,totalBills,totalFun],backgroundColor:['#ffeb3b','#03a9f4','#4caf50','#ff5722','#9c27b0']}]
},
options:{responsive:true,plugins:{legend:{display:false}}}
});
}

// ===== Init =====
calculateBalance();
</script>

</body>
</html>
