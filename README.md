<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roblox Adventure Land - Fun for Kids!</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #00b2ee, #ff6b35, #4ecdc4);
            margin: 0;
            padding: 0;
            color: #fff;
            overflow-x: hidden;
        }
        header {
            text-align: center;
            padding: 20px;
            background: rgba(0, 0, 0, 0.3);
        }
        h1 {
            font-size: 3em;
            text-shadow: 3px 3px #000;
            margin: 0;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        .section {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 20px;
            margin: 20px 0;
            text-align: center;
        }
        .block {
            display: inline-block;
            width: 80px;
            height: 80px;
            background: #4a90e2;
            margin: 5px;
            border: 2px solid #fff;
            border-radius: 10px;
            cursor: pointer;
            transition: transform 0.3s;
        }
        .block:hover {
            transform: scale(1.1);
        }
        #buildArea {
            min-height: 300px;
            border: 3px dashed #fff;
            background: rgba(255, 255, 255, 0.05);
        }
        button {
            background: #ff6b35;
            color: #fff;
            border: none;
            padding: 10px 20px;
            border-radius: 10px;
            font-size: 1.2em;
            cursor: pointer;
            margin: 10px;
            transition: background 0.3s;
        }
        button:hover {
            background: #e55a2b;
        }
        .quiz-question {
            font-size: 1.5em;
            margin-bottom: 20px;
        }
        .quiz-options button {
            display: block;
            width: 80%;
            margin: 10px auto;
        }
        #score {
            font-size: 2em;
            color: #ffd700;
        }
        .maze {
            display: grid;
            grid-template-columns: repeat(10, 30px);
            grid-template-rows: repeat(10, 30px);
            gap: 1px;
            background: #000;
            margin: 20px auto;
            width: fit-content;
            padding: 10px;
            border-radius: 10px;
        }
        .maze-cell {
            width: 30px;
            height: 30px;
            background: #fff;
            position: relative;
        }
        .wall { background: #333; }
        .start { background: #00ff00; }
        .end { background: #ff0000; }
        .player { background: #0000ff; border-radius: 50%; }
        .instructions {
            text-align: left;
            font-size: 1.1em;
        }
        footer {
            text-align: center;
            padding: 10px;
            background: rgba(0, 0, 0, 0.3);
            position: fixed;
            bottom: 0;
            width: 100%;
        }
    </style>
</head>
<body>
    <header>
        <h1>🟦 Roblox Adventure Land! 🎮</h1>
        <p>Welcome to the ultimate fun zone inspired by Roblox! Build, quiz, and maze your way to adventure!</p>
    </header>

    <div class="container">
        <div class="section">
            <h2>🏗️ Build Your World!</h2>
            <p>Click blocks to build your dream Roblox world! Drag them around.</p>
            <div id="buildArea" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
            <div>
                <div class="block" draggable="true" ondragstart="drag(event)" style="background: #4a90e2;"></div>
                <div class="block" draggable="true" ondragstart="drag(event)" style="background: #7ed321;"></div>
                <div class="block" draggable="true" ondragstart="drag(event)" style="background: #f5cd30;"></div>
                <div class="block" draggable="true" ondragstart="drag(event)" style="background: #d0021b;"></div>
            </div>
            <button onclick="clearBuild()">Clear World</button>
        </div>

        <div class="section">
            <h2>🧠 Roblox Quiz Time!</h2>
            <p>Test your Roblox knowledge! How many can you get right?</p>
            <div class="quiz-question" id="question"></div>
            <div class="quiz-options" id="options"></div>
            <div id="score">Score: 0</div>
            <button onclick="nextQuestion()">Next Question</button>
        </div>

        <div class="section">
            <h2>🌀 Escape the Noob Maze!</h2>
            <p>Use arrow keys to move the blue player (you) to the red goal! Avoid walls.</p>
            <div class="instructions">
                <p>Tip: Use ← ↑ → ↓ keys to navigate!</p>
            </div>
            <div id="maze" class="maze" tabindex="0" onkeydown="movePlayer(event)"></div>
            <button onclick="resetMaze()">Reset Maze</button>
        </div>
    </div>

    <footer>
        <p>Made with ❤️ for Roblox fans! Play safe and have fun! 🚀</p>
    </footer>

    <script>
        // Build Section
        function allowDrop(ev) { ev.preventDefault(); }
        function drag(ev) { ev.dataTransfer.setData("text", ev.target.style.background); }
        function drop(ev) {
            ev.preventDefault();
            var color = ev.dataTransfer.getData("text");
            var newBlock = document.createElement('div');
            newBlock.className = 'block';
            newBlock.style.background = color;
            newBlock.draggable = true;
            newBlock.ondragstart = drag;
            ev.target.appendChild(newBlock);
        }
        function clearBuild() {
            document.getElementById('buildArea').innerHTML = '';
        }

        // Quiz Section
        const quizData = [
            { q: "What year was Roblox created?", o: ["2015", "2006", "2020"], a: 1 },
            { q: "Who is the founder of Roblox?", o: ["Mark Zuckerberg", "David Baszucki", "Elon Musk"], a: 1 },
            { q: "What is the currency in Roblox?", o: ["Dollars", "Robux", "Coins"], a: 1 },
            { q: "What does Roblox stand for?", o: ["Robots Blocks", "Roblox Online", "Robots Explore Blocks Online"], a: 2 }
        ];
        let currentQuiz = 0;
        let score = 0;

        function showQuestion() {
            const q = quizData[currentQuiz];
            document.getElementById('question').innerText = q.q;
            const opts = document.getElementById('options');
            opts.innerHTML = '';
            q.o.forEach((opt, i) => {
                const btn = document.createElement('button');
                btn.innerText = opt;
                btn.onclick = () => checkAnswer(i, q.a);
                opts.appendChild(btn);
            });
        }

        function checkAnswer(selected, correct) {
            if (selected === correct) score++;
            document.getElementById('score').innerText = `Score: ${score}`;
            currentQuiz++;
        }

        function nextQuestion() {
            if (currentQuiz < quizData.length) {
                showQuestion();
            } else {
                alert(`Quiz over! Final score: ${score}/${quizData.length} 🎉`);
                currentQuiz = 0;
                score = 0;
                document.getElementById('score').innerText = 'Score: 0';
            }
        }
        showQuestion(); // Initial load

        // Maze Section
        const mazeGrid = [
            [1,1,1,1,1,1,1,1,1,1],
            [1,0,0,0,0,0,0,0,0,1],
            [1,0,1,1,0,1,1,0,0,1],
            [1,0,0,1,0,0,1,0,0,1],
            [1,0,1,1,1,0,1,1,0,1],
            [1,0,0,0,0,0,0,0,0,1],
            [1,1,1,1,1,1,1,1,2,1],
            [1,0,0,0,0,0,0,0,0,1],
            [1,0,1,1,0,1,1,0,0,1],
            [1,1,1,1,1,1,1,1,1,1]
        ]; // 0 empty, 1 wall, 2 end
        let playerX = 1, playerY = 1;

        function generateMaze() {
            const maze = document.getElementById('maze');
            maze.innerHTML = '';
            for (let y = 0; y < 10; y++) {
                for (let x = 0; x < 10; x++) {
                    const cell = document.createElement('div');
                    cell.className = 'maze-cell';
                    if (mazeGrid[y][x] === 1) cell.classList.add('wall');
                    if (y === 1 && x === 1) cell.classList.add('start');
                    if (mazeGrid[y][x] === 2) cell.classList.add('end');
                    if (y === playerY && x === playerX) {
                        const player = document.createElement('div');
                        player.className = 'player';
                        cell.appendChild(player);
                    }
                    maze.appendChild(cell);
                }
            }
        }

        function movePlayer(e) {
            let newX = playerX, newY = playerY;
            switch(e.key) {
                case 'ArrowUp': newY--; break;
                case 'ArrowDown': newY++; break;
                case 'ArrowLeft': newX--; break;
                case 'ArrowRight': newX++; break;
            }
            if (newX >= 0 && newX < 10 && newY >= 0 && newY < 10 && mazeGrid[newY][newX] !== 1) {
                playerX = newX;
                playerY = newY;
                generateMaze();
                if (mazeGrid[newY][newX] === 2) {
                    alert('You escaped! 🏆');
                    resetMaze();
                }
            }
        }

        function resetMaze() {
            playerX = 1;
            playerY = 1;
            generateMaze();
        }
        generateMaze();
    </script>
</body>
</html>
