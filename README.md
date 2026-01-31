<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pocket Traker</title>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

<style>
:root{
  --bg:#0f172a;
  --card:#020617;
  --accent:#22c55e;
  --text:#e5e7eb;
}
.light{
  --bg:#f8fafc;
  --card:#ffffff;
  --accent:#16a34a;
  --text:#020617;
}
*{box-sizing:border-box;font-family:system-ui}
body{
  margin:0;
  background:var(--bg);
  color:var(--text);
}
header{
  padding:16px;
  display:flex;
  justify-content:space-between;
  align-items:center;
}
header h1{
  font-size:22px;
  font-weight:800;
}
header button{
  background:var(--accent);
  border:none;
  padding:8px 12px;
  border-radius:8px;
  color:white;
  cursor:pointer;
}

.container{
  padding:16px;
  display:grid;
  gap:16px;
}

.card{
  background:var(--card);
  border-radius:14px;
  padding:16px;
  box-shadow:0 10px 30px rgba(0,0,0,.3);
  animation:fade .6s ease;
}
@keyframes fade{
  from{opacity:0;transform:translateY(20px)}
  to{opacity:1;transform:none}
}

.card h3{
  margin:0 0 10px;
  font-size:16px;
  opacity:.9;
}
.value{
  font-size:26px;
  font-weight:800;
  color:var(--accent);
}

.grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(150px,1fr));
  gap:14px;
}

input,button{
  width:100%;
  padding:12px;
  border-radius:10px;
  border:none;
  outline:none;
  font-size:14px;
}
input{
  background:#111827;
  color:white;
}
.light input{background:#e5e7eb;color:black}

.action{
  background:var(--accent);
  color:white;
  font-weight:700;
  cursor:pointer;
}

.footer{
  text-align:center;
  opacity:.6;
  font-size:13px;
  padding:20px 0;
}

.hero{
  text-align:center;
}
.hero img{
  width:80px;
  margin-bottom:10px;
}

</style>
</head>
<body>

<header>
  <h1>💰 Pocket Traker</h1>
  <button onclick="toggleMode()">🌗</button>
</header>

<div class="container">

  <div class="hero card">
    <img src="https://cdn-icons-png.flaticon.com/512/2331/2331941.png">
    <input id="username" placeholder="Enter your name">
    <button class="action" onclick="saveName()">Save Name</button>
    <p id="welcome"></p>
  </div>

  <div class="grid">
    <div class="card">
      <h3>Salary</h3>
      <div class="value" id="salary">0</div>
    </div>
    <div class="card">
      <h3>Loan</h3>
      <div class="value" id="loan">0</div>
    </div>
    <div class="card">
      <h3>Current Balance</h3>
      <div class="value" id="balance">0</div>
    </div>
    <div class="card">
      <h3>Total Expense</h3>
      <div class="value" id="expense">0</div>
    </div>
  </div>

  <div class="card">
    <h3>Update Amounts</h3>
    <input id="salaryInput" type="number" placeholder="Add Salary">
    <input id="loanInput" type="number" placeholder="Add Loan">
    <button class="action" onclick="updateMoney()">Update</button>
  </div>

  <div class="card">
    <h3>Add Daily Expense</h3>
    <input id="expenseInput" type="number" placeholder="Expense Amount">
    <button class="action" onclick="addExpense()">Add Expense</button>
  </div>

</div>

<div class="footer">
  Premium Pocket Traker • Sweetie Edition 💖
</div>

<script>
let data = JSON.parse(localStorage.getItem("pocket")) || {
  name:"",
  salary:0,
  loan:0,
  expense:0
};

function render(){
  document.getElementById("salary").innerText = data.salary;
  document.getElementById("loan").innerText = data.loan;
  document.getElementById("expense").innerText = data.expense;
  document.getElementById("balance").innerText =
    data.salary - data.loan - data.expense;

  if(data.name){
    document.getElementById("welcome").innerText =
      "Welcome, " + data.name + " ❤️";
  }
}

function saveName(){
  data.name = username.value;
  save();
}

function updateMoney(){
  data.salary += Number(salaryInput.value||0);
  data.loan += Number(loanInput.value||0);
  salaryInput.value="";
  loanInput.value="";
  save();
}

function addExpense(){
  data.expense += Number(expenseInput.value||0);
  expenseInput.value="";
  save();
}

function save(){
  localStorage.setItem("pocket",JSON.stringify(data));
  render();
}

function toggleMode(){
  document.body.classList.toggle("light");
  localStorage.setItem("mode",
    document.body.classList.contains("light")?"light":"dark"
  );
}
if(localStorage.getItem("mode")==="light"){
  document.body.classList.add("light");
}

render();
</script>

</body>
</html>
