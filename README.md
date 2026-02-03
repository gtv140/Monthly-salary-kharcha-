<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker Mobile App</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
body{font-family:'Roboto',sans-serif;margin:0;padding:0;background:#f0f4f8;color:#333;transition:0.3s;}
body.dark{background:#1e1e2f;color:#eee;}
header{display:flex;justify-content:space-between;align-items:center;background:linear-gradient(135deg,#667eea,#764ba2);color:white;padding:20px;border-bottom-left-radius:20px;border-bottom-right-radius:20px;box-shadow:0 5px 15px rgba(0,0,0,0.2);}
header h1{font-size:1.8em;margin:0;}
.darkmode-btn{background:transparent;color:white;border:none;font-size:1.5em;cursor:pointer;}
.container{max-width:1000px;margin:20px auto;padding:0 15px;}
h2{text-align:center;color:#764ba2;margin-bottom:10px;}
#balance{background:#764ba2;color:white;font-size:1.3em;text-align:center;padding:15px;border-radius:15px;margin-bottom:20px;box-shadow:0 5px 15px rgba(0,0,0,0.1);}
body.dark #balance{background:#2b2b3f;color:#eee;}
.category-icons{display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin-bottom:15px;}
.category-icons button{flex:1;min-width:60px;padding:10px;border-radius:15px;border:none;cursor:pointer;font-size:1.2em;transition:0.3s;color:white;}
.food{background:#ff9800;} .fuel{background:#795548;} .entertainment{background:#ff5722;} .personal{background:#f44336;} .gambling{background:#9c27b0;} .saving{background:#2196f3;} .income{background:#4caf50;}
.category-icons button:hover{opacity:0.8;}
form{display:flex;flex-wrap:wrap;gap:10px;background:white;padding:15px;border-radius:15px;box-shadow:0 8px 20px rgba(0,0,0,0.1);margin-bottom:10px;}
form input, form select, form button{padding:10px;border-radius:10px;border:1px solid #ccc;font-size:1em;}
form button{background:#764ba2;color:white;border:none;cursor:pointer;flex:1;transition:0.3s;}
form button:hover{background:#667eea;}
body.dark form{background:#2b2b3f;border-color:#555;color:#eee;}
body.dark form input, body.dark form select{background:#2b2b3f;border-color:#555;color:#eee;}
.entry-card{background:white;padding:12px;margin-bottom:10px;border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.1);display:flex;justify-content:space-between;align-items:center;}
.entry-card span{flex:1;text-align:center;}
.delete-btn{background:#ff4d4f;color:white;padding:5px 10px;border:none;border-radius:8px;cursor:pointer;transition:0.3s;}
.delete-btn:hover{background:#d9363e;}
body.dark .entry-card{background:#2b2b3f;color:#eee;}
.totals{margin:10px 0;font-weight:bold;text-align:right;}
.chart-container{background:white;padding:15px;border-radius:15px;box-shadow:0 8px 20px rgba(0,0,0,0.1);margin-bottom:20px;}
body.dark .chart-container{background:#2b2b3f;}
button.export-btn{padding:8px 15px;background:#4caf50;color:white;border:none;border-radius:10px;cursor:pointer;margin-right:5px;}
button.export-btn:hover{background:#388e3c;}
@media(max-width:600px){.entry-card{flex-direction:column;gap:5px;}}
</style>
</head>
<body>

<header>
<h1>Pocket Tracker</h1>
<button class="darkmode-btn" onclick="toggleDarkMode()">🌓</button>
</header>

<div class="container">
<div id="balance">Balance: 0</div>

<h2>Add New Entry</h2>
<div class="category-icons">
<button class="income" onclick="selectCategory('Income')">💵</button>
<button class="saving" onclick="selectCategory('Saving')">💰</button>
<button class="food" onclick="selectCategory('Food')">🍔</button>
<button class="entertainment" onclick="selectCategory('Entertainment')">🎬</button>
<button class="fuel" onclick="selectCategory('Fuel')">⛽</button>
<button class="personal" onclick="selectCategory('Personal')">💸</button>
<button class="gambling" onclick="selectCategory('Gambling')">🎲</button>
</div>

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

<div id="entriesList"></div>
<div class="totals" id="totals"></div>

<div class="chart-container">
<h2>Summary Charts</h2>
<canvas id="summaryChart" style="margin-bottom:15px;"></canvas>
<canvas id="pieChart"></canvas>
</div>

<button class="export-btn" onclick="exportCSV()">📄 CSV</button>
<button class="export-btn" onclick="exportPDF()">📑 PDF</button>
</div>

<script>
let entries=JSON.parse(localStorage.getItem('entries'))||[];

function toggleDarkMode(){ document.body.classList.toggle('dark'); }

function selectCategory(cat){document.getElementById('type').value=cat;}

function saveData(){localStorage.setItem('entries',JSON.stringify(entries));}

function updateBalance(){
let totals={Income:0,Saving:0,Food:0,Entertainment:0,Fuel:0,Personal:0,Gambling:0};
entries.forEach(e=>{totals[e.type]?totals[e.type]+=Number(e.amount):null;});
let balance=totals.Income+totals.Saving-(totals.Food+totals.Entertainment+totals.Fuel+totals.Personal+totals.Gambling);
document.getElementById('balance').innerText=`Balance: ${balance}`;
document.getElementById('totals').innerText=`Income:${totals.Income} | Saving:${totals.Saving} | Food:${totals.Food} | Entertainment:${totals.Entertainment} | Fuel:${totals.Fuel} | Personal:${totals.Personal} | Gambling:${totals.Gambling}`;
return totals;
}

function updateEntriesList(filtered=null){
const data=filtered||entries;
const container=document.getElementById('entriesList');
container.innerHTML='';
data.forEach((e,i)=>{
const card=document.createElement('div');
card.className='entry-card';
card.innerHTML=`<span>${e.date}</span><span>${e.type}</span><span>${e.amount}</span><span>${e.note}</span><button class="delete-btn" onclick="deleteEntry(${i})">❌</button>`;
container.appendChild(card);
});
updateCharts();
updateBalance();
}

function deleteEntry(index){if(confirm("Are you sure?")){entries.splice(index,1);saveData();updateEntriesList();}}

document.getElementById('trackerForm').addEventListener('submit',function(e){
e.preventDefault();
const entry={date:document.getElementById('date').value,type:document.getElementById('type').value,amount:document.getElementById('amount').value,note:document.getElementById('note').value};
entries.push(entry);saveData();this.reset();updateEntriesList();
});

// ===== Search =====
document.getElementById('searchBar')?.addEventListener('input',function(){
const q=this.value.toLowerCase();
const filtered=entries.filter(e=>e.date.includes(q)||e.type.toLowerCase().includes(q)||e.note.toLowerCase().includes(q));
updateEntriesList(filtered);
});

// ===== Charts =====
const ctx=document.getElementById('summaryChart').getContext('2d');
const ctxPie=document.getElementById('pieChart').getContext('2d');
let summaryChart,pieChart;

function updateCharts(){
let totals={Income:0,Saving:0,Food:0,Entertainment:0,Fuel:0,Personal:0,Gambling:0};
entries.forEach(e=>{if(totals[e.type]!==undefined) totals[e.type]+=Number(e.amount);});
if(summaryChart) summaryChart.destroy();
summaryChart=new Chart(ctx,{type:'bar',data:{labels:['Income','Saving','Food','Entertainment','Fuel','Personal','Gambling'],datasets:[{label:'Amount',data:[totals.Income,totals.Saving,totals.Food,totals.Entertainment,totals.Fuel,totals.Personal,totals.Gambling],backgroundColor:['#4caf50','#2196f3','#ff9800','#ff5722','#795548','#f44336','#9c27b0']}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true}}}});
if(pieChart) pieChart.destroy();
pieChart=new Chart(ctxPie,{type:'pie',data:{labels:['Income','Saving','Food','Entertainment','Fuel','Personal','Gambling'],datasets:[{data:[totals.Income,totals.Saving,totals.Food,totals.Entertainment,totals.Fuel,totals.Personal,totals.Gambling],backgroundColor:['#4caf50','#2196f3','#ff9800','#ff5722','#795548','#f44336','#9c27b0']}]},options:{responsive:true}});
}

// ===== Alerts =====
function checkAlerts(){
let totals={Personal:0,Gambling:0};
entries.forEach(e=>{ if(e.type==='Personal') totals.Personal+=Number(e.amount); if(e.type==='Gambling') totals.Gambling+=Number(e.amount); });
if(totals.Personal>10000) alert("Warning: Personal spending limit exceeded!");
if(totals.Gambling>5000) alert("Warning: Gambling spending limit exceeded!");
}

// ===== Export CSV/PDF =====
function exportCSV(){let csv='Date,Type,Amount,Note\n';entries.forEach(e=>{csv+=`${e.date},${e.type},${e.amount},${e.note}\n`;});const blob=new Blob([csv],{type:'text/csv'});const link=document.createElement('a');link.href=URL.createObjectURL(blob);link.download='pocket_tracker.csv';link.click();}
function exportPDF(){const { jsPDF } = window.jspdf;const doc=new jsPDF();let y=10;doc.setFontSize(12);doc.text('Pocket Tracker',105,10,{align:'center'});entries.forEach(e=>{y+=10;doc.text(`${e.date} | ${e.type} | ${e.amount} | ${e.note}`,10,y);});doc.save('pocket_tracker.pdf');}

// ===== Initial Load =====
updateEntriesList();
</script>

</body>
</html>
