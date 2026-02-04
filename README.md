<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
*{box-sizing:border-box;font-family:Poppins;margin:0;padding:0;}
body{background:#f2f4f8;color:#333;transition:.3s;}
body.dark{background:#121212;color:#eee;}

.app{max-width:480px;margin:auto;padding-bottom:80px}

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
}
.cat{
padding:12px;border-radius:14px;background:#eef;font-size:12px;cursor:pointer;transition:.2s;
}
.cat:hover{transform:scale(1.05);}
.dark .cat{background:#2b2b3f}

/* Entry List */
.entry{display:flex;justify-content:space-between;padding:8px;background:#fff;border-radius:12px;margin-bottom:6px;box-shadow:0 4px 10px rgba(0,0,0,0.1);}
.dark .entry{background:#2b2b3f;color:#eee;}
.entry span{font-size:12px;flex:1;text-align:center;}
.entry button{background:#ff4d4f;color:white;border:none;padding:5px 8px;border-radius:8px;cursor:pointer;}
.entry button:hover{background:#d9363e}

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
<h1>Pocket Tracker Pro</h1>
<div class="balance">Balance: Rs <span id="balance">0</span></div>
<div class="small">Track smart, live better 💡</div>
<button onclick="toggleDark()">🌓 Toggle Dark</button>
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

</div>

<!-- Bottom Nav -->
<div class="nav">
<button onclick="scrollTop()">🏠 Home</button>
<button onclick="document.getElementById('amount').scrollIntoView({behavior:'smooth'})">➕ Add</button>
<button onclick="document.getElementById('chart').scrollIntoView({behavior:'smooth'})">📊 Charts</button>
</div>

<script>
// Data storage
let data = JSON.parse(localStorage.getItem('pocketPro')) || {
balance:0,
categories:["Income","Saving","Food","Fuel","Entertainment","Personal","Gambling","Bills","Loans","Transport","Shopping","Rent","Education","Medical","Other"],
entries:[]
};

// Save data
function save(){localStorage.setItem('pocketPro',JSON.stringify(data))}

// Load categories
function loadCategories(){
let catSel=document.getElementById('category');
let grid=document.getElementById('cats');
catSel.innerHTML=""; grid.innerHTML="";
data.categories.forEach(c=>{
catSel.innerHTML+=`<option>${c}</option>`;
grid.innerHTML+=`<div class="cat">${c}</div>`;
})
}

// Add entry
function addEntry(){
let amt=+document.getElementById('amount').value;
if(!amt)return;
let t=document.getElementById('type').value;
let cat=document.getElementById('category').value;
let note=document.getElementById('note').value;
data.entries.unshift({type:t,cat:cat,amt:amt,note:note});
data.balance += (t=="in"?amt:-amt);
save();render();
document.getElementById('amount').value="";
document.getElementById('note').value="";
}

// Render entries and balance
function render(){
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
drawChart(totals);
}

// Delete entry
function deleteEntry(i){
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

loadCategories();render();
</script>
</body>
</html>
