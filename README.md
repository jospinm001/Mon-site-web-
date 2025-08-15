<!doctype html>
<html lang="fr">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Queen Calculator — Commerce</title>
<style>
:root{
  --bg1:#ff9ac1; --bg2:#7b61ff; --card:#fff; --text:#1b1130; --muted:#6d6d78;
  --accent1:#ff2d95; --accent2:#7b61ff;
}
*{box-sizing:border-box}
body{
  margin:0; font-family:Inter, Arial, sans-serif; background:linear-gradient(135deg,var(--bg1),var(--bg2));
  min-height:100vh; display:flex; align-items:center; justify-content:center; padding:18px; color:var(--text);
}
.app{
  width:100%; max-width:1100px; background: linear-gradient(180deg, rgba(255,255,255,0.98), #fff);
  border-radius:16px; box-shadow:0 20px 50px rgba(0,0,0,0.18); overflow:hidden; display:grid;
  grid-template-columns: 1fr 380px; gap:18px;
}

/* header */
.header{grid-column:1/-1; display:flex; align-items:center; gap:12px; padding:16px 18px}
.logo{display:flex; align-items:center; gap:12px}
.logo img{width:64px; height:64px; border-radius:12px; box-shadow:0 8px 24px rgba(0,0,0,0.12)}
.title{font-weight:800; font-size:20px}
.subtitle{font-size:12px; color:var(--muted)}

/* left main: calculator + features */
.left{padding:12px}
.card{background:var(--card); border-radius:12px; padding:12px; margin-bottom:12px; box-shadow:0 6px 18px rgba(0,0,0,0.04)}
.calc {
  display:grid; grid-template-columns: 1fr 220px; gap:12px; align-items:start;
}
.display {
  background:linear-gradient(90deg,#fff,#fbfbff); padding:12px; border-radius:10px; border:1px solid #f0f0f2;
  min-height:68px; display:flex; flex-direction:column; justify-content:center;
}
.screen { font-size:24px; font-weight:800; text-align:right; padding-right:6px; word-break:break-all; }
.subscreen { font-size:12px; color:var(--muted); text-align:right; padding-right:6px; margin-top:6px; }

/* keypad */
.keys { display:grid; grid-template-columns: repeat(4,1fr); gap:8px; margin-top:12px; }
.button {
  padding:14px; border-radius:10px; border:none; font-weight:700; font-size:16px; cursor:pointer;
  background:linear-gradient(90deg,var(--accent1),var(--accent2)); color:#fff; box-shadow:0 6px 12px rgba(123,97,255,0.18);
  transition: transform .08s ease, box-shadow .12s;
}
.button.ghost { background:#fff; color:var(--text); border:1px solid #eee; box-shadow:none; font-weight:600; }
.button.op { background: linear-gradient(90deg,#ffb86b,#ff7b5a); }

/* small controls */
.controls { display:flex; gap:8px; margin-top:8px; flex-wrap:wrap; }

/* right panel: commercial summary */
.right{padding:12px}
.summary .line{display:flex; justify-content:space-between; padding:10px; border-radius:8px}
.line.light{background:linear-gradient(90deg,#fff,#fafafa); border:1px solid rgba(0,0,0,0.03)}
.big{font-weight:900; font-size:18px}
.muted{color:var(--muted); font-size:13px}

/* product table */
.prod-table { width:100%; border-collapse:collapse; margin-top:8px; font-size:14px }
.prod-table th, .prod-table td { text-align:left; padding:8px; border-bottom:1px dashed #eee }
.prod-table th{ font-weight:800 }

/* history */
.history { max-height:180px; overflow:auto; padding:8px; background:#fafafa; border-radius:8px; margin-top:8px }

/* footer */
.footer{grid-column:1/-1; padding:12px 18px; display:flex; justify-content:space-between; color:var(--muted); font-size:13px}

/* responsive */
@media (max-width:980px){
  .app{grid-template-columns:1fr; padding:12px}
  .calc{grid-template-columns:1fr}
  .header{flex-direction:column; align-items:flex-start; gap:6px}
}
</style>
</head>
<body>
<div class="app" id="app">
  <div class="header">
    <div class="logo">
      <img alt="Queen" src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><defs><linearGradient id='g' x1='0' x2='1'><stop offset='0' stop-color='%23ff8ab6'/><stop offset='1' stop-color='%237b61ff'/></linearGradient></defs><rect rx='12' width='100' height='100' fill='url(%23g)'/><text x='50' y='58' font-size='38' fill='white' font-family=Arial font-weight='700' text-anchor='middle'>Q</text></svg>">
    </div>
    <div>
      <div class="title">Queen Calculator — Commerce</div>
      <div class="subtitle">I love you • TVA • Remise • Conversion • Historique</div>
    </div>

    <div style="margin-left:auto; display:flex; gap:8px; align-items:center">
      <button class="button ghost" onclick="exportCSV()">Export CSV</button>
      <button class="button ghost" onclick="printReceipt()">Imprimer</button>
      <button class="button" onclick="calculateAll()">Calculer</button>
    </div>
  </div>

  <!-- LEFT: Calculator + Features -->
  <div class="left">
    <div class="card">
      <div class="calc">
        <!-- display -->
        <div>
          <div class="display">
            <div id="screen" class="screen">0</div>
            <div id="subscreen" class="subscreen">0</div>
          </div>

          <div class="controls" style="margin-top:10px">
            <button class="button ghost" onclick="clearAll()">C</button>
            <button class="button ghost" onclick="backspace()">←</button>
            <button class="button ghost" onclick="toggleSign()">±</button>
            <button class="button op" onclick="pressOp('/')">÷</button>
          </div>

          <div class="keys">
            <button class="button" onclick="pressNum('7')">7</button>
            <button class="button" onclick="pressNum('8')">8</button>
            <button class="button" onclick="pressNum('9')">9</button>
            <button class="button op" onclick="pressOp('*')">×</button>

            <button class="button" onclick="pressNum('4')">4</button>
            <button class="button" onclick="pressNum('5')">5</button>
            <button class="button" onclick="pressNum('6')">6</button>
            <button class="button op" onclick="pressOp('-')">−</button>

            <button class="button" onclick="pressNum('1')">1</button>
            <button class="button" onclick="pressNum('2')">2</button>
            <button class="button" onclick="pressNum('3')">3</button>
            <button class="button op" onclick="pressOp('+')">+</button>

            <button class="button" onclick="pressNum('0')" style="grid-column: span 2">0</button>
            <button class="button" onclick="pressNum('.')">.</button>
            <button class="button" onclick="equals()">=</button>
          </div>
        </div>

        <!-- Right side of calc: commercial quick tools -->
        <div>
          <div style="display:flex;gap:8px; margin-bottom:8px">
            <input id="itemName" type="text" placeholder="Produit (optionnel)" style="flex:1; padding:10px; border-radius:8px; border:1px solid #eee">
            <input id="itemQty" type="number" min="1" value="1" style="width:80px; padding:10px; border-radius:8px; border:1px solid #eee">
          </div>

          <div style="display:flex; gap:8px;">
            <input id="itemPrice" type="number" step="0.01" placeholder="Prix €" style="flex:1; padding:10px; border-radius:8px; border:1px solid #eee">
            <input id="itemCost" type="number" step="0.01" placeholder="Coût €" style="width:120px; padding:10px; border-radius:8px; border:1px solid #eee">
          </div>

          <div style="display:flex; gap:8px; margin-top:8px">
            <button class="button" onclick="addProductFromCalc()">Ajouter</button>
            <button class="button ghost" onclick="clearProductInputs()">Effacer</button>
          </div>

          <div style="margin-top:12px; font-size:13px; color:var(--muted)">
            <div style="margin-bottom:6px"><strong>Conversion rapide</strong></div>
            <div style="display:flex; gap:8px">
              <input id="fromRate" type="number" step="0.0001" value="1" style="flex:1; padding:8px; border-radius:8px; border:1px solid #eee">
              <input id="toSymbol" type="text" value="X" style="width:80px; padding:8px; border-radius:8px; border:1px solid #eee">
            </div>
            <div style="margin-top:8px; display:flex; gap:8px">
              <button class="button ghost" onclick="applyConversion()">Convertir</button>
              <button class="button ghost" onclick="clearConversion()">Reset</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Products list (added from calc or manual) -->
    <div class="card">
      <div style="display:flex; justify-content:space-between; align-items:center">
        <div><strong>Panier / Produits</strong></div>
        <div style="color:var(--muted); font-size:13px">Double-clic pour éditer</div>
      </div>
      <table class="prod-table" id="prodTable" style="margin-top:8px">
        <thead><tr><th>Produit</th><th>Qty</th><th>Prix</th><th>Coût</th><th>Total</th><th></th></tr></thead>
        <tbody id="prodBody"></tbody>
      </table>

      <div style="display:flex; gap:8px; margin-top:8px">
        <button class="button" onclick="calcFromProducts()">Calculer Totaux</button>
        <button class="button ghost" onclick="clearProducts()">Vider</button>
      </div>
    </div>

    <!-- Advanced commercial features -->
    <div class="card">
      <div style="display:flex; gap:8px">
        <div style="flex:1">
          <label>TVA (%)</label>
          <input id="tax" type="number" step="0.01" value="16">
        </div>
        <div style="flex:1">
          <label>Remise</label>
          <div style="display:flex; gap:8px">
            <input id="discount" type="number" step="0.01" value="0">
            <select id="discountType"><option value="percent">%</option><option value="fixed">€</option></select>
          </div>
        </div>
      </div>

      <div style="display:flex; gap:8px; margin-top:10px">
        <div style="flex:1">
          <label>Intérêt (%)</label>
          <input id="interest" type="number" step="0.01" value="0" placeholder="intérêt annuel">
        </div>
        <div style="flex:1">
          <label>Durée (ans)</label>
          <input id="interestYears" type="number" min="0" value="1">
        </div>
      </div>

      <div style="display:flex; gap:8px; margin-top:10px">
        <button class="button ghost" onclick="calcInterestSimple()">Intérêt simple</button>
        <button class="button ghost" onclick="calcInterestCompound()">Intérêt composé</button>
      </div>
    </div>
  </div>

  <!-- RIGHT: Summary & history -->
  <div class="right">
    <div class="card summary">
      <div class="line light"><div class="muted">Sous-total</div><div id="subtotal" class="big">0.00 €</div></div>
      <div class="line"><div class="muted">Coût total</div><div id="totalCost">0.00 €</div></div>
      <div class="line"><div class="muted">Bénéfice</div><div id="profit" class="big">0.00 €</div></div>
      <div class="line"><div class="muted">Marge</div><div id="margin">0 %</div></div>

      <div class="line light" style="margin-top:8px"><div class="muted">Remise</div><div id="appliedDiscount">0 €</div></div>
      <div class="line"><div class="muted">TVA</div><div id="appliedTax">0 €</div></div>
      <div class="line light" style="margin-top:8px"><div class="muted">Total final</div><div id="finalTotal" class="big">0.00 €</div></div>

      <div style="display:flex; gap:8px; margin-top:12px">
        <button class="button" onclick="calculateAll()">Calculer tout</button>
        <button class="button ghost" onclick="printReceipt()">Imprimer reçu</button>
      </div>

      <div style="margin-top:12px">
        <label>Historique</label>
        <div id="history" class="history"></div>
      </div>

      <div style="margin-top:12px">
        <label>Charger son de validation (optionnel)</label>
        <input id="soundFile" type="file" accept="audio/*">
      </div>
    </div>
  </div>

  <div class="footer">
    <div>Queen Calculator • I love you 💜</div>
    <div class="muted">Fonctions commerciales: conversion, TVA, remise, intérêts, marge, export</div>
  </div>
</div>

<script>
/* --- Calculator core --- */
let current = '0';
let prev = null;
let op = null;
let entering = true;

const screen = document.getElementById('screen');
const subscreen = document.getElementById('subscreen');

function refreshScreen(){
  screen.textContent = current;
  subscreen.textContent = prev ? `${prev} ${op || ''}` : '';
}
function pressNum(d){
  if(!entering){ current = '0'; entering = true; }
  if(d === '.' && current.includes('.')) return;
  if(current === '0' && d !== '.') current = d;
  else current = current + d;
  refreshScreen();
}
function pressOp(o){
  if(prev !== null && op !== null && entering){
    compute();
  } else {
    prev = current;
  }
  op = o;
  entering = false;
  refreshScreen();
}
function compute(){
  if(prev === null || op === null) return;
  // compute digit by digit to avoid float surprise: use Number on strings
  const a = parseFloat(prev);
  const b = parseFloat(current);
  let res = 0;
  if(op === '+') res = a + b;
  if(op === '-') res = a - b;
  if(op === '*') res = a * b;
  if(op === '/') {
    if(b === 0){ alert('Erreur: division par zéro'); res = a; }
    else res = a / b;
  }
  // keep precision
  current = (Math.round(res * 100) / 100).toString();
  prev = null; op = null; entering = false;
  refreshScreen();
}
function equals(){ compute(); }
function clearAll(){
  current = '0'; prev = null; op = null; entering = true; refreshScreen();
}
function backspace(){
  if(!entering){ current = '0'; entering = true; refreshScreen(); return; }
  if(current.length <= 1){ current = '0'; }
  else current = current.slice(0, -1);
  refreshScreen();
}
function toggleSign(){
  if(current.startsWith('-')) current = current.slice(1);
  else if(current !== '0') current = '-' + current;
  refreshScreen();
}

/* --- Products & cart --- */
let products = JSON.parse(localStorage.getItem('qc_products')||'[]');

function renderProducts(){
  const tbody = document.getElementById('prodBody');
  tbody.innerHTML = '';
  products.forEach((p, idx)=>{
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${escapeHtml(p.name||'--')}</td>
      <td>${p.qty}</td>
      <td>${p.price.toFixed(2)} €</td>
      <td>${p.cost.toFixed(2)} €</td>
      <td>${(p.price*p.qty).toFixed(2)} €</td>
      <td><button onclick="editProduct(${idx})" class="button ghost">Édit</button></td>`;
    tbody.appendChild(tr);
  });
  saveProducts();
}
function saveProducts(){ localStorage.setItem('qc_products', JSON.stringify(products)); }

function addProductFromCalc(){
  const name = document.getElementById('itemName').value.trim();
  const qty = Math.max(1, parseInt(document.getElementById('itemQty').value) || 1);
  const price = parseFloat(document.getElementById('itemPrice').value) || parseFloat(current) || 0;
  const cost = parseFloat(document.getElementById('itemCost').value) || 0;
  products.push({name, qty, price, cost});
  renderProducts();
  playSound();
  document.getElementById('itemName').value=''; document.getElementById('itemQty').value='1'; document.getElementById('itemPrice').value=''; document.getElementById('itemCost').value='';
}

function editProduct(i){
  const p = products[i];
  const name = prompt('Nom produit', p.name) || p.name;
  const qty = parseInt(prompt('Quantité', p.qty)) || p.qty;
  const price = parseFloat(prompt('Prix unitaire (€)', p.price)) || p.price;
  const cost = parseFloat(prompt('Coût unitaire (€)', p.cost)) || p.cost;
  products[i]={name, qty, price, cost};
  renderProducts();
}

/* clear & reset */
function clearProducts(){ if(!confirm('Vider le panier ?')) return; products=[]; renderProducts(); updateSummary(); }

/* compute totals from products */
function calcFromProducts(){
  let subtotal=0, totalCost=0;
  products.forEach(p=>{ subtotal += p.price*p.qty; totalCost += p.cost*p.qty; });
  document.getElementById('subtotal').textContent = subtotal.toFixed(2) + ' €';
  document.getElementById('totalCost').textContent = totalCost.toFixed(2) + ' €';
  const profit = subtotal - totalCost;
  document.getElementById('profit').textContent = profit.toFixed(2) + ' €';
  const margin = subtotal ? ((profit/subtotal)*100).toFixed(2) : '0.00';
  document.getElementById('margin').textContent = margin + ' %';
  addHistory({time:new Date().toLocaleString(), subtotal, totalCost, profit, final:subtotal});
  saveProducts();
}

/* full calculation with discount & tax & conversion */
function calculateAll(){
  let subtotal=0, totalCost=0;
  products.forEach(p=>{ subtotal += p.price*p.qty; totalCost += p.cost*p.qty; });

  const discount = parseFloat(document.getElementById('discount').value) || 0;
  const discountType = document.getElementById('discountType').value;
  const tax = parseFloat(document.getElementById('tax').value) || 0;
  const rounding = 0.01;

  let discountAmount = (discountType === 'percent') ? subtotal*(discount/100) : discount;
  if(discountAmount > subtotal) discountAmount = subtotal;
  const afterDiscount = subtotal - discountAmount;
  const taxAmount = afterDiscount * (tax/100);
  let final = afterDiscount + taxAmount;

  // rounding
  final = Math.round(final / rounding) * rounding;

  const profit = subtotal - totalCost;
  const margin = subtotal ? ((profit/subtotal)*100).toFixed(2) : '0.00';

  document.getElementById('subtotal').textContent = subtotal.toFixed(2) + ' €';
  document.getElementById('totalCost').textContent = totalCost.toFixed(2) + ' €';
  document.getElementById('profit').textContent = profit.toFixed(2) + ' €';
  document.getElementById('margin').textContent = margin + ' %';
  document.getElementById('appliedDiscount').textContent = '-' + discountAmount.toFixed(2) + ' €';
  document.getElementById('appliedTax').textContent = taxAmount.toFixed(2) + ' €';
  document.getElementById('finalTotal').textContent = final.toFixed(2) + ' €';

  addHistory({time:new Date().toLocaleString(), subtotal, totalCost, profit, final});
  playSound();
  saveProducts();
}

/* interest calculators */
function calcInterestSimple(){
  const principal = parseFloat(prompt('Montant principal (€)', '1000')) || 0;
  const rate = parseFloat(document.getElementById('interest').value) || 0;
  const years = parseFloat(document.getElementById('interestYears').value) || 1;
  const interest = principal * (rate/100) * years;
  alert(`Intérêt simple: ${interest.toFixed(2)} € (total: ${(principal+interest).toFixed(2)} €)`);
}
function calcInterestCompound(){
  const principal = parseFloat(prompt('Montant principal (€)', '1000')) || 0;
  const rate = parseFloat(document.getElementById('interest').value) || 0;
  const years = parseFloat(document.getElementById('interestYears').value) || 1;
  const times = parseInt(prompt('Nombre de fois par an (ex: 12)', '12')) || 12;
  const total = principal * Math.pow(1 + (rate/100)/times, times*years);
  alert(`Intérêt composé: total après ${years} an(s): ${total.toFixed(2)} €`);
}

/* Conversion */
function applyConversion(){
  const rate = parseFloat(document.getElementById('fromRate').value) || 1;
  const symbol = document.getElementById('toSymbol').value || 'X';
  const value = parseFloat(current) || 0;
  const converted = value * rate;
  subscreen.textContent = `${value} € → ${converted.toFixed(2)} ${symbol}`;
  playSound();
}
function clearConversion(){ document.getElementById('fromRate').value = 1; document.getElementById('toSymbol').value = 'X'; subscreen.textContent=''; }

/* History management */
function addHistory(entry){
  const arr = JSON.parse(localStorage.getItem('qc_history')||'[]');
  arr.unshift(entry);
  if(arr.length>200) arr.pop();
  localStorage.setItem('qc_history', JSON.stringify(arr));
  renderHistory();
}
function renderHistory(){
  const arr = JSON.parse(localStorage.getItem('qc_history')||'[]');
  const el = document.getElementById('history'); el.innerHTML = '';
  arr.forEach(h=>{
    const d = document.createElement('div');
    d.style.padding='6px 4px'; d.style.borderBottom='1px solid #f6f6f6';
    d.innerHTML = `<strong>${h.time}</strong><div class="muted">Total: ${h.final ? h.final.toFixed(2) : (h.subtotal? h.sub
