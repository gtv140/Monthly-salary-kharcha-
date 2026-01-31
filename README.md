<html lang="en">
<head>
<meta charset="UTF-8">
<title>Pocket Tracker</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<style>
body{
    font-family:'Roboto',sans-serif;
    margin:0;
    padding:0;
    background: linear-gradient(135deg,#1e3c72,#2a5298);
    color:#fff;
    overflow-x:hidden;
}
h1,h2{text-align:center;margin:10px;font-weight:700;}
button{cursor:pointer;transition:0.3s;font-weight:700;}
button:hover{opacity:0.85;}
input{padding:8px;border-radius:10px;border:2px solid #03a9f4;outline:none;margin:3px;width:120px;}
header{
    background:linear-gradient(135deg,#29b6f6,#00acc1);
    padding:15px;
    text-align:center;
    font-size:32px;
    font-weight:bold;
    color:#fff;
    text-shadow:2px 2px 5px rgba(0,0,0,0.3);
    box-shadow:0 4px 10px rgba(0,0,0,0.3);
    border-bottom-left-radius:15px;
    border-bottom-right-radius:15px;
}

/* Info Cards */
.info-section{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    margin:20px 0;
}
.info-box{
    background:linear-gradient(145deg,#00acc1,#29b6f6);
    box-shadow:0 8px 20px rgba(0,0,0,0.35);
    border-radius:15px;
    margin:10px;
    padding:20px;
    text-align:center;
    width:140px;
    cursor:pointer;
    transition:0.4s;
    font-weight:bold;
    color:#fff;
}
.info-box:hover{
    transform:scale(1.08);
    box-shadow:0 10px 30px rgba(0,0,0,0.45);
}

/* Chart Section */
canvas{background:#fff; border-radius:15px; padding:10px;}

/* Footer */
footer{
    background:#0288d1; 
    color:#fff; 
    text-align:center; 
    padding:20px; 
    margin-top:40px; 
    border-radius:10px;
}

/* Responsive */
@media(max-width:768px){
    .info-section{flex-direction:column; align-items:center;}
    .info-box{width:80%;}
}
</style>
</head>
<body>

<header>💼 Pocket Tracker 💼</header>

<section id="dashboard" style="padding:20px; background:#f0f4f8; color:#333;">
  <h2 style="text-align:center; margin-bottom:20px; color:#1e3c72;">Dashboard</h2>
  
  <!-- Expense Cards -->
  <div class="info-section">
    <div class="info-box" onclick="addExpense('Food')">
      <h3>🍔 Food</h3>
      <span id="foodTotal">0 PKR</span>
    </div>
    <div class="info-box" onclick="addExpense('Fuel')">
      <h3>⛽ Fuel</h3>
      <span id="fuelTotal">0 PKR</span>
    </div>
    <div class="info-box" onclick="addExpense('Snacks')">
      <h3>🍿 Snacks</h3>
      <span id="snacksTotal">0 PKR</span>
    </div>
    <div class="info-box" onclick="addExpense('Bills')">
      <h3>💡 Bills</h3>
      <span id="billsTotal">0 PKR</span>
    </div>
    <div class="info-box" onclick="addExpense('Entertainment')">
      <h3>🎮 Fun</h3>
      <span id="funTotal">0 PKR</span>
    </div>
  </div>
  
  <!-- Chart Section -->
  <h2 style="text-align:center; margin:30px 0 10px; color:#1e3c72;">Visual Overview</h2>
  <canvas id="expenseChart" style="max-width:800px; margin:auto; display:block;"></canvas>
  
  <!-- Footer -->
  <footer>
    &copy; 2026 Pocket Tracker | Made with ❤️ by Sweetie
  </footer>
</section>

<script>
// ===== Local Storage =====
let expenses = JSON.parse(localStorage.getItem('expenses')) || {
  Food:0, Fuel:0, Snacks:0, Bills:0, Entertainment:0
};

// ===== Update Cards =====
function updateCards(){
  document.getElementById('foodTotal').innerText=expenses.Food+" PKR";
  document.getElementById('fuelTotal').innerText=expenses.Fuel+" PKR";
  document.getElementById('snacksTotal').innerText=expenses.Snacks+" PKR";
  document.getElementById('billsTotal').innerText=expenses.Bills+" PKR";
  document.getElementById('funTotal').innerText=expenses.Entertainment+" PKR";
  drawChart();
}

// ===== Add Expense =====
function addExpense(type){
  let val=parseInt(prompt(`Enter ${type} expense in PKR:`,"0")) || 0;
  expenses[type]+=val;
  localStorage.setItem('expenses', JSON.stringify(expenses));
  updateCards();
}

// ===== Chart =====
let ctx=document.getElementById('expenseChart').getContext('2d');
let expenseChart;
function drawChart(){
  if(expenseChart) expenseChart.destroy();
  expenseChart=new Chart(ctx,{
    type:'bar',
    data:{
      labels:Object.keys(expenses),
      datasets:[{
        label:'Expenses in PKR',
        data:Object.values(expenses),
        backgroundColor:[
          '#ff6384','#36a2eb','#ffcd56','#4bc0c0','#9966ff'
        ]
      }]
    },
    options:{
      responsive:true,
      plugins:{
        legend:{display:false},
        tooltip:{enabled:true}
      },
      scales:{
        y:{beginAtZero:true}
      }
    }
  });
}

// ===== Initialize =====
updateCards();
</script>

</body>
</html>
