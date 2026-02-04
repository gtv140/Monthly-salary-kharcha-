<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Pocket Tracker – Unlimited</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body{margin:0;font-family:sans-serif;background:#f4f6f9}
.dark{background:#111;color:#eee}
.app{max-width:480px;margin:auto;padding:10px}
.card{background:#fff;border-radius:14px;padding:10px;margin-bottom:10px}
.dark .card{background:#1e1e1e}
input,select,button{width:100%;padding:10px;border-radius:10px;border:1px solid #ccc}
button{background:#6c5ce7;color:#fff;border:none}
.row{display:flex;gap:6px}
.small{font-size:12px;color:#777}
</style>
</head>

<body>
<div class="app">

<div class="card">
<h3>💰 Balance: <span id="balance">0</span></h3>
<button onclick="toggleDark()">🌙 Dark Mode</button>
</div>

<div class="card">
<h4>Add Category (Unlimited)</h4>
<div class="row">
<input id="newCat" placeholder="Category name">
<button onclick="addCategory()">Add</button>
</div>
</div>

<div class="card">
<h4>Add Entry</h4>
<input type="date" id="date">
<select id="category"></select>
<input type="number" id="amount" placeholder="Amount">
<select id="type">
<option value="in">Income</option>
<option value="out">Expense</option>
</select>
<input id="tags" placeholder="Tags (comma)">
<select id="repeat">
<option value="none">One time</option>
<option value="daily">Daily</option>
<option value="monthly">Monthly</option>
</select>
<button onclick="addEntry()">Save</button>
</div>

<div class="card">
<h4>Search / Filter</h4>
<input id="search" placeholder="Search note / tag" oninput="render()">
</div>

<div class="card">
<h4>Entries</h4>
<div id="list"></div>
</div>

<div class="card">
<button onclick="exportCSV()">⬇ Export CSV</button>
<button onclick="importData()">⬆ Import JSON</button>
</div>

<canvas id="chart"></canvas>

</div>

<script>
let data = JSON.parse(localStorage.getItem("data")) || {
 categories:["Income","Food"],
 entries:[]
};

function save(){localStorage.setItem("data",JSON.stringify(data))}
function toggleDark(){document.body.classList.toggle("dark")}

function addCategory(){
 let c=document.getElementById("newCat").value;
 if(c){data.categories.push(c);save();loadCats()}
}

function loadCats(){
 let s=document.getElementById("category");
 s.innerHTML="";
 data.categories.forEach(c=>{
  s.innerHTML+=`<option>${c}</option>`;
 });
}

function addEntry(){
 data.entries.push({
  date:date.value,
  cat:category.value,
  amt:+amount.value,
  type:type.value,
  tags:tags.value,
  repeat:repeat.value
 });
 save();render();
}

function render(){
 let q=search.value.toLowerCase();
 let list=document.getElementById("list");
 list.innerHTML="";
 let bal=0;
 data.entries.filter(e=>
  e.cat.toLowerCase().includes(q)||
  e.tags.toLowerCase().includes(q)
 ).forEach((e,i)=>{
  bal+= e.type=="in"?e.amt:-e.amt;
  list.innerHTML+=`
   <div class="small">
   ${e.date} | ${e.cat} | ${e.amt} | ${e.tags}
   </div>`;
 });
 balance.innerText=bal;
 drawChart();
}

function drawChart(){
 let totals={};
 data.entries.forEach(e=>{
  totals[e.cat]=(totals[e.cat]||0)+e.amt;
 });
 new Chart(chart,{
  type:"pie",
  data:{labels:Object.keys(totals),
  datasets:[{data:Object.values(totals)}]}
 });
}

function exportCSV(){
 let csv="date,cat,amount,type,tags\n";
 data.entries.forEach(e=>{
  csv+=`${e.date},${e.cat},${e.amt},${e.type},${e.tags}\n`;
 });
 let a=document.createElement("a");
 a.href=URL.createObjectURL(new Blob([csv]));
 a.download="pocket.csv";a.click();
}

function importData(){
 let f=document.createElement("input");
 f.type="file";
 f.onchange=()=>{
  let r=new FileReader();
  r.onload=e=>{
   data=JSON.parse(e.target.result);
   save();loadCats();render();
  }
  r.readAsText(f.files[0]);
 }
 f.click();
}

loadCats();render();
</script>
</body>
</html>
