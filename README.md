<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker Ultimate Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
body{font-family:'Roboto',sans-serif;margin:0;padding:0;background:#f0f4f8;color:#333;transition:0.3s;}
body.dark{background:#1e1e2f;color:#eee;}
header{background:linear-gradient(135deg,#667eea,#764ba2);color:white;text-align:center;padding:50px 20px;border-bottom-left-radius:20px;border-bottom-right-radius:20px;box-shadow:0 5px 15px rgba(0,0,0,0.2);}
header h1{font-size:2.5em;margin:0;}
header p{font-size:1.1em;margin-top:5px;opacity:0.9;}
.container{max-width:1000px;margin:-30px auto 50px;padding:0 15px;}
h2{text-align:center;color:#764ba2;margin-bottom:15px;}
form{display:flex;flex-wrap:wrap;gap:10px;background:white;padding:15px;border-radius:15px;box-shadow:0 8px 20px rgba(0,0,0,0.1);margin-bottom:15px;}
form input, form select, form button{padding:10px;border-radius:10px;border:1px solid #ccc;font-size:1em;}
form button{background:#764ba2;color:white;border:none;cursor:pointer;flex:1;transition:0.3s;}
form button:hover{background:#667eea;}
body.dark form{background:#2b2b3f;border-color:#555;color:#eee;}
body.dark form input, body.dark form select{background:#2b2b3f;border-color:#555;color:#eee;}
#searchBar{width:100%;padding:10px;margin-bottom:15px;border-radius:10px;border:1px solid #ccc;font-size:1em;}
body.dark #searchBar{background:#2b2b3f;border-color:#555;color:#eee;}
table{width:100%;border-collapse: collapse;border-radius:15px;overflow:hidden;box-shadow:0 8px 20px rgba(0,0,0,0.1);background:white;}
th, td{padding:12px;text-align:center;}
th{background:#764ba2;color:white;font-weight:700;}
tr:nth-child(even){background:#f9f9f9;}
tr:hover{background:#e0e0ff;transition:0.3s;}
button.delete-btn{background:#ff4d4f;color:white;padding:5px 10px;border:none;border-radius:8px;cursor:pointer;transition:0.3s;}
button.delete-btn:hover{background:#d9363e;}
body.dark table{background:#2b2b3f;color:#eee;}
body.dark th{background:#764ba2;}
body.dark tr:nth-child(even){background:#33334d;}
body.dark tr:hover{background:#55557a;}
.totals{margin-top:15px;text-align:right;font-weight:bold;font-size:1em;}
.chart-container{margin-top:30px;background:white;padding:15px;border-radius:15px;box-shadow:0 8px 20px rgba(0,0,0,0.1);margin-bottom:20px;}
body.dark .chart-container{background:#2b2b3f;}
button.export-btn, button.darkmode-btn{margin-top:10px;padding:8px 18px;border:none;border-radius:10px;cursor:pointer;transition:0.3s;}
button.export-btn{background:#4caf50;color:white;}
button.export-btn:hover{background:#388e3c;}
button.darkmode-btn{background:#33334d;color:white;margin-left:10px;}
button.darkmode-btn:hover{background:#55557a;}
@media(max-width:600px){form{flex-direction:column;}}
</style>
</head>
<body>

<header>
<h1>Pocket Tracker Ultimate Pro</h1>
<p>Daily, Weekly, Monthly aur Lifetime Expenses Track Karein</p>
<button class="darkmode-btn" onclick="toggleDarkMode()">Toggle Dark Mode</button>
</header>

<div class="container">
<h2>Add New Entry</h2>
<form id="trackerForm">
<input type="date" id="date" required>
<select id="type" required>
<option value="">Select Type</option>
<option value="Income">Income</option>
<option value="Saving">Saving</option>
<option value="Food">Food</option>
<option value="Entertainment">Entertainment</option>
<option value="Fuel">Fuel</option>
<option value="Personal">Personal</option>
<option value="Gambling">Gambling</option>
</select>
<input type="number" id="amount" placeholder="Amount" required>
<input type="text" id="note" placeholder="Note">
<button type="submit">Add Entry</button>
</form>

<input type="text" id="searchBar" placeholder="Search by date, type or note...">

<h2>All Entries</h2>
<table id="entriesTable">
<thead><tr><th>Date</th><th>Type</th><th>Amount</th><th>Note</th><th>Action</th></tr></thead>
<tbody></tbody>
</table>

<div class="totals" id="totals"></div>

<div class="chart-container">
<h2>Summary Charts</h2>
<canvas id="summaryChart" style="margin-bottom:20px;"></canvas>
<canvas id="pieChart"></canvas>
</div>

<button class="export-btn" onclick="exportCSV()">Export CSV</button>
</div>

<script>
let entries=JSON.parse(localStorage.getItem('entries'))||[];

// ===== Dark Mode Toggle =====
function toggleDarkMode(){ document.body.classList.toggle('dark'); }

// ===== Save Data =====
function saveData(){ localStorage.setItem('entries',JSON.stringify(entries)); }

// ===== Update Table & Balance =====
function updateTable(filteredEntries=null){
const data=filteredEntries||entries;
const tbody=document.querySelector('#entriesTable tbody');
tbody.innerHTML='';
let totals={Income:0, Saving:0, Food:0, Entertainment:0, Fuel:0, Personal:0, Gambling:0};
data.forEach((entry,index)=>{
const tr=document.createElement('tr');
tr.innerHTML=`<td>${entry.date}</td><td>${entry.type}</td><td>${entry.amount}</td><td>${entry.note}</td>
<td><button class="delete-btn" onclick="deleteEntry(${index})">Delete</button></td>`;
tbody.appendChild(tr);
if(totals[entry.type]!==undefined) totals[entry.type]+=Number(entry.amount);
});
let balance=totals.Income+totals.Saving-(totals.Food+totals.Entertainment+totals.Fuel+totals.Personal+totals.Gambling);
document.getElementById('totals').innerText=
`Income:${totals.Income} | Saving:${totals.Saving} | Food:${totals.Food} | Entertainment:${totals.Entertainment} | Fuel:${totals.Fuel} | Personal:${totals.Personal} | Gambling:${totals.Gambling} | Balance:${balance}`;
updateCharts();
checkAlerts();
}

// ===== Delete Entry =====
function deleteEntry(index){ if(confirm("Are you sure?")){ entries.splice(index,1); saveData(); updateTable(); } }

// ===== Add Entry =====
document.getElementById('trackerForm').addEventListener('submit',function(e){
e.preventDefault();
const entry={
date:document.getElementById('date').value,
type:document.getElementById('type').value,
amount:document.getElementById('amount').value,
note:document.getElementById('note').value
};
entries.push(entry); saveData(); this.reset(); updateTable();
});

// ===== Search =====
document.getElementById('searchBar').addEventListener('input',function(){
const q=this.value.toLowerCase();
const filtered=entries.filter(e=>e.date.includes(q)||e.type.toLowerCase().includes(q)||e.note.toLowerCase().includes(q));
updateTable(filtered);
});

// ===== Charts =====
const ctx=document.getElementById('summaryChart').getContext('2d');
const ctxPie=document.getElementById('pieChart').getContext('2d');
let summaryChart,pieChart;

function updateCharts(){
let totals={Income:0, Saving:0, Food:0, Entertainment:0, Fuel:0, Personal:0, Gambling:0};
entries.forEach(e=>{ if(totals[e.type]!==undefined) totals[e.type]+=Number(e.amount); });
if(summaryChart) summaryChart.destroy();
summaryChart=new Chart(ctx,{type:'bar',data:{labels:['Income','Saving','Food','Entertainment','Fuel','Personal','Gambling'],datasets:[{label:'Amount',data:[totals.Income,totals.Saving,totals.Food,totals.Entertainment,totals.Fuel,totals.Personal,totals.Gambling],backgroundColor:['#4caf50','#2196f3','#ffeb3b','#ff9800','#795548','#ff5722','#9c27b0']}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true}}}});
if(pieChart) pieChart.destroy();
pieChart=new Chart(ctxPie,{type:'pie',data:{labels:['Income','Saving','Food','Entertainment','Fuel','Personal','Gambling'],datasets:[{data:[totals.Income,totals.Saving,totals.Food,totals.Entertainment,totals.Fuel,totals.Personal,totals.Gambling],backgroundColor:['#4caf50','#2196f3','#ffeb3b','#ff9800','#795548','#ff5722','#9c27b0']}]},options:{responsive:true}});
}

// ===== Alerts =====
function checkAlerts(){
let totals={Personal:0,Gambling:0};
entries.forEach(e=>{ if(e.type==='Personal') totals.Personal+=Number(e.amount); if(e.type==='Gambling') totals.Gambling+=Number(e.amount); });
if(totals.Personal>10000) alert("Warning: Personal spending limit exceeded!");
if(totals.Gambling>5000) alert("Warning: Gambling spending limit exceeded!");
}

// ===== Export CSV =====
function exportCSV(){
let csv='Date,Type,Amount,Note\n';
entries.forEach(e=>{ csv+=`${e.date},${e.type},${e.amount},${e.note}\n`; });
const blob=new Blob([csv],{type:'text/csv'});
const link=document.createElement('a');
link.href=URL.createObjectURL(blob);
link.download='pocket_tracker.csv';
link.click();
}

// ===== Initial Load =====
updateTable();
</script>
</body>
</html>
