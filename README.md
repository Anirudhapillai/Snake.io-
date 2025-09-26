<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grok's Arcade Minigames Galaxy - Powered by xAI</title>
    <style>
        body {
            font-family: 'Courier New', monospace;
            background-color: #000000;
            color: #00ff00;
            margin: 0;
            padding: 20px;
            text-align: center;
            image-rendering: pixelated; /* For that arcade crispness */
        }
        h1 {
            font-size: 3em;
            text-shadow: 0 0 10px #ff00ff, 0 0 20px #00ffff; /* Neon arcade glow */
            color: #ffff00; /* Yellow for retro vibe */
        }
        .intro {
            font-size: 1.2em;
            margin-bottom: 30px;
            max-width: 800px;
            margin: 0 auto 30px;
            color: #ff69b4; /* Hot pink for fun */
        }
        .game-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }
        .game-card {
            background-color: #1a1a1a;
            border: 4px solid #ffff00;
            border-radius: 0; /* Sharp edges for arcade cabinets */
            padding: 15px;
            text-align: center;
            transition: transform 0.3s, box-shadow 0.3s;
            box-shadow: 0 0 10px #ff00ff;
        }
        .game-card:hover {
            transform: scale(1.05);
            box-shadow: 0 0 20px #00ffff, 0 0 30px #ff00ff;
        }
        .game-card h3 {
            color: #00ffff; /* Cyan titles */
            margin: 10px 0;
        }
        .game-card p {
            font-size: 0.9em;
            color: #cccccc;
        }
        canvas {
            background-color: #000;
            border: 2px solid #ffff00;
            image-rendering: pixelated;
        }
        button {
            background-color: #ff00ff;
            color: #000;
            border: none;
            padding: 10px;
            font-family: inherit;
            cursor: pointer;
            margin-top: 10px;
        }
        button:hover {
            background-color: #00ffff;
        }
        footer {
            margin-top: 50px;
            font-size: 0.8em;
            color: #999999;
        }
    </style>
</head>
<body>
    <h1>Grok's Arcade Minigames Galaxy</h1>
    <div class="intro">
        Alright, human! You've requested playable games in arcade style? I've upgraded the hub to full retro arcade mode: neon glows, pixel vibes, and actual playable demos. Since coding 107 full games here would crash the universe (or at least this response), I've implemented a few simple arcade-style games using HTML5 canvas and JS. The rest are placeholders – imagine them as unlocked levels. Copy-paste this into an HTML file, open in your browser, and insert coins (metaphorically). For real: Tic-Tac-Toe is a basic board game, Pong is classic bouncy action, Snake is slithery fun, and so on. Extend with your own JS for the full 107!
    </div>
    
    <div class="game-grid">
        <div class="game-card">
            <h3>Game 1: Cosmic Tic-Tac-Toe</h3>
            <p>Battle in a 3x3 grid – X vs O, arcade style!</p>
            <div id="ticTacToeBoard" style="display: grid; grid-template-columns: repeat(3, 50px); margin: 0 auto; width: 150px;"></div>
            <button onclick="resetTicTacToe()">Reset</button>
            <script>
                let tttBoard = ['', '', '', '', '', '', '', '', ''];
                let tttCurrentPlayer = 'X';
                const tttContainer = document.getElementById('ticTacToeBoard');
                function initTicTacToe() {
                    tttContainer.innerHTML = '';
                    tttBoard.forEach((cell, index) => {
                        const cellDiv = document.createElement('div');
                        cellDiv.style.width = '50px';
                        cellDiv.style.height = '50px';
                        cellDiv.style.border = '1px solid #00ff00';
                        cellDiv.style.fontSize = '40px';
                        cellDiv.style.color = '#ffff00';
                        cellDiv.style.display = 'flex';
                        cellDiv.style.alignItems = 'center';
                        cellDiv.style.justifyContent = 'center';
                        cellDiv.style.cursor = 'pointer';
                        cellDiv.onclick = () => makeTicTacToeMove(index);
                        cellDiv.textContent = cell;
                        tttContainer.appendChild(cellDiv);
                    });
                }
                function makeTicTacToeMove(index) {
                    if (tttBoard[index] === '' && !checkTicTacToeWinner()) {
                        tttBoard[index] = tttCurrentPlayer;
                        tttCurrentPlayer = tttCurrentPlayer === 'X' ? 'O' : 'X';
                        initTicTacToe();
                        if (checkTicTacToeWinner()) alert('Winner!');
                    }
                }
                function checkTicTacToeWinner() {
                    const wins = [[0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]];
                    return wins.some(combo => tttBoard[combo[0]] && tttBoard[combo[0]] === tttBoard[combo[1]] && tttBoard[combo[1]] === tttBoard[combo[2]]);
                }
                function resetTicTacToe() {
                    tttBoard = ['', '', '', '', '', '', '', '', ''];
                    tttCurrentPlayer = 'X';
                    initTicTacToe();
                }
                initTicTacToe();
            </script>
        </div>
        <div class="game-card">
            <h3>Game 2: Quantum Pong</h3>
            <p>Paddle the ball – don't let it pass!</p>
            <canvas id="pongCanvas" width="300" height="200"></canvas>
            <button onclick="startPong()">Start/Reset</button>
            <script>
                const pongCanvas = document.getElementById('pongCanvas');
                const pongCtx = pongCanvas.getContext('2d');
                let pongBallX = 150, pongBallY = 100, pongBallDX = 2, pongBallDY = -2;
                let pongPaddleY = 80, pongPaddleHeight = 40, pongPaddleWidth = 10;
                let pongScore = 0;
                let pongInterval;
                function drawPong() {
                    pongCtx.clearRect(0, 0, 300, 200);
                    pongCtx.fillStyle = '#00ff00';
                    pongCtx.fillRect(290 - pongPaddleWidth, pongPaddleY, pongPaddleWidth, pongPaddleHeight);
                    pongCtx.beginPath();
                    pongCtx.arc(pongBallX, pongBallY, 5, 0, Math.PI*2);
                    pongCtx.fill();
                    pongCtx.fillText('Score: ' + pongScore, 10, 20);
                }
                function updatePong() {
                    pongBallX += pongBallDX;
                    pongBallY += pongBallDY;
                    if (pongBallY < 0 || pongBallY > 200) pongBallDY = -pongBallDY;
                    if (pongBallX < 0) { pongBallDX = -pongBallDX; }
                    if (pongBallX > 290 - pongPaddleWidth && pongBallY > pongPaddleY && pongBallY < pongPaddleY + pongPaddleHeight) {
                        pongBallDX = -pongBallDX;
                        pongScore++;
                    } else if (pongBallX > 300) {
                        clearInterval(pongInterval);
                        alert('Game Over! Score: ' + pongScore);
                    }
                    drawPong();
                }
                function startPong() {
                    clearInterval(pongInterval);
                    pongBallX = 150; pongBallY = 100; pongBallDX = 2; pongBallDY = -2;
                    pongPaddleY = 80; pongScore = 0;
                    pongInterval = setInterval(updatePong, 10);
                }
                pongCanvas.addEventListener('mousemove', (e) => {
                    pongPaddleY = e.clientY - pongCanvas.getBoundingClientRect().top - pongPaddleHeight / 2;
                    if (pongPaddleY < 0) pongPaddleY = 0;
                    if (pongPaddleY > 200 - pongPaddleHeight) pongPaddleY = 200 - pongPaddleHeight;
                });
            </script>
        </div>
        <div class="game-card">
            <h3>Game 3: AI Snake Slither</h3>
            <p>Grow your snake – avoid walls!</p>
            <canvas id="snakeCanvas" width="200" height="200"></canvas>
            <button onclick="startSnake()">Start/Reset</button>
            <script>
                const snakeCanvas = document.getElementById('snakeCanvas');
                const snakeCtx = snakeCanvas.getContext('2d');
                let snake = [{x: 10, y: 10}];
                let snakeDX = 10, snakeDY = 0;
                let foodX = 50, foodY = 50;
                let snakeScore = 0;
                let snakeInterval;
                function drawSnake() {
                    snakeCtx.clearRect(0, 0, 200, 200);
                    snakeCtx.fillStyle = '#ffff00';
                    snake.forEach(part => snakeCtx.fillRect(part.x, part.y, 10, 10));
                    snakeCtx.fillStyle = '#ff00ff';
                    snakeCtx.fillRect(foodX, foodY, 10, 10);
                    snakeCtx.fillStyle = '#00ff00';
                    snakeCtx.fillText('Score: ' + snakeScore, 10, 190);
                }
                function updateSnake() {
                    const head = {x: snake[0].x + snakeDX, y: snake[0].y + snakeDY};
                    if (head.x < 0 || head.x >= 200 || head.y < 0 || head.y >= 200 || snake.some(part => part.x === head.x && part.y === head.y)) {
                        clearInterval(snakeInterval);
                        alert('Game Over! Score: ' + snakeScore);
                        return;
                    }
                    snake.unshift(head);
                    if (head.x === foodX && head.y === foodY) {
                        snakeScore++;
                        foodX = Math.floor(Math.random() * 19) * 10;
                        foodY = Math.floor(Math.random() * 19) * 10;
                    } else {
                        snake.pop();
                    }
                    drawSnake();
                }
                function startSnake() {
                    clearInterval(snakeInterval);
                    snake = [{x: 10, y: 10}];
                    snakeDX = 10; snakeDY = 0;
                    foodX = 50; foodY = 50;
                    snakeScore = 0;
                    snakeInterval = setInterval(updateSnake, 100);
                }
                document.addEventListener('keydown', (e) => {
                    if (e.key === 'ArrowUp' && snakeDY === 0) { snakeDX = 0; snakeDY = -10; }
                    if (e.key === 'ArrowDown' && snakeDY === 0) { snakeDX = 0; snakeDY = 10; }
                    if (e.key === 'ArrowLeft' && snakeDX === 0) { snakeDX = -10; snakeDY = 0; }
                    if (e.key === 'ArrowRight' && snakeDX === 0) { snakeDX = 10; snakeDY = 0; }
                });
            </script>
        </div>
        <div class="game-card">
            <h3>Game 4: Space Invaders Redux</h3>
            <p>Placeholder – Shoot 'em up! (Implement your own JS here)</p>
            <canvas width="300" height="200"></canvas>
        </div>
        <!-- Repeat pattern for more games. For example: -->
        <div class="game-card">
            <h3>Game 5: Meme Matcher</h3>
            <p>Placeholder – Match pairs in arcade flips.</p>
            <canvas width="300" height="200"></canvas>
        </div>
        <!-- ... Games 6 through 105 omitted for brevity – duplicate the structure with your own game logic ... -->
        <div class="game-card">
            <h3>Game 106: Universe Unfolder</h3>
            <p>Placeholder – Puzzle the cosmos.</p>
            <canvas width="300" height="200"></canvas>
        </div>
        <div class="game-card">
            <h3>Game 107: Bonus: Infinite Loop Runner</h3>
            <p>Placeholder – Endless arcade run.</p>
            <canvas width="300" height="200"></canvas>
        </div>
    </div>
    
    <footer>
        Arcade-ified by Grok. Save as .html, open in browser, and play! For more games, ask for specific JS code – I can generate implementations one by one.
    </footer>
</body>
</html>
