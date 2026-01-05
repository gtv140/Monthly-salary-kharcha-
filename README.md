<pocket>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Traker</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
body{
    font-family:'Roboto',sans-serif;
    margin:0;padding:0;
    background: linear-gradient(135deg,#00c6ff,#0072ff);
    color:#fff;
}
header{
    text-align:center;
    font-size:32px;
    font-weight:700;
    padding:20px;
    background:linear-gradient(90deg,#ff512f,#dd2476);
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
}
#datetime{text-align:center;margin:10px;font-weight:bold;}
.container{max-width:1000px;margin:auto;padding:10px;}
.boxes{
    display:flex;
    flex-wrap:wrap;
    justify-content:space-around;
    margin-bottom:20px;
}
.box{
    flex:1;
    min-width:180px;
    margin:10px;
    background:rgba(255,255,255,0.1);
    padding:20px;
    border-radius:15px;
    text-align:center;
    font-weight:bold;
    box-shadow:0 8px 20px rgba(0,0,0,0.3);
    transition:0.3s;
}
.box:hover{transform:scale(1.05);}
input{padding:8px;border-radius:10px;border:none;width:100px;margin-top:5px;text-align:center;}
button{padding:8px 12px;border:none;border-radius:8px;font-weight:bold;margin-top:10px;cursor:pointer;transition:0.3s;}
button:hover{opacity:0.8;}
table{
    width:100%;border-collapse:collapse;margin-top:20px;background:rgba(255,255,255,0.95);color:#333;border-radius:10px;overflow:hidden;
}
th,td{padding:10px;text-align:center;border:1px solid #ddd;}
th{background:#0072ff;color:#fff;}
tr:nth-child(even){background:rgba(0,0,0,0.05);}
tr:hover{background:rgba(0,0,0,0.1);}
#themeToggle{position:fixed;top:20px;right:20px;padding:10px;background:#ff512f;color:#fff;font-weight:bold;border-radius:10px;cursor:pointer;}
</style>
</head>
<body>
<header>Pocket Traker</header>
<div id="datetime"></div>
<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<div class="container" id="loginScreen">
    <h2>Login / Register</h2>
    Username: <input type="text" id="loginUser"><br>
    Password: <input type="password" id="loginPass"><br>
    <button onclick="login()">Login</button>
    <button onclick="register()">Register</button>
    <p id="loginMsg" style="color:#ffeb3b;"></p>
</div>

<div class="container" id="dashboard" style="display:none;">
    <div style="text-align:right;"><span id="welcomeUser"></span> <button onclick="logout()">Logout</button></div>
    <div class="boxes">
        <div class="box">Username<br><span id="infoUser">-</span></div>
        <div class="box">Salary<br><input type="number" id="salaryInput" value="0" onchange="updateSalary()"></div>
        <div class="box">Goal Saving<br><input type="number" id="goalInput" value="10000" onchange="updateGoal()"></div>
        <div class="box">Loan Amount<br><input type="number" id="loanInput" value="0" onchange="updateLoan()"></div>
    </div>

    <div class="boxes">
        <div class="box">Total Expense<br><span id="totalExpense">0</span></div>
        <div class="box">Remaining Balance<br><span id="remaining">0</span></div>
        <div class="box">Current Saving<br><span id="currentSaving">0</span></div>
        <div class="box">Goal Achieved<br><span id="goalStatus">0%</span></div>
    </div>

    <h2>Daily Expenses</h2>
    <div class="boxes">
        <div class="box" onclick="addDaily('food')">🍔 Food</div>
        <div class="box" onclick="addDaily('fuel')">⛽ Fuel</div>
        <div class="box" onclick="addDaily('snacks')">🍿 Snacks</div>
        <div class="box" onclick="addDaily('bills')">💡 Bills</div>
        <div class="box" onclick="addDaily('fun')">🎮 Fun</div>
        <div class="box" onclick="clearAll()">🗑️ Clear All</div>
    </div>

    <table id="dailyTable">
        <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Total</th><th>Date</th></tr>
    </table>
</div>

<script>
let darkTheme=true;
function updateDateTime(){
    const now=new Date();
    document.getElementById('datetime').innerText=now.toLocaleDateString()+' | '+now.toLocaleTimeString();
}
setInterval(updateDateTime,1000);
updateDateTime();

function toggleTheme(){
    if(darkTheme){
        document.body.style.background="#fff";
        document.body.style.color="#333";
        darkTheme=false;
    } else{
        document.body.style.background="linear-gradient(135deg,#00c6ff,#0072ff)";
        document.body.style.color="#fff";
        darkTheme=true;
    }
}

let currentUser=null;
let users=JSON.parse(localStorage.getItem('users')||"{}");
let dailyData=[],salary=0,goal=10000,loan=0;

window.onload=function(){
    const savedUser=localStorage.getItem('currentUser');
    if(savedUser && users[savedUser]){
        currentUser=savedUser;
        loadUserData();
    }
}

function login(){
    const user=document.getElementById('loginUser').value.trim();
    const pass=document.getElementById('loginPass').value;
    if(users[user] && users[user].pass===pass){
        currentUser=user;
        localStorage.setItem('currentUser',currentUser);
        loadUserData();
    } else {document.getElementById('loginMsg').innerText='Invalid login!';}
}

function register(){
    const user=document.getElementById('loginUser').value.trim();
    const pass=document.getElementById('loginPass').value;
    if(user && pass){
        if(users[user]){document.getElementById('loginMsg').innerText='User exists!'; return;}
        users[user]={pass:pass,dailyData:[],salary:0,goal:10000,loan:0};
        localStorage.setItem('users',JSON.stringify(users));
        document.getElementById('loginMsg').innerText='Registered! Now login.';
    }
}

function loadUserData(){
    document.getElementById('loginScreen').style.display='none';
    document.getElementById('dashboard').style.display='block';
    document.getElementById('welcomeUser').innerText=`Welcome, ${currentUser}`;
    const u=users[currentUser];
    dailyData=u.dailyData||[];
    salary=u.salary||0;
    goal=u.goal||10000;
    loan=u.loan||0;
    document.getElementById('salaryInput').value=salary;
    document.getElementById('goalInput').value=goal;
    document.getElementById('loanInput').value=loan;
    document.getElementById('infoUser').innerText=currentUser;
    calculate();
}

function logout(){
    localStorage.removeItem('currentUser');
    currentUser=null;
    document.getElementById('dashboard').style.display='none';
    document.getElementById('loginScreen').style.display='block';
}

function addDaily(type){
    const value=parseInt(prompt(`Enter ${type} expense:`,"0"))||0;
    const today={food:0,fuel:0,snacks:0,bills:0,fun:0,total:0,date:new Date().toLocaleDateString()};
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today.date){
        let last=dailyData[dailyData.length-1];
        last[type]+=value;
        last.total=last.food+last.fuel+last.snacks+last.bills+last.fun;
    } else {
        today[type]=value;
        today.total=today.food+today.fuel+today.snacks+today.bills+today.fun;
        dailyData.push(today);
    }
    saveUserData();
    calculate();
}

function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;saveUserData();calculate();}
function updateGoal(){goal=parseInt(document.getElementById('goalInput').value)||10000;saveUserData();calculate();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;saveUserData();calculate();}
function saveUserData(){users[currentUser].dailyData=dailyData;users[currentUser].salary=salary;users[currentUser].goal=goal;users[currentUser].loan=loan;localStorage.setItem('users',JSON.stringify(users));}
function clearAll(){if(confirm("Delete all data?")){dailyData=[];saveUserData();calculate();}}

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
    const remaining=salary-totalExpense;
    const currentSaving=Math.max(0,remaining);
    document.getElementById('totalExpense').innerText=totalExpense;
    document.getElementById('remaining').innerText=remaining;
    document.getElementById('currentSaving').innerText=currentSaving;
    const goalPercent=Math.min(Math.round((currentSaving/goal)*100),100);
    document.getElementById('goalStatus').innerText=goalPercent+'%';
}
</script>
</body>
</html>
