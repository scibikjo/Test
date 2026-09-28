<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Moderner Taschenrechner</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome für Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e1b4b 100%);
            touch-action: manipulation;
        }

        /* Glaseffekt und Schatten */
        .glass-panel {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
        }

        /* Custom Button Styling */
        .calc-btn {
            position: relative;
            user-select: none;
            transition: all 0.15s ease-in-out;
            outline: none;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.25rem;
            font-weight: 500;
            border-radius: 1rem;
        }

        .calc-btn:active {
            transform: scale(0.93);
        }

        /* Button-Varianten */
        .btn-number {
            background-color: rgba(51, 65, 85, 0.6);
            color: #f8fafc;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        .btn-number:hover {
            background-color: rgba(71, 85, 105, 0.8);
        }

        .btn-operator {
            background-color: rgba(79, 70, 229, 0.25);
            color: #818cf8;
            border: 1px solid rgba(129, 140, 248, 0.2);
            font-weight: 600;
        }
        .btn-operator:hover {
            background-color: rgba(79, 70, 229, 0.45);
            color: #ffffff;
        }

        .btn-operator.active {
            background-color: #6366f1;
            color: #ffffff;
            box-shadow: 0 0 15px rgba(99, 102, 241, 0.5);
        }

        .btn-action {
            background-color: rgba(239, 68, 68, 0.15);
            color: #f87171;
            border: 1px solid rgba(248, 113, 113, 0.2);
        }
        .btn-action:hover {
            background-color: rgba(239, 68, 68, 0.3);
            color: #ffffff;
        }

        .btn-equals {
            background: linear-gradient(135deg, #6366f1 0%, #4f46e5 100%);
            color: #ffffff;
            box-shadow: 0 4px 15px rgba(79, 70, 229, 0.4);
            font-weight: 600;
        }
        .btn-equals:hover {
            background: linear-gradient(135deg, #4f46e5 0%, #4338ca 100%);
            box-shadow: 0 6px 20px rgba(79, 70, 229, 0.6);
        }

        /* Smooth scrollbar für Historie */
        .custom-scrollbar::-webkit-scrollbar {
            width: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: transparent;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 4px;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover {
            background: rgba(255, 255, 255, 0.25);
        }
    </style>
</head>
<body class="min-h-screen flex items-center justify-center p-4 text-slate-100">

    <!-- Hauptcontainer -->
    <div class="glass-panel w-full max-w-md rounded-3xl p-6 relative overflow-hidden flex flex-col justify-between shadow-2xl">
        
        <!-- Kopfzeile mit Modus/Titel und Historien-Toggle -->
        <div class="flex items-center justify-between mb-4 px-2">
            <div class="flex items-center gap-2">
                <span class="w-3 h-3 rounded-full bg-indigo-500 animate-pulse"></span>
                <span class="text-xs font-semibold tracking-wider text-slate-400 uppercase">Rechner</span>
            </div>
            <button id="toggle-history-btn" class="text-slate-400 hover:text-indigo-400 transition-colors p-2 rounded-lg hover:bg-slate-800/50" title="Historie anzeigen/ausblenden">
                <i class="fa-solid me-1 fa-clock-rotate-left text-sm"></i>
                <span class="text-xs font-medium">Historie</span>
            </button>
        </div>

        <!-- Displays: Historie & Aktuelle Anzeige -->
        <div class="flex flex-col justify-end text-right p-4 mb-6 bg-slate-900/60 rounded-2xl border border-slate-800/80 min-h-[120px] overflow-hidden">
            <!-- Vorherige Rechnung / Formel -->
            <div id="previous-display" class="text-slate-400 text-sm h-6 mb-1 font-mono overflow-x-auto whitespace-nowrap custom-scrollbar"></div>
            <!-- Haupt-Eingabe/Ergebnis -->
            <div id="current-display" class="text-4xl sm:text-5xl font-light tracking-tight font-mono text-white overflow-x-auto whitespace-nowrap custom-scrollbar">0</div>
        </div>

        <!-- Keyboard Tasten-Grid -->
        <div class="grid grid-cols-4 gap-3">
            <!-- Reihe 1 -->
            <button class="calc-btn btn-action h-14" data-action="clear" title="Alles Löschen (Esc)">AC</button>
            <button class="calc-btn btn-action h-14" data-action="backspace" title="Rücktaste (Backspace)">
                <i class="fa-solid fa-backspace text-lg"></i>
            </button>
            <button class="calc-btn btn-operator h-14" data-action="percent" title="Prozent (%)">%</button>
            <button class="calc-btn btn-operator h-14" data-operator="/" title="Dividieren (/)">÷</button>

            <!-- Reihe 2 -->
            <button class="calc-btn btn-number h-14" data-number="7">7</button>
            <button class="calc-btn btn-number h-14" data-number="8">8</button>
            <button class="calc-btn btn-number h-14" data-number="9">9</button>
            <button class="calc-btn btn-operator h-14" data-operator="*" title="Multiplizieren (*)">×</button>

            <!-- Reihe 3 -->
            <button class="calc-btn btn-number h-14" data-number="4">4</button>
            <button class="calc-btn btn-number h-14" data-number="5">5</button>
            <button class="calc-btn btn-number h-14" data-number="6">6</button>
            <button class="calc-btn btn-operator h-14" data-operator="-" title="Subtrahieren (-)">−</button>

            <!-- Reihe 4 -->
            <button class="calc-btn btn-number h-14" data-number="1">1</button>
            <button class="calc-btn btn-number h-14" data-number="2">2</button>
            <button class="calc-btn btn-number h-14" data-number="3">3</button>
            <button class="calc-btn btn-operator h-14" data-operator="+" title="Addieren (+)">+</button>

            <!-- Reihe 5 -->
            <button class="calc-btn btn-number h-14" data-action="toggle-sign" title="Vorzeichen wechseln (±)">±</button>
            <button class="calc-btn btn-number h-14" data-number="0">0</button>
            <button class="calc-btn btn-number h-14" data-number="." title="Komma (.)">,</button>
            <button class="calc-btn btn-equals h-14" data-action="calculate" title="Berechnen (Enter)">=</button>
        </div>

        <!-- Overlay Drawer für Historie -->
        <div id="history-drawer" class="absolute inset-0 bg-slate-900/95 backdrop-blur-md z-20 translate-y-full transition-transform duration-300 ease-in-out p-6 flex flex-col">
            <div class="flex items-center justify-between mb-4 border-b border-slate-800 pb-3">
                <h3 class="font-semibold text-slate-200 flex items-center gap-2">
                    <i class="fa-solid fa-clock-rotate-left text-indigo-400"></i> Verlauf
                </h3>
                <div class="flex items-center gap-2">
                    <button id="clear-history-btn" class="text-xs text-rose-400 hover:text-rose-300 p-2 rounded-lg hover:bg-rose-500/10 transition-colors" title="Verlauf leeren">
                        <i class="fa-solid fa-trash-can me-1"></i> Leeren
                    </button>
                    <button id="close-history-btn" class="text-slate-400 hover:text-white p-2 rounded-lg hover:bg-slate-800 transition-colors">
                        <i class="fa-solid fa-xmark text-lg"></i>
                    </button>
                </div>
            </div>
            <!-- Historien-Liste -->
            <div id="history-list" class="flex-1 overflow-y-auto custom-scrollbar space-y-3 pr-1 text-right">
                <div id="history-empty" class="text-center text-slate-500 py-12 text-sm">
                    Noch keine Berechnungen vorhanden
                </div>
            </div>
        </div>
    </div>

    <script>
        class Calculator {
            constructor(previousDisplayElement, currentDisplayElement, historyListElement) {
                this.previousDisplayElement = previousDisplayElement;
                this.currentDisplayElement = currentDisplayElement;
                this.historyListElement = historyListElement;
                this.history = [];
                this.clear();
            }

            clear() {
                this.currentOperand = '0';
                this.previousOperand = '';
                this.operation = undefined;
                this.resetNextInput = false;
                this.updateDisplay();
            }

            delete() {
                if (this.resetNextInput) {
                    this.clear();
                    return;
                }
                if (this.currentOperand === '0') return;
                if (this.currentOperand.length === 1 || (this.currentOperand.length === 2 && this.currentOperand.startsWith('-'))) {
                    this.currentOperand = '0';
                } else {
                    this.currentOperand = this.currentOperand.toString().slice(0, -1);
                }
                this.updateDisplay();
            }

            appendNumber(number) {
                if (this.resetNextInput) {
                    this.currentOperand = '';
                    this.resetNextInput = false;
                }
                if (number === '.' && this.currentOperand.includes('.')) return;
                if (this.currentOperand === '0' && number !== '.') {
                    this.currentOperand = number.toString();
                } else {
                    this.currentOperand = this.currentOperand.toString() + number.toString();
                }
                this.updateDisplay();
            }

            toggleSign() {
                if (this.currentOperand === '0') return;
                this.currentOperand = (parseFloat(this.currentOperand) * -1).toString();
                this.updateDisplay();
            }

            percentage() {
                if (this.currentOperand === '0') return;
                this.currentOperand = (parseFloat(this.currentOperand) / 100).toString();
                this.updateDisplay();
            }

            chooseOperation(operation) {
                if (this.currentOperand === '' && this.previousOperand === '') return;
                
                if (this.previousOperand !== '' && !this.resetNextInput) {
                    this.compute();
                }

                this.operation = operation;
                this.previousOperand = this.currentOperand;
                this.resetNextInput = true;
                this.updateDisplay();
            }

            compute() {
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
                    case '*':
                        computation = prev * current;
                        break;
                    case '/':
                        if (current === 0) {
                            this.currentOperand = 'Fehler';
                            this.previousOperand = '';
                            this.operation = undefined;
                            this.resetNextInput = true;
                            this.updateDisplay();
                            return;
                        }
                        computation = prev / current;
                        break;
                    default:
                        return;
                }

                // Genauigkeit bei Fließkommazahlen korrigieren
                computation = Math.round(computation * 1e10) / 1e10;

                const expression = `${this.formatDisplayNumber(prev)} ${this.getOperatorSymbol(this.operation)} ${this.formatDisplayNumber(current)}`;
                this.addHistoryItem(expression, computation);

                this.currentOperand = computation.toString();
                this.operation = undefined;
                this.previousOperand = '';
                this.resetNextInput = true;
                this.updateDisplay();
            }

            getOperatorSymbol(op) {
                switch (op) {
                    case '+': return '+';
                    case '-': return '−';
                    case '*': return '×';
                    case '/': return '÷';
                    default: return '';
                }
            }

            formatDisplayNumber(number) {
                if (number === 'Fehler') return 'Fehler';
                const stringNumber = number.toString();
                const integerDigits = parseFloat(stringNumber.split('.')[0]);
                const decimalDigits = stringNumber.split('.')[1];
                let integerDisplay;
                
                if (isNaN(integerDigits)) {
                    integerDisplay = '';
                } else {
                    integerDisplay = integerDigits.toLocaleString('de-DE', { maximumFractionDigits: 0 });
                }

                if (decimalDigits != null) {
                    return `${integerDisplay},${decimalDigits}`;
                } else {
                    return integerDisplay;
                }
            }

            updateDisplay() {
                if (this.currentOperand === 'Fehler') {
                    this.currentDisplayElement.innerText = 'Fehler';
                } else {
                    this.currentDisplayElement.innerText = this.formatDisplayNumber(this.currentOperand);
                }

                if (this.operation != null) {
                    this.previousDisplayElement.innerText = 
                        `${this.formatDisplayNumber(this.previousOperand)} ${this.getOperatorSymbol(this.operation)}`;
                } else {
                    this.previousDisplayElement.innerText = '';
                }

                // Aktiven Operator hervorheben
                document.querySelectorAll('[data-operator]').forEach(btn => {
                    if (btn.dataset.operator === this.operation && this.resetNextInput) {
                        btn.classList.add('active');
                    } else {
                        btn.classList.remove('active');
                    }
                });
            }

            addHistoryItem(expression, result) {
                const historyObj = { expression, result: this.formatDisplayNumber(result) };
                this.history.unshift(historyObj);
                this.renderHistory();
            }

            renderHistory() {
                const emptyMsg = document.getElementById('history-empty');
                if (this.history.length === 0) {
                    if (emptyMsg) emptyMsg.classList.remove('hidden');
                    this.historyListElement.innerHTML = '';
                    if (emptyMsg) this.historyListElement.appendChild(emptyMsg);
                    return;
                }

                this.historyListElement.innerHTML = '';
                this.history.forEach((item) => {
                    const el = document.createElement('div');
                    el.className = 'p-3 rounded-xl bg-slate-800/50 hover:bg-slate-800 transition-colors cursor-pointer border border-slate-700/40 text-right';
                    el.innerHTML = `
                        <div class="text-xs text-slate-400 font-mono">${item.expression} =</div>
                        <div class="text-lg font-semibold text-indigo-300 font-mono">${item.result}</div>
                    `;
                    el.addEventListener('click', () => {
                        // Klick lädt das Ergebnis in den Rechner
                        this.currentOperand = item.result.replace(/\./g, '').replace(',', '.');
                        this.resetNextInput = true;
                        this.updateDisplay();
                        toggleHistory(false);
                    });
                    this.historyListElement.appendChild(el);
                });
            }

            clearHistory() {
                this.history = [];
                this.renderHistory();
            }
        }

        // Initialisierung
        const previousDisplayElement = document.getElementById('previous-display');
        const currentDisplayElement = document.getElementById('current-display');
        const historyListElement = document.getElementById('history-list');
        const historyDrawer = document.getElementById('history-drawer');

        const calculator = new Calculator(previousDisplayElement, currentDisplayElement, historyListElement);

        // Event Listener für Buttons
        document.querySelectorAll('[data-number]').forEach(button => {
            button.addEventListener('click', () => {
                calculator.appendNumber(button.dataset.number);
            });
        });

        document.querySelectorAll('[data-operator]').forEach(button => {
            button.addEventListener('click', () => {
                calculator.chooseOperation(button.dataset.operator);
            });
        });

        document.querySelectorAll('[data-action]').forEach(button => {
            button.addEventListener('click', () => {
                const action = button.dataset.action;
                switch (action) {
                    case 'clear':
                        calculator.clear();
                        break;
                    case 'backspace':
                        calculator.delete();
                        break;
                    case 'calculate':
                        calculator.compute();
                        break;
                    case 'toggle-sign':
                        calculator.toggleSign();
                        break;
                    case 'percent':
                        calculator.percentage();
                        break;
                }
            });
        });

        // Tastaturunterstützung
        window.addEventListener('keydown', (e) => {
            if ((e.key >= '0' && e.key <= '9')) {
                calculator.appendNumber(e.key);
            } else if (e.key === '.' || e.key === ',') {
                calculator.appendNumber('.');
            } else if (e.key === '+' || e.key === '-' || e.key === '*' || e.key === '/') {
                calculator.chooseOperation(e.key);
            } else if (e.key === 'Enter' || e.key === '=') {
                e.preventDefault();
                calculator.compute();
            } else if (e.key === 'Backspace') {
                calculator.delete();
            } else if (e.key === 'Escape') {
                calculator.clear();
            } else if (e.key === '%') {
                calculator.percentage();
            }
        });

        // Historie Ein-/Ausblenden
        function toggleHistory(show) {
            if (show) {
                historyDrawer.classList.remove('translate-y-full');
            } else {
                historyDrawer.classList.add('translate-y-full');
            }
        }

        document.getElementById('toggle-history-btn').addEventListener('click', () => toggleHistory(true));
        document.getElementById('close-history-btn').addEventListener('click', () => toggleHistory(false));
        document.getElementById('clear-history-btn').addEventListener('click', () => calculator.clearHistory());
    </script>
</body>
</html>
