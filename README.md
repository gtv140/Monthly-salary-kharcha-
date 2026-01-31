<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Tracker Modern</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base Styles ===== */
body{
    font-family:'Roboto',sans-serif;
    margin:0;padding:0;
    background: linear-gradient(135deg,#1e3c72,#2a5298);
    color:#fff;
    overflow-x:hidden;
    transition:0.5s;
}
h1,h2{text-align:center;margin:10px;font-weight:700;}
#datetime{text-align:center;font-weight:bold;margin-bottom:15px;color:#ffeb3b;letter-spacing:1px;font-size:18px;}
button{cursor:pointer;transition:0.3s;font-weight:700;border:none;border-radius:8px;padding:8px 12px;}
button:hover{opacity:0.85;}
input{padding:8px;border-radius:10px;border:2px solid #03a9f4;outline:none;margin:3px;width:120px;}
input:focus{border-color:#0288d1;box-shadow:0 0 8px #0288d1;}

/* ===== Header ===== */
header{
    background:linear-gradient(135deg,#ff6f61,#de6262);
    padding:15px;
    text-align:center;
    font-size:28px;
    font-weight:bold;
    color:#fff;
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
    box-shadow:0 4px 10px rgba(0,0,0,0.3);
    border-bottom-left-radius:15px;
    border-bottom-right-radius:15px;
}

/* ===== Cards ===== */
.cards{display:flex;flex-wrap:wrap;justify-content:center;margin:20px;}
.card{
    background:rgba(255,255,255,0.1);
    backdrop-filter:blur(10px);
    border-radius:15px;
    margin:10px;
    padding:20px;
    text-align:center;
    width:160px;
    box-shadow:0 8px 20px rgba(0,0,0,0.35);
    transition:0.4s;
    cursor:pointer;
}
.card:hover{transform:scale(1.05);box-shadow:0 10px 30px rgba(0,0,0,0.45);}
.card span{display:block;font-size:30px;margin-bottom:10px;}

/* ===== Table ===== */
table{
    border-collapse: collapse;
    width:100%;
    margin-top:20px;
    background:rgba(255,255,255,0.95);
    color:#333;
    border-radius:12px;
    overflow:hidden;
    box-shadow:0 6px 15px rgba(0,0,0,0.2);
}
th,td{border:1px solid #e0e0e0;padding:10px;text-align:center;}
th{background:#0288d1;color:#fff;letter-spacing:1px;}
tr:nth-child(even){background:rgba(0,172,193,0.1);}
tr:hover{background:rgba(0,172,193,0.2);}

/* ===== Theme Toggle ===== */
#themeToggle{position:fixed;top:15px;right:15px;padding:8px 12px;background:#0288d1;color:#fff;z-index:1000;}

/* ===== Floating Background Lights ===== */
.floating{
    position:absolute;
    border-radius:50%;
    opacity:0.3;
    animation:float 10s infinite;
    z-index:0;
}
@keyframes float{
    0%{transform:translateY(0) rotate(0deg);}
    50%{transform:translateY(-50px) rotate(180deg);}
    100%{transform:translateY(0) rotate(360deg);}
}

/* ===== Responsive ===== */
@media(max-width:600px){
    .cards{flex-direction:column;align-items:center;}
    .card{width:80%;}
    input{width:80%;}
}
</style>
</head>
<body>

<header>💼 Pocket Tracker 💼</header>
<div id="datetime"></div>
<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<!-- Dashboard -->
<div id="dashboard" style="padding:10px;">

    <!-- User Info Cards -->
    <div class="cards">
        <div class="card">
            <span>👤</span>
            Username<br><b id="infoUser">User</b>
        </div>
        <div class="card">
            <span>💰</span>
            Current Balance<br><b id="currentBalance">0</b>
        </div>
        <div class="card">
            <span>📈</span>
            Total Saving<br><b id="totalSaving">0</b>
        </div>
        <div class="card">
            <span>💸</span>
            Total Expense<br><b id="totalExpense">0</b>
        </div>
    </div>

    <!-- Input Fields -->
    <div class="cards">
        <div class="card">
            Salary<br><input type="number" id="salaryInput" value="0" onchange="updateSalary()">
        </div>
        <div class="card">
            Loan<br><input type="number" id="loanInput" value="0" onchange="updateLoan()">
        </div>
    </div>

    <!-- Expense Buttons -->
    <h2 style="text-align:center;">Daily Expenses</h2>
    <div class="cards">
        <div class="card" onclick="addDaily('food')"><span>🍔</span>Food</div>
        <div class="card" onclick="addDaily('fuel')"><span>⛽</span>Fuel</div>
        <div class="card" onclick="addDaily('snacks')"><span>🍿</span>Snacks</div>
        <div class="card" onclick="addDaily('bills')"><span>💡</span>Bills</div>
        <div class="card" onclick="addDaily('entertainment')"><span>🎮</span>Fun</div>
        <div class="card" onclick="clearAll()"><span>🗑️</span>Clear All</div>
    </div>

    <!-- Daily Table -->
    <h2 style="text-align:center;">Daily Expenses Table</h2>
    <table id="dailyTable">
        <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Date</th></tr>
    </table>

    <!-- Chart -->
    <h2 style="text-align:center;">Visual Overview</h2>
    <canvas id="chart" width="400" height="200"></canvas>
</div>

<!-- Floating Lights -->
<div class="floating" style="width:15px;height:15px;background:#ffeb3b;top:50px;left:30px;"></div>
<div class="floating" style="width:12px;height:12px;background:#03a9f4;top:150px;left:120px;"></div>
<div class="floating" style="width:18px;height:18px;background:#4caf50;top:300px;left:200px;"></div>
<div class="floating" style="width:10px;height:10px;background:#ff5722;top:400px;left:50px;"></div>

<script>
// ===== Timer =====
function updateDateTime(){
    const now=new Date();
    document.getElementById('datetime').innerText=now.toLocaleDateString()+' | '+now.toLocaleTimeString();
}
setInterval(updateDateTime,1000);
updateDateTime();

// ===== Theme Toggle =====
let darkTheme=true;
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

// ===== Data =====
let dailyData=JSON.parse(localStorage.getItem('dailyData')||"[]");
let salary=0,loan=0;

// ===== Update Cards =====
function updateCards(){
    const totalExpense=dailyData.reduce((a,b)=>a+b.food+b.fuel+b.snacks+b.bills+b.entertainment,0)+loan;
    const currentBalance=Math.max(0,salary-totalExpense);
    const totalSaving=currentBalance;
    document.getElementById('currentBalance').innerText=currentBalance;
    document.getElementById('totalExpense').innerText=totalExpense;
    document.getElementById('totalSaving').innerText=totalSaving;
}

// ===== Add Daily Expense =====
function addDaily(type){
    const value=parseInt(prompt(`Enter ${type} amount:`,"0"))||0;
    const today=new Date().toLocaleDateString();
    let last=dailyData[dailyData.length-1];
    if(last && last.date===today){
        last[type]=(last[type]||0)+value;
    } else {
        let newDay={food:0,fuel:0,snacks:0,bills:0,entertainment:0,date:today};
        newDay[type]=value;
        dailyData.push(newDay);
    }
    saveData();
    renderTable();
    updateCards();
}

// ===== Update Salary & Loan =====
function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;saveData();updateCards();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;saveData();updateCards();}

// ===== Clear All =====
function clearAll(){if(confirm("Delete all data?")){dailyData=[];saveData();renderTable();updateCards();}}

// ===== Save / Load =====
function saveData(){localStorage.setItem('dailyData',JSON.stringify(dailyData));localStorage.setItem('salary',salary);localStorage.setItem('loan',loan);}
function loadData(){salary=parseInt(localStorage.getItem('salary'))||0;loan=parseInt(localStorage.getItem('loan'))||0;document.getElementById('salaryInput').value=salary;document.getElementById('loanInput').value=loan;updateCards();renderTable();}
loadData();

// ===== Render Table =====
function renderTable(){
    const table=document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Date</th></tr>";
    dailyData.forEach((day,i)=>{
        const row=table.insertRow();
        row.insertCell(0).innerText=i+1;
        row.insertCell(1).innerText=day.food||0;
        row.insertCell(2).innerText=day.fuel||0;
        row.insertCell(3).innerText=day.snacks||0;
        row.insertCell(4).innerText=day.bills||0;
        row.insertCell(5).innerText=day.entertainment||0;
        const total=(day.food||0)+(day.fuel||0)+(day.snacks||0)+(day.bills||0)+(day.entertainment||0);
        row.insertCell(6).innerText=total;
        row.insertCell(7).innerText=day.date;
    });
    renderChart();
}

// ===== Chart =====
let chart=null;
function renderChart(){
    const ctx=document.getElementById('chart').getContext('2d');
    const labels=dailyData.map(d=>d.date);
    const data=dailyData.map(d=>(d.food||0)+(d.fuel||0)+(d.snacks||0)+(d.bills||0)+(d.entertainment||0));
    if(chart) chart.destroy();
    chart=new Chart(ctx,{
        type:'bar',
        data:{
            labels:labels,
            datasets:[{label:'Daily Expense',data:data,backgroundColor:'rgba(255,99,132,0.6)'}]
        },
        options:{responsive:true,plugins:{legend:{display:true}}}
    });
}
</script>

</body>
</html>
