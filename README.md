<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base Styles ===== */
body{
    font-family:'Roboto',sans-serif;
    margin:0;
    padding:0;
    background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
    color:#fff;
    overflow-x:hidden;
    transition:0.5s;
}
h1,h2{text-align:center;margin:10px;font-weight:700;}
#datetime{text-align:center;font-weight:bold;margin-bottom:15px;color:#ffeb3b;letter-spacing:1px;font-size:18px;}
button{cursor:pointer;transition:0.3s;font-weight:700;border:none;border-radius:8px;padding:8px 12px;}
button:hover{opacity:0.85;transform:scale(1.05);}
input{padding:8px;border-radius:10px;border:2px solid #03a9f4;outline:none;margin:5px;width:160px;}
input:focus{border-color:#0288d1;box-shadow:0 0 10px #0288d1;}

/* ===== Header ===== */
header{
    background:linear-gradient(135deg,#ff416c,#ff4b2b);
    padding:20px;
    text-align:center;
    font-size:36px;
    font-weight:bold;
    color:#fff;
    text-shadow:2px 2px 6px rgba(0,0,0,0.4);
    box-shadow:0 4px 12px rgba(0,0,0,0.4);
    border-bottom-left-radius:20px;
    border-bottom-right-radius:20px;
}

/* ===== Dashboard ===== */
.dashboard{display:flex;flex-wrap:wrap;justify-content:center;margin:20px;}
.card{
    background:linear-gradient(145deg,#ff4b2b,#ff416c);
    box-shadow:0 10px 25px rgba(0,0,0,0.4);
    border-radius:20px;
    margin:12px;
    padding:25px;
    text-align:center;
    width:150px;
    cursor:pointer;
    transition:0.5s;
    font-weight:bold;
    color:#fff;
    display:flex;
    flex-direction:column;
    align-items:center;
    text-shadow:0 1px 2px rgba(0,0,0,0.3);
}
.card img{width:50px;height:50px;margin-bottom:8px;}
.card:hover{transform:scale(1.1) rotate(2deg);box-shadow:0 12px 30px rgba(0,0,0,0.5);}

.infoBox{display:flex;flex-wrap:wrap;justify-content:center;margin:20px;}
.infoBox div{
    text-align:center;
    background:rgba(255,255,255,0.1);
    backdrop-filter:blur(12px);
    color:#fff;
    padding:25px;
    margin:10px;
    border-radius:20px;
    flex:1;
    min-width:160px;
    box-shadow:0 10px 25px rgba(0,0,0,0.35);
    transition:0.5s;
    font-weight:bold;
}
.infoBox div:hover{transform:scale(1.05) rotate(-1deg);}

/* ===== Table ===== */
table{
    border-collapse: collapse;
    width:95%;
    margin:20px auto;
    background:rgba(255,255,255,0.95);
    color:#333;
    border-radius:12px;
    overflow:hidden;
    box-shadow:0 6px 20px rgba(0,0,0,0.3);
}
th,td{border:1px solid #e0e0e0;padding:12px;text-align:center;}
th{background:#ff416c;color:#fff;letter-spacing:1px;}
tr:nth-child(even){background:rgba(255,75,43,0.1);}
tr:hover{background:rgba(255,75,43,0.2);}

/* ===== Theme Toggle ===== */
#themeToggle{position:fixed;top:15px;right:15px;background:#ff416c;color:#fff;box-shadow:0 4px 12px rgba(0,0,0,0.4);}

/* ===== Floating Lights ===== */
.floating{
    position:absolute;
    border-radius:50%;
    opacity:0.3;
    animation:float 12s infinite;
    z-index:0;
}
@keyframes float{
    0%{transform:translateY(0) rotate(0deg);}
    50%{transform:translateY(-60px) rotate(180deg);}
    100%{transform:translateY(0) rotate(360deg);}
}

@media(max-width:768px){
    .dashboard{flex-direction:column;align-items:center;}
    .infoBox{flex-direction:column;align-items:center;}
}
</style>
</head>
<body>

<header>🚀 Pocket Tracker Pro 🚀</header>
<div id="datetime"></div>
<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<!-- Username Input -->
<div id="loginScreen" style="text-align:center; margin-top:40px;">
    <h2>Enter Your Name</h2>
    <input type="text" id="usernameInput" placeholder="Your Name">
    <button onclick="startDashboard()">Start</button>
</div>

<!-- Dashboard -->
<div id="dashboard" style="display:none;position:relative;z-index:1;">

    <div style="text-align:right;margin-bottom:10px;">
        <span id="welcomeUser" style="font-weight:bold;font-size:18px;"></span>
        <button onclick="logout()">Logout</button>
    </div>

    <h2>💰 User Info</h2>
    <div class="infoBox">
        <div>Username<br><span id="infoUser">-</span></div>
        <div>Salary<br><input type="number" id="salaryInput" value="0" onchange="updateSalary()"></div>
        <div>Loan<br><input type="number" id="loanInput" value="0" onchange="updateLoan()"></div>
        <div>Current Balance<br><span id="currentBalance">0</span></div>
        <div>Saving<br><span id="currentSaving">0</span></div>
    </div>

    <h2>Daily Expenses</h2>
    <div class="dashboard" id="expenseCards">
        <div class="card" title="Add Food" onclick="addDaily('food')">
            <img src="https://img.icons8.com/color/48/000000/hamburger.png"/>
            🍔 Food
        </div>
        <div class="card" title="Add Fuel" onclick="addDaily('fuel')">
            <img src="https://img.icons8.com/color/48/000000/fuel.png"/>
            ⛽ Fuel
        </div>
        <div class="card" title="Add Snacks" onclick="addDaily('snacks')">
            <img src="https://img.icons8.com/color/48/000000/candy.png"/>
            🍿 Snacks
        </div>
        <div class="card" title="Add Bills" onclick="addDaily('bills')">
            <img src="https://img.icons8.com/color/48/000000/bill.png"/>
            💡 Bills
        </div>
        <div class="card" title="Add Fun" onclick="addDaily('fun')">
            <img src="https://img.icons8.com/color/48/000000/game-controller.png"/>
            🎮 Fun
        </div>
        <div class="card" title="Clear All Data" onclick="clearAll()">
            <img src="https://img.icons8.com/color/48/000000/delete-sign.png"/>
            🗑️ Clear
        </div>
    </div>

    <h2>Daily Expenses Table</h2>
    <table id="dailyTable">
        <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
    </table>

    <h2>Visual Overview</h2>
    <canvas id="chart" width="400" height="200"></canvas>

</div>

<!-- Floating Lights -->
<div class="floating" style="width:20px;height:20px;background:#ffeb3b;top:50px;left:40px;"></div>
<div class="floating" style="width:15px;height:15px;background:#03a9f4;top:200px;left:100px;"></div>
<div class="floating" style="width:18px;height:18px;background:#4caf50;top:350px;left:250px;"></div>
<div class="floating" style="width:12px;height:12px;background:#ff5722;top:400px;left:50px;"></div>

<script>
// ===== Timer =====
function updateDateTime(){
    const now=new Date();
    const options={weekday:'long',year:'numeric',month:'long',day:'numeric'};
    document.getElementById('datetime').innerText=now.toLocaleDateString('en-US',options)+' | '+now.toLocaleTimeString();
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
        document.body.style.background="linear-gradient(135deg,#0f2027,#203a43,#2c5364)";
        document.body.style.color="#fff";
        darkTheme=true;
    }
}

// ===== Dashboard Logic =====
let currentUser=null;
let dailyData=[];
let salary=0,loan=0;

function startDashboard(){
    const user=document.getElementById('usernameInput').value.trim();
    if(user){
        currentUser=user;
        localStorage.setItem('username',currentUser);
        document.getElementById('loginScreen').style.display='none';
        document.getElementById('dashboard').style.display='block';
        document.getElementById('welcomeUser').innerText=`Welcome, ${currentUser}!`;
        loadData();
    }
}

function loadData(){
    const storedData=JSON.parse(localStorage.getItem(currentUser)||"{}");
    dailyData=storedData.dailyData||[];
    salary=storedData.salary||0;
    loan=storedData.loan||0;
    document.getElementById('salaryInput').value=salary;
    document.getElementById('loanInput').value=loan;
    document.getElementById('infoUser').innerText=currentUser;
    calculate();
}

function saveData(){
    const data={dailyData,salary,loan};
    localStorage.setItem(currentUser,JSON.stringify(data));
}

function logout(){
    document.getElementById('dashboard').style.display='none';
    document.getElementById('loginScreen').style.display='block';
}

// ===== Expenses =====
function addDaily(type){
    const value=parseInt(prompt(`Enter ${type} amount:`,"0"))||0;
    const today={food:0,fuel:0,snacks:0,bills:0,fun:0,total:0,date:new Date().toLocaleDateString()};
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today.date){
        let last=dailyData[dailyData.length-1];
        last[type]+=value;
        last.total=last.food+last.fuel+last.snacks+last.bills+last.fun;
    } else {today[type]=value;today.total=today.food+today.fuel+today.snacks+today.bills+today.fun;dailyData.push(today);}
    saveData();
    calculate();
}

function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;saveData();calculate();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;saveData();calculate();}
function clearAll(){if(confirm("Delete all data?")){dailyData=[];saveData();calculate();}}

// ===== Dashboard Calculation =====
function calculate(){
    const table=document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>";
    let totalExpense=loan;
    dailyData.forEach((day,index)=>{
        const row=table.insertRow();
        row.insertCell(0).innerText=index+1;
        row.insertCell(1).innerText=day.food;
        row.insertCell(2).innerText=day.fuel;
        row.insertCell(3).innerText=day.snacks;
        row.insertCell(4).innerText=day.bills;
        row.insertCell(5).innerText=day.fun;
        row.insertCell(6).innerText=day.total;
        row.insertCell(7).innerText=day.date;
        totalExpense+=day.total;
    });
    const currentBalance=Math.max(0,salary-totalExpense);
    const saving=currentBalance;
    document.getElementById('currentBalance').innerText=currentBalance;
    document.getElementById('currentSaving').innerText=saving;

    const ctx=document.getElementById('chart').getContext('2d');
    if(window.barChart) window.barChart.destroy();
    window.barChart=new Chart(ctx,{
        type:'bar',
        data:{
            labels:['Salary','Total Expense','Current Saving'],
            datasets:[{
                label:'PKR',
                data:[salary,totalExpense,saving],
                backgroundColor:['#4caf50','#f44336','#2196f3']
            }]
        },
        options:{responsive:true,plugins:{legend:{display:false}}}
    });
}

// Load last username
window.onload=function(){
    const savedUser=localStorage.getItem('username');
    if(savedUser){
        document.getElementById('usernameInput').value=savedUser;
    }
}
</script>
</body>
</html>
