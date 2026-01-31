<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Traker</title>

<!-- Fonts & Icons -->
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

<!-- Chart -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<!-- PWA -->
<link rel="manifest" href="data:application/json,{
  &quot;name&quot;:&quot;Pocket Traker&quot;,
  &quot;short_name&quot;:&quot;Pocket&quot;,
  &quot;start_url&quot;:&quot;.&quot;,
  &quot;display&quot;:&quot;standalone&quot;,
  &quot;background_color&quot;:&quot;#020617&quot;,
  &quot;theme_color&quot;:&quot;#38bdf8&quot;
}">

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:Poppins}
body{
  background:linear-gradient(135deg,#020617,#0f172a);
  color:#fff;min-height:100vh
}
header{
  position:sticky;top:0;z-index:9;
  padding:14px 20px;
  background:rgba(255,255,255,.08);
  backdrop-filter:blur(14px);
  display:flex;justify-content:space-between;align-items:center
}
.logo{font-size:22px;font-weight:700}
.container{padding:20px;max-width:1100px;margin:auto}
.hero{
  display:grid;grid-template-columns:1fr 1fr;
  gap:20px;align-items:center
}
.hero img{
  width:100%;border-radius:22px;
  box-shadow:0 20px 40px rgba(0,0,0,.4)
}
h1{font-size:32px}
p{opacity:.8}

.cards{
  margin-top:24px;
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(160px,1fr));
  gap:14px
}
.card{
  background:rgba(255,255,255,.09);
  border-radius:18px;
  padding:16px;
  transition:.3s;
}
.card:hover{transform:translateY(-6px)}
.card i{font-size:22px;color:#38bdf8}
.card h3{font-size:13px;opacity:.7;margin-top:6px}
.card p{font-size:22px;font-weight:700}

.section{
  margin-top:24px;
  background:rgba(255,255,255,.08);
  border-radius:20px;
  padding:16px
}

.form,.expense{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
  gap:10px;margin-top:14px
}
input,select,button{
  padding:12px;border-radius:14px;border:none;
  background:#020617;color:#fff
}
button{
  background:#38bdf8;color:#000;
  font-weight:600;cursor:pointer
}
.list li{
  display:flex;justify-content:space-between;
  padding:6px 0;
  border-bottom:1px solid rgba(255,255,255,.1)
}
.progress{
  height:10px;border-radius:20px;
  background:#020617;margin-top:12px;overflow:hidden
}
.progress div{
  height:100%;
  background:linear-gradient(90deg,#38bdf8,#22c55e);
  width:0%;transition:.5s
}

/* Bottom Nav */
.nav{
  position:fixed;bottom:0;left:0;right:0;
  background:rgba(2,6,23,.9);
  display:flex;justify-content:space-around;
  padding:10px 0
}
.nav i{font-size:20px;color:#94a3b8}
.nav i.active{color:#38bdf8}

@media(max-width:768px){
  .hero{grid-template-columns:1fr}
  h1{font-size:26px}
}
</style>
</head>

<body>

<header>
  <div class="logo">💰 Pocket Traker</div>
  <button onclick="resetAll()">Reset</button>
</header>

<div class="container">

  <section class="hero">
    <div>
      <h1>Smart Money, Modern Life</h1>
      <p>Track salary, expenses & savings like premium finance apps.</p>
    </div>
    <img src="https://images.unsplash.com/photo-1554224154-22dec7ec8818?q=80&w=1200">
  </section>

  <section class="cards">
    <div class="card"><i class="fa-solid fa-user"></i><h3>User</h3><p id="u">—</p></div>
    <div class="card"><i class="fa-solid fa-wallet"></i><h3>Salary</h3><p id="sal">0</p></div>
    <div class="card"><i class="fa-solid fa-hand-holding-dollar"></i><h3>Loan</h3><p id="loan">0</p></div>
    <div class="card"><i class="fa-solid fa-piggy-bank"></i><h3>Savings</h3><p id="save">0</p></div>
    <div class="card"><i class="fa-solid fa-coins"></i><h3>Balance</h3><p id="bal">0</p></div>
  </section>

  <section class="section">
    <h3>Profile Setup</h3>
    <div class="form">
      <input id="name" placeholder="Username">
      <input id="salary" type="number" placeholder="Salary">
      <input id="loanIn" type="number" placeholder="Loan">
      <input id="saveIn" type="number" placeholder="Saving Goal">
      <button onclick="saveMain()">Save</button>
    </div>
    <div class="progress"><div id="bar"></div></div>
  </section>

  <section class="section">
    <h3>Add Expense</h3>
    <div class="expense">
      <input id="expText" placeholder="Note">
      <input id="expAmt" type="number" placeholder="Amount">
      <select id="cat">
        <option>Food</option><option>Travel</option>
        <option>Bills</option><option>Fun</option><option>Other</option>
      </select>
      <button onclick="addExpense()">Add</button>
    </div>
  </section>

  <section class="section">
    <h3>Expenses</h3>
    <ul id="expList" class="list"></ul>
  </section>

  <section class="section">
    <h3>Analytics</h3>
    <canvas id="chart"></canvas>
  </section>

</div>

<div class="nav">
  <i class="fa-solid fa-house active"></i>
  <i class="fa-solid fa-chart-pie"></i>
  <i class="fa-solid fa-plus"></i>
  <i class="fa-solid fa-user"></i>
</div>

<script>
let data=JSON.parse(localStorage.getItem("pocket"))||{
  name:"",salary:0,loan:0,save:0,balance:0,expenses:[]
};
let chart;

function saveMain(){
  data.name=name.value;
  data.salary=+salary.value;
  data.loan=+loanIn.value;
  data.save=+saveIn.value;
  data.balance=data.salary-data.loan-data.save;
  store();
}
function addExpense(){
  if(!expAmt.value)return;
  data.expenses.push({t:expText.value,a:+expAmt.value,c:cat.value});
  data.balance-=+expAmt.value;
  expText.value=expAmt.value="";
  store();
}
function store(){
  localStorage.setItem("pocket",JSON.stringify(data));
  render();
}
function resetAll(){
  localStorage.removeItem("pocket");
  location.reload();
}
function render(){
  u.textContent=data.name||"Guest";
  sal.textContent=data.salary;
  loan.textContent=data.loan;
  save.textContent=data.save;
  bal.textContent=data.balance;
  expList.innerHTML="";
  data.expenses.forEach(e=>{
    expList.innerHTML+=`<li><span>${e.t} (${e.c})</span><b>- ${e.a}</b></li>`;
  });
  let p=data.save?Math.min(((data.salary-data.balance)/data.save)*100,100):0;
  bar.style.width=p+"%";
  drawChart();
}
function drawChart(){
  let sums={};
  data.expenses.forEach(e=>sums[e.c]=(sums[e.c]||0)+e.a);
  let labels=Object.keys(sums),vals=Object.values(sums);
  if(chart)chart.destroy();
  chart=new Chart(chart,{
    type:"doughnut",
    data:{labels,datasets:[{data:vals}]},
    options:{plugins:{legend:{labels:{color:"#fff"}}}}
  });
}
render();

/* Service Worker (offline) */
if("serviceWorker"in navigator){
  const code=`self.addEventListener('fetch',e=>{})`;
  navigator.serviceWorker.register(
    URL.createObjectURL(new Blob([code],{type:"text/javascript"}))
  );
}
</script>
</body>
</html>
