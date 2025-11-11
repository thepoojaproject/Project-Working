<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Retro Radio Player</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
  /* ===== Retro Windows 98 Theme ===== */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "MS Sans Serif", "Tahoma", sans-serif;
  }

  body {
    background: #c0c0c0;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }

  .player {
    background: #d4d0c8;
    border: 2px solid #808080;
    border-top-color: #fff;
    border-left-color: #fff;
    width: 380px;
    padding: 12px;
    box-shadow: inset -1px -1px #808080, inset 1px 1px #fff;
  }

  .title-bar {
    background: linear-gradient(to right, #000080, #0000a0);
    color: white;
    font-weight: bold;
    padding: 3px 8px;
    font-size: 13px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .title-bar .controls {
    display: flex;
    gap: 4px;
  }

  .title-bar button {
    width: 16px;
    height: 16px;
    border: 1px solid #fff;
    background: #c0c0c0;
    box-shadow: inset -1px -1px #808080, inset 1px 1px #fff;
    cursor: pointer;
  }

  .station-info {
    border: 2px solid #808080;
    border-top-color: #fff;
    border-left-color: #fff;
    background: #d4d0c8;
    padding: 10px;
    margin-top: 8px;
    text-align: center;
  }

  .station-info img {
    width: 220px;
    height: auto;
    margin-bottom: 6px;
    border: 1px solid #808080;
  }

  .station-name {
    font-size: 13px;
    background: #fff;
    padding: 4px;
    border: 1px solid #808080;
    margin-bottom: 6px;
  }

  select {
    width: 100%;
    font-size: 13px;
    padding: 3px;
    border: 2px solid #808080;
    border-top-color: #fff;
    border-left-color: #fff;
    background: #fff;
    color: black;
  }

  button {
    background: #d4d0c8;
    border: 2px solid #808080;
    border-top-color: #fff;
    border-left-color: #fff;
    cursor: pointer;
    font-size: 13px;
    padding: 5px 10px;
  }

  button:active {
    border: 2px solid #fff;
    border-top-color: #808080;
    border-left-color: #808080;
  }

  .controls-panel {
    display: flex;
    justify-content: space-around;
    margin-top: 8px;
  }

  .visualizer {
    display: flex;
    justify-content: center;
    align-items: flex-end;
    height: 30px;
    margin: 8px 0;
    gap: 2px;
  }

  .bar {
    width: 4px;
    background: #000080;
    transition: height 0.15s;
  }

  .progress-container {
    height: 12px;
    background: #c0c0c0;
    border: 2px solid #808080;
    border-top-color: #fff;
    border-left-color: #fff;
    margin: 8px 0;
  }

  .progress-bar {
    height: 100%;
    width: 0%;
    background: #000080;
  }

  .volume-control {
    display: flex;
    align-items: center;
    gap: 5px;
  }

  input[type="range"] {
    flex: 1;
    -webkit-appearance: none;
    height: 6px;
    background: #fff;
    border: 1px solid #808080;
  }

  input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 10px;
    height: 16px;
    background: #c0c0c0;
    border: 2px solid #808080;
    border-top-color: #fff;
    border-left-color: #fff;
    cursor: pointer;
  }

  .status {
    background: #fff;
    border: 2px solid #808080;
    border-top-color: #fff;
    border-left-color: #fff;
    padding: 4px;
    font-size: 12px;
    margin-top: 8px;
  }

  footer {
    text-align: center;
    font-size: 12px;
    margin-top: 8px;
    color: #000;
  }
</style>
</head>
<body>

<div class="player">
  <div class="title-bar">
    <span>🎙️ Retro Radio Player</span>
    <div class="controls">
      <button title="Minimize"></button>
      <button title="Maximize"></button>
      <button title="Close"></button>
    </div>
  </div>

  <div class="station-info">
    <img src=" https://i.ibb.co/M5jBWByV/image.png " alt="Radio Logo">
    <div class="station-name" id="stationName">Loading stations...</div>
    <select id="stationSelect"></select>
  </div>

  <div class="controls-panel">
    <button id="prevBtn"><i class="fas fa-backward"></i></button>
    <button id="playBtn"><i class="fas fa-play"></i></button>
    <button id="pauseBtn" disabled><i class="fas fa-pause"></i></button>
    <button id="stopBtn" disabled><i class="fas fa-stop"></i></button>
    <button id="nextBtn"><i class="fas fa-forward"></i></button>
  </div>

  <div class="visualizer" id="visualizer"></div>

  <div class="progress-container">
    <div class="progress-bar" id="progressBar"></div>
  </div>

  <div class="volume-control">
    <span><i class="fas fa-volume-up"></i></span>
    <input type="range" id="volume" min="0" max="1" step="0.1" value="0.5">
  </div>

  <div class="status" id="status">Ready to play...</div>

  <footer>Made with ❤️ by Armeen</footer>
</div>

<script>
const audio = new Audio();
const playBtn = document.getElementById('playBtn');
const pauseBtn = document.getElementById('pauseBtn');
const stopBtn = document.getElementById('stopBtn');
const prevBtn = document.getElementById('prevBtn');
const nextBtn = document.getElementById('nextBtn');
const stationSelect = document.getElementById('stationSelect');
const stationName = document.getElementById('stationName');
const progressBar = document.getElementById('progressBar');
const visualizer = document.getElementById('visualizer');
const volumeSlider = document.getElementById('volume');
const status = document.getElementById('status');

const stations = [
  { url: "https://eu8.fastcast4u.com/proxy/clyedupq?mp=%2F1", name: "Classic Rock FM" },
  { url: "https://stream-uk1.radioparadise.com/aac-320", name: "Radio Paradise" },
  { url: "https://us1.internet-radio.com/proxy/abcd?mp=/stream", name: "Chillout Lounge" },
  { url: "https://icecast.rtl.fr/rtl-1-44-128", name: "RTL Radio" }
];

stations.forEach((s, i) => {
  const opt = document.createElement("option");
  opt.value = i;
  opt.textContent = s.name;
  stationSelect.appendChild(opt);
});

let currentStation = 0;
let visualizerInterval;

for (let i = 0; i < 25; i++) {
  const bar = document.createElement('div');
  bar.className = 'bar';
  visualizer.appendChild(bar);
}
const bars = document.querySelectorAll('.bar');

function startVisualizer() {
  stopVisualizer();
  visualizerInterval = setInterval(() => {
    bars.forEach(bar => {
      bar.style.height = Math.random() * 30 + 2 + "px";
    });
  }, 100);
}

function stopVisualizer() {
  clearInterval(visualizerInterval);
  bars.forEach(bar => bar.style.height = "3px");
}

playBtn.onclick = () => {
  audio.src = stations[stationSelect.value].url;
  audio.play().then(() => {
    status.textContent = "Playing: " + stations[stationSelect.value].name;
    playBtn.disabled = true;
    pauseBtn.disabled = false;
    stopBtn.disabled = false;
    startVisualizer();
  });
};

pauseBtn.onclick = () => {
  audio.pause();
  status.textContent = "Paused";
  playBtn.disabled = false;
  pauseBtn.disabled = true;
  stopVisualizer();
};

stopBtn.onclick = () => {
  audio.pause();
  audio.src = "";
  playBtn.disabled = false;
  pauseBtn.disabled = true;
  stopBtn.disabled = true;
  status.textContent = "Stopped";
  stopVisualizer();
};

prevBtn.onclick = () => {
  currentStation = (currentStation - 1 + stations.length) % stations.length;
  stationSelect.value = currentStation;
  playBtn.click();
};

nextBtn.onclick = () => {
  currentStation = (currentStation + 1) % stations.length;
  stationSelect.value = currentStation;
  playBtn.click();
};

volumeSlider.oninput = e => {
  audio.volume = e.target.value;
};
</script>

</body>
</html>
