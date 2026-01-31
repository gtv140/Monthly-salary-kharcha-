<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Traker</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
/* ===== Base Styles ===== */
body{
    font-family:'Roboto',sans-serif;
    margin:0;padding:0;
    background: linear-gradient(135deg,#0f2027,#203a43,#2c5364);
    color:#fff;
    overflow-x:hidden;
    transition:0.5s;
}
h1,h2{text-align:center;margin:10px;font-weight:700;}
#datetime{text-align:center;font-weight:bold;margin-bottom:15px;color:#ffeb3b;letter-spacing:1px;font-size:18px;}
button{cursor:pointer;transition:0.3s;font-weight:700;border:none;border-radius:10px;}
button:hover{opacity:0.85;}
input{padding:10px;border-radius:12px;border:2px solid #00bcd4;outline:none;margin:5px;width:150px;}
input:focus{border-color:#03a9f4;box-shadow:0 0 8px #03a9f4;}

/* ===== Header ===== */
header{
    background:linear-gradient(135deg,#1cb5e0,#000851);
    padding:20px;
    text-align:center;
    font-size:32px;
    font-weight:bold;
    color:#fff;
    text-shadow:2px 2px 8px rgba(0,0,0,0.4);
    border-bottom-left-radius:20px;
    border-bottom-right-radius:20px;
    box-shadow:0 5px 20px rgba(0,0,0,0.5);
}

/* ===== Dashboard Cards ===== */
.dashboard{display:flex;flex-wrap:wrap;justify-content:center;margin:20px 0;}
.card{
    backdrop-filter:blur(10px);
    background:rgba(255,255,255,0.1);
    border-radius:20px;
    margin:10px;
    padding:25px;
    text-align:center;
    width:150px;
    cursor:pointer;
    transition:0.5s;
    box-shadow:0 8px 25px rgba(0,0,0,0.3);
    font-weight:bold;
}
.card:hover{transform:scale(1.1);box-shadow:0 12px 35px rgba(0,0,0,0.5);}
.card span{font-size:28px;display:block;margin-bottom:10px;}

/* ===== Info Boxes ===== */
.infoBox{
    display:flex;
    flex-wrap:wrap;
    justify-content:space-around;
    margin:20px;
}
.infoBox div{
    text-align:center;
    background:rgba(255,255,255,0.1);
    backdrop-filter:blur(12px);
    color:#fff;
    padding:25px;
    margin:10px;
    border-radius:20px;
    flex:1;
    min-width:150px;
    box-shadow:0 10px 30px rgba(0,0,0,0.25);
    transition:0.4s;
    font-weight:bold;
}
.infoBox div:hover{transform:scale(1.05);}

/* ===== Table ===== */
table{
    border-collapse: collapse;
    width:100%;
    margin-top:20px;
    background:rgba(255,255,255,0.95);
    color:#333;
    border-radius:15px;
    overflow:hidden;
    box-shadow:0 6px 15px rgba(0,0,0,0.2);
}
th,td{border:1px solid #e0e0e0;padding:10px;text-align:center;}
th{background:#03a9f4;color:#fff;letter-spacing:1px;}
tr:nth-child(even){background:rgba(0,188,212,0.1);}
tr:hover{background:rgba(0,188,212,0.2);}

/* ===== Theme Toggle ===== */
#themeToggle{position:fixed;top:15px;right:15px;padding:10px 15px;background:#03a9f4;color:#fff;box-shadow:0 4px 10px rgba(0,0,0,0.3);}

/* ===== Modal ===== */
.modal{
    position:fixed;
    top:0;left:0;width:100%;height:100%;
    background:rgba(0,0,0,0.5);
    display:none;
    justify-content:center;
    align-items:center;
    z-index:10;
}
.modalContent{
    background:linear-gradient(135deg,#00bcd4,#2196f3);
    padding:30px;
    border-radius:25px;
    text-align:center;
    color:#fff;
    box-shadow:0 10px 30px rgba(0,0,0,0.5);
}

/* ===== Responsive ===== */
@media(max-width:600px){
    .infoBox{flex-direction:column;align-items:center;}
    .card{width:120px;padding:15px;}
    input{width:120px;}
}
</style>
</head>
<body>

<header>📊 Pocket Traker 📊</header>
<div id="datetime"></div>
<button id="themeToggle" onclick="toggleTheme()">Toggle Theme</button>

<!-- Username Input -->
<div id="usernameScreen" style="text-align:center;margin-top:30px;">
    <h2>Enter Your Name</h2>
    <input type="text" id="usernameInput" placeholder="Your Name">
    <button onclick="enterUsername()">Start Dashboard</button>
</div>

<!-- Dashboard -->
<div id="dashboard" style="display:none;">
    <div style="text-align:right;margin:10px 20px;">
        <span id="welcomeUser" style="font-weight:bold;font-size:18px;"></span>
    </div>

    <h2>💰 User Info</h2>
    <div class="infoBox">
        <div>Username<br><span id="infoUser">-</span></div>
        <div>Current Balance<br><span id="currentBalance">0</span></div>
        <div>Total Saving<br><span id="currentSaving">0</span></div>
        <div>Loan<br><span id="loanAmount">0</span></div>
    </div>

    <h2>Add Data</h2>
    <div class="dashboard" id="expenseCards">
        <div class="card" onclick="addDaily('food')"><span>🍔</span>Food</div>
        <div class="card" onclick="addDaily('fuel')"><span>⛽</span>Fuel</div>
        <div class="card" onclick="addDaily('snacks')"><span>🍿</span>Snacks</div>
        <div class="card" onclick="addDaily('bills')"><span>💡</span>Bills</div>
        <div class="card" onclick="addDaily('entertainment')"><span>🎮</span>Fun</div>
        <div class="card" onclick="updateSalary()">💰 Update Salary</div>
        <div class="card" onclick="updateLoan()">🏦 Update Loan</div>
        <div class="card" onclick="clearAll()">🗑️ Clear All</div>
    </div>

    <h2>Daily Expenses Table</h2>
    <table id="dailyTable">
        <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Daily Total</th><th>Date</th></tr>
    </table>

    <h2>Visual Overview</h2>
    <canvas id="chart" width="400" height="200"></canvas>
</div>

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
        document.body.style.background="#f1f1f1";
        document.body.style.color="#333";
        darkTheme=false;
    }else{
        document.body.style.background="linear-gradient(135deg,#0f2027,#203a43,#2c5364)";
        document.body.style.color="#fff";
        darkTheme=true;
    }
}

// ===== Local Storage & User =====
let username=null;
let dailyData=[],salary=0,loanAmount=0;

// Enter Username
function enterUsername(){
    username=document.getElementById('usernameInput').value.trim()||'User';
    localStorage.setItem('username',username);
    dailyData=JSON.parse(localStorage.getItem(username+'_data')||'[]');
    salary=parseInt(localStorage.getItem(username+'_salary'))||0;
    loanAmount=parseInt(localStorage.getItem(username+'_loan'))||0;
    document.getElementById('usernameScreen').style.display='none';
    document.getElementById('dashboard').style.display='block';
    document.getElementById('welcomeUser').innerText=`Welcome, ${username}!`;
    updateDashboard();
}

// Add Daily Expense
function addDaily(type){
    let value=parseInt(prompt(`Enter ${type} amount:`))||0;
    const today=new Date().toLocaleDateString();
    const dayObj={food:0,fuel:0,snacks:0,bills:0,entertainment:0,total:0,date:today};
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today){
        let last=dailyData[dailyData.length-1];
        last[type]+=value;
        last.total=last.food+last.fuel+last.snacks+last.bills+last.entertainment;
    } else {
        dayObj[type]=value;
        dayObj.total=dayObj.food+dayObj.fuel+dayObj.snacks+dayObj.bills+dayObj.entertainment;
        dailyData.push(dayObj);
    }
    localStorage.setItem(username+'_data',JSON.stringify(dailyData));
    updateDashboard();
}

// Update Salary
function updateSalary(){
    let s=parseInt(prompt("Enter Your Monthly Salary:"))||0;
    salary=s;
    localStorage.setItem(username+'_salary',salary);
    updateDashboard();
}

// Update Loan
function updateLoan(){
    let l=parseInt(prompt("Enter Loan Amount:"))||0;
    loanAmount=l;
    localStorage.setItem(username+'_loan',loanAmount);
    updateDashboard();
}

// Clear All
function clearAll(){
    if(confirm("Clear all data?")){
        dailyData=[];
        localStorage.setItem(username+'_data','[]');
        updateDashboard();
    }
}

// Update Dashboard
function updateDashboard(){
    let totalExp=loanAmount;
    let saving=0;
    const table=document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Fun</th><th>Daily Total</th><th>Date</th></tr>";
    dailyData.forEach((day,i)=>{
        const row=table.insertRow();
        row.insertCell(0).innerText=i+1;
        row.insertCell(1).innerText=day.food;
        row.insertCell(2).innerText=day.fuel;
        row.insertCell(3).innerText=day.snacks;
        row.insertCell(4).innerText=day.bills;
        row.insertCell(5).innerText=day.entertainment;
        row.insertCell(6).innerText=day.total;
        row.insertCell(7).innerText=day.date;
        totalExp+=day.total;
    });
    saving=Math.max(0,salary-totalExp);
    document.getElementById('infoUser').innerText=username;
    document.getElementById('currentBalance').innerText=salary-totalExp;
    document.getElementById('currentSaving').innerText=saving;
    document.getElementById('loanAmount').innerText=loanAmount;

    // Chart
    const ctx=document.getElementById('chart').getContext('2d');
    if(window.barChart) window.barChart.destroy();
    window.barChart=new Chart(ctx,{
        type:'bar',
        data:{
            labels:['Salary','Expenses','Saving'],
            datasets:[{
                label:'PKR',
                data:[salary,totalExp,saving],
                backgroundColor:['#03a9f4','#f44336','#4caf50']
            }]
        },
        options:{
            responsive:true,
            plugins:{legend:{display:false}}
        }
    });
}
</script>
</body>
</html>
