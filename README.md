<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker – Salary Auto</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
*{box-sizing:border-box;font-family:Poppins;margin:0;padding:0;}
body{background:#f2f4f8;color:#333;transition:.3s;}
body.dark{background:#121212;color:#eee;}

.app{max-width:480px;margin:auto;padding-bottom:100px}

/* Header */
.header{
background:linear-gradient(135deg,#6c5ce7,#00cec9);
color:white;padding:20px;border-radius:0 0 25px 25px;text-align:center;
}
.header h1{font-size:22px;font-weight:600;}
.header .balance{font-size:28px;margin-top:8px;font-weight:600;}
.header .small{font-size:14px;opacity:.85;}

/* Cards */
.card{
background:rgba(255,255,255,0.85);
backdrop-filter:blur(10px);
border-radius:18px;
padding:15px;margin:12px;
}
.dark .card{background:#1a1d24;color:#eee}

/* Inputs */
input,select,button{width:100%;padding:12px;border-radius:12px;border:none;margin-top:6px;font-size:14px;}
button{background:#6c5ce7;color:white;font-weight:600;cursor:pointer;transition:.2s;}
button:hover{transform:scale(1.03);}

/* Category Grid */
.grid{
display:grid;
grid-template-columns:repeat(4,1fr);
gap:10px;text-align:center;margin-top:10px;
word-wrap:break-word; 
}
.cat{
padding:12px;border-radius:14px;background:#eef;font-size:12px;cursor:pointer;transition:.2s;
word-break:break-word; line-height:1.2;
}
.cat:hover{transform:scale(1.05);}
.dark .cat{background:#2b2b3f}

/* Entry List */
.entry{display:flex;justify-content:space-between;padding:8px;background:#fff;border-radius:12px;margin-bottom:6px;box-shadow:0 4px 10px rgba(0,0,0,0.1);}
.dark .entry{background:#2b2b3f;color:#eee;}
.entry span{font-size:12px;flex:1;text-align:center;}
.entry button{background:#ff4d4f;color:white;border:none;padding:5px 8px;border-radius:8px;cursor:pointer;}
.entry button:hover{background:#d9363e}

/* Budget Progress */
.progress{width:100%;background:#ddd;border-radius:12px;height:12px;margin-top:8px;overflow:hidden;}
.progress-bar{height:100%;background:#6c5ce7;width:0%;transition:.3s;}
.dark .progress{background:#2b2b3f}

/* Bottom Nav */
.nav{
position:fixed;bottom:0;width:100%;max-width:480px;
display:flex;justify-content:space-around;background:linear-gradient(135deg,#6c5ce7,#00cec9);
padding:10px;border-radius:12px 12px 0 0;color:white;
}
.nav button{background:none;border:none;color:white;font-size:18px;cursor:pointer;transition:.2s;}
.nav button:hover{transform:scale(1.1);}
</style>
</head>

<body>
<div class="app">

<!-- Header -->
<div class="header">
<h1>Pocket Tracker – Salary Auto</h1>
<div class="balance">Balance: Rs <span id="balance">0</span></div>
<div class="small" id="budget-msg">Track smart, live better 💡</div>
<button onclick="toggleDark()">🌓 Toggle Dark</button>
</div>

<!-- Set Monthly Salary -->
<div class="card">
<h4>Set Monthly Salary</h4>
<input type="number" id="salary" placeholder="Enter your salary">
<button onclick="setSalary()">Set Salary</button>
</div>

<!-- Add Entry -->
<div class="card">
<h4>Add Transaction</h4>
<select id="type">
<option value="in">Income</option>
<option value="out">Expense</option>
</select>
<input type="number" id="amount" placeholder="Amount">
<select id="category"></select>
<input id="note" placeholder="Note / Tag">
<select id="repeat">
<option value="none">One time</option>
<option value="daily">Daily</option>
<option value="weekly">Weekly</option>
<option value="monthly">Monthly</option>
</select>
<button onclick="addEntry()">Add Entry</button>
</div>

<!-- Quick Categories -->
<div class="card">
<h4>Quick Categories</h4>
<div class="grid" id="cats"></div>
</div>

<!-- Entries History -->
<div class="card">
<h4>History</h4>
<div id="entries"></div>
</div>

<!-- Charts -->
<div class="card">
<canvas id="chart"></canvas>
</div>

<!-- Monthly Budget -->
<div class="card">
<h4>Monthly Budget Progress</h4>
<div class="progress"><div class="progress-bar" id="progress-bar"></div></div>
</div>

<!-- Export / Import -->
<div class="card">
<button onclick="exportJSON()">⬇ Export JSON</button>
<button onclick="importJSON()">⬆ Import JSON</button>
</div>

</div>

<!-- Bottom Nav -->
<div class="nav">
<button onclick="scrollTop()">🏠 Home</button>
<button onclick="document.getElementById('amount').scrollIntoView({behavior:'smooth'})">➕ Add</button>
<button onclick="document.getElementById('chart').scrollIntoView({behavior:'smooth'})">📊 Charts</button>
</div>

<script>
// Data storage
let data = JSON.parse(localStorage.getItem('pocketSalary')) || {
balance:0,
salary:0,
categories:["Income","Saving","Food","Fuel","Entertainment","Personal","Gambling","Bills","Loans","Transport","Shopping","Rent","Education","Medical","Other"],
entries:[]
};

// Save data
function save(){localStorage.setItem('pocketSalary',JSON.stringify(data))}

// Set Salary
function setSalary(){
let s=+document.getElementById('salary').value;
if(!s) return;
data.salary=s;
data.balance=s;
save();render();
document.getElementById('salary').value="";
}

// Load categories
function loadCategories(){
let catSel=document.getElementById('category');
let grid=document.getElementById('cats');
catSel.innerHTML=""; grid.innerHTML="";
data.categories.forEach(c=>{
catSel.innerHTML+=`<option>${c}</option>`;
grid.innerHTML+=`<div class="cat" onclick="quickAdd('${c}')">${c}</div>`;
})
}

// Quick add
function quickAdd(cat){
let amt=prompt("Enter amount for "+cat);
if(!amt) return;
data.entries.unshift({type:'out',cat:cat,amt:+amt,note:'Quick',repeat:'none'});
data.balance-=+amt;
save();render();
}

// Add entry
function addEntry(){
let amt=+document.getElementById('amount').value;
if(!amt)return;
let t=document.getElementById('type').value;
let cat=document.getElementById('category').value;
let note=document.getElementById('note').value;
let repeat=document.getElementById('repeat').value;
let entry={type:t,cat:cat,amt:amt,note:note,repeat:repeat};
data.entries.unshift(entry);
data.balance += (t=="in"?amt:-amt);
save();render();
document.getElementById('amount').value="";
document.getElementById('note').value="";
}

// Apply recurring transactions
function applyRecurring(){
data.entries.forEach(e=>{
if(e.repeat=='daily'){data.balance += (e.type=="in"?e.amt:-e.amt);}
if(e.repeat=='weekly'){data.balance += (e.type=="in"?e.amt:-e.amt);}
if(e.repeat=='monthly'){data.balance += (e.type=="in"?e.amt:-e.amt);}
})
save();
}

// Render entries, balance, progress bar
function render(){
applyRecurring();
document.getElementById('balance').innerText=data.balance;
let list=document.getElementById('entries');
list.innerHTML="";
let totals={};
data.entries.forEach((e,i)=>{
list.innerHTML+=`<div class="entry">
<span>${e.cat}</span>
<span>${e.amt}</span>
<span>${e.note}</span>
<button onclick="deleteEntry(${i})">❌</button>
</div>`;
totals[e.cat]=(totals[e.cat]||0)+e.amt;
});

// Draw chart
drawChart(totals);

// Update budget progress
let budget=data.salary || 1;
let prog=Math.min(100,Math.round((budget-data.balance)/budget*100));
document.getElementById('progress-bar').style.width=prog+"%";
document.getElementById('budget-msg').innerText=`Salary Used: ${prog}%`;
}

// Delete entry
function deleteEntry(i){
let e=data.entries[i];
if(e.type=='out') data.balance += e.amt;
if(e.type=='in') data.balance -= e.amt;
data.entries.splice(i,1);
save();render();
}

// Draw Pie Chart
let ch;
function drawChart(totals){
if(ch) ch.destroy();
let ctx=document.getElementById('chart').getContext('2d');
ch=new Chart(ctx,{
type:'pie',
data:{
labels:Object.keys(totals),
datasets:[{data:Object.values(totals),
backgroundColor:['#4caf50','#2196f3','#ff9800','#795548','#ff5722','#f44336','#9c27b0','#00bcd4','#607d8b','#9c27b0','#e91e63','#3f51b5','#ffeb3b','#009688','#795548']}]
},
options:{responsive:true}
});
}

// Dark mode toggle
function toggleDark(){document.body.classList.toggle('dark');}

// Scroll top
function scrollTop(){window.scrollTo({top:0,behavior:'smooth'});}

// Export / Import JSON
function exportJSON(){
let a=document.createElement('a');
a.href=URL.createObjectURL(new Blob([JSON.stringify(data)]));
a.download="pocketSalary.json";
a.click();
}
function importJSON(){
let f=document.createElement('input');
f.type='file';
f.onchange=e=>{
let reader=new FileReader();
reader.onload=ev=>{
data=JSON.parse(ev.target.result);
save();loadCategories();render();
}
reader.readAsText(f.target.files[0]);
}
f.click();
}

loadCategories();render();
</script>
</body>
</html>
