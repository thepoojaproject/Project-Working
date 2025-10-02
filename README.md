
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pixel Dash Adventure</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/feather-icons"></script>
    <style>
        canvas {
            image-rendering: pixelated;
            border: 6px solid #1e293b;
            background: #0f172a;
            display: block;
        }
        .game-container {
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
        }
        .pixel-font {
            font-family: 'Courier New', monospace;
            letter-spacing: -0.5px;
        }
        .glow-text {
            text-shadow: 0 0 8px rgba(255, 255, 255, 0.7);
        }
        .btn-pixel {
            border-radius: 4px;
            border-bottom: 3px solid rgba(0, 0, 0, 0.2);
            transition: all 0.1s ease;
        }
        .btn-pixel:active {
            transform: translateY(2px);
            border-bottom-width: 1px;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col items-center justify-center p-4">
    <div class="text-center mb-6">
        <h1 class="text-4xl md:text-5xl font-bold mb-2 pixel-font glow-text">PIXEL DASH</h1>
        <p class="text-slate-400">Collect coins, avoid enemies!</p>
    </div>

    <div class="game-container relative mb-6">
        <canvas id="game" width="256" height="256" class="mx-auto"></canvas>
        <div id="gameOver" class="hidden absolute inset-0 bg-black bg-opacity-70 flex flex-col items-center justify-center">
            <h2 class="text-3xl font-bold mb-4 text-red-500 pixel-font">GAME OVER</h2>
            <button id="restartBtn" class="btn-pixel bg-indigo-600 hover:bg-indigo-500 text-white px-6 py-2 font-bold">
                PLAY AGAIN
            </button>
        </div>
    </div>

    <div class="bg-slate-800 rounded-lg p-4 w-full max-w-md">
        <div class="flex justify-between items-center mb-4">
            <div class="text-center">
                <p class="text-sm text-slate-400">SCORE</p>
                <p id="score" class="text-2xl font-bold">0</p>
            </div>
            <div class="text-center">
                <p class="text-sm text-slate-400">LIVES</p>
                <p id="lives" class="text-2xl font-bold text-red-500">3</p>
            </div>
            <div class="text-center">
                <p class="text-sm text-slate-400">LEVEL</p>
                <p id="level" class="text-2xl font-bold text-green-500">1</p>
            </div>
        </div>

        <div class="flex justify-center gap-4">
            <button id="pauseBtn" class="btn-pixel bg-slate-700 hover:bg-slate-600 px-4 py-2 flex items-center gap-2">
                <i data-feather="pause" class="w-4 h-4"></i>
                PAUSE
            </button>
            <button id="helpBtn" class="btn-pixel bg-slate-700 hover:bg-slate-600 px-4 py-2 flex items-center gap-2">
                <i data-feather="help-circle" class="w-4 h-4"></i>
                HELP
            </button>
        </div>
    </div>

    <div class="mt-8 text-sm text-slate-500 text-center">
        <p>Use arrow keys or WASD to move</p>
        <p class="mt-1">Press SPACE to pause</p>
    </div>

    <div class="fixed bottom-4 right-4 text-xs text-slate-500">
        made with <i data-feather="heart" class="w-3 h-3 inline text-red-400"></i> by Pixel Team
    </div>

    <script>
        feather.replace();
        
        // Game configuration
        const TILE = 16;
        const TILES = 16;
        const CANVAS_SIZE = TILE * TILES;
        const PLAYER_COLOR = '#ffffff';
        const COIN_COLOR = '#fbbf24';
        const ENEMY_COLOR = '#ef4444';
        const BG_COLOR = '#0f172a';

        // Canvas setup
        const canvas = document.getElementById('game');
        const ctx = canvas.getContext('2d');
        ctx.imageSmoothingEnabled = false;

        // Game state
        let score = 0;
        let lives = 3;
        let level = 1;
        let running = true;
        let paused = false;
        let lastFrameTime = 0;

        // DOM elements
        const scoreEl = document.getElementById('score');
        const livesEl = document.getElementById('lives');
        const levelEl = document.getElementById('level');
        const pauseBtn = document.getElementById('pauseBtn');
        const restartBtn = document.getElementById('restartBtn');
        const helpBtn = document.getElementById('helpBtn');
        const gameOverEl = document.getElementById('gameOver');

        // Player object
        const player = {
            x: Math.floor(TILES/2),
            y: Math.floor(TILES/2),
            speed: 6,
            size: TILE-2,
            px: 0, py: 0,
            targetX: null, targetY: null
        };

        // Game objects
        let coin = null;
        let enemies = [];

        // Input handling
        const keys = {};
        window.addEventListener('keydown', e => {
            keys[e.key.toLowerCase()] = true;
            if(e.key === ' ') {
                togglePause();
                e.preventDefault();
            }
        });
        window.addEventListener('keyup', e => {
            keys[e.key.toLowerCase()] = false;
        });

        // Utility functions
        function randTile() { return Math.floor(Math.random() * TILES); }

        function placeCoin() {
            let tries = 0;
            while(tries < 200) {
                const x = randTile(), y = randTile();
                if ((x === player.x && y === player.y) || 
                    enemies.some(en => Math.round(en.x) === x && Math.round(en.y) === y)) {
                    tries++;
                    continue;
                }
                coin = {x, y};
                return;
            }
            coin = {x: 0, y: 0};
        }

        function addEnemy() {
            const side = Math.random() < 0.5 ? 'h' : 'v';
            let ex, ey;
            if(side === 'h') {
                ex = Math.random() < 0.5 ? 0 : TILES-1;
                ey = randTile();
            } else {
                ey = Math.random() < 0.5 ? 0 : TILES-1;
                ex = randTile();
            }
            if (ex === player.x && ey === player.y) return;
            enemies.push({
                x: ex,
                y: ey,
                spd: 0.6 + Math.random() * 0.6,
                color: `hsl(${Math.random() * 60 + 330}, 80%, 60%)`
            });
        }

        // Game initialization
        function resetGame() {
            score = 0;
            lives = 3;
            level = 1;
            running = true;
            paused = false;
            player.x = Math.floor(TILES/2);
            player.y = Math.floor(TILES/2);
            player.px = player.x * TILE;
            player.py = player.y * TILE;
            enemies = [];
            placeCoin();
            addEnemy();
            updateUI();
            gameOverEl.classList.add('hidden');
        }

        // UI updates
        function updateUI() {
            scoreEl.textContent = score;
            livesEl.textContent = lives;
            levelEl.textContent = level;
        }

        // Player movement
        let moveCooldown = 0;
        function handleInput() {
            moveCooldown -= 1/60;
            if(moveCooldown > 0) return;
            
            const left = keys['arrowleft'] || keys['a'];
            const right = keys['arrowright'] || keys['d'];
            const up = keys['arrowup'] || keys['w'];
            const down = keys['arrowdown'] || keys['s'];
            
            if(left) { tryMovePlayer(-1, 0); moveCooldown = 0.12; }
            else if(right) { tryMovePlayer(1, 0); moveCooldown = 0.12; }
            else if(up) { tryMovePlayer(0, -1); moveCooldown = 0.12; }
            else if(down) { tryMovePlayer(0, 1); moveCooldown = 0.12; }
        }

        function tryMovePlayer(dx, dy) {
            const nx = Math.floor(player.x) + dx;
            const ny = Math.floor(player.y) + dy;
            if (nx < 0 || ny < 0 || nx >= TILES || ny >= TILES) return;
            player.targetX = nx;
            player.targetY = ny;
        }

        // Game logic
        function updateEntities(dt) {
            // Player movement interpolation
            if(player.targetX !== null) {
                const tx = player.targetX * TILE;
                const ty = player.targetY * TILE;
                const dx = tx - player.px;
                const dy = ty - player.py;
                const dist = Math.hypot(dx, dy);
                
                if(dist < 1) {
                    player.px = tx;
                    player.py = ty;
                    player.x = player.targetX;
                    player.y = player.targetY;
                    player.targetX = player.targetY = null;
                } else {
                    const step = Math.min(dist, 14 * dt * 60);
                    player.px += dx * (step/dist);
                    player.py += dy * (step/dist);
                }
            } else {
                player.px = player.x * TILE;
                player.py = player.y * TILE;
            }

            // Enemy movement
            for(const en of enemies) {
                const dx = player.x - en.x;
                const dy = player.y - en.y;
                
                if(Math.abs(dx) > Math.abs(dy)) {
                    en.x += Math.sign(dx) * Math.min(1, en.spd * dt * 2);
                } else {
                    en.y += Math.sign(dy) * Math.min(1, en.spd * dt * 2);
                }
                
                en.x = Math.max(0, Math.min(TILES-1, en.x));
                en.y = Math.max(0, Math.min(TILES-1, en.y));
            }
        }

        function checkCollisions() {
            // Coin collection
            if(coin && Math.round(player.x) === coin.x && Math.round(player.y) === coin.y) {
                score += 10;
                placeCoin();
                
                // Level progression
                if(score % 30 === 0) {
                    addEnemy();
                    level = Math.min(99, 1 + Math.floor(score/30));
                    // Visual effect for level up
                    canvas.style.borderColor = '#10b981';
                    setTimeout(() => {
                        canvas.style.borderColor = '#1e293b';
                    }, 300);
                }
                
                updateUI();
            }

            // Enemy collisions
            for(const en of enemies) {
                if(Math.hypot(en.x - player.x, en.y - player.y) < 0.6) {
                    lives--;
                    updateUI();
                    
                    // Visual feedback for hit
                    canvas.style.borderColor = '#ef4444';
                    setTimeout(() => {
                        canvas.style.borderColor = '#1e293b';
                    }, 200);
                    
                    // Reset player position
                    player.x = Math.floor(TILES/2);
                    player.y = Math.floor(TILES/2);
                    player.px = player.x * TILE;
                    player.py = player.y * TILE;
                    player.targetX = player.targetY = null;
                    
                    // Push enemy away
                    en.x = Math.max(0, Math.min(TILES-1, en.x + (en.x < player.x ? -1 : 1)));
                    en.y = Math.max(0, Math.min(TILES-1, en.y + (en.y < player.y ? -1 : 1)));
                    
                    if(lives <= 0) {
                        running = false;
                        setTimeout(showGameOver, 200);
                    }
                    break;
                }
            }
        }

        function showGameOver() {
            gameOverEl.classList.remove('hidden');
        }

        // Rendering
        function render() {
            // Clear canvas
            ctx.fillStyle = BG_COLOR;
            ctx.fillRect(0, 0, CANVAS_SIZE, CANVAS_SIZE);
            
            // Grid background
            ctx.fillStyle = '#1e293b';
            for(let x = 0; x < TILES; x++) {
                for(let y = 0; y < TILES; y++) {
                    if((x + y) % 2 === 0) {
                        ctx.fillRect(x * TILE, y * TILE, TILE, TILE);
                    }
                }
            }
            
            // Coin
            if(coin) {
                ctx.fillStyle = COIN_COLOR;
                const cx = coin.x * TILE + 3;
                const cy = coin.y * TILE + 3;
                ctx.fillRect(cx, cy, TILE-6, TILE-6);
                
                // Coin shine effect
                ctx.fillStyle = 'rgba(255, 255, 255, 0.3)';
                ctx.fillRect(cx + 2, cy + 2, 4, 2);
            }
            
            // Enemies
            for(const en of enemies) {
                ctx.fillStyle = en.color || ENEMY_COLOR;
                const ex = Math.round(en.x * TILE + 1);
                const ey = Math.round(en.y * TILE + 1);
                ctx.fillRect(ex, ey, TILE-2, TILE-2);
                
                // Enemy eyes
                ctx.fillStyle = '#ffffff';
                const eyeSize = 3;
                const eyeOffset = 3;
                ctx.fillRect(ex + eyeOffset, ey + eyeOffset, eyeSize, eyeSize);
                ctx.fillRect(ex + TILE - eyeOffset - eyeSize, ey + eyeOffset, eyeSize, eyeSize);
            }
            
            // Player
            ctx.fillStyle = PLAYER_COLOR;
            ctx.fillRect(
                Math.round(player.px) + 1, 
                Math.round(player.py) + 1, 
                player.size, 
                player.size
            );
            
            // Player eyes (facing direction)
            const eyeSize = 3;
            const eyeY = Math.round(player.py) + 5;
            if(player.targetX !== null) {
                // Moving - eyes face direction
                const dx = player.targetX - player.x;
                const dy = player.targetY - player.y;
                
                if(Math.abs(dx) > Math.abs(dy)) {
                    // Horizontal movement
                    const eyeX = dx > 0 ? 
                        Math.round(player.px) + player.size - 4 : 
                        Math.round(player.px) + 2;
                    ctx.fillStyle = '#000000';
                    ctx.fillRect(eyeX, eyeY, eyeSize, eyeSize);
                    ctx.fillRect(eyeX, eyeY + 6, eyeSize, eyeSize);
                } else {
                    // Vertical movement
                    const eyeX = Math.round(player.px) + 5;
                    const eyeYPos = dy > 0 ? 
                        Math.round(player.py) + player.size - 4 : 
                        Math.round(player.py) + 2;
                    ctx.fillStyle = '#000000';
                    ctx.fillRect(eyeX, eyeYPos, eyeSize, eyeSize);
                    ctx.fillRect(eyeX + 6, eyeYPos, eyeSize, eyeSize);
                }
            } else {
                // Stationary - centered eyes
                ctx.fillStyle = '#000000';
                ctx.fillRect(
                    Math.round(player.px) + 5, 
                    eyeY, 
                    eyeSize, 
                    eyeSize
                );
                ctx.fillRect(
                    Math.round(player.px) + 11, 
                    eyeY, 
                    eyeSize, 
                    eyeSize
                );
            }
        }

        // Game loop
        function gameLoop(timestamp) {
            if(!running) return;
            
            if(!lastFrameTime) lastFrameTime = timestamp;
            const deltaTime = (timestamp - lastFrameTime) / 1000;
            lastFrameTime = timestamp;
            
            if(!paused) {
                handleInput();
                updateEntities(deltaTime);
                checkCollisions();
            }
            
            render();
            requestAnimationFrame(gameLoop);
        }

        // Controls
        function togglePause() {
            if(!running) return;
            paused = !paused;
            pauseBtn.innerHTML = paused ? 
                '<i data-feather="play" class="w-4 h-4"></i> RESUME' : 
                '<i data-feather="pause" class="w-4 h-4"></i> PAUSE';
            feather.replace();
        }

        pauseBtn.addEventListener('click', togglePause);
        restartBtn.addEventListener('click', () => {
            resetGame();
            lastFrameTime = 0;
            if(!running) {
                running = true;
            }
            requestAnimationFrame(gameLoop);
        });

        helpBtn.addEventListener('click', () => {
            alert('PIXEL DASH ADVENTURE\n\nMove with arrow keys or WASD\nCollect coins to score points\nAvoid enemies - they chase you!\nEvery 30 points increases level\nLose all lives and game over!');
        });

        // Keyboard shortcuts
        window.addEventListener('keydown', e => {
            if(e.key.toLowerCase() === 'r') {
                resetGame();
                lastFrameTime = 0;
                if(!running) {
                    running = true;
                }
                requestAnimationFrame(gameLoop);
            }
        });

        // Initialize game
        resetGame();
        requestAnimationFrame(gameLoop);
    </script>
</body>
</html>
