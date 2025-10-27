<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Radio Player</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    :root {
      --primary: #4a00e0;
      --primary-dark: #3a00b0;
      --secondary: #8e2de2;
      --accent: #00c6ff;
      --text: #333;
      --text-light: #666;
      --background: #f8f9fa;
      --card: #ffffff;
      --shadow: 0 8px 30px rgba(0, 0, 0, 0.08);
      --shadow-hover: 0 15px 40px rgba(0, 0, 0, 0.12);
      --radius: 16px;
    }
    
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
      background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
      color: var(--text);
    }
    
    .player {
      background: var(--card);
      padding: 30px 25px;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      text-align: center;
      width: 100%;
      max-width: 400px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      position: relative;
      overflow: hidden;
    }
    
    .player:hover {
      box-shadow: var(--shadow-hover);
      transform: translateY(-5px);
    }
    
    .player::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 4px;
      background: linear-gradient(to right, var(--primary), var(--secondary), var(--accent));
    }
    
    .station-info {
      margin-bottom: 24px;
      position: relative;
    }
    
    .logo {
      max-width: 280px;
      margin: 0 auto 15px;
      display: block;
    }
    
    .station-name {
      font-size: 1.1rem;
      color: var(--text);
      margin: 12px 0;
      height: 24px;
      font-weight: 500;
    }
    
    .station-select-container {
      position: relative;
      margin: 12px 0 20px;
    }
    
    select {
      width: 100%;
      padding: 12px 16px;
      border-radius: 10px;
      border: 1px solid #e0e0e0;
      background-color: white;
      font-size: 14px;
      appearance: none;
      background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23333' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
      background-repeat: no-repeat;
      background-position: right 16px center;
      background-size: 16px;
      transition: all 0.2s ease;
      box-shadow: 0 2px 5px rgba(0,0,0,0.05);
    }
    
    select:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(74, 0, 224, 0.1);
    }
    
    .presets {
      display: flex;
      gap: 10px;
      margin: 15px 0;
      justify-content: center;
      flex-wrap: wrap;
    }
    
    .preset-btn {
      background: #f1f3f4;
      border: none;
      padding: 8px 12px;
      border-radius: 20px;
      font-size: 12px;
      cursor: pointer;
      transition: all 0.2s ease;
      color: var(--text-light);
    }
    
    .preset-btn:hover {
      background: #e8eaed;
      transform: translateY(-2px);
    }
    
    .preset-btn.active {
      background: var(--primary);
      color: white;
    }
    
    .controls {
      display: flex;
      justify-content: center;
      gap: 15px;
      margin: 25px 0;
    }
    
    button {
      background: #f1f3f4;
      color: var(--text);
      border: none;
      padding: 14px;
      border-radius: 50%;
      cursor: pointer;
      font-size: 16px;
      width: 60px;
      height: 60px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    
    button:hover {
      background: #e8eaed;
      transform: translateY(-3px);
      box-shadow: 0 6px 12px rgba(0,0,0,0.15);
    }
    
    button:active {
      transform: translateY(-1px);
    }
    
    button:disabled {
      background: #f8f9fa;
      color: #aaa;
      cursor: not-allowed;
      transform: none;
      box-shadow: 0 2px 4px rgba(0,0,0,0.05);
    }
    
    #playBtn {
      background: var(--primary);
      color: white;
      width: 70px;
      height: 70px;
    }
    
    #playBtn:hover {
      background: var(--primary-dark);
    }
    
    #playBtn:disabled {
      background: #e0e0e0;
      color: #aaa;
    }
    
    .volume-control {
      display: flex;
      align-items: center;
      gap: 12px;
      margin: 25px 0;
    }
    
    input[type="range"] {
      flex: 1;
      height: 6px;
      border-radius: 3px;
      background: #e0e0e0;
      outline: none;
      -webkit-appearance: none;
    }
    
    input[type="range"]::-webkit-slider-thumb {
      appearance: none;
      width: 18px;
      height: 18px;
      border-radius: 50%;
      background: var(--primary);
      cursor: pointer;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
      transition: all 0.2s ease;
    }
    
    input[type="range"]::-webkit-slider-thumb:hover {
      transform: scale(1.2);
    }
    
    input[type="range"]::-moz-range-thumb {
      width: 18px;
      height: 18px;
      border-radius: 50%;
      background: var(--primary);
      cursor: pointer;
      border: none;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }
    
    #volDisplay {
      min-width: 40px;
      font-size: 14px;
      color: var(--text-light);
      font-weight: 500;
    }
    
    .status {
      margin-top: 16px;
      padding: 10px 12px;
      border-radius: 10px;
      font-size: 14px;
      background-color: #f0f4ff;
      color: var(--primary);
      font-weight: 500;
      transition: all 0.3s ease;
    }
    
    .status.playing {
      background-color: #e8f5e9;
      color: #2e7d32;
    }
    
    .status.error {
      background-color: #ffebee;
      color: #c62828;
    }
    
    .progress-container {
      width: 100%;
      height: 4px;
      background-color: #e9ecef;
      border-radius: 2px;
      margin: 20px 0 10px;
      overflow: hidden;
    }
    
    .progress-bar {
      height: 100%;
      background: linear-gradient(to right, var(--primary), var(--secondary));
      width: 0%;
      transition: width 0.3s ease;
    }
    
    .frequency-display {
      font-size: 14px;
      color: var(--text-light);
      margin-top: 8px;
      font-weight: 500;
    }
    
    .visualizer {
      display: flex;
      justify-content: center;
      align-items: flex-end;
      height: 40px;
      margin: 15px 0;
      gap: 3px;
    }
    
    .bar {
      width: 4px;
      background: linear-gradient(to top, var(--primary), var(--accent));
      border-radius: 2px;
      transition: height 0.2s ease;
    }
    
    footer {
      text-align: center;
      padding: 15px;
      color: var(--text-light);
      font-size: 14px;
      margin-top: 20px;
    }
    
    .heart {
      color: #ff0000;
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
    
    /* Loading animation */
    .loading {
      display: inline-block;
      width: 20px;
      height: 20px;
      border: 3px solid rgba(255,255,255,.3);
      border-radius: 50%;
      border-top-color: #fff;
      animation: spin 1s ease-in-out infinite;
    }
    
    @keyframes spin {
      to { transform: rotate(360deg); }
    }
    
    .favorite-btn {
      position: absolute;
      top: 10px;
      right: 10px;
      background: none;
      border: none;
      font-size: 20px;
      color: #ccc;
      cursor: pointer;
      width: auto;
      height: auto;
      padding: 5px;
      box-shadow: none;
    }
    
    .favorite-btn:hover {
      transform: scale(1.2);
      background: none;
    }
    
    .favorite-btn.active {
      color: #ff4081;
    }
    
    @media (max-width: 480px) {
      .player {
        padding: 20px 15px;
      }
      
      button {
        width: 55px;
        height: 55px;
      }
      
      #playBtn {
        width: 65px;
        height: 65px;
      }
      
      .logo {
        max-width: 220px;
      }
    }
  </style>
</head>
<body>
  <div class="player">
    <div class="station-info">
      <img src="https://i.ibb.co/gMPqyWJS/image.png" alt="Radio Logo" class="logo">
      <div class="station-name" id="stationName">Loading stations...</div>
      
      <div class="station-select-container">
        <label for="stationSelect">Select Station</label>
        <select id="stationSelect">
          <!-- Dynamically populated -->
        </select>
      </div>
      
      <button class="favorite-btn" id="favoriteBtn" title="Add to favorites">
        <i class="far fa-heart"></i>
      </button>
    </div>

    <div class="presets" id="presets">
      <!-- Preset buttons will be added here -->
    </div>

    <div class="visualizer" id="visualizer">
      <!-- Audio visualizer bars -->
    </div>

    <div class="progress-container" id="progressContainer">
      <div class="progress-bar" id="progressBar"></div>
    </div>

    <div class="controls">
      <button id="prevBtn" title="Previous">
        <i class="fas fa-backward"></i>
      </button>
      <button id="stopBtn" disabled title="Stop">
        <i class="fas fa-stop"></i>
      </button>
      <button id="playBtn" title="Play">
        <i class="fas fa-play"></i>
      </button>
      <button id="pauseBtn" disabled title="Pause">
        <i class="fas fa-pause"></i>
      </button>
      <button id="nextBtn" title="Next">
        <i class="fas fa-forward"></i>
      </button>
    </div>

    <div class="volume-control">
      <span style="font-size: 14px; color: #888;">
        <i class="fas fa-volume-up"></i>
      </span>
      <input type="range" id="volume" min="0" max="1" step="0.1" value="0.5">
      <span id="volDisplay">50%</span>
    </div>

    <div class="frequency-display" id="frequencyDisplay">--.-- FM</div>

    <div class="status" id="status">Ready to play</div>
  </div>

  <footer>
    <p>Made with <span class="heart">❤</span> By Armeen</p>
  </footer>

  <script>
    const audio = new Audio();
    const playBtn = document.getElementById('playBtn');
    const pauseBtn = document.getElementById('pauseBtn');
    const stopBtn = document.getElementById('stopBtn');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const volumeSlider = document.getElementById('volume');
    const status = document.getElementById('status');
    const volDisplay = document.getElementById('volDisplay');
    const stationSelect = document.getElementById('stationSelect');
    const stationName = document.getElementById('stationName');
    const progressContainer = document.getElementById('progressContainer');
    const progressBar = document.getElementById('progressBar');
    const frequencyDisplay = document.getElementById('frequencyDisplay');
    const visualizer = document.getElementById('visualizer');
    const favoriteBtn = document.getElementById('favoriteBtn');
    const presetsContainer = document.getElementById('presets');

    // Create visualizer bars
    for (let i = 0; i < 30; i++) {
      const bar = document.createElement('div');
      bar.className = 'bar';
      bar.style.height = '5px';
      visualizer.appendChild(bar);
    }

    const bars = document.querySelectorAll('.bar');

    // Fallback hardcoded stations
    const fallbackStations = [
      { url: "https://eu8.fastcast4u.com/proxy/clyedupq?mp=%2F1", name: "Classic Rock FM", frequency: "98.5" },
      { url: "https://stream-uk1.radioparadise.com/aac-320", name: "Radio Paradise", frequency: "101.3" },
      { url: "https://us1.internet-radio.com/proxy/abcd?mp=/stream", name: "Chillout Lounge", frequency: "89.7" },
      { url: "https://icecast.rtl.fr/rtl-1-44-128", name: "RTL Radio", frequency: "102.5" },
      { url: "https://stream.radioparadise.com/rock-128", name: "Rock Paradise", frequency: "94.3" },
      { url: "https://somafm.com/dronezone130.pls", name: "Drone Zone", frequency: "87.9" }
    ];

    let stations = [];
    let currentStationIndex = 0;
    let favorites = JSON.parse(localStorage.getItem('radioFavorites')) || [];
    let visualizerInterval;

    // Load stations from API
    async function loadStations() {
      try {
        status.textContent = 'Loading stations...';
        status.className = 'status';
        
        const response = await fetch('https://api.radio-browser.info/json/stations?order=clickcount&reverse=true&limit=20');
        if (!response.ok) throw new Error('API response failed');
        const apiStations = await response.json();
        
        // Filter out invalid stations (no URL)
        const validStations = apiStations
          .filter(station => station.url_resolved || station.url)
          .map(station => ({
            url: station.url_resolved || station.url,
            name: station.name,
            frequency: station.countrycode || "--.--"
          }));

        if (validStations.length === 0) throw new Error('No valid stations found');

        stations = validStations;
        populateSelect(stations);
        updateStationInfo();
        createPresetButtons();
        status.textContent = 'Ready to play';
      } catch (error) {
        console.error('API fetch error:', error);
        status.textContent = 'Using fallback stations (API unavailable)';
        status.className = 'status error';
        stations = fallbackStations;
        populateSelect(stations);
        updateStationInfo();
        createPresetButtons();
      }
    }

    // Populate the select dropdown
    function populateSelect(stations) {
      stationSelect.innerHTML = stations.map((station, index) => 
        `<option value="${index}" data-name="${station.name}" data-frequency="${station.frequency}">
          ${station.name} (${station.frequency} ${station.frequency.length === 5 ? 'FM' : ''})
        </option>`
      ).join('');
    }

    // Create preset buttons
    function createPresetButtons() {
      presetsContainer.innerHTML = '';
      // Show first 5 stations as presets
      stations.slice(0, 5).forEach((station, index) => {
        const button = document.createElement('button');
        button.className = 'preset-btn';
        button.textContent = `${index + 1}`;
        button.title = station.name;
        button.addEventListener('click', () => {
          stationSelect.value = index;
          stationSelect.dispatchEvent(new Event('change'));
          
          // Add active class temporarily
          document.querySelectorAll('.preset-btn').forEach(btn => btn.classList.remove('active'));
          button.classList.add('active');
          setTimeout(() => button.classList.remove('active'), 1000);
        });
        presetsContainer.appendChild(button);
      });
    }

    // Set initial volume
    audio.volume = 0.5;

    // Update station name and frequency when selection changes
    function updateStationInfo() {
      const selectedOption = stationSelect.options[stationSelect.selectedIndex];
      if (selectedOption) {
        stationName.textContent = selectedOption.getAttribute('data-name');
        const freq = selectedOption.getAttribute('data-frequency');
        frequencyDisplay.textContent = freq.length === 5 ? `${freq} FM` : freq;
        
        // Update favorite button state
        const isFavorite = favorites.includes(parseInt(stationSelect.value));
        updateFavoriteButton(isFavorite);
      }
    }

    // Update favorite button appearance
    function updateFavoriteButton(isFavorite) {
      const icon = favoriteBtn.querySelector('i');
      if (isFavorite) {
        favoriteBtn.classList.add('active');
        icon.className = 'fas fa-heart';
      } else {
        favoriteBtn.classList.remove('active');
        icon.className = 'far fa-heart';
      }
    }

    // Toggle favorite status
    function toggleFavorite() {
      const stationIndex = parseInt(stationSelect.value);
      const isCurrentlyFavorite = favorites.includes(stationIndex);
      
      if (isCurrentlyFavorite) {
        // Remove from favorites
        favorites = favorites.filter(fav => fav !== stationIndex);
      } else {
        // Add to favorites
        favorites.push(stationIndex);
      }
      
      // Save to localStorage
      localStorage.setItem('radioFavorites', JSON.stringify(favorites));
      
      // Update button
      updateFavoriteButton(!isCurrentlyFavorite);
    }

    // Volume control
    volumeSlider.addEventListener('input', (e) => {
      audio.volume = e.target.value;
      volDisplay.textContent = Math.round(audio.volume * 100) + '%';
    });

    // Play button
    playBtn.addEventListener('click', () => {
      // If no source is set, use the selected station
      if (!audio.src || audio.src !== stations[stationSelect.value].url) {
        audio.src = stations[stationSelect.value].url;
      }
      
      // Show loading state
      playBtn.innerHTML = '<div class="loading"></div>';
      playBtn.disabled = true;
      
      audio.play().then(() => {
        playBtn.innerHTML = '<i class="fas fa-play"></i>';
        playBtn.disabled = true;
        pauseBtn.disabled = false;
        stopBtn.disabled = false;
        status.textContent = `Playing: ${stations[stationSelect.value].name}`;
        status.className = 'status playing';
        
        // Start visualizer animation
        startVisualizer();
      }).catch((error) => {
        playBtn.innerHTML = '<i class="fas fa-play"></i>';
        playBtn.disabled = false;
        status.textContent = 'Error: Could not play. Check stream URL.';
        status.className = 'status error';
        console.error('Playback error:', error);
      });
    });

    // Pause button
    pauseBtn.addEventListener('click', () => {
      audio.pause();
      playBtn.disabled = false;
      pauseBtn.disabled = true;
      status.textContent = 'Paused';
      status.className = 'status';
      
      // Stop visualizer
      stopVisualizer();
    });

    // Stop button
    stopBtn.addEventListener('click', () => {
      audio.pause();
      audio.currentTime = 0;
      playBtn.disabled = false;
      pauseBtn.disabled = true;
      stopBtn.disabled = true;
      status.textContent = 'Stopped';
      status.className = 'status';
      progressBar.style.width = '0%';
      
      // Stop visualizer
      stopVisualizer();
    });

    // Previous station button
    prevBtn.addEventListener('click', () => {
      currentStationIndex = parseInt(stationSelect.value);
      currentStationIndex = (currentStationIndex - 1 + stations.length) % stations.length;
      stationSelect.value = currentStationIndex;
      stationSelect.dispatchEvent(new Event('change'));
    });

    // Next station button
    nextBtn.addEventListener('click', () => {
      currentStationIndex = parseInt(stationSelect.value);
      currentStationIndex = (currentStationIndex + 1) % stations.length;
      stationSelect.value = currentStationIndex;
      stationSelect.dispatchEvent(new Event('change'));
    });

    // Station change
    stationSelect.addEventListener('change', (e) => {
      currentStationIndex = parseInt(e.target.value);
      audio.src = stations[currentStationIndex].url;
      updateStationInfo();
      
      // If already playing, automatically switch to new station
      if (!pauseBtn.disabled) {
        playBtn.innerHTML = '<div class="loading"></div>';
        playBtn.disabled = true;
        
        audio.play().then(() => {
          playBtn.innerHTML = '<i class="fas fa-play"></i>';
          status.textContent = `Playing: ${stations[currentStationIndex].name}`;
          status.className = 'status playing';
        }).catch((error) => {
          playBtn.innerHTML = '<i class="fas fa-play"></i>';
          playBtn.disabled = false;
          status.textContent = 'Error: Could not play station.';
          status.className = 'status error';
          console.error('Station error:', error);
        });
      } else {
        status.textContent = 'Station changed';
        status.className = 'status';
      }
    });

    // Favorite button
    favoriteBtn.addEventListener('click', toggleFavorite);

    // Update status and progress
    audio.addEventListener('loadstart', () => {
      status.textContent = 'Loading...';
      status.className = 'status';
      progressBar.style.width = '0%';
    });
    
    audio.addEventListener('progress', () => {
      if (audio.buffered.length > 0 && audio.duration) {
        const bufferedPercent = (audio.buffered.end(0) / audio.duration) * 100;
        progressBar.style.width = bufferedPercent + '%';
      }
    });
    
    audio.addEventListener('canplay', () => {
      status.textContent = 'Ready to play';
      status.className = 'status';
    });
    
    audio.addEventListener('waiting', () => {
      status.textContent = 'Buffering...';
      status.className = 'status';
    });
    
    audio.addEventListener('playing', () => {
      status.textContent = `Playing: ${stations[currentStationIndex].name}`;
      status.className = 'status playing';
    });
    
    audio.addEventListener('error', (e) => {
      status.textContent = 'Stream error. Try another station.';
      status.className = 'status error';
      console.error('Audio error:', e);
    });
    
    audio.addEventListener('ended', () => {
      status.textContent = 'Playback ended';
      status.className = 'status';
      playBtn.disabled = false;
      pauseBtn.disabled = true;
      stopBtn.disabled = true;
      stopVisualizer();
    });

    // Visualizer functions
    function startVisualizer() {
      stopVisualizer();
      visualizerInterval = setInterval(() => {
        bars.forEach(bar => {
          const randomHeight = Math.floor(Math.random() * 30) + 5;
          bar.style.height = `${randomHeight}px`;
          bar.style.opacity = Math.random() > 0.1 ? '1' : '0.7';
        });
      }, 150);
    }

    function stopVisualizer() {
      if (visualizerInterval) {
        clearInterval(visualizerInterval);
        visualizerInterval = null;
      }
      bars.forEach(bar => {
        bar.style.height = '5px';
        bar.style.opacity = '0.3';
      });
    }

    // Initialize on load
    window.addEventListener('load', loadStations);

    // Add keyboard shortcuts
    document.addEventListener('keydown', (e) => {
      if (e.code === 'Space') {
        e.preventDefault();
        if (pauseBtn.disabled) {
          playBtn.click();
        } else {
          pauseBtn.click();
        }
      } else if (e.code === 'ArrowLeft') {
        prevBtn.click();
      } else if (e.code === 'ArrowRight') {
        nextBtn.click();
      }
    });
  </script>
</body>
</html>
