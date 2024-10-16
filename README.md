<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            height: 100vh;
            margin: 0;
            background-color: #000;
        }
        canvas {
            border: 1px solid white;
        }
        .controls {
            display: flex;
            justify-content: center;
            margin-top: 20px;
        }
        .controls button {
            width: 50px;
            height: 50px;
            margin: 5px;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <!-- Nút điều khiển ảo cho điện thoại -->
    <div class="controls">
        <button id="left">⬅️</button>
        <div>
            <button id="up">⬆️</button><br>
            <button id="down">⬇️</button>
        </div>
        <button id="right">➡️</button>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const gridSize = 20;
        const canvasSize = 400;
        let snake = [{x: 200, y: 200}];
        let food = {x: Math.floor(Math.random() * (canvasSize / gridSize)) * gridSize, y: Math.floor(Math.random() * (canvasSize / gridSize)) * gridSize};
        let dx = gridSize;
        let dy = 0;
        let score = 0;
        let gameOver = false;
        const gameSpeed = 200;  // Điều chỉnh tốc độ tại đây

        document.addEventListener('keydown', changeDirection);
        document.getElementById('up').addEventListener('click', () => setDirection(0, -gridSize));
        document.getElementById('down').addEventListener('click', () => setDirection(0, gridSize));
        document.getElementById('left').addEventListener('click', () => setDirection(-gridSize, 0));
        document.getElementById('right').addEventListener('click', () => setDirection(gridSize, 0));

        function gameLoop() {
            if (gameOver) return;
            setTimeout(() => {
                clearCanvas();
                drawFood();
                moveSnake();
                drawSnake();
                checkGameOver();
                gameLoop();
            }, gameSpeed);
        }

        function clearCanvas() {
            ctx.fillStyle = 'black';
            ctx.fillRect(0, 0, canvasSize, canvasSize);
        }

        function drawSnake() {
            ctx.fillStyle = 'green';
            snake.forEach(part => ctx.fillRect(part.x, part.y, gridSize, gridSize));
        }

        function moveSnake() {
            const head = {x: snake[0].x + dx, y: snake[0].y + dy};

            if (head.x === food.x && head.y === food.y) {
                score++;
                food = {x: Math.floor(Math.random() * (canvasSize / gridSize)) * gridSize, y: Math.floor(Math.random() * (canvasSize / gridSize)) * gridSize};
            } else {
                snake.pop();
            }
            snake.unshift(head);
        }

        function drawFood() {
            ctx.fillStyle = 'red';
            ctx.fillRect(food.x, food.y, gridSize, gridSize);
        }

        function changeDirection(event) {
            if (event.key === 'ArrowUp' && dy === 0) {
                dx = 0;
                dy = -gridSize;
            } else if (event.key === 'ArrowDown' && dy === 0) {
                dx = 0;
                dy = gridSize;
            } else if (event.key === 'ArrowLeft' && dx === 0) {
                dx = -gridSize;
                dy = 0;
            } else if (event.key === 'ArrowRight' && dx === 0) {
                dx = gridSize;
                dy = 0;
            }
        }

        function setDirection(newDx, newDy) {
            if ((newDx !== 0 && dx === 0) || (newDy !== 0 && dy === 0)) {
                dx = newDx;
                dy = newDy;
            }
        }

        function checkGameOver() {
            const head = snake[0];
            if (head.x < 0 || head.x >= canvasSize || head.y < 0 || head.y >= canvasSize || snake.slice(1).some(part => part.x === head.x && part.y === head.y)) {
                gameOver = true;
                alert('Game Over! Score: ' + score);
            }
        }

        gameLoop();
    </script>
</body>
</html>
