
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Minimal Stopwatch</title>
<style>
  body {
    display: flex;
    flex-direction: column;
    justify-content: center; /* vertically center content */
    align-items: center;    /* horizontally center content */
    height: 100vh;
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f4f4f4;
    color: #333;
  }

  #stopwatch-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  }

  #time {
    font-size: 4em;
    letter-spacing: 4px;
    margin-bottom: 20px;
  }

  .buttons {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
  }

  .buttons button {
    padding: 10px 20px;
    font-size: 1em;
    cursor: pointer;
    border: 1px solid #333;
    border-radius: 5px;
    background: #fff;
    transition: 0.2s;
  }

  .buttons button:hover {
    background: #ddd;
  }

  #laps {
    width: 80%;
    max-width: 400px;
    padding: 10px;
    margin-bottom: 20px;
  }

  #laps ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  #laps li {
    padding: 5px 0;
    border-bottom: 1px solid #ccc;
  }

  footer {
    font-size: 1em;
    margin-top: auto;
    padding: 15px;
  }

  .heart {
    color: red;
    display: inline-block;
    animation: beat 1s infinite;
  }

  @keyframes beat {
    0%, 100% { transform: scale(1); }
    25% { transform: scale(1.2); }
    50% { transform: scale(1); }
    75% { transform: scale(1.2); }
  }
</style>
</head>
<body>

<div id="stopwatch-container">
  <div id="time">00:00:00.00</div>

  <div class="buttons">
    <button onclick="start()">Start</button>
    <button onclick="stop()">Stop</button>
    <button onclick="reset()">Reset</button>
    <button onclick="lap()">Lap</button>
  </div>

  <div id="laps">
    <ul id="lapList"></ul>
  </div>
</div>

<footer>
  Made in <span class="heart">💖</span> By Armeen
</footer>

<script>
  let hours = 0, minutes = 0, seconds = 0, milliseconds = 0;
  let interval;
  let lapCounter = 0;

  function updateTime() {
    milliseconds += 10;
    if(milliseconds >= 1000) { milliseconds = 0; seconds++; }
    if(seconds >= 60) { seconds = 0; minutes++; }
    if(minutes >= 60) { minutes = 0; hours++; }

    const display =
      (hours < 10 ? "0"+hours : hours) + ":" +
      (minutes < 10 ? "0"+minutes : minutes) + ":" +
      (seconds < 10 ? "0"+seconds : seconds) + "." +
      (milliseconds/10 < 10 ? "0"+Math.floor(milliseconds/10) : Math.floor(milliseconds/10));

    document.getElementById('time').textContent = display;
  }

  function start() {
    if(!interval){
      interval = setInterval(updateTime, 10);
    }
  }

  function stop() {
    clearInterval(interval);
    interval = null;
  }

  function reset() {
    stop();
    hours = minutes = seconds = milliseconds = 0;
    lapCounter = 0;
    document.getElementById('time').textContent = "00:00:00.00";
    document.getElementById('lapList').innerHTML = '';
  }

  function lap() {
    lapCounter++;
    const time = document.getElementById('time').textContent;
    const li = document.createElement('li');
    li.textContent = `Lap ${lapCounter}: ${time}`;
    document.getElementById('lapList').appendChild(li);
  }
</script>

</body>
</html>
