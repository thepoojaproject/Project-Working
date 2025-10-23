
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minimal Scientific Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, sans-serif;
        }
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: #f5f5f5;
            padding: 20px;
        }
        
        .calculator {
            background-color: #fff;
            border-radius: 12px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.08);
            width: 320px;
            overflow: hidden;
            padding: 20px;
        }
        
        .display {
            background-color: #f8f9fa;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 15px;
            text-align: right;
            color: #333;
            min-height: 80px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        
        .previous-operand {
            font-size: 0.9rem;
            color: #666;
            min-height: 1.2rem;
            word-wrap: break-word;
            word-break: break-all;
        }
        
        .current-operand {
            font-size: 1.8rem;
            font-weight: 500;
            word-wrap: break-word;
            word-break: break-all;
        }
        
        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-gap: 8px;
        }
        
        button {
            border: none;
            border-radius: 6px;
            padding: 12px 0;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.1s ease;
            color: #333;
            background-color: #f0f0f0;
        }
        
        button:hover {
            background-color: #e0e0e0;
        }
        
        button:active {
            transform: scale(0.97);
        }
        
        .operation {
            background-color: #e9ecef;
        }
        
        .operation:hover {
            background-color: #dee2e6;
        }
        
        .equals {
            background-color: #007bff;
            color: white;
        }
        
        .equals:hover {
            background-color: #0069d9;
        }
        
        .scientific {
            background-color: #f8f9fa;
            font-size: 0.85rem;
        }
        
        .scientific:hover {
            background-color: #e9ecef;
        }
        
        .clear {
            background-color: #f8f9fa;
        }
        
        .clear:hover {
            background-color: #e9ecef;
        }
        
        .zero {
            grid-column: span 2;
        }
        
        .mode-toggle {
            background-color: #f8f9fa;
            font-size: 0.8rem;
            margin-top: 10px;
            width: 100%;
            padding: 8px;
        }
        
        .mode-toggle:hover {
            background-color: #e9ecef;
        }
        
        @media (max-width: 480px) {
            .calculator {
                width: 100%;
                padding: 15px;
            }
            
            button {
                padding: 10px 0;
                font-size: 0.9rem;
            }
            
            .current-operand {
                font-size: 1.5rem;
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
            <button class="clear" data-action="clear">C</button>
            <button class="clear" data-action="clear-entry">CE</button>
            <button class="scientific" data-action="backspace">⌫</button>
            <button class="scientific" data-operation="%">%</button>
            <button class="scientific" data-operation="√">√</button>
            
            <button class="scientific" data-operation="x²">x²</button>
            <button class="scientific" data-operation="1/x">1/x</button>
            <button class="scientific" data-operation="x^y">x^y</button>
            <button class="scientific" data-operation="log">log</button>
            <button class="operation" data-operation="÷">÷</button>
            
            <button class="scientific" data-operation="sin">sin</button>
            <button class="scientific" data-operation="cos">cos</button>
            <button class="scientific" data-operation="tan">tan</button>
            <button class="number" data-number="7">7</button>
            <button class="operation" data-operation="×">×</button>
            
            <button class="scientific" data-operation="π">π</button>
            <button class="scientific" data-operation="e">e</button>
            <button class="scientific" data-operation="±">±</button>
            <button class="number" data-number="8">8</button>
            <button class="number" data-number="9">9</button>
            
            <button class="scientific" data-operation="ln">ln</button>
            <button class="scientific" data-operation="10^x">10^x</button>
            <button class="operation" data-operation="-">-</button>
            <button class="number" data-number="4">4</button>
            <button class="number" data-number="5">5</button>
            
            <button class="number" data-number="6">6</button>
            <button class="operation" data-operation="+">+</button>
            <button class="number" data-number="1">1</button>
            <button class="number" data-number="2">2</button>
            <button class="number" data-number="3">3</button>
            
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
                    case '%':
                        this.currentOperand = (current / 100).toString();
                        break;
                    default:
                        return;
                }
                
                this.shouldResetScreen = true;
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
        
        // Action buttons
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
                }
                
                calculator.updateDisplay();
            });
        });
        
        // Mode toggle
        const modeToggle = document.getElementById('mode-toggle');
        modeToggle.addEventListener('click', () => {
            const isScientific = calculator.toggleMode();
            const scientificButtons = document.querySelectorAll('.scientific');
            
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
