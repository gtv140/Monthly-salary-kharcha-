<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker – Modern Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Roboto',sans-serif;}
body{background:linear-gradient(135deg,#1e3c72,#2a5298);color:#fff;transition:0.5s;}
h1,h2{text-align:center;margin:15px 0;}
.container{max-width:1200px;margin:20px auto;padding:0 15px;}
button{cursor:pointer;padding:8px 15px;border:none;border-radius:8px;font-weight:bold;transition:0.3s;}
button:hover{opacity:0.85;}
input{padding:8px;border-radius:8px;border:2px solid #03a9f4;outline:none;width:120px;margin:5px;}
input:focus{border-color:#0288d1;box-shadow:0 0 8px #0288d1;}

/* Header */
header{padding:20px;text-align:center;font-size:28px;font-weight:bold;color:#fff; text-shadow:2px 2px 5px rgba(0,0,0,0.3);}

/* Info Cards */
.infoBox{display:flex;flex-wrap:wrap;justify-content:space-around;margin:20px 0;}
.card{flex:1;min-width:150px;background:rgba(255,255,255,0.15);backdrop-filter:blur(10px);margin:10px;padding:20px;border-radius:15px;text-align:center;transition:0.4s;box-shadow:0 8px 20px rgba(0,0,0,0.25);}
.card:hover{transform:scale(1.05);box-shadow:0 10px 30px rgba(0,0,0,0.45);}

/* Dashboard Buttons */
.dashboard{display:flex;flex-wrap:wrap;justify-content:center;margin:20px 0;}
.dashboard button{background:linear-gradient(135deg,#ff6a00,#ffb347);color:#fff;margin:10px;width:130px;height:130px;display:flex;flex-direction:column;align-items:center;justify-content:center;font-weight:bold;font-size:14px;text-align:center;transition:0.4s;}
.dashboard button img{width:50px;height:50px;margin-bottom:10px;}
.dashboard button:hover{transform:scale(1.1);box-shadow:0 10px 30px rgba(0,0,0,0.45);}

/* Table */
table{width:100%;border-collapse:collapse;margin-top:20px;background:rgba(255,255,255,0.95);color:#333;border-radius:12px;overflow:hidden;box-shadow:0 6px 15px rgba(0,0,0,0.2);}
th,td{border:1px solid #e0e0e0;padding:10px;text-align:center;}
th{background:#0288d1;color:#fff;}
tr:nth-child(even){background:rgba(0,172,193,0.1);}
tr:hover{background:rgba(0,172,193,0.2);}

/* Toggle Theme */
#themeToggle{position:fixed;top:20px;right:20px;padding:8px 12px;background:#0288d1;color:#fff;border-radius:8px;font-weight:bold;box-shadow:0 4px 10px rgba(0,0,0,0.3);}

/* Responsive */
@media(max-width:768px){.infoBox,.dashboard{flex-direction:column;align-items:center;}}
</style>
</head>
<body>
<header>💼 Pocket Tracker – Modern Dashboard 💼</header>
<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<div class="container">
<h2>👤 User Info</h2>
<div class="infoBox">
<div class="card">Username<br><span id="username">SweetieUser</span></div>
<div class="card">Salary<br><input type="number" id="salaryInput" value="0" onchange="updateSalary()"></div>
<div class="card">Loan<br><input type="number" id="loanInput" value="0" onchange="updateLoan()"></div>
<div class="card">Goal Saving<br><input type="number" id="goalInput" value="10000" onchange="updateGoal()"></div>
<div class="card">Remaining Balance<br><span id="balance">0 PKR</span></div>
</div>

<h2>💰 Daily Expenses</h2>
<div class="dashboard">
<button onclick="addExpense('food')"><img src="https://source.unsplash.com/50x50/?food" alt="Food">Food</button>
<button onclick="addExpense('fuel')"><img src="https://source.unsplash.com/50x50/?fuel" alt="Fuel">Fuel</button>
<button onclick="addExpense('snacks')"><img src="https://source.unsplash.com/50x50/?snacks" alt="Snacks">Snacks</button>
<button onclick="addExpense('bills')"><img src="https://source.unsplash.com/50x50/?bills" alt="Bills">Bills</button>
<button onclick="addExpense('entertainment')"><img src="https://source.unsplash.com/50x50/?fun" alt="Fun">Fun</button>
<button onclick="clearAll()"><img src="https://source.unsplash.com/50x50/?delete" alt="Clear">Clear</button>
<button onclick="exportCSV()"><img src="https://source.unsplash.com/50x50/?report" alt="Export">Export CSV</button>
<button onclick="backupJSON()"><img src="https://source.unsplash.com/50x50/?backup" alt="Backup">Backup</button>
<button onclick="restoreJSON()"><img src="https://source.unsplash.com/50x50/?restore" alt="Restore">Restore</button>
</div>

<h2>📊 Daily Expense Table</h2>
<table id="dailyTable">
<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
</table><script>
// ===== Theme Toggle =====
let darkTheme = true;
function toggleTheme(){
    if(darkTheme){
        document.body.style.background="#fff";
        document.body.style.color="#333";
        darkTheme=false;
    }else{
        document.body.style.background="linear-gradient(135deg,#1e3c72,#2a5298)";
        document.body.style.color="#fff";
        darkTheme=true;
    }
}

// ===== User Data & Local Storage =====
let username = localStorage.getItem('username') || "SweetieUser";
document.getElementById('username').innerText = username;

let salary = parseInt(localStorage.getItem('salary')) || 0;
let loan = parseInt(localStorage.getItem('loan')) || 0;
let goal = parseInt(localStorage.getItem('goal')) || 10000;
let dailyData = JSON.parse(localStorage.getItem('dailyData')) || [];

// ===== Update Inputs =====
function updateSalary(){salary = parseInt(document.getElementById('salaryInput').value)||0;saveData();updateBalance();}
function updateLoan(){loan = parseInt(document.getElementById('loanInput').value)||0;saveData();updateBalance();}
function updateGoal(){goal = parseInt(document.getElementById('goalInput').value)||10000;saveData();updateBalance();}

// ===== Save to Local Storage =====
function saveData(){
    localStorage.setItem('salary',salary);
    localStorage.setItem('loan',loan);
    localStorage.setItem('goal',goal);
    localStorage.setItem('dailyData',JSON.stringify(dailyData));
}

// ===== Add Expense =====
function addExpense(type){
    let value = parseInt(prompt(`Enter ${type} amount in PKR:`,"0"))||0;
    const today = new Date().toLocaleDateString();
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today){
        dailyData[dailyData.length-1][type] += value;
        dailyData[dailyData.length-1].total += value;
    } else {
        let entry = {food:0,fuel:0,snacks:0,bills:0,entertainment:0,total:0,date:today};
        entry[type] = value;
        entry.total = value;
        dailyData.push(entry);
    }
    saveData();
    updateTable();
    updateBalance();
}

// ===== Update Table =====
function updateTable(){
    const table = document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    let totalExpense = 0;
    dailyData.forEach((day,index)=>{
        const row = table.insertRow();
        row.insertCell(0).innerText=index+1;
        row.insertCell(1).innerText=day.food;
        row.insertCell(2).innerText=day.fuel;
        row.insertCell(3).innerText=day.snacks;
        row.insertCell(4).innerText=day.bills;
        row.insertCell(5).innerText=day.entertainment;
        row.insertCell(6).innerText=day.total;
        row.insertCell(7).innerText=day.date;
        totalExpense+=day.total;
    });
}

// ===== Update Balance =====
function updateBalance(){
    let totalExpense = dailyData.reduce((sum,day)=>sum+day.total,0) + loan;
    let remaining = salary - totalExpense;
    document.getElementById('balance').innerText = remaining + " PKR";
    drawChart();
}

// ===== Clear All Data =====
function clearAll(){
    if(confirm("Are you sure you want to delete all data?")){
        dailyData=[];
        saveData();
        updateTable();
        updateBalance();
    }
}

// ===== Backup & Restore =====
function backupJSON(){
    const blob = new Blob([JSON.stringify({salary,loan,goal,dailyData,username})],{type:'application/json'});
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download='PocketTrackerBackup.json';
    link.click();
}
function restoreJSON(){
    const fileInput = document.createElement('input');
    fileInput.type='file';
    fileInput.accept='.json';
    fileInput.onchange = e=>{
        const file = e.target.files[0];
        const reader = new FileReader();
        reader.onload = ev=>{
            const data = JSON.parse(ev.target.result);
            salary = data.salary || 0;
            loan = data.loan || 0;
            goal = data.goal || 10000;
            dailyData = data.dailyData || [];
            username = data.username || "SweetieUser";
            document.getElementById('username').innerText = username;
            document.getElementById('salaryInput').value=salary;
            document.getElementById('loanInput').value=loan;
            document.getElementById('goalInput').value=goal;
            saveData();
            updateTable();
            updateBalance();
            alert('Backup restored!');
        };
        reader.readAsText(file);
    };
    fileInput.click();
}

// ===== Export CSV =====
function exportCSV(){
    let csv = "Day,Food,Fuel,Snacks,Bills,Fun,Total,Date\n";
    dailyData.forEach((day,index)=>{
        csv += `${index+1},${day.food},${day.fuel},${day.snacks},${day.bills},${day.entertainment},${day.total},${day.date}\n`;
    });
    const blob = new Blob([csv],{type:'text/csv'});
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download='PocketTracker.csv';
    link.click();
}

// ===== Chart =====
function drawChart(){
    const ctx = document.getElementById('chart') || document.createElement('canvas');
    if(!ctx.parentNode) document.body.appendChild(ctx); 
    const chartData = {
        labels:['Salary','Total Expense','Remaining Balance'],
        datasets:[{
            label:'PKR',
            data:[
                salary,
                dailyData.reduce((sum,day)=>sum+day.total,0)+loan,
                salary-(dailyData.reduce((sum,day)=>sum+day.total,0)+loan)
            ],
            backgroundColor:['#0288d1','#ff6a00','#4caf50']
        }]
    };
    if(window.barChart) window.barChart.destroy();
    window.barChart = new Chart(ctx.getContext('2d'),{
        type:'bar',
        data: chartData,
        options:{responsive:true,plugins:{legend:{display:false}}}
    });
}

// ===== Initial Load =====
updateTable();
updateBalance();
</script>
</body>
</html>
