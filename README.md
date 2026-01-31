<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker - Mobile Modern</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
/* ===== Base ===== */
body{margin:0;font-family:'Roboto',sans-serif;background:#f0f4ff;color:#333;}
h1,h2,h3{margin:0 0 10px 0;font-weight:700;}
p{margin:0 0 10px 0;}
.container{width:95%;max-width:500px;margin:0 auto;}

/* ===== Header ===== */
header{background:#2a5298;color:#fff;padding:15px 0;text-align:center;box-shadow:0 4px 12px rgba(0,0,0,0.1);}
header .logo{font-size:28px;font-weight:700;}
nav{margin-top:10px;}
nav a{margin:0 10px;color:#fff;font-weight:500;transition:0.3s;}
nav a:hover{color:#ffd700;}

/* ===== Hero ===== */
.hero{background:linear-gradient(135deg,#1e3c72,#2a5298);color:#fff;text-align:center;padding:40px 20px;border-radius:12px;margin:15px 0;}
.hero h1{font-size:28px;margin-bottom:10px;}
.hero p{font-size:16px;margin-bottom:15px;}
.hero button{padding:10px 20px;font-size:16px;font-weight:bold;border:none;border-radius:8px;background:#ffd700;color:#2a5298;cursor:pointer;transition:0.3s;}
.hero button:hover{opacity:0.85;}

/* ===== Dashboard Boxes ===== */
.dashboard{display:flex;flex-direction:column;gap:15px;margin-bottom:20px;}
.box{background:#fff;padding:20px;border-radius:12px;box-shadow:0 6px 15px rgba(0,0,0,0.1);display:flex;justify-content:space-between;align-items:center;transition:0.3s;}
.box:hover{transform:translateY(-5px);box-shadow:0 10px 20px rgba(0,0,0,0.15);}
.box h3{font-size:16px;color:#2a5298;}
.box span{font-size:20px;font-weight:700;}

/* ===== Table ===== */
table{width:100%;border-collapse:collapse;margin-bottom:20px;background:#fff;border-radius:12px;overflow:hidden;box-shadow:0 6px 15px rgba(0,0,0,0.1);}
th,td{padding:10px;text-align:center;border-bottom:1px solid #eee;}
th{background:#2a5298;color:#fff;font-weight:600;}
tr:hover{background:#f0f8ff;}

/* ===== Features / Tips ===== */
.features,.tips{margin-bottom:20px;}
.features h2,.tips h2{text-align:center;margin-bottom:15px;color:#2a5298;font-size:22px;}
.card-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.card{background:#fff;padding:15px;border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.1);text-align:center;transition:0.3s;}
.card i{font-size:24px;color:#2a5298;margin-bottom:8px;}
.card:hover{transform:translateY(-4px);box-shadow:0 8px 20px rgba(0,0,0,0.15);}
.card p{font-size:14px;color:#555;}

/* ===== Footer ===== */
footer{background:#2a5298;color:#fff;text-align:center;padding:15px;margin-top:20px;border-radius:12px;}
footer p{margin:5px;font-size:14px;}
</style>
</head>
<body>

<div class="container">
  <!-- Header -->
  <header>
    <div class="logo">Pocket Tracker</div>
  </header>

  <!-- Hero -->
  <section class="hero">
    <h1>Manage Your Money Easily</h1>
    <p>Track daily expenses, savings, and budget all in one place.</p>
    <button onclick="scrollToSection('dashboard')">Go to Dashboard</button>
  </section>

  <!-- Dashboard -->
  <section id="dashboard">
    <h2 style="text-align:center;color:#2a5298;margin-bottom:15px;">Your Dashboard</h2>
    <div class="dashboard">
      <div class="box"><h3>Username</h3><span id="infoUser">User</span></div>
      <div class="box"><h3>Current Balance</h3><span id="remaining">0</span> PKR</div>
      <div class="box"><h3>Current Saving</h3><span id="currentSaving">0</span> PKR</div>
      <div class="box"><h3>Total Expense</h3><span id="totalExpense">0</span> PKR</div>
    </div>

    <!-- Daily Table -->
    <table id="dailyTable">
      <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Daily Total</th><th>Date</th></tr>
    </table>
  </section>

  <!-- Features -->
  <section class="features">
    <h2>Features</h2>
    <div class="card-grid">
      <div class="card"><i class="fas fa-wallet"></i><p>Quickly add daily expenses.</p></div>
      <div class="card"><i class="fas fa-chart-line"></i><p>Visual charts for savings and expenses.</p></div>
      <div class="card"><i class="fas fa-piggy-bank"></i><p>Set and track saving goals.</p></div>
      <div class="card"><i class="fas fa-coins"></i><p>Manage salary and loans.</p></div>
    </div>
  </section>

  <!-- Tips -->
  <section class="tips">
    <h2>Tips</h2>
    <div class="card-grid">
      <div class="card"><i class="fas fa-lightbulb"></i><p>Track daily to save efficiently.</p></div>
      <div class="card"><i class="fas fa-hand-holding-usd"></i><p>Set a monthly saving goal.</p></div>
      <div class="card"><i class="fas fa-clock"></i><p>Review weekly expenses.</p></div>
      <div class="card"><i class="fas fa-mobile-alt"></i><p>Use anywhere on mobile.</p></div>
    </div>
  </section>

  <!-- Footer -->
  <footer>
    <p>&copy; 2026 Pocket Tracker</p>
    <p>Contact: support@pockettracker.com</p>
  </footer>
</div>

<script>
// ===== Sample Dashboard Data =====
let dailyData=[],salary=50000,goal=10000,loanAmount=10000;

function updateDashboard(){
    let totalExpense=0;
    dailyData.forEach(d=>{totalExpense+=d.total;});
    const remaining=salary-totalExpense-loanAmount;
    const currentSaving=Math.max(0,remaining);
    document.getElementById('infoUser').innerText='User';
    document.getElementById('totalExpense').innerText=totalExpense;
    document.getElementById('remaining').innerText=remaining;
    document.getElementById('currentSaving').innerText=currentSaving;

    // Update table
    const table=document.getElementById('dailyTable');
    table.innerHTML="<tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Daily Total</th><th>Date</th></tr>";
    dailyData.forEach((d,i)=>{
        const row=table.insertRow();
        row.insertCell(0).innerText=i+1;
        row.insertCell(1).innerText=d.food;
        row.insertCell(2).innerText=d.fuel;
        row.insertCell(3).innerText=d.snacks;
        row.insertCell(4).innerText=d.bills;
        row.insertCell(5).innerText=d.entertainment;
        row.insertCell(6).innerText=d.total;
        row.insertCell(7).innerText=d.date;
    });
}

// ===== Scroll Helper =====
function scrollToSection(id){document.getElementById(id).scrollIntoView({behavior:'smooth'});}

// ===== Sample Data =====
dailyData=[
  {food:50,fuel:40,snacks:30,bills:70,entertainment:60,total:250,date:'2026-02-01'},
  {food:60,fuel:50,snacks:20,bills:80,entertainment:40,total:250,date:'2026-02-02'},
  {food:55,fuel:45,snacks:25,bills:75,entertainment:50,total:250,date:'2026-02-03'}
];
updateDashboard();
</script>
</body>
</html>
