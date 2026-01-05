<Monthly>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>PocketTracker - Ultimate Dashboard</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body {
    font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg,#e0f7fa,#80deea);
    margin:0;
    padding:20px;
    color:#333;
}
h1,h2{text-align:center;color:#00796b;margin-bottom:10px;}
#datetime{text-align:center;font-weight:bold;margin-bottom:15px;color:#004d40;}
.dashboard{display:flex;flex-wrap:wrap;justify-content:center;margin-bottom:20px;}
.card{background:#fff;box-shadow:0 4px 12px rgba(0,0,0,0.15);border-radius:12px;margin:10px;padding:15px;text-align:center;width:120px;cursor:pointer;transition:0.3s;}
.card:hover{transform:scale(1.05);box-shadow:0 6px 16px rgba(0,0,0,0.25);}
.card span{display:block;font-size:24px;margin-bottom:5px;}
table{border-collapse: collapse;width:100%;margin-top:20px;background:#fff;border-radius:8px;overflow:hidden;box-shadow:0 4px 10px rgba(0,0,0,0.1);}
th,td{border:1px solid #e0e0e0;padding:8px;text-align:center;color:#333;}
th{background:#b2dfdb;}
.progress-bar{height:25px;background:#c8e6c9;border-radius:12px;overflow:hidden;margin:10px 0;}
.progress{height:100%;background:#00796b;width:0%;color:#fff;text-align:center;line-height:25px;font-weight:bold;transition:0.5s;}
input.salaryInput,input.loginInput,input.licenseInput,input.goalInput,input.loanInput{width:90px;text-align:center;border-radius:8px;border:2px solid #4a90e2;padding:5px;outline:none;transition:0.3s;margin-top:5px;}
input.salaryInput:focus,input.loginInput:focus,input.licenseInput:focus,input.goalInput:focus,input.loanInput:focus{border-color:#357ABD; box-shadow:0 0 5px #357ABD;}
#loginScreen,#licenseScreen,#dashboard{display:none;}
button{padding:5px 10px;margin-top:5px;border-radius:8px;border:none;background:#00796b;color:#fff;cursor:pointer;transition:0.3s;}
button:hover{background:#004d40;}
.infoBox{display:flex;flex-wrap:wrap;justify-content:space-around;margin:20px 0;background:#fff;padding:15px;border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.15);}
.infoBox div{text-align:center;background:linear-gradient(135deg,#81d4fa,#29b6f6);color:#fff;padding:10px;margin:5px;border-radius:10px;flex:1;min-width:120px;box-shadow:0 3px 8px rgba(0,0,0,0.15);}
</style>
</head>
<body>

<h1>💼 PocketTracker - Ultimate Dashboard 💼</h1>
<div id="datetime"></div>

<!-- Login Screen -->
<div id="loginScreen">
    <h2>Login</h2>
    Username: <input type="text" id="loginUser" class="loginInput"><br>
    Password: <input type="password" id="loginPass" class="loginInput"><br>
    <button onclick="login()">Login</button>
    <button onclick="register()">Register</button>
    <p id="loginMsg" style="color:red;"></p>
</div>

<!-- License Screen -->
<div id="licenseScreen">
    <h2>Enter License Key</h2>
    <input type="text" id="licenseInput" class="licenseInput" placeholder="XXXX-XXXX"><br>
    <button onclick="checkLicense()">Unlock Dashboard</button>
    <p id="licenseMsg" style="color:red;"></p>
</div>

<!-- Dashboard -->
<div id="dashboard">
    <div style="text-align:right;margin-bottom:10px;">
        <span id="welcomeUser" style="font-weight:bold;"></span>
        <button onclick="logout()">Logout</button>
    </div>

    <h2>💰 User Info & Goals</h2>
    <div class="infoBox">
        <div>Username<br><span id="infoUser">-</span></div>
        <div>Salary<br><input type="number" class="salaryInput" id="salaryInput" value="0" onchange="updateSalary()"></div>
        <div>Goal Saving<br><input type="number" class="goalInput" id="goalInput" value="10000" onchange="updateGoal()"></div>
        <div>Loan Amount<br><input type="number" class="loanInput" id="loanInput" value="0" onchange="updateLoan()"></div>
    </div>

    <h2>Dashboard Summary</h2>
    <div class="infoBox">
        <div>Total Expense<br><span id="totalExpense">0</span></div>
        <div>Remaining Balance<br><span id="remaining">0</span></div>
        <div>Current Saving<br><span id="currentSaving">0</span></div>
        <div>Goal Achieved<br><span id="goalStatus">0%</span></div>
    </div>

    <h2>Daily Expenses</h2>
    <div class="dashboard">
        <div class="card" title="Add Food" onclick="addDaily('food')"><span>🍔</span>Food</div>
        <div class="card" title="Add Fuel" onclick="addDaily('fuel')"><span>⛽</span>Fuel</div>
        <div class="card" title="Add Snacks" onclick="addDaily('snacks')"><span>🍿</span>Snacks</div>
        <div class="card" title="Add Bills" onclick="addDaily('bills')"><span>💡</span>Bills</div>
        <div class="card" title="Add Entertainment" onclick="addDaily('entertainment')"><span>🎮</span>Fun</div>
        <div class="card" title="Clear All Data" onclick="clearAll()"><span>🗑️</span>Clear</div>
        <div class="card" title="Export CSV" onclick="exportCSV()"><span>📊</span>Export</div>
    </div>

    <h2>Daily Expenses Table</h2>
    <table id="dailyTable">
    <tr>
    <th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th>
    <th>Daily Total</th><th>Total incl. Loan</th><th>Date</th>
    </tr>
    </table>

    <h2>Visual Overview</h2>
    <canvas id="chart" width="400" height="200"></canvas>
</div>

<script>
// Timer
function updateDateTime(){
    const now=new Date();
    const options={weekday:'long',year:'numeric',month:'long',day:'numeric'};
    document.getElementById('datetime').innerText=now.toLocaleDateString('en-US',options)+' | '+now.toLocaleTimeString();
}
setInterval(updateDateTime,1000);
updateDateTime();

// Multi-User Data
let currentUser=null;
let users = JSON.parse(localStorage.getItem('users')||"{}");
let dailyData=[],loanAmount=0,salary=0,goal=10000;

// Persistent login
window.onload=function(){
    const savedUser = localStorage.getItem('currentUser');
    if(savedUser && users[savedUser]){
        currentUser=savedUser;
        loadUserData();
    } else {
        document.getElementById('loginScreen').style.display='block';
    }
}

// Login/Register
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
        users[user]={pass:pass,license:false,dailyData:[],salary:0,goal:10000,loan:0};
        localStorage.setItem('users',JSON.stringify(users));
        document.getElementById('loginMsg').innerText='Registered! Login now.';
    }
}

// Load User Data
function loadUserData(){
    document.getElementById('loginScreen').style.display='none';
    if(!users[currentUser].license){
        document.getElementById('licenseScreen').style.display='block';
    } else {showDashboard();}
}

// License
function checkLicense(){
    const key=document.getElementById('licenseInput').value.trim();
    if(key==='ABCD-1234'){
        users[currentUser].license=true;
        localStorage.setItem('users',JSON.stringify(users));
        showDashboard();
    } else {document.getElementById('licenseMsg').innerText='Invalid License!';}
}

// Show Dashboard
function showDashboard(){
    document.getElementById('licenseScreen').style.display='none';
    document.getElementById('dashboard').style.display='block';
    document.getElementById('welcomeUser').innerText=`Welcome, ${currentUser}!`;
    const u=users[currentUser];
    dailyData=u.dailyData||[];
    salary=u.salary||0;
    goal=u.goal||10000;
    loanAmount=u.loan||0;
    document.getElementById('salaryInput').value=salary;
    document.getElementById('goalInput').value=goal;
    document.getElementById('loanInput').value=loanAmount;
    calculate();
}

// Logout
function logout(){
    localStorage.removeItem('currentUser');
    currentUser=null;
    document.getElementById('dashboard').style.display='none';
    document.getElementById('loginScreen').style.display='block';
}

// Add daily expense
function addDaily(type){
    const value=parseInt(prompt(`Enter ${type} amount in PKR:`,"0"))||0;
    const today={food:0,fuel:0,snacks:0,bills:0,entertainment:0,total:0,date:new Date().toLocaleDateString()};
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today.date){
        let last=dailyData[dailyData.length-1];
        last[type]+=value;
        last.total=last.food+last.fuel+last.snacks+last.bills+last.entertainment;
    } else {today[type]=value;today.total=today.food+today.fuel+today.snacks+today.bills+today.entertainment;dailyData.push(today);}
    saveUserData();
    calculate();
}

// Update salary, goal, loan
function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;saveUserData();calculate();}
function updateGoal(){goal=parseInt(document.getElementById('goalInput').value)||10000;saveUserData();calculate();}
function updateLoan(){loanAmount=parseInt(document.getElementById('loanInput').value)||0;saveUserData();calculate();}

// Save User Data
function saveUserData(){
    users[currentUser].dailyData=dailyData;
    users[currentUser].salary=salary;
    users[currentUser].goal=goal;
    users[currentUser].loan=loanAmount;
    localStorage.setItem('users',JSON.stringify(users));
}

// Clear all
function clearAll(){if(confirm("Delete all data?")){dailyData=[];saveUserData();calculate();}}

// Calculate & Update Dashboard
function calculate(){
    const table=document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Daily Total</th><th>Total incl. Loan</th><th>Date</th></tr>";
    let totalExpense=loanAmount;
    dailyData.forEach((day,index)=>{
        const row=table.insertRow();
        row.insertCell(0).innerText=index+1;
        row.insertCell(1).innerText=day.food;
        row.insertCell(2).innerText=day.fuel;
        row.insertCell(3).innerText=day.snacks;
        row.insertCell(4).innerText=day.bills;
        row.insertCell(5).innerText=day.entertainment;
        row.insertCell(6).innerText=day.total;
        const totalWithLoan=day.total + Math.round(loanAmount/dailyData.length||0);
        row.insertCell(7).innerText=totalWithLoan;
        row.insertCell(8).innerText=day.date;
        totalExpense+=day.total;
    });
    const remaining=salary-totalExpense;
    const currentSaving=Math.max(0,remaining);
    document.getElementById('totalExpense').innerText=totalExpense;
    document.getElementById('remaining').innerText=remaining;
    document.getElementById('currentSaving').innerText=currentSaving;
    const goalPercent=Math.min(Math.round((currentSaving/goal)*100),100);
    document.getElementById('goalStatus').innerText=goalPercent+'%';
    
    // Chart
    const ctx=document.getElementById('chart').getContext('2d');
    if(window.barChart) window.barChart.destroy();
    window.barChart=new Chart(ctx,{type:'bar',data:{labels:['Salary','Total Expense','Current Saving'],datasets:[{label:'PKR',data:[salary,totalExpense,currentSaving],backgroundColor:['#00796b','#f39c12','#2ecc71'],borderColor:['#004d40','#d35400','#27ae60'],borderWidth:2}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true}}}});
}

// Export CSV
function exportCSV(){
    let csv='Day,Food,Fuel,Snacks,Bills,Entertainment,Daily Total,Total incl. Loan,Date\n';
    dailyData.forEach((d,index)=>{
        const totalWithLoan=d.total + Math.round(loanAmount/dailyData.length||0);
        csv+=`${index+1},${d.food},${d.fuel},${d.snacks},${d.bills},${d.entertainment},${d.total},${totalWithLoan},${d.date}\n`;
    });
    let blob=new Blob([csv],{type:'text/csv'});
    let link=document.createElement('a');
    link.href=URL.createObjectURL(blob);
    link.download='PocketTracker_Ultimate.csv';
    link.click();
}
</script>
</body>
</html>
