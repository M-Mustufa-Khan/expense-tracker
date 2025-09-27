<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Expense Tracker</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1 class="first-heading">Expense Tracker Application</h1>
  
  <div class="container">
    <h4>Your Balance:</h4>
    <h1 id="balance">$0.00</h1>

    <div class="inc-exp-container">
      <div>
        <h4>Income</h4>
        <p id="money-plus" class="money plus">+$0.00</p>
      </div>
      <div>
        <h4>Expense</h4>
        <p id="money-minus" class="money minus">-$0.00</p>
      </div>
    </div> 

    <h3>History</h3>
    <ul id="list" class="list">
      <li class="minus">
        Cash <span>-$500.00</span>
        <button class="delete-btn">X</button>
      </li>
    </ul>

    <h3>Add New Transaction</h3>
    <form id="form">
      <div class="form-control">
        <label for="text">Text</label>
        <input type="text" id="text" placeholder="Enter text..." />
      </div>
      <div class="form-control">
        <label for="amount">Amount</label>
        <input type="number" id="amount" placeholder="Enter Amount..." />
        <small>(negative = expense, positive = income)</small>
      </div>
      <button class="btn">Add Transaction</button>
    </form>
  </div>

  <script src="script.js"></script>
</body>
</html>

// CSS File Started

:root {
  --box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background-color: #f9f9f9;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  padding: 30px 10px;
  min-height: 100vh;
  color: #333;
}

.container {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: var(--box-shadow);
  width: 100%;
  max-width: 400px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  font-size: 22px;
}

h4 {
  text-transform: uppercase;
  font-size: 14px;
  margin-bottom: 5px;
  font-weight: 600;
}

#balance {
  font-size: 28px;
  margin: 5px 0 20px;
  font-weight: bold;
}

.inc-exp-container {
  background: #fff;
  box-shadow: var(--box-shadow);
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
  padding: 20px;
  border-radius: 6px;
}

.inc-exp-container > div {
  flex: 1;
  text-align: center;
}

.inc-exp-container > div:first-of-type {
  border-right: 1px solid #bbb;
}

.inc-exp-container h4 {
  margin-bottom: 8px;
}

.money {
  font-size: 18px;
  font-weight: bold;
}

.money.plus {
  color: #2ecc71; /* green */
}

.money.minus {
  color: #e74c3c; /* red */
}

/* History section */
h3 {
  margin: 20px 0 10px;
  font-size: 16px;
  border-bottom: 1px solid #ccc;
  padding-bottom: 5px;
}

.list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.list li {
  background: #fff;
  box-shadow: var(--box-shadow);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px;
  margin: 8px 0;
  border-radius: 4px;
  font-size: 15px;
}

.list li.plus {
  border-right: 5px solid #2ecc71;
}

.list li.minus {
  border-right: 5px solid #e74c3c;
}

.delete-btn {
  background: #e74c3c;
  border: none;
  color: #fff;
  padding: 1px 6px;
  padding-bottom: 3px;
  border-radius: 10%;
  cursor: pointer;
  margin-left: 10px;
  opacity: 0;
}

.delete-btn:hover {
  opacity: 1;
}

/* Form */
.form-control {
  margin-bottom: 10px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-size: 14px;
  font-weight: 500;
}

input[type="text"],
input[type="number"] {
  width: 100%;
  padding: 10px;
  border: 1px solid #dedede;
  border-radius: 4px;
  font-size: 16px;
}

.btn {
  cursor: pointer;
  background-color: #880db8;
  color: #fff;
  border: none;
  padding: 12px;
  width: 100%;
  border-radius: 6px;
  margin-top: 10px;
  font-size: 16px;
}

.btn:hover {
  background-color: #671789;
}


//Javascript file started 


const balance = document.getElementById('balance');
const money_plus = document.getElementById('money-plus');
const money_minus = document.getElementById('money-minus');
const list = document.getElementById('list');
const form = document.getElementById('form');
const text = document.getElementById('text');
const amount = document.getElementById('amount');

const localStorageTransactions = JSON.parse(
    localStorage.getItem('transactions')
);
let transactions = localStorage.getItem('transactions') !== null ? localStorageTransactions : [];

function addTransaction(e) {
    e.preventDefault();
    if (text.value.trim() === '' || amount.value.trim() === '') {
        alert('Please enter a text and amount');
    } else {
        const transaction = {
            id: generateID(),
            text: text.value,
            amount: +amount.value
        };
        transactions.push(transaction);
        addTransactionDOM(transaction);
        addLocalStorage();
        updateValues();
        text.value = '';
        amount.value = '';

    }
}
function generateID() {
    return Math.round(Math.random() * 100000000);
}
function addTransactionDOM(transaction) {
    const sign = transaction.amount < 0 ? '-' : '+';
    const item = document.createElement('li');
    item.classList.add(transaction.amount < 0 ? 'minus' : 'plus');
    item.innerHTML = `
        ${transaction.text} <span>${sign}$${Math.abs(transaction.amount).toFixed(2)}</span>
        <button class="delete-btn" onclick="removeTransaction(${transaction.id})">X</button>
    `;
    list.appendChild(item);
}  
function updateValues() {
    const amounts = transactions.map(transaction => transaction.amount);
    const total = amounts.reduce((acc, item) => (acc += item), 0).toFixed(2);
    const income = amounts
        .filter(item => item > 0)
        .reduce((acc, item) => (acc += item), 0)
        .toFixed(2);
    const expense = (
        amounts.filter(item => item < 0).reduce((acc, item) => (acc += item), 0) * -1
    ).toFixed(2);
    balance.innerText = `$${total}`;
    money_plus.innerText = `$${income}`;
    money_minus.innerText = `-$${expense}`;
}  
function removeTransaction(id) {
    transactions = transactions.filter(transaction => transaction.id !== id);
    addLocalStorage();
    init();
}
function init() {
    list.innerHTML = '';
    transactions.forEach(addTransactionDOM);
    updateValues();
}
init();

form.addEventListener('submit', addTransaction);

function addLocalStorage() {
    localStorage.setItem('transactions', JSON.stringify(transactions));
}
