<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scientific Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            padding: 20px;
        }
        
        .calculator {
            background-color: #1e1e2e;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            width: 400px;
            overflow: hidden;
            padding: 20px;
        }
        
        .display {
            background-color: #2d3047;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            text-align: right;
            color: white;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        
        .previous-operand {
            font-size: 1.2rem;
            color: #a0a0a0;
            min-height: 1.5rem;
            word-wrap: break-word;
            word-break: break-all;
        }
        
        .current-operand {
            font-size: 2.5rem;
            font-weight: 500;
            word-wrap: break-word;
            word-break: break-all;
        }
        
        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-gap: 12px;
        }
        
        button {
            border: none;
            border-radius: 10px;
            padding: 15px 0;
            font-size: 1.2rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
            color: white;
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        button:active {
            transform: translateY(0);
        }
        
        .number {
            background-color: #3a3e5b;
        }
        
        .number:hover {
            background-color: #4a5078;
        }
        
        .operation {
            background-color: #ff9500;
        }
        
        .operation:hover {
            background-color: #ffaa33;
        }
        
        .scientific {
            background-color: #585a7c;
        }
        
        .scientific:hover {
            background-color: #6c6f9c;
        }
        
        .equals {
            background-color: #ff2d55;
            grid-column: span 2;
        }
        
        .equals:hover {
            background-color: #ff4d6d;
        }
        
        .clear {
            background-color: #585a7c;
        }
        
        .clear:hover {
            background-color: #6c6f9c;
        }
        
        .zero {
            grid-column: span 2;
        }
        
        .memory {
            background-color: #2d3047;
            font-size: 1rem;
        }
        
        .memory:hover {
            background-color: #3a3e5b;
        }
        
        .mode-toggle {
            background-color: #2d3047;
            font-size: 1rem;
            margin-top: 15px;
            width: 100%;
        }
        
        .mode-toggle:hover {
            background-color: #3a3e5b;
        }
        
        @media (max-width: 480px) {
            .calculator {
                width: 100%;
                padding: 15px;
            }
            
            button {
                padding: 12px 0;
                font-size: 1rem;
            }
            
            .current-operand {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display">
            <div class="previous-operand"></div>
            <div class="current-operand">0</div>
        </div>
        <div class="buttons-grid">
            <button class="clear scientific" data-action="clear">C</button>
            <button class="clear scientific" data-action="clear-entry">CE</button>
            <button class="scientific" data-action="backspace">⌫</button>
            <button class="scientific" data-operation="%">%</button>
            <button class="scientific" data-operation="1/x">1/x</button>
            
            <button class="scientific" data-operation="x²">x²</button>
            <button class="scientific" data-operation="√">√</button>
            <button class="scientific" data-operation="x^y">x^y</button>
            <button class="scientific" data-operation="log">log</button>
            <button class="scientific" data-operation="ln">ln</button>
            
            <button class="scientific" data-operation="sin">sin</button>
            <button class="scientific" data-operation="cos">cos</button>
            <button class="scientific" data-operation="tan">tan</button>
            <button class="scientific" data-operation="π">π</button>
            <button class="scientific" data-operation="e">e</button>
            
            <button class="memory" data-action="mc">MC</button>
            <button class="memory" data-action="mr">MR</button>
            <button class="memory" data-action="m-plus">M+</button>
            <button class="memory" data-action="m-minus">M-</button>
            <button class="operation" data-operation="÷">÷</button>
            
            <button class="number" data-number="7">7</button>
            <button class="number" data-number="8">8</button>
            <button class="number" data-number="9">9</button>
            <button class="operation" data-operation="×">×</button>
            <button class="scientific" data-operation="10^x">10^x</button>
            
            <button class="number" data-number="4">4</button>
            <button class="number" data-number="5">5</button>
            <button class="number" data-number="6">6</button>
            <button class="operation" data-operation="-">-</button>
            <button class="scientific" data-operation="x!">x!</button>
            
            <button class="number" data-number="1">1</button>
            <button class="number" data-number="2">2</button>
            <button class="number" data-number="3">3</button>
            <button class="operation" data-operation="+">+</button>
            <button class="scientific" data-operation="±">±</button>
            
            <button class="number zero" data-number="0">0</button>
            <button class="number" data-number=".">.</button>
            <button class="equals" data-action="calculate">=</button>
        </div>
        <button class="mode-toggle" id="mode-toggle">Switch to Basic Mode</button>
    </div>

    <script>
        class Calculator {
            constructor(previousOperandElement, currentOperandElement) {
                this.previousOperandElement = previousOperandElement;
                this.currentOperandElement = currentOperandElement;
                this.clear();
                this.memory = 0;
                this.isScientificMode = true;
            }
            
            clear() {
                this.currentOperand = '0';
                this.previousOperand = '';
                this.operation = undefined;
                this.shouldResetScreen = false;
            }
            
            clearEntry() {
                this.currentOperand = '0';
            }
            
            delete() {
                if (this.currentOperand.length === 1) {
                    this.currentOperand = '0';
                } else {
                    this.currentOperand = this.currentOperand.slice(0, -1);
                }
            }
            
            appendNumber(number) {
                if (this.shouldResetScreen) {
                    this.currentOperand = '';
                    this.shouldResetScreen = false;
                }
                
                if (number === '.' && this.currentOperand.includes('.')) return;
                
                if (this.currentOperand === '0' && number !== '.') {
                    this.currentOperand = number;
                } else {
                    this.currentOperand += number;
                }
            }
            
            chooseOperation(operation) {
                if (this.currentOperand === '') return;
                
                if (this.previousOperand !== '') {
                    this.calculate();
                }
                
                this.operation = operation;
                this.previousOperand = this.currentOperand;
                this.shouldResetScreen = true;
            }
            
            calculate() {
                let computation;
                const prev = parseFloat(this.previousOperand);
                const current = parseFloat(this.currentOperand);
                
                if (isNaN(prev) || isNaN(current)) return;
                
                switch (this.operation) {
                    case '+':
                        computation = prev + current;
                        break;
                    case '-':
                        computation = prev - current;
                        break;
                    case '×':
                        computation = prev * current;
                        break;
                    case '÷':
                        computation = prev / current;
                        break;
                    case 'x^y':
                        computation = Math.pow(prev, current);
                        break;
                    default:
                        return;
                }
                
                this.currentOperand = computation.toString();
                this.operation = undefined;
                this.previousOperand = '';
                this.shouldResetScreen = true;
            }
            
            scientificOperation(operation) {
                const current = parseFloat(this.currentOperand);
                
                if (isNaN(current)) return;
                
                switch (operation) {
                    case 'x²':
                        this.currentOperand = (current * current).toString();
                        break;
                    case '√':
                        this.currentOperand = Math.sqrt(current).toString();
                        break;
                    case '1/x':
                        this.currentOperand = (1 / current).toString();
                        break;
                    case '±':
                        this.currentOperand = (-current).toString();
                        break;
                    case 'log':
                        this.currentOperand = Math.log10(current).toString();
                        break;
                    case 'ln':
                        this.currentOperand = Math.log(current).toString();
                        break;
                    case '10^x':
                        this.currentOperand = Math.pow(10, current).toString();
                        break;
                    case 'sin':
                        this.currentOperand = Math.sin(current * Math.PI / 180).toString();
                        break;
                    case 'cos':
                        this.currentOperand = Math.cos(current * Math.PI / 180).toString();
                        break;
                    case 'tan':
                        this.currentOperand = Math.tan(current * Math.PI / 180).toString();
                        break;
                    case 'π':
                        this.currentOperand = Math.PI.toString();
                        break;
                    case 'e':
                        this.currentOperand = Math.E.toString();
                        break;
                    case 'x!':
                        this.currentOperand = this.factorial(current).toString();
                        break;
                    case '%':
                        this.currentOperand = (current / 100).toString();
                        break;
                    default:
                        return;
                }
                
                this.shouldResetScreen = true;
            }
            
            factorial(n) {
                if (n < 0) return NaN;
                if (n === 0 || n === 1) return 1;
                
                let result = 1;
                for (let i = 2; i <= n; i++) {
                    result *= i;
                }
                return result;
            }
            
            memoryOperation(action) {
                const current = parseFloat(this.currentOperand);
                
                switch (action) {
                    case 'mc':
                        this.memory = 0;
                        break;
                    case 'mr':
                        this.currentOperand = this.memory.toString();
                        break;
                    case 'm-plus':
                        this.memory += current;
                        break;
                    case 'm-minus':
                        this.memory -= current;
                        break;
                }
            }
            
            toggleMode() {
                this.isScientificMode = !this.isScientificMode;
                return this.isScientificMode;
            }
            
            updateDisplay() {
                this.currentOperandElement.innerText = this.currentOperand;
                
                if (this.operation != null) {
                    this.previousOperandElement.innerText = 
                        `${this.previousOperand} ${this.operation}`;
                } else {
                    this.previousOperandElement.innerText = '';
                }
            }
        }
        
        // Initialize calculator
        const previousOperandElement = document.querySelector('.previous-operand');
        const currentOperandElement = document.querySelector('.current-operand');
        const calculator = new Calculator(previousOperandElement, currentOperandElement);
        
        // Number buttons
        document.querySelectorAll('[data-number]').forEach(button => {
            button.addEventListener('click', () => {
                calculator.appendNumber(button.innerText);
                calculator.updateDisplay();
            });
        });
        
        // Operation buttons
        document.querySelectorAll('[data-operation]').forEach(button => {
            button.addEventListener('click', () => {
                calculator.chooseOperation(button.innerText);
                calculator.updateDisplay();
            });
        });
        
        // Scientific operation buttons
        document.querySelectorAll('.scientific[data-operation]').forEach(button => {
            button.addEventListener('click', () => {
                calculator.scientificOperation(button.getAttribute('data-operation'));
                calculator.updateDisplay();
            });
        });
        
        // Memory buttons
        document.querySelectorAll('[data-action]').forEach(button => {
            button.addEventListener('click', () => {
                const action = button.getAttribute('data-action');
                
                if (action === 'calculate') {
                    calculator.calculate();
                } else if (action === 'clear') {
                    calculator.clear();
                } else if (action === 'clear-entry') {
                    calculator.clearEntry();
                } else if (action === 'backspace') {
                    calculator.delete();
                } else if (['mc', 'mr', 'm-plus', 'm-minus'].includes(action)) {
                    calculator.memoryOperation(action);
                }
                
                calculator.updateDisplay();
            });
        });
        
        // Mode toggle
        const modeToggle = document.getElementById('mode-toggle');
        modeToggle.addEventListener('click', () => {
            const isScientific = calculator.toggleMode();
            const scientificButtons = document.querySelectorAll('.scientific, .memory');
            
            if (isScientific) {
                modeToggle.innerText = 'Switch to Basic Mode';
                scientificButtons.forEach(button => {
                    button.style.display = 'block';
                });
            } else {
                modeToggle.innerText = 'Switch to Scientific Mode';
                scientificButtons.forEach(button => {
                    button.style.display = 'none';
                });
            }
        });
    </script>
</body>
</html>
