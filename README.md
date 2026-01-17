<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Galactic Workspace</title>
    <style>
        /* ========================
           CSS STYLES
           ======================== */
        :root {
            --bg-deep: #050510;
            --neon-blue: #00f3ff;
            --neon-purple: #bc13fe;
            --glass-bg: rgba(255, 255, 255, 0.05);
            --glass-border: rgba(255, 255, 255, 0.1);
            --text-main: #ffffff;
            --text-muted: #a0a0c0;
            --font-ui: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            --font-mono: 'Courier New', Courier, monospace;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-deep);
            color: var(--text-main);
            font-family: var(--font-ui);
            height: 100vh;
            overflow: hidden;
        }

        /* --- Animated Background --- */
        .background-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            background: radial-gradient(ellipse at bottom, #1b2735 0%, #090a0f 100%);
            overflow: hidden;
        }

        /* Stars using Box Shadow */
        .stars, .stars2, .stars3 {
            width: 1px;
            height: 1px;
            background: transparent;
            position: absolute;
            top: 0; left: 0;
        }

        .stars {
            box-shadow: 10vw 10vh #FFF, 20vw 80vh #FFF, 50vw 50vh #FFF, 80vw 10vh #FFF, 90vw 90vh #FFF, 
                        15vw 35vh #FFF, 45vw 15vh #FFF, 70vw 60vh #FFF, 30vw 90vh #FFF;
            animation: animStar 50s linear infinite;
        }
        .stars2 {
            width: 2px;
            height: 2px;
            box-shadow: 12vw 12vh #FFF, 25vw 85vh #FFF, 55vw 55vh #FFF, 85vw 15vh #FFF, 95vw 95vh #FFF;
            animation: animStar 100s linear infinite;
        }

        @keyframes animStar {
            from { transform: translateY(0px); }
            to { transform: translateY(-2000px); }
        }

        /* --- Layout --- */
        .main-layout {
            display: flex;
            height: 100vh;
            padding: 20px;
            gap: 20px;
        }

        /* --- Glassmorphism Utility --- */
        .glass-panel {
            background: var(--glass-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid var(--glass-border);
            border-radius: 16px;
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.5);
            padding: 20px;
        }

        /* --- Sidebar (Notes) --- */
        .sidebar {
            flex: 1;
            min-width: 300px;
            max-width: 350px;
            display: flex;
            flex-direction: column;
            position: relative;
            overflow: hidden;
        }

        .sidebar-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            border-bottom: 1px solid var(--glass-border);
            padding-bottom: 10px;
        }

        .neon-btn {
            background: transparent;
            border: 1px solid var(--neon-blue);
            color: var(--neon-blue);
            padding: 5px 15px;
            border-radius: 20px;
            cursor: pointer;
            transition: 0.3s;
            text-transform: uppercase;
            font-size: 0.8rem;
            font-weight: bold;
        }

        .neon-btn:hover {
            background: var(--neon-blue);
            color: #000;
            box-shadow: 0 0 10px var(--neon-blue);
        }

        .notes-area {
            flex-grow: 1;
            position: relative;
            overflow-y: auto;
        }

        /* Sticky Note Item */
        .sticky-note {
            position: absolute;
            width: 200px;
            background: rgba(30, 30, 40, 0.9);
            border: 1px solid var(--neon-purple);
            border-radius: 8px;
            padding: 0;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            display: flex;
            flex-direction: column;
            transition: box-shadow 0.2s;
        }

        .sticky-note:hover {
            box-shadow: 0 0 15px rgba(188, 19, 254, 0.3);
            z-index: 10 !important;
        }

        .note-header {
            background: rgba(188, 19, 254, 0.2);
            padding: 5px 10px;
            cursor: grab;
            display: flex;
            justify-content: space-between;
            border-top-left-radius: 8px;
            border-top-right-radius: 8px;
        }

        .note-header:active {
            cursor: grabbing;
        }

        .delete-note {
            cursor: pointer;
            color: #ff4d4d;
            font-weight: bold;
        }

        .note-content {
            background: transparent;
            border: none;
            color: var(--text-main);
            padding: 10px;
            width: 100%;
            min-height: 80px;
            resize: none;
            outline: none;
            font-family: inherit;
        }

        /* --- Workspace (Right Side) --- */
        .workspace {
            flex: 3;
            display: flex;
            gap: 20px;
        }

        /* --- Calculator --- */
        .calculator-section {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .calculator {
            width: 100%;
            max-width: 320px;
        }

        .display {
            background: rgba(0,0,0,0.3);
            padding: 15px;
            border-radius: 10px;
            text-align: right;
            margin-bottom: 15px;
            word-wrap: break-word;
            word-break: break-all;
        }

        .prev-operand {
            color: var(--text-muted);
            font-size: 0.9rem;
            height: 1.2rem;
        }

        .curr-operand {
            font-size: 2rem;
            color: var(--text-main);
        }

        .buttons-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        .buttons-grid button {
            height: 50px;
            border-radius: 10px;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            transition: 0.2s;
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-main);
        }

        .buttons-grid button:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        .span-two { grid-column: span 2; }

        .operator-btn { color: var(--neon-blue) !important; font-weight: bold; }
        .action-btn { color: #ff6b6b !important; }
        .equal-btn {
            background: var(--neon-blue) !important;
            color: #000 !important;
            font-weight: bold;
        }

        /* --- Writing Sheet --- */
        .writing-section {
            flex: 2;
            display: flex;
            flex-direction: column;
        }

        .paper-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            color: var(--text-muted);
        }

        #save-status {
            font-size: 0.8rem;
            color: var(--neon-blue);
            opacity: 0;
            transition: opacity 0.5s;
        }

        .paper-container {
            flex-grow: 1;
            background: rgba(255,255,255,0.08);
            border-radius: 8px;
            position: relative;
            overflow: hidden;
        }

        #writing-pad {
            width: 100%;
            height: 100%;
            background: transparent;
            border: none;
            color: #ddd;
            padding: 20px;
            font-family: var(--font-mono);
            font-size: 1.1rem;
            line-height: 1.6;
            resize: none;
            outline: none;
        }

        /* Scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: rgba(0,0,0,0.2); }
        ::-webkit-scrollbar-thumb { background: var(--neon-blue); border-radius: 4px; }

        @media (max-width: 900px) {
            .main-layout { flex-direction: column; overflow-y: auto; }
            .sidebar { min-height: 300px; max-width: 100%; }
            .workspace { flex-direction: column; }
            .calculator-section, .writing-section { min-height: 400px; }
        }
    </style>
</head>
<body>

    <div class="background-container">
        <div class="stars"></div>
        <div class="stars2"></div>
        <div class="stars3"></div>
    </div>

    <div class="main-layout">
        
        <aside class="sidebar glass-panel">
            <div class="sidebar-header">
                <h2>Comms / Notes</h2>
                <button id="add-note-btn" class="neon-btn">+ New</button>
            </div>
            <div id="notes-container" class="notes-area">
                </div>
        </aside>

        <main class="workspace">
            
            <section class="calculator-section glass-panel">
                <div class="calculator">
                    <div class="display">
                        <div id="prev-operand" class="prev-operand"></div>
                        <div id="curr-operand" class="curr-operand">0</div>
                    </div>
                    <div class="buttons-grid">
                        <button class="span-two action-btn" data-action="clear">AC</button>
                        <button class="action-btn" data-action="delete">DEL</button>
                        <button class="operator-btn" data-op="÷">÷</button>
                        
                        <button class="num-btn" data-num="7">7</button>
                        <button class="num-btn" data-num="8">8</button>
                        <button class="num-btn" data-num="9">9</button>
                        <button class="operator-btn" data-op="×">×</button>
                        
                        <button class="num-btn" data-num="4">4</button>
                        <button class="num-btn" data-num="5">5</button>
                        <button class="num-btn" data-num="6">6</button>
                        <button class="operator-btn" data-op="-">-</button>
                        
                        <button class="num-btn" data-num="1">1</button>
                        <button class="num-btn" data-num="2">2</button>
                        <button class="num-btn" data-num="3">3</button>
                        <button class="operator-btn" data-op="+">+</button>
                        
                        <button class="num-btn" data-num=".">.</button>
                        <button class="num-btn" data-num="0">0</button>
                        <button class="span-two equal-btn" data-action="compute">=</button>
                    </div>
                </div>
            </section>

            <section class="writing-section glass-panel">
                <div class="paper-header">
                    <h2>Captain's Log</h2>
                    <span id="save-status">Saved</span>
                </div>
                <div class="paper-container">
                    <textarea id="writing-pad" placeholder="Start writing your mission log..."></textarea>
                </div>
            </section>

        </main>
    </div>

    <script>
        /* ========================
           JAVASCRIPT LOGIC
           ======================== */

        /* --- 1. CALCULATOR --- */
        class Calculator {
            constructor(prevOperandTextElement, currOperandTextElement) {
                this.prevOperandTextElement = prevOperandTextElement;
                this.currOperandTextElement = currOperandTextElement;
                this.clear();
            }

            clear() {
                this.currOperand = '';
                this.prevOperand = '';
                this.operation = undefined;
            }

            delete() {
                this.currOperand = this.currOperand.toString().slice(0, -1);
            }

            appendNumber(number) {
                if (number === '.' && this.currOperand.includes('.')) return;
                this.currOperand = this.currOperand.toString() + number.toString();
            }

            chooseOperation(operation) {
                if (this.currOperand === '') return;
                if (this.prevOperand !== '') {
                    this.compute();
                }
                this.operation = operation;
                this.prevOperand = this.currOperand;
                this.currOperand = '';
            }

            compute() {
                let computation;
                const prev = parseFloat(this.prevOperand);
                const current = parseFloat(this.currOperand);
                if (isNaN(prev) || isNaN(current)) return;
                
                switch (this.operation) {
                    case '+': computation = prev + current; break;
                    case '-': computation = prev - current; break;
                    case '×': computation = prev * current; break;
                    case '÷': computation = prev / current; break;
                    default: return;
                }
                this.currOperand = computation;
                this.operation = undefined;
                this.prevOperand = '';
            }

            updateDisplay() {
                this.currOperandTextElement.innerText = this.currOperand;
                if (this.operation != null) {
                    this.prevOperandTextElement.innerText = `${this.prevOperand} ${this.operation}`;
                } else {
                    this.prevOperandTextElement.innerText = '';
                }
            }
        }

        const prevOperandEl = document.getElementById('prev-operand');
        const currOperandEl = document.getElementById('curr-operand');
        const calculator = new Calculator(prevOperandEl, currOperandEl);

        document.querySelectorAll('.num-btn').forEach(button => {
            button.addEventListener('click', () => {
                calculator.appendNumber(button.dataset.num);
                calculator.updateDisplay();
            });
        });

        document.querySelectorAll('.operator-btn').forEach(button => {
            button.addEventListener('click', () => {
                calculator.chooseOperation(button.dataset.op);
                calculator.updateDisplay();
            });
        });

        document.querySelector('[data-action="compute"]').addEventListener('click', () => {
            calculator.compute();
            calculator.updateDisplay();
        });

        document.querySelector('[data-action="clear"]').addEventListener('click', () => {
            calculator.clear();
            calculator.updateDisplay();
        });

        document.querySelector('[data-action="delete"]').addEventListener('click', () => {
            calculator.delete();
            calculator.updateDisplay();
        });

        /* --- 2. WRITING SHEET --- */
        const writingPad = document.getElementById('writing-pad');
        const saveStatus = document.getElementById('save-status');
        const savedText = localStorage.getItem('space_writing_content');
        if (savedText) writingPad.value = savedText;

        writingPad.addEventListener('input', () => {
            localStorage.setItem('space_writing_content', writingPad.value);
            showSavedIndicator();
        });

        function showSavedIndicator() {
            saveStatus.style.opacity = '1';
            setTimeout(() => { saveStatus.style.opacity = '0'; }, 2000);
        }

        /* --- 3. STICKY NOTES --- */
        const notesContainer = document.getElementById('notes-container');
        const addNoteBtn = document.getElementById('add-note-btn');
        let notes = JSON.parse(localStorage.getItem('space_notes')) || [];

        function renderNotes() {
            notesContainer.innerHTML = '';
            notes.forEach(note => {
                createNoteElement(note.id, note.content, note.x, note.y);
            });
        }

        function createNoteElement(id, content, x, y) {
            const noteEl = document.createElement('div');
            noteEl.classList.add('sticky-note');
            noteEl.style.left = `${x}px`;
            noteEl.style.top = `${y}px`;
            noteEl.setAttribute('id', id);

            noteEl.innerHTML = `
                <div class="note-header">
                    <span>:::</span>
                    <span class="delete-note" onclick="deleteNote(${id})">×</span>
                </div>
                <textarea class="note-content" oninput="updateNoteContent(${id}, this.value)">${content}</textarea>
            `;

            // Draggable Logic
            const header = noteEl.querySelector('.note-header');
            let isDragging = false;
            let startX, startY, initialLeft, initialTop;

            header.addEventListener('mousedown', (e) => {
                isDragging = true;
                startX = e.clientX;
                startY = e.clientY;
                initialLeft = noteEl.offsetLeft;
                initialTop = noteEl.offsetTop;
                
                document.querySelectorAll('.sticky-note').forEach(n => n.style.zIndex = 1);
                noteEl.style.zIndex = 100;
            });

            window.addEventListener('mousemove', (e) => {
                if (!isDragging) return;
                const dx = e.clientX - startX;
                const dy = e.clientY - startY;
                noteEl.style.left = `${initialLeft + dx}px`;
                noteEl.style.top = `${initialTop + dy}px`;
            });

            window.addEventListener('mouseup', () => {
                if (isDragging) {
                    isDragging = false;
                    updateNotePosition(id, noteEl.style.left, noteEl.style.top);
                }
            });

            notesContainer.appendChild(noteEl);
        }

        addNoteBtn.addEventListener('click', () => {
            const newNote = { id: Date.now(), content: '', x: 20, y: 20 };
            notes.push(newNote);
            saveNotes();
            renderNotes();
        });

        window.updateNoteContent = (id, content) => {
            const note = notes.find(n => n.id === id);
            if (note) { note.content = content; saveNotes(); }
        };

        window.updateNotePosition = (id, xStr, yStr) => {
            const note = notes.find(n => n.id === id);
            if (note) { note.x = parseInt(xStr); note.y = parseInt(yStr); saveNotes(); }
        };

        window.deleteNote = (id) => {
            notes = notes.filter(n => n.id !== id);
            saveNotes();
            renderNotes();
        };

        function saveNotes() {
            localStorage.setItem('space_notes', JSON.stringify(notes));
        }

        renderNotes();
    </script>
</body>
</html>
