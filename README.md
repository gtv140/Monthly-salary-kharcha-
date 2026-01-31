<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Tracker - Modern Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
/* ===== Base Styles ===== */
body{margin:0;font-family:'Roboto',sans-serif;background:#f4f7ff;color:#333;line-height:1.6;}
a{text-decoration:none;color:inherit;}
.container{width:90%;max-width:1200px;margin:0 auto;}

/* ===== Header / Navbar ===== */
header{background:#2a5298;color:#fff;padding:20px 0;box-shadow:0 4px 15px rgba(0,0,0,0.1);}
header .logo{font-size:28px;font-weight:700;text-align:center;}
nav{margin-top:10px;text-align:center;}
nav a{margin:0 15px;font-weight:500;color:#fff;transition:0.3s;}
nav a:hover{color:#ffd700;}

/* ===== Hero Section ===== */
.hero{display:flex;flex-direction:column;align-items:center;justify-content:center;text-align:center;padding:60px 20px;background:linear-gradient(135deg,#1e3c72,#2a5298);color:#fff;}
.hero h1{font-size:48px;margin:10px 0;}
.hero p{font-size:20px;margin:10px 0;}
.hero .cta{margin-top:20px;}
.hero .cta button{padding:12px 25px;font-size:16px;font-weight:bold;border:none;border-radius:8px;background:#ffd700;color:#2a5298;cursor:pointer;transition:0.3s;}
.hero .cta button:hover{opacity:0.85;}

/* ===== Features Section ===== */
.features{padding:60px 0;background:#f4f7ff;text-align:center;}
.features h2{font-size:32px;margin-bottom:40px;}
.features .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:30px;}
.features .card{background:#fff;border-radius:12px;box-shadow:0 8px 20px rgba(0,0,0,0.1);padding:30px;transition:0.4s;}
.features .card:hover{transform:translateY(-8px);box-shadow:0 12px 25px rgba(0,0,0,0.15);}
.features .card i{font-size:40px;color:#2a5298;margin-bottom:15px;}
.features .card h3{font-size:20px;margin-bottom:10px;}
.features .card p{font-size:16px;color:#555;}

/* ===== Dashboard Section ===== */
.dashboard-section{padding:60px 0;background:#e0f2ff;}
.dashboard-section h2{text-align:center;font-size:32px;margin-bottom:40px;}
.dashboard{display:flex;flex-wrap:wrap;gap:20px;justify-content:center;}
.box{background:#fff;padding:25px;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.1);flex:1;min-width:220px;text-align:center;transition:0.3s;}
.box:hover{transform:translateY(-6px);box-shadow:0 10px 25px rgba(0,0,0,0.15);}
.box h3{font-size:20px;margin-bottom:10px;}
.box span{font-size:24px;font-weight:700;color:#2a5298;}

/* ===== Daily Expenses Table ===== */
.table-section{padding:60px 0;text-align:center;}
.table-section table{width:100%;border-collapse:collapse;background:#fff;box-shadow:0 8px 20px rgba(0,0,0,0.05);border-radius:12px;overflow:hidden;}
.table-section th, .table-section td{padding:12px;border-bottom:1px solid #eee;}
.table-section th{background:#2a5298;color:#fff;font-weight:600;}
.table-section tr:hover{background:#f0f8ff;}

/* ===== Tips Section ===== */
.tips{padding:60px 20px;background:#fff;text-align:center;}
.tips h2{font-size:32px;margin-bottom:40px;}
.tips .tip-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:30px;}
.tips .tip-card{background:#e0f2ff;padding:25px;border-radius:12px;box-shadow:0 6px 20px rgba(0,0,0,0.1);}
.tips .tip-card i{font-size:30px;color:#2a5298;margin-bottom:15px;display:block;}
.tips .tip-card p{font-size:16px;}

/* ===== Footer ===== */
footer{background:#2a5298;color:#fff;text-align:center;padding:25px;margin-top:40px;}
footer p{margin:5px;}

/* ===== Responsive ===== */
@media(max-width:768px){.hero h1{font-size:36px;} .hero p{font-size:18px;}}
</style>
</head>
<body>

<!-- Header -->
<header>
  <div class="logo">Pocket Tracker</div>
  <nav>
    <a href="#features">Features</a>
    <a href="#dashboard">Dashboard</a>
    <a href="#tips">Tips</a>
    <a href="#contact">Contact</a>
  </nav>
</header>

<!-- Hero Section -->
<section class="hero">
  <h1>Manage Your Money Effortlessly</h1>
  <p>Track expenses, savings, and budget with ease — anywhere, anytime!</p>
  <div class="cta">
    <button onclick="scrollToSection('dashboard')">Go to Dashboard</button>
  </div>
</section>

<!-- Features Section -->
<section id="features" class="features">
  <h2>Awesome Features</h2>
  <div class="grid">
    <div class="card"><i class="fas fa-wallet"></i><h3>Expense Tracker</h3><p>Add daily expenses quickly and see totals.</p></div>
    <div class="card"><i class="fas fa-chart-line"></i><h3>Visual Charts</h3><p>See your progress with beautiful graphs.</p></div>
    <div class="card"><i class="fas fa-piggy-bank"></i><h3>Goal Saving</h3><p>Set savings goals and track progress.</p></div>
    <div class="card"><i class="fas fa-coins"></i><h3>Salary & Loan</h3><p>Manage salary, loans, and remaining balance.</p></div>
    <div class="card"><i class="fas fa-mobile-alt"></i><h3>Mobile Friendly</h3><p>Use on any device seamlessly.</p></div>
    <div class="card"><i class="fas fa-cloud-upload-alt"></i><h3>Backup & Restore</h3><p>Save data locally and restore anytime.</p></div>
  </div>
</section>

<!-- Dashboard Section -->
<section id="dashboard" class="dashboard-section">
  <h2>Your Dashboard</h2>
  <div class="dashboard">
    <div class="box"><h3>Username</h3><span id="infoUser">User</span></div>
    <div class="box"><h3>Current Balance</h3><span id="remaining">0</span> PKR</div>
    <div class="box"><h3>Current Saving</h3><span id="currentSaving">0</span> PKR</div>
    <div class="box"><h3>Total Expense</h3><span id="totalExpense">0</span> PKR</div>
  </div>
  <!-- Daily Expenses Table -->
  <div class="table-section">
    <h2>Daily Expenses</h2>
    <table id="dailyTable">
      <tr><th>Day</th><th>Food</th><th>Fuel</th><th>Snacks</th><th>Bills</th><th>Entertainment</th><th>Daily Total</th><th>Date</th></tr>
    </table>
  </div>
</section>

<!-- Tips Section -->
<section id="tips" class="tips">
  <h2>Money & Budget Tips</h2>
  <div class="tip-grid">
    <div class="tip-card"><i class="fas fa-lightbulb"></i><p>Track your daily expenses carefully to save money.</p></div>
    <div class="tip-card"><i class="fas fa-hand-holding-usd"></i><p>Set a monthly saving goal and stick to it.</p></div>
    <div class="tip-card"><i class="fas fa-clock"></i><p>Review expenses weekly to avoid overspending.</p></div>
    <div class="tip-card"><i class="fas fa-mobile-alt"></i><p>Use Pocket Tracker anywhere, even on mobile.</p></div>
  </div>
</section>

<!-- Footer -->
<footer id="contact">
  <p>&copy; 2026 Pocket Tracker. All Rights Reserved.</p>
  <p>Contact: support@pockettracker.com</p>
</footer>

<!-- ===== Scripts ===== -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
// ===== Sample Dashboard Data =====
let dailyData=[],salary=50000,goal=10000,loanAmount=10000;

function updateDashboard(){
    let totalExpense=0;
    dailyData.forEach(d=>{
        totalExpense+=d.total;
    });
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

// ===== Sample Daily Data =====
dailyData=[
  {food:50,fuel:40,snacks:30,bills:70,entertainment:60,total:250,date:'2026-02-01'},
  {food:60,fuel:50,snacks:20,bills:80,entertainment:40,total:250,date:'2026-02-02'},
  {food:55,fuel:45,snacks:25,bills:75,entertainment:50,total:250,date:'2026-02-03'}
];
updateDashboard();
</script>
</body>
</html>
