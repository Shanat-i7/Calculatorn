<!DOCTYPE html>
<html>    
<head>

</head>
<div class="calc">
        <div id="screen" class="screen">
            <div class="screen-operator"></div>
            <div class="screen-calculation"></div>
        </div>
        <button class="button-c">C</button>
        <button class="button-del">←</button>
        <button class="op-divide">÷</button>
        <button class="num-7">7</button>
        <button class="num-8">8</button>
        <button class="num-9">9</button>
        <button class="op-multiply">×</button>
        <button class="num-4">4</button>
        <button class="num-5">5</button>
        <button class="num-6">6</button>
        <button class="op-substract">-</button>
        <button class="num-1">1</button>
        <button class="num-2">2</button>
        <button class="num-3">3</button>
        <button class="op-add">+</button>
        <button class="num-0">0</button>
        <button class="op-equal">=</button>
    </div>



<body>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@600&family=Unica+One&display=swap');

body {
    background-color: #212F3D ;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    margin:0;
}
  
.calc {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: repeat(6, 1fr);
    grid-column-gap: 0px;
    grid-row-gap: 0px;
    width: 500px;
    margin: 1rem auto;
    border: 15px solid #F7F7F7;
    border-radius:7px;
    box-shadow: rgba(0, 0, 0, 0.3) 0px 19px 38px, rgba(0, 0, 0, 0.22) 0px 15px 12px;
}
  
.calc > * {
    font-size: 3rem;
    line-height: 7rem;
    text-align: center;
    border: 5px solid #F7F7F7;
}

.calc > button {
    font-family: 'Inter', sans-serif;
    font-weight: 600;
}
  
.screen { grid-area: 1 / 1 / 2 / 5; }
.button-c { grid-area: 2 / 1 / 3 / 3; }
.button-del { grid-area: 2 / 3 / 3 / 4; }
.op-divide { grid-area: 2 / 4 / 3 / 5; }
.num-7 { grid-area: 3 / 1 / 4 / 2; }
.num-8 { grid-area: 3 / 2 / 4 / 3; }
.num-9 { grid-area: 3 / 3 / 4 / 4; }
.op-multiply { grid-area: 3 / 4 / 4 / 5; }
.num-4 { grid-area: 4 / 1 / 5 / 2; }
.num-5 { grid-area: 4 / 2 / 5 / 3; }
.num-6 { grid-area: 4 / 3 / 5 / 4; }
.op-substract { grid-area: 4 / 4 / 5 / 5; }
.num-1 { grid-area: 5 / 1 / 6 / 2; }
.num-2 { grid-area: 5 / 2 / 6 / 3; }
.num-3 { grid-area: 5 / 3 / 6 / 4; }
.op-add { grid-area: 5 / 4 / 6 / 5; }
.num-0 { grid-area: 6 / 1 / 7 / 4; }
.op-equal { grid-area: 6 / 4 / 7 / 5; }

[class^="op"] {
    /* background: ; */
    background: #CB3544;
    color: #FFF;
}

[class^="op"]:hover {
    background: #BE2B3A;
}

[class^="op"]:active {
    background: #8B1823;
}
  
[class^="num"], [class^="button"] {
    background: #566573;
    color: #EAECEE;
}

[class^="num"]:active, [class^="button"]:active {
    background:#2C3E50;
}

[class^="num"]:hover, [class^="button"]:hover {
    background:#495764;
}

button:hover {
    cursor: pointer;
}
  
.screen {
    position:relative;
    padding: 0 1rem;
    display: flex;
    justify-content: space-between;
    font-family: 'Unica One', cursive;
    font-weight: 400;
    font-size: 4rem;
    background-color: #1D2227;
    color: white;
    text-align: right;
}

.screen-operator {
    color: #ccc;
    line-height: 7rem;
    text-align: center;
}

.screen-calculation {
  max-width: 400px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
<script>
  
  let result = 0;
let onScreen = '0';
let operator;
let mathBuffer;
let screen = document.querySelector('.screen-calculation');

function buttonClick(value) {
    if (isNaN(parseInt(value))) {
      handleClick(value);
    } else {
      handleNumber(value);
    }
    render();
}

function render() {
    screen.innerText = onScreen;
    (typeof operator == "undefined") ? showOperator('') : showOperator(operator);
}

function handleNumber(value) {    
    if (onScreen === '0' || onScreen === result) {
        onScreen = value;
    } else {
        onScreen += value;
    }
}

function handleMath(value) {
    if (onScreen === '0') {
        return;
    }

    let mathBuffer = parseInt(onScreen);

    if (result === 0) {
        result = mathBuffer;
    } else {
        doMath(mathBuffer)
    }

    operator = value;
    onScreen = result;
}

function doMath(mathBuffer) {
    if (operator === "×") { result *= mathBuffer; }
    if (operator === "-") { result -= mathBuffer; }
    if (operator === "+") { result += mathBuffer; }
    if (operator === "÷") { result /= mathBuffer; }
}

function showOperator(val) {
    let currentOperator = document.querySelector('.screen-operator');
    currentOperator.innerText = val;
}

function handleClick(value) {
   switch(value) {
        case "C":
            result = 0;
            mathBuffer = 0;
            operator = undefined;
            onScreen = "0";
            break;
        case "←":
            if (onScreen.length === 1) {
                onScreen = "0";
            } else {
                onScreen = onScreen.substring(0, onScreen.length - 1);
            }
            break;
        case "=":
            if (operator === null) {
                return;
            }
            doMath(parseInt(onScreen));
            operator = value;            
            onScreen = +result;
            result = 0;
            break;
        case "+":
        case "-":
        case "×":
        case "÷":
            handleMath(value);
            break;    
   }
}

function initCalc() {
    const calcButtons = document.querySelectorAll("button");
    for (let i = 0; i < calcButtons.length; i++) {
        calcButtons[i].addEventListener("click", function(event) {
            buttonClick(event.target.innerText);          
        });
    }
    render();
}

initCalc();
  
  
  </script>
</body>
</html>




 
