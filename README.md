<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>minim - Focus & Task Suite</title>
    <style>
        :root {
            --bg-main: #0a0d14;
            --bg-card: #131722;
            --bg-hover: #1c2230;
            --accent: #6366f1;
            --accent-glow: rgba(99, 102, 241, 0.4);
            --accent-secondary: #06b6d4;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --border: #222a3d;
            --success: #10b981;
            --danger: #f43f5e;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-main);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            background-image: 
                radial-gradient(circle at 10% 20%, rgba(99, 102, 241, 0.08) 0%, transparent 40%),
                radial-gradient(circle at 90% 80%, rgba(6, 182, 212, 0.06) 0%, transparent 40%);
        }

        /* Top Navigation Bar */
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 16px 32px;
            border-bottom: 1px solid var(--border);
            background: rgba(19, 23, 34, 0.8);
            backdrop-filter: blur(12px);
            z-index: 10;
        }

        .logo {
            font-size: 1.5rem;
            font-weight: 800;
            letter-spacing: 2px;
            color: var(--accent);
            text-transform: lowercase;
            text-shadow: 0 0 20px var(--accent-glow);
        }

        .nav-tabs {
            display: flex;
            gap: 12px;
        }

        .tab-btn {
            background: transparent;
            border: none;
            color: var(--text-muted);
            padding: 8px 18px;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .tab-btn:hover {
            color: var(--text-main);
            background: var(--bg-hover);
        }

        .tab-btn.active {
            background: var(--accent);
            color: white;
            box-shadow: 0 0 20px var(--accent-glow);
        }

        /* Main App View Container */
        .app-container {
            flex: 1;
            display: flex;
            position: relative;
            overflow: hidden;
        }

        .view {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.4s ease, transform 0.4s ease;
            transform: scale(0.98);
        }

        .view.active {
            opacity: 1;
            pointer-events: auto;
            transform: scale(1);
        }

        /* Home View Styles */
        .home-view {
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 40px;
            gap: 24px;
        }

        .home-view h1 {
            font-size: 3.5rem;
            font-weight: 900;
            letter-spacing: -1px;
            background: linear-gradient(135deg, #fff 30%, var(--text-muted));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: floatText 4s ease-in-out infinite alternate;
        }

        @keyframes floatText {
            0% { transform: translateY(0); }
            100% { transform: translateY(-8px); }
        }

        .home-view p {
            color: var(--text-muted);
            max-width: 520px;
            font-size: 1.15rem;
            line-height: 1.6;
        }

        .start-btn {
            font-size: 1.15rem;
            padding: 16px 40px;
            border-radius: 14px;
            background: linear-gradient(135deg, var(--accent), var(--accent-secondary));
            color: white;
            font-weight: 700;
            cursor: pointer;
            border: none;
            box-shadow: 0 0 30px var(--accent-glow);
            transition: transform 0.2s ease, box-shadow 0.3s ease;
        }

        .start-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 0 40px var(--accent-glow);
        }

        /* Split Screen (Focus + Organize) View */
        .split-view {
            display: grid;
            grid-template-columns: 1fr 1fr;
            width: 100%;
            height: 100%;
            padding: 24px;
            gap: 24px;
        }

        .panel {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 24px;
            display: flex;
            flex-direction: column;
            overflow-y: auto;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            backdrop-filter: blur(10px);
        }

        .panel h2 {
            font-size: 1.25rem;
            margin-bottom: 16px;
            color: var(--text-main);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        /* Todo List Styles */
        .task-input-group {
            display: flex;
            gap: 12px;
            margin-bottom: 16px;
        }

        .task-input {
            flex: 1;
            background: var(--bg-main);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 14px;
            color: var(--text-main);
            font-size: 1rem;
            transition: border-color 0.2s ease, box-shadow 0.2s ease;
        }

        .task-input:focus {
            outline: none;
            border-color: var(--accent);
            box-shadow: 0 0 10px var(--accent-glow);
        }

        .btn {
            background: var(--accent);
            color: white;
            border: none;
            padding: 0 24px;
            border-radius: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .btn:hover {
            box-shadow: 0 0 15px var(--accent-glow);
            filter: brightness(1.1);
        }

        .btn:active {
            transform: scale(0.96);
        }

        .task-list {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 10px;
            overflow-y: auto;
            flex: 1;
        }

        .task-item {
            background: var(--bg-main);
            border: 1px solid var(--border);
            padding: 14px 18px;
            border-radius: 14px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            animation: slideIn 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .task-item:hover {
            border-color: var(--accent);
            transform: translateX(4px);
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(12px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .task-item.completed {
            opacity: 0.6;
            background: rgba(16, 185, 129, 0.03);
            border-color: rgba(16, 185, 129, 0.2);
        }

        .task-item.completed span {
            text-decoration: line-through;
            color: var(--text-muted);
        }

        .task-left {
            display: flex;
            align-items: center;
            gap: 14px;
            cursor: pointer;
            flex: 1;
        }

        .task-checkbox {
            width: 20px;
            height: 20px;
            accent-color: var(--success);
            cursor: pointer;
        }

        .task-delete {
            background: transparent;
            border: none;
            color: var(--text-muted);
            cursor: pointer;
            font-size: 1.1rem;
            padding: 4px 8px;
            border-radius: 6px;
            transition: all 0.2s ease;
        }

        .task-delete:hover {
            color: var(--danger);
            background: rgba(244, 63, 94, 0.1);
        }

        /* Timer Panel Styles */
        .timer-display {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 24px;
        }

        .time-ring {
            font-size: 6rem;
            font-weight: 900;
            letter-spacing: -3px;
            color: var(--text-main);
            text-shadow: 0 0 40px var(--accent-glow);
            font-variant-numeric: tabular-nums;
        }

        .timer-controls {
            display: flex;
            gap: 14px;
        }

        .sound-selector {
            margin-top: 10px;
            display: flex;
            flex-direction: column;
            gap: 8px;
            width: 100%;
            max-width: 280px;
        }

        .sound-selector select {
            background: var(--bg-main);
            border: 1px solid var(--border);
            color: var(--text-main);
            padding: 12px;
            border-radius: 10px;
            font-size: 0.9rem;
            cursor: pointer;
            outline: none;
            transition: border-color 0.2s ease;
        }

        .sound-selector select:focus {
            border-color: var(--accent);
        }

        /* ADHD Dopamine Streak Counter */
        .streak-banner {
            font-size: 0.85rem;
            color: var(--success);
            background: rgba(16, 185, 129, 0.1);
            border: 1px solid rgba(16, 185, 129, 0.2);
            padding: 6px 14px;
            border-radius: 20px;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            margin-bottom: 12px;
        }
    </style>
</head>
<body>

    <header>
        <div class="logo">minim</div>
        <div class="nav-tabs">
            <button class="tab-btn active" onclick="switchView('home')">Home</button>
            <button class="tab-btn" onclick="switchView('split')">Focus & Organize</button>
        </div>
    </header>

    <div class="app-container">
        <!-- Home View -->
        <div id="home-view" class="view home-view active">
            <div class="streak-banner" id="streakBadge">🔥 0 Tasks Destroyed Today</div>
            <h1>Lock In. Zero Friction.</h1>
            <p>Your ultra-clean, high-dopamine productivity sanctuary built for deep focus sessions on laptop and iPad.</p>
            <button class="start-btn" onclick="switchView('split')">Enter Workspace</button>
        </div>

        <!-- Split Screen View -->
        <div id="split-view" class="view">
            <div class="split-view">
                <!-- To-Do Panel -->
                <div class="panel">
                    <h2>To-Do Tasks</h2>
                    <div class="task-input-group">
                        <input type="text" id="taskInput" class="task-input" placeholder="What needs to be done? (Press Enter)">
                        <button class="btn" onclick="addTask()">Add</button>
                    </div>
                    <ul id="taskList" class="task-list"></ul>
                </div>

                <!-- Timer Panel -->
                <div class="panel">
                    <h2>Focus Timer</h2>
                    <div class="timer-display">
                        <div class="time-ring" id="timeDisplay">25:00</div>
                        <div class="timer-controls">
                            <button class="btn" onclick="toggleTimer()">Start / Pause</button>
                            <button class="btn" style="background: var(--bg-hover); color: var(--text-main);" onclick="resetTimer()">Reset</button>
                        </div>
                        <div class="sound-selector">
                            <label style="font-size: 0.85rem; color: var(--text-muted);">Custom Completion Sound</label>
                            <select id="soundChoice">
                                <option value="chime">Digital Chime</option>
                                <option value="bell">Zen Bell</option>
                                <option value="pop">Pop Bubble</option>
                                <option value="laser">Retro Laser</option>
                                <option value="success">Success Chord</option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // View Switcher logic
        function switchView(target) {
            document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
            
            if(target === 'home') {
                document.getElementById('home-view').classList.add('active');
                document.querySelectorAll('.tab-btn')[0].classList.add('active');
            } else {
                document.getElementById('split-view').classList.add('active');
                document.querySelectorAll('.tab-btn')[1].classList.add('active');
            }
        }

        // To-Do List & Dopamine Streak Logic
        let tasks = JSON.parse(localStorage.getItem('minim_tasks')) || [];
        let completedToday = parseInt(localStorage.getItem('minim_streak')) || 0;

        function updateStreakDisplay() {
            document.getElementById('streakBadge').innerText = `🔥 ${completedToday} Tasks Destroyed Today`;
            localStorage.setItem('minim_streak', completedToday);
        }

        function renderTasks() {
            const list = document.getElementById('taskList');
            list.innerHTML = '';
            tasks.forEach((task, index) => {
                const li = document.createElement('li');
                li.className = `task-item ${task.completed ? 'completed' : ''}`;
                li.innerHTML = `
                    <div class="task-left" onclick="toggleTask(${index})">
                        <input type="checkbox" class="task-checkbox" ${task.completed ? 'checked' : ''} onclick="event.stopPropagation(); toggleTask(${index})">
                        <span>${escapeHtml(task.text)}</span>
                    </div>
                    <button class="task-delete" onclick="deleteTask(event, ${index})">✕</button>
                `;
                list.appendChild(li);
            });
            localStorage.setItem('minim_tasks', JSON.stringify(tasks));
            updateStreakDisplay();
        }

        function addTask() {
            const input = document.getElementById('taskInput');
            const text = input.value.trim();
            if(!text) return;
            tasks.push({ text, completed: false });
            input.value = '';
            renderTasks();
        }

        function toggleTask(index) {
            tasks[index].completed = !tasks[index].completed;
            if(tasks[index].completed) {
                completedToday++;
                playSound();
            } else {
                if(completedToday > 0) completedToday--;
            }
            renderTasks();
        }

        function deleteTask(event, index) {
            event.stopPropagation();
            tasks.splice(index, 1);
            renderTasks();
        }

        document.getElementById('taskInput').addEventListener('keypress', (e) => {
            if(e.key === 'Enter') addTask();
        });

        function escapeHtml(str) {
            return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
        }

        // Timer Logic
        let timeLeft = 1500;
        let timerId = null;
        let isRunning = false;

        function updateTimerDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            document.getElementById('timeDisplay').innerText = 
                `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        function toggleTimer() {
            if(isRunning) {
                clearInterval(timerId);
                isRunning = false;
            } else {
                isRunning = true;
                timerId = setInterval(() => {
                    if(timeLeft > 0) {
                        timeLeft--;
                        updateTimerDisplay();
                    } else {
                        clearInterval(timerId);
                        isRunning = false;
                        playSound();
                        alert("Session complete! Great job locking in.");
                    }
                }, 1000);
            }
        }

        function resetTimer() {
            clearInterval(timerId);
            isRunning = false;
            timeLeft = 1500;
            updateTimerDisplay();
        }

        // Audio Feedback Synthesizer (5 Custom Sound Effects)
        function playSound() {
            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = audioCtx.createOscillator();
                const gainNode = audioCtx.createGain();
                osc.connect(gainNode);
                gainNode.connect(audioCtx.destination);
                
                const type = document.getElementById('soundChoice').value;
                if(type === 'chime') {
                    osc.frequency.setValueAtTime(587.33, audioCtx.currentTime); // D5
                    gainNode.gain.setValueAtTime(0.1, audioCtx.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 1.2);
                    osc.start();
                    osc.stop(audioCtx.currentTime + 1.2);
                } else if(type === 'bell') {
                    osc.frequency.setValueAtTime(440, audioCtx.currentTime); // A4
                    gainNode.gain.setValueAtTime(0.15, audioCtx.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 2.0);
                    osc.start();
                    osc.stop(audioCtx.currentTime + 2.0);
                } else if(type === 'pop') {
                    osc.frequency.setValueAtTime(800, audioCtx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(200, audioCtx.currentTime + 0.1);
                    gainNode.gain.setValueAtTime(0.1, audioCtx.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.1);
                    osc.start();
                    osc.stop(audioCtx.currentTime + 0.1);
                } else if(type === 'laser') {
                    osc.frequency.setValueAtTime(900, audioCtx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(100, audioCtx.currentTime + 0.3);
                    gainNode.gain.setValueAtTime(0.1, audioCtx.currentTime);
                    gainNode.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.3);
                    osc.start();
                    osc.stop(audioCtx.currentTime + 0.3);
                } else {
                    // Success Chord
                    [523.25, 659.25, 783.99].forEach((freq, i) => {
                        const o = audioCtx.createOscillator();
                        const g = audioCtx.createGain();
                        o.connect(g);
                        g.connect(audioCtx.destination);
                        o.frequency.setValueAtTime(freq, audioCtx.currentTime + (i * 0.1));
                        g.gain.setValueAtTime(0.08, audioCtx.currentTime + (i * 0.1));
                        g.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.8);
                        o.start(audioCtx.currentTime + (i * 0.1));
                        o.stop(audioCtx.currentTime + 0.8);
                    });
                }
            } catch(e) {
                console.log("Audio context not initialized yet.");
            }
        }

        renderTasks();
        updateTimerDisplay();
    </script>
</body>
</html>
