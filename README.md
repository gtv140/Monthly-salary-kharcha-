<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body{margin:0;font-family:sans-serif;background:#f4f6f9;color:#333;transition:.3s;}
body.dark{background:#121212;color:#eee;}
.container{max-width:480px;margin:auto;padding:12px;}
header{display:flex;justify-content:space-between;align-items:center;background:linear-gradient(135deg,#667eea,#764ba2);color:white;padding:12px;border-radius:12px 12px 0 0;}
header h1{font-size:1.2em;}
header .balance{font-size:0.9em;}
header button{background:none;border:none;color:white;font-size:1.3em;cursor:pointer;}
.banner{background:#764ba2;color:white;padding:10px;border-radius:12px;margin:10px 0;text-align:center;font-weight:500;}
.category-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:10px;}
.category-grid button{padding:10px;border:none;border-radius:12px;color:white;font-weight:500;font-size:0.8em;display:flex;flex-direction:column;align-items:center;justify-content:center;cursor:pointer;transition:.2s;}
.category-grid button:hover{transform:scale(1.05);}
.income{background:#4caf50;} .saving{background:#2196f3;} .food{background:#ff9800;} .fuel{background:#795548;} .entertainment{background:#ff5722;} .personal{background:#f44336;} .gambling{background:#9c27b0;} 
.bills{background:#00bcd4;} .loans{background:#607d8b;} .transport{background:#9c27b0;} .shopping{background:#e91e63;} .rent{background:#3f51b5;} .education{background:#ffeb3b;} .medical{background:#009688;} .other{background:#795548;}
form{display:flex;flex-direction:column;gap:6px;background:white;padding:10px;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.1);margin-bottom:10px;}
form input, form select, form button{padding:10px;border-radius:10px;border:1px solid #ccc;font-size:0.9em;width:100%;}
form button{background:#764ba2;color:white;border:none;}
form button:hover{background:#667eea;}
body.dark form{background:#2b2b3f;border-color:#555;color:#eee;}
body.dark form input,body.dark form select{background:#2b2b3f;border-color:#555;color:#eee;}
.entry-card{background:white;padding:8px;margin-bottom:6px;border-radius:12px;box-shadow:0 4px 10px rgba(0,0,0,0.1);display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;transition:.3s;}
.entry-card:hover{transform:translateY(-2px);}
.entry-card span{flex:1;text-align:center;font-size:0.8em;}
.delete-btn{background:#ff4d4f;color:white;padding:5px 8px;border:none;border-radius:8px;cursor:pointer;}
.delete-btn:hover{background:#d9363e;}
body.dark .entry-card{background:#2b2b3f;color:#eee;}
canvas{width:100%!important;height:200px!important;margin-bottom:10px;}
.bottom-nav{position:fixed;bottom:0;width:100%;max-width:480px;display:flex;justify-content:space-around;background:linear-gradient(135deg,#667eea,#764ba2);padding:8px;border-radius:12px 12px 0 0;color:white;}
.bottom-nav button{background:none;border:none;color:white;font-size:1.3em;cursor:pointer;}
</style>
</head>
<body>
<div class="container">
<header>
<h1>Pocket Tracker</h1>
<div class="balance">Balance: <span id="balance">0</span></div>
<button id="darkmodeBtn">🌓</button>
</header>
<div class="banner">💡 روزانہ اپنے مالیات کا حساب رکھیں!</div>

<div class="category-grid">
<button class="income" data-cat="Income">💵<span>Income</span></button>
<button class="saving" data-cat="Saving">💰<span>Saving</span></button>
<button class="food" data-cat="Food">🍔<span>Food</span></button>
<button class="fuel" data-cat="Fuel">⛽<span>Fuel</span></button>
<button class="entertainment" data-cat="Entertainment">🎬<span>Entertainment</span></button>
<button class="personal" data-cat="Personal">💸<span>Personal</span></button>
<button class="gambling" data-cat="Gambling">🎲<span>Gambling</span></button>
<button class="bills" data-cat="Bills">💡<span>Bills</span></button>
<button class="loans" data-cat="Loans">💳<span>Loans</span></button>
<button class="transport" data-cat="Transport">🚗<span>Transport</span></button>
<button class="shopping" data-cat="Shopping">🛒<span>Shopping</span></button>
<button class="rent" data-cat="Rent">🏠<span>Rent</span></button>
<button class="education" data-cat="Education">📚<span>Education</span></button>
<button class="medical" data-cat="Medical">💊<span>Medical</span></button>
<button class="other" data-cat="Other">📝<span>Other</span></button>
</div>

<form id="entryForm">
<input type="date" id="date" required>
<select id="type" required>
<option value="">Select Type</option>
<option value="Income">Income</option>
<option value="Saving">Saving</option>
<option value="Food">Food</option>
<option value="Fuel">Fuel</option>
<option value="Entertainment">Entertainment</option>
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

<div id="entries"></div>

<canvas id="barChart"></canvas>
<canvas id="pieChart"></canvas>

<div class="bottom-nav">
<button onclick="scrollTop()">🏠</button>
<button onclick="document.getElementById('entryForm').scrollIntoView({behavior:'smooth'})">➕</button>
<button onclick="document.getElementById('barChart').scrollIntoView({behavior:'smooth'})">📊</button>
<button id="toggleDark">🌓</button>
</div>
</div>

<script>
// Dark mode
document.getElementById('darkmodeBtn').onclick = ()=>{
  document.body.classList.toggle('dark');
}
document.getElementById('toggleDark').onclick = ()=>{
  document.body.classList.toggle('dark');
}

// Entries
let entries = JSON.parse(localStorage.getItem('entries')) || [];

function updateBalance(){
  let balance = entries.reduce((a,e)=>{
    return a + ((e.type==='Income'||e.type==='Saving')?Number(e.amount):-Number(e.amount))
  },0);
  document.getElementById('balance').innerText = balance;
}
function renderEntries(){
  let container = document.getElementById('entries');
  container.innerHTML = '';
  entries.forEach((e,i)=>{
    let div = document.createElement('div');
    div.className = 'entry-card';
    div.innerHTML = `<span>${e.date}</span><span>${e.type}</span><span>${e.amount}</span><span>${e.note}</span><button class="delete-btn" onclick="deleteEntry(${i})">❌</button>`;
    container.appendChild(div);
  });
}
function deleteEntry(i){
  entries.splice(i,1);
  localStorage.setItem('entries',JSON.stringify(entries));
  renderEntries();
  updateBalance();
}

// Form submit
document.getElementById('entryForm').onsubmit = (e)=>{
  e.preventDefault();
  let date = document.getElementById('date').value;
  let type = document.getElementById('type').value;
  let amount = document.getElementById('amount').value;
  let note = document.getElementById('note').value;
  entries.push({date,type,amount,note});
  localStorage.setItem('entries',JSON.stringify(entries));
  renderEntries();
  updateBalance();
  e.target.reset();
}

// Charts
function renderCharts(){
  let totals = {};
  ['Income','Saving','Food','Entertainment','Fuel','Personal','Gambling','Bills','Loans','Transport','Shopping','Rent','Education','Medical','Other'].forEach(c=>totals[c]=0);
  entries.forEach(e=>totals[e.type]+=Number(e.amount));
  let ctxBar = document.getElementById('barChart').getContext('2d');
  let ctxPie = document.getElementById('pieChart').getContext('2d');
  if(window.barChart) window.barChart.destroy();
  if(window.pieChart) window.pieChart.destroy();
  window.barChart = new Chart(ctxBar,{
    type:'bar',
    data:{
      labels:Object.keys(totals),
      datasets:[{label:'Amount',data:Object.values(totals),backgroundColor:['#4caf50','#2196f3','#ff9800','#795548','#ff5722','#f44336','#9c27b0','#00bcd4','#607d8b','#9c27b0','#e91e63','#3f51b5','#ffeb3b','#009688','#795548']}]
    },
    options:{responsive:true,plugins:{legend:{display:false}},scales:{y:{beginAtZero:true}}}
  });
  window.pieChart = new Chart(ctxPie,{
    type:'pie',
    data:{
      labels:Object.keys(totals),
      datasets:[{data:Object.values(totals),backgroundColor:['#4caf50','#2196f3','#ff9800','#795548','#ff5722','#f44336','#9c27b0','#00bcd4','#607d8b','#9c27b0','#e91e63','#3f51b5','#ffeb3b','#009688','#795548']}]
    },
    options:{responsive:true}
  });
}

function scrollTop(){window.scrollTo({top:0,behavior:'smooth'});}

renderEntries();
updateBalance();
renderCharts();
</script>
</body>
</html>
