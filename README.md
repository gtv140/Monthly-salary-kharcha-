<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker – Modern Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
/* ===== Base Styles ===== */
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:'Roboto',sans-serif;background:#f0f4f8;color:#333;}
h1,h2{text-align:center;margin:10px;font-weight:700;}
.container{width:90%;max-width:1200px;margin:20px auto;}
button{cursor:pointer;font-weight:700;border:none;border-radius:10px;padding:10px 15px;transition:0.3s;}
button:hover{opacity:0.85;}

/* ===== Header ===== */
header{
    background:linear-gradient(135deg,#6a11cb,#2575fc);
    padding:20px;
    text-align:center;
    font-size:28px;
    font-weight:bold;
    color:#fff;
    border-radius:12px;
    box-shadow:0 5px 15px rgba(0,0,0,0.3);
}

/* ===== Info Boxes ===== */
.infoBox{
    display:flex;
    flex-wrap:wrap;
    justify-content:space-around;
    margin:20px 0;
}
.infoBox .card{
    background:linear-gradient(145deg,#ff6a00,#ee0979);
    color:#fff;
    flex:1;
    min-width:150px;
    margin:10px;
    padding:20px;
    border-radius:15px;
    text-align:center;
    font-weight:bold;
    box-shadow:0 6px 15px rgba(0,0,0,0.25);
    transition:0.4s;
}
.infoBox .card:hover{transform:scale(1.05);}

/* ===== Dashboard Cards ===== */
.dashboard{display:flex;flex-wrap:wrap;justify-content:center;margin:20px 0;}
.dashboard .card{
    background:#fff;
    margin:10px;
    padding:20px;
    border-radius:12px;
    width:150px;
    text-align:center;
    font-weight:bold;
    box-shadow:0 6px 15px rgba(0,0,0,0.2);
    transition:0.4s;
    cursor:pointer;
}
.dashboard .card:hover{transform:scale(1.05);box-shadow:0 8px 20px rgba(0,0,0,0.3);}
.dashboard .card img{width:50px;height:50px;margin-bottom:10px;}

/* ===== Table ===== */
table{
    width:100%;
    border-collapse:collapse;
    margin-top:20px;
    border-radius:12px;
    overflow:hidden;
    box-shadow:0 6px 15px rgba(0,0,0,0.2);
}
th,td{padding:10px;text-align:center;}
th{background:#2575fc;color:#fff;}
tr:nth-child(even){background:#f2f2f2;}
tr:hover{background:#e0f0ff;}

/* ===== Responsive ===== */
@media(max-width:768px){
    .infoBox,.dashboard{flex-direction:column;align-items:center;}
}
</style>
</head>
<body>

<header>Pocket Tracker 💼</header>

<div class="container">
    <!-- User Info -->
    <div class="infoBox">
        <div class="card">
            <h3>Username</h3>
            <p id="username">SweetieUser</p>
        </div>
        <div class="card">
            <h3>Current Balance</h3>
            <p id="balance">0 PKR</p>
        </div>
        <div class="card">
            <h3>Salary</h3>
            <p><input type="number" id="salaryInput" value="0" onchange="updateSalary()"></p>
        </div>
        <div class="card">
            <h3>Loan</h3>
            <p><input type="number" id="loanInput" value="0" onchange="updateLoan()"></p>
        </div>
        <div class="card">
            <h3>Goal Saving</h3>
            <p><input type="number" id="goalInput" value="10000" onchange="updateGoal()"></p>
        </div>
    </div>

    <!-- Dashboard Options -->
    <div class="dashboard">
        <div class="card" onclick="addExpense('food')">
            <img src="https://images.unsplash.com/photo-1604908177523-05a13f79a49d?&w=100&h=100" alt="Food">
            <p>Food</p>
        </div>
        <div class="card" onclick="addExpense('fuel')">
            <img src="https://images.unsplash.com/photo-1579871580381-06aa38b96c80?&w=100&h=100" alt="Fuel">
            <p>Fuel</p>
        </div>
        <div class="card" onclick="addExpense('snacks')">
            <img src="https://images.unsplash.com/photo-1611080622986-8048e159f7cb?&w=100&h=100" alt="Snacks">
            <p>Snacks</p>
        </div>
        <div class="card" onclick="addExpense('bills')">
            <img src="https://images.unsplash.com/photo-1556155092-8707de31f9c0?&w=100&h=100" alt="Bills">
            <p>Bills</p>
        </div>
        <div class="card" onclick="addExpense('entertainment')">
            <img src="https://images.unsplash.com/photo-1519337265831-281ec6cc8514?&w=100&h=100" alt="Fun">
            <p>Fun</p>
        </div>
    </div>

    <!-- Daily Expenses Table -->
    <h2>Daily Expenses</h2>
    <table id="dailyTable">
        <tr>
            <th>Day</th>
            <th>Food</th>
            <th>Fuel</th>
            <th>Snacks</th>
            <th>Bills</th>
            <th>Fun</th>
            <th>Total</th>
            <th>Date</th>
        </tr>
    </table><script>
// ===== User & Expenses =====
let username="SweetieUser";
let dailyData=[];
let salary=0,loan=0,goal=10000;

// ===== Local Storage Load =====
if(localStorage.getItem('pocketData')){
    const saved=JSON.parse(localStorage.getItem('pocketData'));
    username=saved.username||username;
    dailyData=saved.dailyData||[];
    salary=saved.salary||0;
    loan=saved.loan||0;
    goal=saved.goal||10000;
    document.getElementById('username').innerText=username;
    document.getElementById('salaryInput').value=salary;
    document.getElementById('loanInput').value=loan;
    document.getElementById('goalInput').value=goal;
    calculate();
}

// ===== Update Inputs =====
function updateSalary(){salary=parseInt(document.getElementById('salaryInput').value)||0;saveData();calculate();}
function updateLoan(){loan=parseInt(document.getElementById('loanInput').value)||0;saveData();calculate();}
function updateGoal(){goal=parseInt(document.getElementById('goalInput').value)||10000;saveData();calculate();}

// ===== Save Data =====
function saveData(){
    localStorage.setItem('pocketData',JSON.stringify({username,dailyData,salary,loan,goal}));
}

// ===== Add Expense =====
function addExpense(type){
    const value=parseInt(prompt(`Enter ${type} expense in PKR:`,"0"))||0;
    const today=new Date().toLocaleDateString();
    let dayObj={food:0,fuel:0,snacks:0,bills:0,entertainment:0,total:0,date:today};
    if(dailyData.length>0 && dailyData[dailyData.length-1].date===today){
        let last=dailyData[dailyData.length-1];
        last[type]+=value;
        last.total=last.food+last.fuel+last.snacks+last.bills+last.entertainment;
    } else{
        dayObj[type]=value;
        dayObj.total=value;
        dailyData.push(dayObj);
    }
    saveData();
    calculate();
}

// ===== Calculate Totals & Update Dashboard =====
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
        row.insertCell(5).innerText=day.entertainment;
        row.insertCell(6).innerText=day.total;
        row.insertCell(7).innerText=day.date;
        totalExpense+=day.total;
    });
    const balance=salary-totalExpense;
    document.getElementById('balance').innerText=balance+" PKR";

    // ===== Chart =====
    const ctx=document.getElementById('chart')?.getContext('2d');
    if(ctx){
        if(window.barChart) window.barChart.destroy();
        window.barChart=new Chart(ctx,{
            type:'bar',
            data:{
                labels:['Salary','Total Expense','Remaining Balance'],
                datasets:[{
                    label:'PKR',
                    data:[salary,totalExpense,balance],
                    backgroundColor:['#2575fc','#ff6a00','#6a11cb']
                }]
            },
            options:{responsive:true,plugins:{legend:{display:false}}}
        });
    }
}

// ===== Create Chart Section =====
const chartSection=document.createElement('div');
chartSection.style.margin="30px auto";
chartSection.style.textAlign="center";
chartSection.innerHTML='<h2>Overview</h2><canvas id="chart" width="400" height="200"></canvas>';
document.querySelector('.container').appendChild(chartSection);

// ===== Initialize =====
calculate();
</script>
</body>
</html>
