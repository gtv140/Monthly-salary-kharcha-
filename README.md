<Monthly>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>PocketTracker Premium</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg,#f0f4f8,#d9e2ec);
    color: #333;
    margin:0; padding:20px;
}
h1,h2 {text-align:center;color:#1a1a1a;}
p.message{text-align:center;font-size:18px;margin-bottom:10px;color:#1a1a1a;}
#datetime{text-align:center;font-weight:bold;margin-bottom:20px;color:#555;}
input, select, textarea {
    padding:8px;margin:5px;border:2px solid #4a90e2;border-radius:8px;outline:none;transition:0.3s;
}
input:focus, select:focus, textarea:focus {border-color:#357ABD; box-shadow:0 0 5px #357ABD;}
button{
    padding:12px 24px;margin:10px 0;background:#4a90e2;color:#fff;font-weight:bold;border:none;border-radius:8px;cursor:pointer;transition:0.3s;
}
button:hover{background:#357ABD;transform:scale(1.05);}
table{
    border-collapse: collapse;width:100%;margin-top:20px;background:#fff;border-radius:8px;overflow:hidden;box-shadow:0 4px 10px rgba(0,0,0,0.1);
}
th,td{border:1px solid #e0e0e0;padding:12px;text-align:center;color:#333;}
th{background:#f5f7fa;font-weight:bold;}
td:hover{background:#f0f4f8;}
canvas{margin-top:20px;background:#fff;border-radius:12px;box-shadow:0 4px 10px rgba(0,0,0,0.1);}
label{display:inline-block;margin:5px;font-weight:bold;}
.progress-bar {height:25px;background:#e0e0e0;border-radius:12px;overflow:hidden;margin:10px 0;}
.progress {height:100%;background:#4a90e2;width:0%;color:#fff;text-align:center;line-height:25px;font-weight:bold;transition:0.5s;}
</style>
</head>
<body>

<h1>🌟 PocketTracker Premium Dashboard 🌟</h1>
<p class="message">Track your salary, categories, saving goal, daily notes, and progress automatically!</p>
<div id="datetime"></div>

<div style="text-align:center;">
  <label>Salary: <input type="number" id="salary" value="49800"></label>
  <label>Ghar Kharcha: <input type="number" id="house" value="20000"></label>
  <label>Loan: <input type="number" id="loan" value="10000"></label>
  <label>Daily Kharcha: <input type="number" id="daily" value="500"></label>
  <label>Savings Goal: <input type="number" id="goal" value="10000"></label>
  <br>
  <button onclick="calculate()">Calculate & Save</button>
  <button onclick="exportCSV()">Export CSV</button>
</div>

<h2>Monthly Overview</h2>
<table>
<tr><th>Total Salary</th><th>Total Kharcha</th><th>Remaining Saving</th><th>Goal Status</th></tr>
<tr>
<td id="totalSalary">0</td>
<td id="totalExpense">0</td>
<td id="remaining">0</td>
<td id="goalStatus">0%</td>
</tr>
</table>
<div class="progress-bar"><div id="progress" class="progress">0%</div></div>

<h2>Daily Expenses & Notes</h2>
<table id="dailyTable"><tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Note</th></tr></table>

<h2>Weekly Summary</h2>
<table>
<tr><th>Week</th><th>Kharcha</th><th>Saving</th></tr>
<tr><td>Week 1</td><td id="w1Expense">0</td><td id="w1Saving">0</td></tr>
<tr><td>Week 2</td><td id="w2Expense">0</td><td id="w2Saving">0</td></tr>
<tr><td>Week 3</td><td id="w3Expense">0</td><td id="w3Saving">0</td></tr>
<tr><td>Week 4</td><td id="w4Expense">0</td><td id="w4Saving">0</td></tr>
</table>

<h2>Visual Overview</h2>
<canvas id="chart" width="400" height="200"></canvas>

<script>
// Timer
function updateDateTime(){
    const now = new Date();
    const options = {weekday:'long', year:'numeric', month:'long', day:'numeric'};
    document.getElementById('datetime').innerText = now.toLocaleDateString('en-US', options) + ' | ' + now.toLocaleTimeString();
}
setInterval(updateDateTime,1000);
updateDateTime();

// Load from localStorage
window.onload = function(){
    if(localStorage.getItem('pocketTrackerData')){
        let data = JSON.parse(localStorage.getItem('pocketTrackerData'));
        document.getElementById('salary').value=data.salary;
        document.getElementById('house').value=data.house;
        document.getElementById('loan').value=data.loan;
        document.getElementById('daily').value=data.daily;
        document.getElementById('goal').value=data.goal;
        calculate();
    }
}

// Main Calculation
function calculate(){
    let salary=parseInt(document.getElementById('salary').value);
    let house=parseInt(document.getElementById('house').value);
    let loan=parseInt(document.getElementById('loan').value);
    let daily=parseInt(document.getElementById('daily').value);
    let goal=parseInt(document.getElementById('goal').value);

    const now=new Date();
    const totalDays=new Date(now.getFullYear(), now.getMonth()+1,0).getDate();

    // Category split
    let food=Math.round(daily*0.4);
    let fuel=Math.round(daily*0.2);
    let snacks=Math.round(daily*0.1);
    let bills=Math.round(daily*0.2);
    let entertainment=Math.round(daily*0.1);

    // Total expense
    let totalExpense=house+loan+daily*totalDays;
    let remaining=salary-totalExpense;

    document.getElementById('totalSalary').innerText=salary;
    document.getElementById('totalExpense').innerText=totalExpense;
    document.getElementById('remaining').innerText=remaining;

    // Goal %
    let goalPercent=Math.min(Math.round((remaining/goal)*100),100);
    document.getElementById('goalStatus').innerText=goalPercent+'%';
    document.getElementById('progress').style.width=goalPercent+'%';
    document.getElementById('progress').innerText=goalPercent+'%';

    // Daily Table
    let dailyTable=document.getElementById('dailyTable');
    dailyTable.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Total</th><th>Note</th></tr>";
    for(let i=1;i<=totalDays;i++){
        let row=dailyTable.insertRow();
        row.insertCell(0).innerText=i;
        row.insertCell(1).innerText=food;
        row.insertCell(2).innerText=fuel;
        row.insertCell(3).innerText=snacks;
        row.insertCell(4).innerText=bills;
        row.insertCell(5).innerText=entertainment;
        row.insertCell(6).innerText=food+fuel+snacks+bills+entertainment;
        row.insertCell(7).innerHTML='<input type="text" placeholder="Note">';
    }

    // Weekly summary (split month into 4 approx weeks)
    let weekDays=Math.ceil(totalDays/4);
    for(let i=1;i<=4;i++){
        let weekExpense=Math.round(daily*weekDays + house/4 + loan/4);
        let weekSaving=Math.round(salary/4 - weekExpense);
        document.getElementById('w'+i+'Expense').innerText=weekExpense;
        document.getElementById('w'+i+'Saving').innerText=weekSaving;
    }

    // Chart
    const ctx=document.getElementById('chart').getContext('2d');
    if(window.barChart) window.barChart.destroy();
    window.barChart=new Chart(ctx,{
        type:'bar',
        data:{
            labels:['Salary','Total Expense','Remaining Saving'],
            datasets:[{label:'PKR',data:[salary,totalExpense,remaining],backgroundColor:['#4a90e2','#f39c12','#2ecc71'],borderColor:['#357ABD','#d35400','#27ae60'],borderWidth:2}]
        },
        options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true}}}
    });

    // Save to localStorage
    localStorage.setItem('pocketTrackerData',JSON.stringify({salary,house,loan,daily,goal}));
}

// Export CSV
function exportCSV(){
    let csv='Day,Food,Fuel,Snacks,Bills,Entertainment,Total\n';
    const rows=document.querySelectorAll('#dailyTable tr');
    rows.forEach((row,index)=>{
        if(index>0){
            const cells=row.querySelectorAll('td');
            let rowData=[];
            cells.forEach(cell=>{rowData.push(cell.innerText);});
            csv+=rowData.join(',')+'\n';
        }
    });
    let blob=new Blob([csv],{type:'text/csv'});
    let link=document.createElement('a');
    link.href=URL.createObjectURL(blob);
    link.download='PocketTracker_Month.csv';
    link.click();
}
</script>

</body>
</html>
