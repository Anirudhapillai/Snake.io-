<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Classic Snake Game</title>
  <style>
    body {
      margin: 0;
      background: black;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      color: white;
      font-family: "Courier New", monospace;
    }
    canvas {
      background: #111;
      border: 2px solid #0f0;
    }
  </style>
</head>
<body>
  <canvas id="game" width="400" height="400"></canvas>
  <script>
    const canvas = document.getElementById("game");
    const ctx = canvas.getContext("2d");

    const gridSize = 20;
    const tileCount = canvas.width / gridSize;

    let snake = [{x: 10, y: 10}];
    let velocity = {x: 0, y: 0};
    let food = {x: 5, y: 5};
    let score = 0;

    function gameLoop() {
      update();
      draw();
    }

    function update() {
      // Move snake
      const head = {x: snake[0].x + velocity.x, y: snake[0].y + velocity.y};
      snake.unshift(head);

      // Check food collision
      if (head.x === food.x && head.y === food.y) {
        score++;
        placeFood();
      } else {
        snake.pop();
      }

      // Check wall collision
      if (head.x < 0 || head.y < 0 || head.x >= tileCount || head.y >= tileCount) {
        resetGame();
      }

      // Check self collision
      for (let i = 1; i < snake.length; i++) {
        if (snake[i].x === head.x && snake[i].y === head.y) {
          resetGame();
        }
      }
    }

    function draw() {
      ctx.fillStyle = "black";
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      // Draw snake
      ctx.fillStyle = "lime";
      snake.forEach(part => ctx.fillRect(part.x * gridSize, part.y * gridSize, gridSize-2, gridSize-2));

      // Draw food
      ctx.fillStyle = "red";
      ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize-2, gridSize-2);

      // Draw score
      ctx.fillStyle = "white";
      ctx.font = "16px Courier";
      ctx.fillText("Score: " + score, 10, 20);
    }

    function placeFood() {
      food.x = Math.floor(Math.random() * tileCount);
      food.y = Math.floor(Math.random() * tileCount);
    }

    function resetGame() {
      snake = [{x: 10, y: 10}];
      velocity = {x: 0, y: 0};
      score = 0;
      placeFood();
    }

    document.addEventListener("keydown", e => {
      switch(e.key) {
        case "ArrowUp":
          if (velocity.y === 0) velocity = {x: 0, y: -1};
          break;
        case "ArrowDown":
          if (velocity.y === 0) velocity = {x: 0, y: 1};
          break;
        case "ArrowLeft":
          if (velocity.x === 0) velocity = {x: -1, y: 0};
          break;
        case "ArrowRight":
          if (velocity.x === 0) velocity = {x: 1, y: 0};
          break;
      }
    });

    setInterval(gameLoop, 100);
  </script>
</body>
</html>
