<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker Mobile Friendly</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
body{font-family:'Roboto',sans-serif;margin:0;padding:0;background:#f7f9fc;color:#333;transition:0.3s;}
body.dark{background:#1c1c2b;color:#eee;}
header{display:flex;justify-content:space-between;align-items:center;background:linear-gradient(135deg,#667eea,#764ba2);color:white;padding:15px;border-bottom-left-radius:15px;border-bottom-right-radius:15px;box-shadow:0 5px 10px rgba(0,0,0,0.2);}
header h1{font-size:1.5em;margin:0;}
.user-balance{font-size:1em;}
.darkmode-btn{background:transparent;color:white;border:none;font-size:1.5em;cursor:pointer;}
.container{max-width:1000px;margin:15px auto;padding:0 10px;}
.banner{background:#764ba2;color:white;text-align:center;padding:12px;border-radius:12px;margin-bottom:15px;box-shadow:0 5px 15px rgba(0,0,0,0.1);}
body.dark .banner{background:#2b2b3f;color:#eee;}
.category-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(60px,1fr));gap:8px;margin-bottom:12px;}
.category-grid button{padding:10px;border-radius:12px;border:none;cursor:pointer;font-size:1em;display:flex;flex-direction:column;align-items:center;justify-content:center;color:white;transition:0.3s;}
.category-grid button:hover{opacity:0.85;transform:scale(1.05);}
.food{background:#ff9800;} .fuel{background:#795548;} .entertainment{background:#ff5722;} .personal{background:#f44336;} .gambling{background:#9c27b0;} .saving{background:#2196f3;} .income{background:#4caf50;}
.bills{background:#00bcd4;} .loans{background:#607d8b;} .transport{background:#9c27b0;} .shopping{background:#e91e63;} .rent{background:#3f51b5;} .education{background:#ffeb3b;} .medical{background:#009688;} .other{background:#795548;}
.more-btn{background:#333;color:white;}
form{display:flex;flex-direction:column;gap:8px;background:white;padding:12px;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.1);margin-bottom:10px;}
form input, form select, form button{padding:10px;border-radius:10px;border:1px solid #ccc;font-size:1em;width:100%;}
form button{background:#764ba2;color:white;border:none;cursor:pointer;transition:0.3s;}
form button:hover{background:#667eea;}
body.dark form{background:#2b2b3f;border-color:#555;color:#eee;}
body.dark form input, body.dark form select{background:#2b2b3f;border-color:#555;color:#eee;}
.entry-card{background:white;padding:10px;margin-bottom:8px;border-radius:12px;box-shadow:0 4px 10px rgba(0,0,0,0.1);display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;transition:0.3s;}
.entry-card:hover{transform:translateY(-2px);}
.entry-card span{flex:1;text-align:center;font-size:0.9em;}
.delete-btn{background:#ff4d4f;color:white;padding:5px 8px;border:none;border-radius:8px;cursor:pointer;transition:0.3s;}
.delete-btn:hover{background:#d9363e;}
body.dark .entry-card{background:#2b2b3f;color:#eee;}
.totals{margin:8px 0;font-weight:bold;text-align:right;font-size:0.9em;}
.chart-container{background:white;padding:12px;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.1);margin-bottom:15px;}
body.dark .chart-container{background:#2b2b3f;}
button.export-btn{padding:6px 12px;background:#4caf50;color:white;border:none;border-radius:10px;cursor:pointer;margin-right:5px;font-size:0.9em;}
button.export-btn:hover{background:#388e3c;}
.bottom-nav{position:fixed;bottom:0;width:100%;display:flex;justify-content:space-around;background:#764ba2;padding:10px 0;border-top-left-radius:15px;border-top-right-radius:15px;box-shadow:0 -5px 15px rgba(0,0,0,0.2);}
.bottom-nav button{background:transparent;border:none;color:white;font-size:1.5em;cursor:pointer;}
@media(max-width:500px){.category-grid{grid-template-columns:repeat(3,1fr);} .entry-card span{font-size:0.8em;}}
</style>
</head>
<body>

<header>
<h1>Pocket Tracker</h1>
<div class="user-balance" id="userBalance">Username: Khan | Balance: 0</div>
<button class="darkmode-btn" onclick="toggleDarkMode()">🌓</button>
</header>

<div class="container">
<div class="banner">💡 Save Smart, Live Better! Track your daily financial life.</div>

<h2>Categories</h2>
<div class="category-grid" id="categoryGrid">
<button class="income" onclick="selectCategory('Income')">💵<br>Income</button>
<button class="saving" onclick="selectCategory('Saving')">💰<br>Saving</button>
<button class="food" onclick="selectCategory('Food')">🍔<br>Food</button>
<button class="fuel" onclick="selectCategory('Fuel')">⛽<br>Fuel</button>
<button class="entertainment" onclick="selectCategory('Entertainment')">🎬<br>Entertainment</button>
<button class="personal" onclick="selectCategory('Personal')">💸<br>Personal</button>
<button class="gambling" onclick="selectCategory('Gambling')">🎲<br>Gambling</button>
<button class="bills" onclick="selectCategory('Bills')">💡<br>Bills</button>
<button class="loans" onclick="selectCategory('Loans')">💳<br>Loans</button>
<button class="transport" onclick="selectCategory('Transport')">🚗<br>Transport</button>
<button class="shopping" onclick="selectCategory('Shopping')">🛒<br>Shopping</button>
<button class="rent" onclick="selectCategory('Rent')">🏠<br>Rent</button>
<button class="education" onclick="selectCategory('Education')">📚<br>Education</button>
<button class="medical" onclick="selectCategory('Medical')">💊<br>Medical</button>
<button class="other" onclick="selectCategory('Other')">📝<br>Other</button>
<button class="more-btn" onclick="openMoreModal()">≡<br>More</button>
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
<option value="Bills">Bills</option>
<option value="Loans">Loans</option>
<option value="Transport">Transport</option>
<option value="Shopping">Shopping</option>
<option value="Rent">Rent</option>
<option value="Education">Education</option>
<option value="Medical">Medical</option>
<option value="Other">Other</option>
</select>
<input type="number" id="amount" placeholder="Amount" required>
<input type="text" id="note" placeholder="Note">
<button type="submit">Add Entry</button>
</form>

<div id="entriesList"></div>
<div class="totals" id="totals"></div>

<div class="chart-container">
<h2>Summary Charts</h2>
<canvas id="summaryChart" style="margin-bottom:12px;"></canvas>
<canvas id="pieChart"></canvas>
</div>

<button class="export-btn" onclick="exportCSV()">📄 CSV</button>
<button class="export-btn" onclick="exportPDF()">📑 PDF</button>

<div class="bottom-nav">
<button onclick="scrollToDashboard()">🏠</button>
<button onclick="scrollToForm()">➕</button>
<button onclick="scrollToCharts()">📊</button>
<button onclick="exportCSV()">📄</button>
<button onclick="toggleDarkMode()">🌓</button>
</div>
</div>

<script>
let entries=JSON.parse(localStorage.getItem('entries'))||[];
let username="Khan";

function toggleDarkMode(){ document.body.classList.toggle('dark'); }
function selectCategory(cat){document.getElementById('type').value=cat;}
function saveData(){localStorage.setItem('entries',JSON.stringify(entries));}

function updateBalance(){
let totals={Income:0,Saving:0,Food:0,Entertainment:0,Fuel:0,Personal:0,Gambling:0,Bills:0,Loans:0,Transport:0,Shopping:0,Rent:0,Education:0,Medical:0,Other:0};
entries.forEach(e=>{totals[e.type]?totals[e.type]+=Number(e.amount):null;});
let balance=totals.Income+totals.Saving-(totals.Food+totals.Entertainment+totals.Fuel+totals.Personal+totals.Gambling+totals.Bills+totals.Loans+totals.Transport+totals.Shopping+totals.Rent+totals.Education+totals.Medical+totals.Other);
document.getElementById('userBalance').innerText=`Username: ${username} | Balance: ${balance}`;
document.getElementById('totals').innerText=`Income:${totals.Income} | Saving:${totals.Saving} | Food:${totals.Food} | Entertainment:${totals.Entertainment} | Fuel:${totals.Fuel} | Personal:${totals.Personal} | Gambling:${totals.Gambling} | Bills:${totals.Bills} | Loans:${totals.Loans} | Transport:${totals.Transport} | Shopping:${totals.Shopping} | Rent:${totals.Rent} | Education:${totals.Education} | Medical:${totals.Medical} | Other:${totals.Other}`;
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

// ===== Charts =====
const ctx=document.getElementById('summaryChart').getContext('2d');
const ctxPie=document.getElementById('pieChart').getContext('2d');
let summaryChart,pieChart;

function updateCharts(){
let totals={Income:0,Saving:0,Food:0,Entertainment:0,Fuel:0,Personal:0,Gambling:0,Bills:0,Loans:0,Transport:0,Shopping:0,Rent:0,Education:0,Medical:0,Other:0};
entries.forEach(e=>{if(totals[e.type]!==undefined) totals[e.type]+=Number(e.amount);});
if(summaryChart) summaryChart.destroy();
summaryChart=new Chart(ctx,{type:'bar',data:{labels:['Income','Saving','Food','Entertainment','Fuel','Personal','Gambling','Bills','Loans','Transport','Shopping','Rent','Education','Medical','Other'],datasets:[{label:'Amount',data:[totals.Income,totals.Saving,totals.Food,totals.Entertainment,totals.Fuel,totals.Personal,totals.Gambling,totals.Bills,totals.Loans,totals.Transport,totals.Shopping,totals.Rent,totals.Education,totals.Medical,totals.Other],backgroundColor:['#4caf50','#2196f3','#ff9800','#ff5722','#795548','#f44336','#9c27b0','#00bcd4','#607d8b','#9c27b0','#e91e63','#3f51b5','#ffeb3b','#009688','#795548']}]},options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true}}}});
if(pieChart) pieChart.destroy();
pieChart=new Chart(ctxPie,{type:'pie',data:{labels:['Income','Saving','Food','Entertainment','Fuel','Personal','Gambling','Bills','Loans','Transport','Shopping','Rent','Education','Medical','Other'],datasets:[{data:[totals.Income,totals.Saving,totals.Food,totals.Entertainment,totals.Fuel,totals.Personal,totals.Gambling,totals.Bills,totals.Loans,totals.Transport,totals.Shopping,totals.Rent,totals.Education,totals.Medical,totals.Other],backgroundColor:['#4caf50','#2196f3','#ff9800','#ff5722','#795548','#f44336','#9c27b0','#00bcd4','#607d8b','#9c27b0','#e91e63','#3f51b5','#ffeb3b','#009688','#795548']}]},options:{responsive:true}});
}

// ===== Export CSV/PDF =====
function exportCSV(){let csv='Date,Type,Amount,Note\n';entries.forEach(e=>{csv+=`${e.date},${e.type},${e.amount},${e.note}\n`;});const blob=new Blob([csv],{type:'text/csv'});const link=document.createElement('a');link.href=URL.createObjectURL(blob);link.download='pocket_tracker.csv';link.click();}
function exportPDF(){const { jsPDF } = window.jspdf;const doc=new jsPDF();let y=10;doc.setFontSize(12);doc.text('Pocket Tracker Daily Life',105,10,{align:'center'});entries.forEach(e=>{y+=10;doc.text(`${e.date} | ${e.type} | ${e.amount} | ${e.note}`,10,y);});doc.save('pocket_tracker.pdf');}

// ===== Navigation Scroll =====
function scrollToDashboard(){window.scrollTo({top:0,behavior:'smooth'});}
function scrollToForm(){document.getElementById('trackerForm').scrollIntoView({behavior:'smooth'});}
function scrollToCharts(){document.querySelector('.chart-container').scrollIntoView({behavior:'smooth'});}

// ===== More Modal (Placeholder) =====
function openMoreModal(){alert("More categories can be added here!");}

// ===== Initial Load =====
updateEntriesList();
</script>
</body>
</html>
