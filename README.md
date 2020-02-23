# Income
This repository shows an example of js code that performs the budget calculation function (Calculation of the daily budget, possible income, savings, mandatory expenses, etc.). If you are interested in how this is done or how I did it, then see below

---

## 1 - First we create an html structure

```html
    <div class="main">
        <div class="data">
            <div class="choose-expenses">Enter required expenses</div>
            <input class="expenses-item" type="text" id="expenses_1" placeholder="Name">
			<input class="expenses-item" type="text" id="expenses_2" placeholder="Price">
			<input class="expenses-item" type="text" id="expenses_3" placeholder="Name">
			<input class="expenses-item" type="text" id="expenses_4" placeholder="Price">
            <button class="expenses-item-btn">Approve</button>
            
            <div class="optionalexpenses">Enter optional expenses</div>
			<input class="optionalexpenses-item" type="text" id="optionalexpenses_1">
			<input class="optionalexpenses-item" type="text" id="optionalexpenses_2">
			<input class="optionalexpenses-item" type="text" id="optionalexpenses_3">
            <button class="optionalexpenses-btn">Approve</button>
            
            <div class="count-budget">Calculation of daily budget</div>
            <button class="count-budget-btn">Calculate</button>
            
            <div class="choose-income-label">Enter possible revenue items separated by commas</div>
            <input class="choose-income" type="text" id="income">
            
            <div class="checksavings">Are there any savings: 
                <input id="savings" type="checkbox" />
            </div>
            <label for="sum">Amount</label>
			<input class="choose-sum" id="sum" type="text">
			<label for="percent">Percent</label>
            <input class="choose-percent" id="percent" type="text">
            
            <div class="open">
                <button class="start" id="start">To start the calculation</button>
            </div>
        </div>
        <div class="result">
            <div class="result-table">
                <div class="budget">Income:</div>
                <div class="budget-value"></div>
    
                <div class="daybudget">Budget for 1 day:</div>
                <div class="daybudget-value"></div>
    
                <div class="level">Income level:</div>
                <div class="level-value"></div>
    
                <div class="expenses">Mandatory expenses:</div>
                <div class="expenses-value"></div>
    
                <div class="optionalexpenses">Possible expenses:</div>
                <div class="optionalexpenses-value"></div>
    
                <div class="income">Additional income: </div>
                <div class="income-value"></div>
    
                <div class="monthsavings">Savings for 1 month:</div>
                <div class="monthsavings-value"></div>
    
                <div class="yearsavings">Savings for 1 year:</div>
                <div class="yearsavings-value"></div>
            </div>
            <div class="time-data">
                <div class="year">
                    Year:
                    <input class="year-value" type="text" readonly>
                </div>
                <div class="month">
                    Month:
                    <input class="month-value" type="text" readonly>
                </div>
                <div class="day">
                    Day:
                    <input class="day-value" type="text" readonly>
                </div>
            </div>
        </div>
    </div>
```
#### _You can use your own classes, tags, structure, etc._
#### _All styles are present in the repository._
---

## 2 - Then, we attach all the necessary elements from the html page to the js file

```js
    let startBtn = document.getElementById("start"),
      budgetValue = document.getElementsByClassName('budget-value')[0],
      dayBudgetValue = document.getElementsByClassName('daybudget-value')[0],
      levelValue = document.getElementsByClassName('level-value')[0],
      expensesValue = document.getElementsByClassName('expenses-value')[0],
      optionalExpensesValue = document.getElementsByClassName('optionalexpenses-value')[0],
      incomeValue = document.getElementsByClassName('income-value')[0],
      monthSavingsValue = document.getElementsByClassName('monthsavings-value')[0],
      yearSavingsValue = document.getElementsByClassName('yearsavings-value')[0],
      expensesItem = document.getElementsByClassName('expenses-item'),
      expensesBtn = document.getElementsByTagName('button')[0],
      optionalExpensesBtn = document.getElementsByTagName('button')[1],
      countBtn = document.getElementsByTagName('button')[2],
      optionalExpensesItem = document.querySelectorAll('.optionalexpenses-item'),
      incomeItem = document.querySelector('.choose-income'),
      checkSavings = document.querySelector('#savings'),
      sumValue = document.querySelector('.choose-sum'),
      percentValue = document.querySelector('.choose-percent'), 
      yearValue = document.querySelector('.year-value'),
      monthValue = document.querySelector('.month-value'),
      dayValue = document.querySelector('.day-value');
```

---

## 3 - Writing the script itself

* ### A - First, create an object (appData)

```js
    let appData = {
        budget: money,
        timeData: time,
        expenses: {},
        optionalExpenses: {},
        income: [],
        savings: false
    };
```

* ### B - And then, we write everything else

```js 
    let money, time;

    startBtn.addEventListener('click', function() {
        time = prompt ("Введите дату в формате YYYY-MM-DD", "");
        money = +prompt ("Ваш бюджет на месяц?", "");

        while (isNaN(money) || money == "" || money == null) {
            money = +prompt ("Ваш бюджет на месяц?", ""); 
        }
        while ( time == "" || time == null) {
            time = +prompt ("Введите дату в формате YYYY-MM-DD", ""); 
        }

        appData.budget = money;
        appData.timeData = time;
        budgetValue.textContent = money.toFixed();
        yearValue.value = new Date(Date.parse(time)).getFullYear();
        monthValue.value = new Date(Date.parse(time)).getMonth() + 1;
        dayValue.value = new Date(Date.parse(time)).getDate();
    });

    expensesBtn.addEventListener('click', function() {
        let sum = 0;

        for (let i = 0; i < expensesItem.length; i++) {
            let a = expensesItem[i].value,
                b = expensesItem[++i].value;

            if ( typeof(a)==='string' && typeof(a) != null && typeof(b) != null && a != "" && b != "" && a.length < 50) {
                console.log ("done");
                appData.expenses[a] = b;
                sum += +b;
            } else {
                console.log ("bad result");
                i--;
            }

        }
        expensesValue.textContent = sum;
    });

    optionalExpensesBtn.addEventListener('click', function() {
        for (let i = 0; i < optionalExpensesItem.length; i++) {
            let opt = optionalExpensesItem[i].value;
            appData.optionalExpenses[i] = opt;
            optionalExpensesValue.textContent += appData.optionalExpenses[i] + ` `;
        }
    });

    countBtn.addEventListener('click', function() {

        if(appData.budget != undefined) {
            appData.moneyPerDay = (appData.budget / 30).toFixed();
            dayBudgetValue.textContent = appData.moneyPerDay;

            if (appData.moneyPerDay < 100) {
                levelValue.textContent = 'Это минимальный уровень достатка!';
            } else if (appData.moneyPerDay > 100 && appData.moneyPerDay < 2000) {
                levelValue.textContent = 'Это средний уровень достатка!';
            } else if (appData.moneyPerDay > 2000) {
                levelValue.textContent ='Это высокий уровень достатка!';
            } else {
                levelValue.textContent = 'Ошибочка...!';
            }
        }else {
            dayBudgetValue.textContent = 'Ошибочка...!';
        }

    });

    incomeItem.addEventListener('input', function() {
        let items = incomeItem.value;
        appData.income = items.split(", ");
        incomeValue.textContent = appData.income;
    });

    checkSavings.addEventListener("click", () => {
        if (appData.savings == true) {
            appData.savings = false;
        } else {
            appData.savings = true;
        }
    });

    sumValue.addEventListener('input', function() {
        if (appData.savings == true) {
            let sum = +sumValue.value,
                percent = +percentValue.value;

            appData.monthIncome = sum/100/12*percent;
            appData.yearIncome = sum/100*percent;

            monthSavingsValue.textContent = appData.monthIncome.toFixed(1);
            yearSavingsValue.textContent = appData.yearIncome.toFixed(1);
        }
    });

    percentValue.addEventListener('input', function() {
        if (appData.savings == true) {
            let sum = +sumValue.value,
                percent = +percentValue.value;

            appData.monthIncome = sum/100/12*percent;
            appData.yearIncome = sum/100*percent;

            monthSavingsValue.textContent = appData.monthIncome.toFixed(1);
            yearSavingsValue.textContent = appData.yearIncome.toFixed(1);
        }
    });
```
---
## 

    
    
    
    
    
    
    
