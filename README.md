const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

const gridSize = 20;
const tileSize = canvas.width / gridSize;

let snake = [
    { x: 8, y: 8 },
    { x: 7, y: 8 },
    { x: 6, y: 8 }
];
let direction = { x: 1, y: 0 };
let food = { x: Math.floor(Math.random() * gridSize), y: Math.floor(Math.random() * gridSize) };

function drawTile(x, y, color) {
    ctx.fillStyle = color;
    ctx.fillRect(x * tileSize, y * tileSize, tileSize, tileSize);
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    snake.forEach(segment => drawTile(segment.x, segment.y, 'lime'));
    drawTile(food.x, food.y, 'red');
}

function update() {
    const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

    if (head.x === food.x && head.y === food.y) {
        food = { x: Math.floor(Math.random() * gridSize), y: Math.floor(Math.random() * gridSize) };
    } else {
        snake.pop();
    }

    if (head.x < 0 || head.y < 0 || head.x >= gridSize || head.y >= gridSize || snake.some(segment => segment.x === head.x && segment.y === head.y)) {
        clearInterval(gameLoop);
        alert('Game Over');
    }

    snake.unshift(head);
}

function changeDirection(event) {
    const keyPressed = event.keyCode;
    const UP = 38;
    const DOWN = 40;
    const LEFT = 37;
    const RIGHT = 39;

    const isMovingUp = direction.y === -1;
    const isMovingDown = direction.y === 1;
    const isMovingRight = direction.x === 1;
    const isMovingLeft = direction.x === -1;

    if (keyPressed === UP && !isMovingDown) {
        direction = { x: 0, y: -1 };
    }

    if (keyPressed === DOWN && !isMovingUp) {
        direction = { x: 0, y: 1 };
    }

    if (keyPressed === LEFT && !isMovingRight) {
        direction = { x: -1, y: 0 };
    }

    if (keyPressed === RIGHT && !isMovingLeft) {
        direction = { x: 1, y: 0 };
    }
}

document.addEventListener('keydown', changeDirection);

function gameLoop() {
    update();
    draw();
}

const gameInterval = setInterval(gameLoop, 100);body {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
    background-color: #000;
    color: #fff;
    font-family: Arial, sans-serif;
}

h1 {
    margin-bottom: 20px;
}

.game-container {
    position: relative;
    width: 400px;
    height: 400px;
    border: 1px solid #fff;
}

canvas {
    display: block;
    background-color: #111;
}<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Snake Game</h1>
    <div class="game-container">
        <canvas id="gameCanvas"></canvas>
    </div>
    <script src="script.js"></script>
</body>
</html>
