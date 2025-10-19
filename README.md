

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Awaking Alarm Clock</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, sans-serif;
        }
       
        body {
            background: #bebebe;
            color: #333;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
       
        .container {
            width: 100%;
            max-width: 500px;
            background: white;
            border-radius: 16px;
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
            padding: 30px;
            position: relative;
            overflow: hidden;
        }
       
        .logo {
            text-align: center;
            margin-bottom: 20px;
        }
       
        .logo img {
            max-width: 200px;
            height: auto;
        }
       
        .clock {
            text-align: center;
            margin-bottom: 30px;
        }
       
        .time {
            font-size: 3.2rem;
            font-weight: 300;
            letter-spacing: -1px;
            margin-bottom: 5px;
            color: #444;
        }
       
        .date {
            color: #666;
            font-size: 1rem;
        }
       
        .timer-display {
            text-align: center;
            font-size: 1.5rem;
            margin: 20px 0;
            padding: 12px;
            background: #f0f0f0;
            border-radius: 10px;
            display: none;
            color: #444;
        }
       
        .alarm-image {
            max-width: 150px;
            height: auto;
            margin: 20px auto;
            display: none;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
       
        .section {
            margin-bottom: 25px;
            padding-bottom: 20px;
            border-bottom: 1px solid #e0e0e0;
        }
       
        .section:last-child {
            border-bottom: none;
            margin-bottom: 0;
        }
       
        h2 {
            font-size: 1.2rem;
            font-weight: 500;
            margin-bottom: 15px;
            color: #555;
        }
       
        .input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
        }
       
        input {
            flex: 1;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 10px;
            font-size: 1rem;
            background: #f9f9f9;
            transition: all 0.2s ease;
        }
       
        input:focus {
            outline: none;
            border-color: #a0a0a0;
            background: white;
        }
       
        button {
            padding: 12px 20px;
            border: none;
            border-radius: 10px;
            font-size: 0.95rem;
            cursor: pointer;
            transition: all 0.2s ease;
            font-weight: 500;
        }
       
        .btn-primary {
            background: #8a8a8a;
            color: white;
        }
       
        .btn-primary:hover {
            background: #7a7a7a;
            transform: translateY(-2px);
        }
       
        .btn-danger {
            background: #a85c5c;
            color: white;
        }
       
        .btn-danger:hover {
            background: #984c4c;
            transform: translateY(-2px);
        }
       
        .btn-secondary {
            background: #a0a0a0;
            color: white;
        }
       
        .btn-secondary:hover {
            background: #909090;
            transform: translateY(-2px);
        }
       
        .message {
            text-align: center;
            margin: 15px 0;
            padding: 12px;
            border-radius: 10px;
            font-size: 0.9rem;
        }
       
        .message.success {
            background: #e0f0e0;
            color: #2d5a2d;
        }
       
        .message.error {
            background: #f8e0e0;
            color: #8a3a3a;
        }
       
        .message.info {
            background: #e0e8f0;
            color: #3a5a8a;
        }
       
        .alarm-list {
            list-style: none;
        }
       
        .alarm-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #f0f0f0;
        }
       
        .alarm-item:last-child {
            border-bottom: none;
        }
       
        .alarm-time {
            font-weight: 500;
            color: #444;
        }
       
        .alarm-label {
            color: #777;
            font-size: 0.9rem;
        }
       
        .empty-state {
            text-align: center;
            color: #888;
            padding: 20px 0;
            font-style: italic;
        }
       
        footer {
            margin-top: 30px;
            text-align: center;
            color: #666;
            font-size: 0.85rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
       
        .heart {
            color: #ff4d6d;
            display: inline-block;
            animation: heartbeat 1.2s infinite;
            transform-origin: center;
        }
       
        @keyframes heartbeat {
            0% {
                transform: scale(1);
            }
            5% {
                transform: scale(1.1);
            }
            10% {
                transform: scale(1);
            }
            15% {
                transform: scale(1.2);
            }
            50% {
                transform: scale(1);
            }
            100% {
                transform: scale(1);
            }
        }
       
        .control-buttons {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }
       
        .control-buttons button {
            flex: 1;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="logo">
            <img src="https://i.ibb.co/tMVg8ZMq/Awakinga.png" alt="Awakinga" border="0">
        </div>
       
        <div class="clock">
            <div id="clock" class="time">00:00:00</div>
            <div id="date" class="date">Monday, January 1, 2023</div>
        </div>
       
        <img id="alarmImage" class="alarm-image" src="https://i.ibb.co/GvmfzF47/ARmeen.png" alt="ARmeen">
       
        <div id="timerDisplay" class="timer-display">Timer: 00:00</div>
       
        <div class="section">
            <h2>Set Alarm</h2>
            <div class="input-group">
                <input type="time" id="alarmTime">
                <button class="btn-primary" onclick="setAlarm()">Set Alarm</button>
            </div>
            <input type="text" id="alarmLabel" placeholder="Alarm label (optional)">
        </div>
       
        <div class="section">
            <h2>Set Timer</h2>
            <div class="input-group">
                <input type="number" id="timerMinutes" min="1" placeholder="Minutes">
                <button class="btn-primary" onclick="setTimer()">Start Timer</button>
            </div>
        </div>
       
        <div id="message" class="message"></div>
       
        <div class="section">
            <h2>Active Alarms</h2>
            <ul id="alarmList" class="alarm-list">
                <li class="empty-state">No active alarms</li>
            </ul>
        </div>
       
        <div class="control-buttons">
            <button id="stopButton" class="btn-danger" onclick="stopAlarm()" style="display: none;">Stop Alarm</button>
            <button id="stopTimerButton" class="btn-secondary" onclick="stopTimer()" style="display: none;">Stop Timer</button>
        </div>
    </div>
   
    <footer>
        Made in <span class="heart">💖</span> By Armeen
    </footer>

    <script>
        // DOM elements
        const clock = document.getElementById('clock');
        const dateElement = document.getElementById('date');
        const timerDisplay = document.getElementById('timerDisplay');
        const alarmImage = document.getElementById('alarmImage');
        const stopButton = document.getElementById('stopButton');
        const stopTimerButton = document.getElementById('stopTimerButton');
        const message = document.getElementById('message');
        const alarmList = document.getElementById('alarmList');
       
        // State variables
        let alarms = [];
        let timerEndTime = null;
        let timerInterval = null;
        let isAlarmRinging = false;
        let audioContext = null;
        let oscillator = null;
       
        // Update clock and date
        function updateClock() {
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const seconds = String(now.getSeconds()).padStart(2, '0');
            clock.textContent = `${hours}:${minutes}:${seconds}`;
           
            // Update date
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            dateElement.textContent = now.toLocaleDateString('en-US', options);
           
            // Check alarms
            checkAlarms(now);
           
            // Update timer if active
            updateTimer(now);
        }
       
        // Check if any alarms should trigger
        function checkAlarms(now) {
            const currentTime = `${String(now.getHours()).padStart(2, '0')}:${String(now.getMinutes()).padStart(2, '0')}`;
           
            alarms.forEach((alarm, index) => {
                if (alarm.time === currentTime && !alarm.triggered) {
                    triggerAlarm(alarm, index);
                }
            });
        }
       
        // Trigger an alarm
        function triggerAlarm(alarm, index) {
            isAlarmRinging = true;
            alarms[index].triggered = true;
           
            // Show message
            showMessage(`Alarm: ${alarm.label || 'Alarm'}`, 'success');
           
            // Show image
            alarmImage.style.display = 'block';
           
            // Play sound
            playAlarmSound();
           
            // Update UI
            stopButton.style.display = 'block';
           
            // Remove from active alarms list
            updateAlarmList();
        }
       
        // Play alarm sound using Web Audio API
        function playAlarmSound() {
            try {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
               
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
               
                oscillator.type = 'sine';
                oscillator.frequency.setValueAtTime(800, audioContext.currentTime);
                oscillator.frequency.setValueAtTime(600, audioContext.currentTime + 0.5);
               
                gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
               
                oscillator.start();
               
                // Stop after 5 seconds if not manually stopped
                setTimeout(() => {
                    if (isAlarmRinging) {
                        stopAlarm();
                    }
                }, 5000);
            } catch (error) {
                console.error('Error playing sound:', error);
                // Fallback: open in new tab
                window.open("https://limewire.com/d/Hzjae#KzNJYARNMK", '_blank');
            }
        }
       
        // Stop alarm sound
        function stopAlarmSound() {
            if (oscillator) {
                oscillator.stop();
                oscillator = null;
            }
            if (audioContext) {
                audioContext.close();
                audioContext = null;
            }
        }
       
        // Update timer display
        function updateTimer(now) {
            if (timerEndTime) {
                const timeLeft = timerEndTime - now;
               
                if (timeLeft <= 0) {
                    // Timer finished
                    timerEndTime = null;
                    timerDisplay.style.display = 'none';
                    stopTimerButton.style.display = 'none';
                    alarmImage.style.display = 'block';
                    showMessage('Timer finished!', 'success');
                    playAlarmSound();
                    clearInterval(timerInterval);
                    timerInterval = null;
                } else {
                    // Update timer display
                    const timerMinutes = Math.floor(timeLeft / 60000);
                    const timerSeconds = Math.floor((timeLeft % 60000) / 1000);
                    timerDisplay.textContent = `Timer: ${String(timerMinutes).padStart(2, '0')}:${String(timerSeconds).padStart(2, '0')}`;
                    timerDisplay.style.display = 'block';
                }
            }
        }
       
        // Set a new alarm
        function setAlarm() {
            const alarmInput = document.getElementById('alarmTime').value;
            const alarmLabel = document.getElementById('alarmLabel').value;
           
            if (!alarmInput) {
                showMessage('Please set a valid alarm time.', 'error');
                return;
            }
           
            // Add to alarms array
            alarms.push({
                time: alarmInput,
                label: alarmLabel || 'Alarm',
                triggered: false
            });
           
            showMessage(`Alarm set for ${alarmInput}`, 'info');
           
            // Reset form
            document.getElementById('alarmTime').value = '';
            document.getElementById('alarmLabel').value = '';
           
            // Update alarm list
            updateAlarmList();
        }
       
        // Stop the currently ringing alarm
        function stopAlarm() {
            isAlarmRinging = false;
            stopAlarmSound();
            stopButton.style.display = 'none';
            alarmImage.style.display = 'none';
            showMessage('Alarm stopped.', 'info');
        }
       
        // Set a timer
        function setTimer() {
            const timerInput = document.getElementById('timerMinutes').value;
           
            if (!timerInput || timerInput <= 0) {
                showMessage('Please enter a valid number of minutes.', 'error');
                return;
            }
           
            const now = new Date();
            timerEndTime = new Date(now.getTime() + timerInput * 60 * 1000);
           
            showMessage(`Timer set for ${timerInput} minute${timerInput > 1 ? 's' : ''}`, 'info');
           
            stopTimerButton.style.display = 'block';
            alarmImage.style.display = 'none';
           
            if (timerInterval) {
                clearInterval(timerInterval);
            }
           
            timerInterval = setInterval(() => {
                updateClock();
            }, 1000);
           
            updateClock();
        }
       
        // Stop the timer
        function stopTimer() {
            timerEndTime = null;
            timerDisplay.style.display = 'none';
            stopTimerButton.style.display = 'none';
            showMessage('Timer stopped.', 'info');
           
            if (timerInterval) {
                clearInterval(timerInterval);
                timerInterval = null;
            }
        }
       
        // Update the alarm list in the UI
        function updateAlarmList() {
            alarmList.innerHTML = '';
           
            const activeAlarms = alarms.filter(alarm => !alarm.triggered);
           
            if (activeAlarms.length === 0) {
                alarmList.innerHTML = '<li class="empty-state">No active alarms</li>';
                return;
            }
           
            activeAlarms.forEach((alarm, index) => {
                const alarmItem = document.createElement('li');
                alarmItem.className = 'alarm-item';
               
                alarmItem.innerHTML = `
                    <div>
                        <div class="alarm-time">${alarm.time}</div>
                        <div class="alarm-label">${alarm.label}</div>
                    </div>
                    <button class="btn-secondary" onclick="deleteAlarm(${alarms.indexOf(alarm)})">Delete</button>
                `;
               
                alarmList.appendChild(alarmItem);
            });
        }
       
        // Delete an alarm
        function deleteAlarm(index) {
            alarms.splice(index, 1);
            updateAlarmList();
            showMessage('Alarm deleted.', 'info');
        }
       
        // Show message
        function showMessage(text, type) {
            message.textContent = text;
            message.className = `message ${type}`;
           
            // Auto-hide after 5 seconds
            setTimeout(() => {
                message.textContent = '';
                message.className = 'message';
            }, 5000);
        }
       
        // Initialize
        updateClock();
        setInterval(updateClock, 1000);
        updateAlarmList();
    </script>
</body>
</html>
