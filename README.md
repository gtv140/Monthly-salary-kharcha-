<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Pocket Tracker Pro</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
*{box-sizing:border-box;font-family:Poppins}
body{margin:0;background:#f2f4f8}
.dark{background:#0f1115;color:#fff}

.app{max-width:420px;margin:auto;padding-bottom:80px}

/* Header */
.header{
background:linear-gradient(135deg,#6c5ce7,#00cec9);
color:#fff;padding:20px;border-radius:0 0 25px 25px;
}
.balance{font-size:26px;font-weight:600}

/* Cards */
.card{
background:rgba(255,255,255,0.85);
backdrop-filter:blur(10px);
border-radius:18px;
padding:15px;margin:12px;
}
.dark .card{background:#1a1d24}

/* Inputs */
input,select,button{
width:100%;padding:12px;border-radius:12px;border:none;margin-top:6px
}
button{background:#6c5ce7;color:#fff;font-weight:600}

/* Categories */
.grid{
display:grid;
grid-template-columns:repeat(4,1fr);
gap:10px;text-align:center
}
.cat{
padding:10px;border-radius:14px;background:#eef;
font-size:12px
}

/* Bottom Nav */
.nav{
position:fixed;bottom:0;left:0;right:0;
background:#fff;display:flex;
justify-content:space-around;padding:10px 0
}
.dark .nav{background:#14161c}
.nav div{text-align:center;font-size:12px}
.nav span{font-size:20px;display:block}

.small{font-size:12px;color:#666}
.dark .small{color:#aaa}
</style>
</head>

<body>
<div class="app">

<div class="header">
<div>Hello 👋</div>
<div class="balance">Rs <span id="balance">0</span></div>
<div class="small">Track smart, live better</div>
</div>

<div class="card">
<h4>Add Transaction</h4>
<select id="type">
<option value="in">Income</option>
<option value="out">Expense</option>
</select>
<input id="amount" type="number" placeholder="Amount">
<select id="category"></select>
<input id="note" placeholder="Note (food, fuel...)">
<button onclick="add()">Save</button>
</div>

<div class="card">
<h4>Quick Categories</h4>
<div class="grid" id="cats"></div>
</div>

<div class="card">
<h4>History</h4>
<div id="list"></div>
</div>

<div class="card">
<canvas id="chart"></canvas>
</div>

</div>

<div class="nav">
<div onclick="toggleDark()"><span>🌙</span>Mode</div>
<div onclick="resetAll()"><span>♻</span>Reset</div>
</div>

<script>
let data=JSON.parse(localStorage.getItem("pocket"))||{
balance:0,
categories:["Food","Fuel","Bills","Loan","Fun","Shopping","Health","Other"],
entries:[]
}

function save(){localStorage.setItem("pocket",JSON.stringify(data))}

function loadCats(){
category.innerHTML="";
cats.innerHTML="";
data.categories.forEach(c=>{
category.innerHTML+=`<option>${c}</option>`;
cats.innerHTML+=`<div class="cat">${c}</div>`;
})
}

function add(){
let amt=+amount.value;
if(!amt)return;
let t=type.value;
data.balance+=t=="in"?amt:-amt;
data.entries.unshift({
cat:category.value,amt,t,note:note.value
})
save();render();
}

function render(){
balance.innerText=data.balance;
list.innerHTML="";
let sum={};
data.entries.forEach(e=>{
list.innerHTML+=`
<div class="small">${e.cat} | ${e.amt} | ${e.note}</div>`;
sum[e.cat]=(sum[e.cat]||0)+e.amt;
})
draw(sum);
}

let ch;
function draw(sum){
if(ch)ch.destroy();
ch=new Chart(chart,{
type:"pie",
data:{labels:Object.keys(sum),
datasets:[{data:Object.values(sum)}]}
})
}

function toggleDark(){document.body.classList.toggle("dark")}
function resetAll(){
if(confirm("Reset all data?")){
localStorage.removeItem("pocket");
location.reload();
}
}

loadCats();render();
</script>
</body>
</html>
