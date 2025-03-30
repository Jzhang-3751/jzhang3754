<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday Cherry - Maze</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            flex-direction: column;
        }
        #maze {
            display: grid;
            grid-template-columns: repeat(30, 20px);
            grid-template-rows: repeat(30, 20px);
            gap: 1px;
            margin-bottom: 20px; /* Add margin so buttons don't overlap maze */
        }
        .cell {
            width: 20px;
            height: 20px;
            background-color: white;
            border: 1px solid #ddd;
        }
        .wall {
            background-color: blue;
        }
        .player {
            background-color: red;
        }
        .goal {
            background-color: green;
        }
        #controls {
            display: flex;
            justify-content: center;
            width: 100%;
        }
        .btn {
            width: 50px;
            height: 50px;
            margin: 5px;
            background-color: #4CAF50;
            border: none;
            color: white;
            font-size: 20px;
            cursor: pointer;
        }
        #message {
            font-size: 24px;
            font-weight: bold;
            color: red;
            display: none;
            animation: blink 1s infinite;
        }
        @keyframes blink {
            50% {
                opacity: 0;
            }
        }
        #hint {
            font-size: 16px;
            color: blue;
            font-weight: bold;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <div id="maze"></div>
    <div id="hint">The blue means the wall.</div>
    <div id="message">Happy Birthday Cherry!</div>
    <div id="controls">
        <button class="btn" id="up">↑</button>
        <button class="btn" id="left">←</button>
        <button class="btn" id="down">↓</button>
        <button class="btn" id="right">→</button>
    </div>

    <script>
        const mazeSize = 30;
        const maze = [];
        let playerPos = { x: 1, y: 1 };
        let goalPos = { x: mazeSize - 2, y: mazeSize - 2 };
        let gameOver = false;

        function generateMaze() {
            for (let i = 0; i < mazeSize; i++) {
                maze[i] = [];
                for (let j = 0; j < mazeSize; j++) {
                    // Decrease the chance of walls, change the probability to > 0.7 for fewer walls
                    maze[i][j] = Math.random() > 0.7 ? 'wall' : 'path'; // Adjusted probability for fewer walls
                }
            }
            maze[0][0] = 'path'; // Start
            maze[mazeSize - 1][mazeSize - 1] = 'goal'; // Goal
            maze[1][1] = 'player'; // Player start
        }

        function renderMaze() {
            const mazeElement = document.getElementById('maze');
            mazeElement.innerHTML = '';
            for (let y = 0; y < mazeSize; y++) {
                for (let x = 0; x < mazeSize; x++) {
                    const cell = document.createElement('div');
                    cell.classList.add('cell');
                    if (maze[y][x] === 'wall') {
                        cell.classList.add('wall');
                    }
                    if (maze[y][x] === 'player') {
                        cell.classList.add('player');
                    }
                    if (maze[y][x] === 'goal') {
                        cell.classList.add('goal');
                    }
                    mazeElement.appendChild(cell);
                }
            }
        }

        function movePlayer(dx, dy) {
            if (gameOver) return;
            const newX = playerPos.x + dx;
            const newY = playerPos.y + dy;
            if (newX >= 0 && newY >= 0 && newX < mazeSize && newY < mazeSize && maze[newY][newX] !== 'wall') {
                maze[playerPos.y][playerPos.x] = 'path';
                playerPos.x = newX;
                playerPos.y = newY;
                maze[playerPos.y][playerPos.x] = 'player';
                renderMaze();
                checkGoal();
            }
        }

        function checkGoal() {
            if (playerPos.x === goalPos.x && playerPos.y === goalPos.y) {
                gameOver = true;
                // Wait for 5 seconds before showing the message
                setTimeout(() => {
                    document.getElementById('message').style.display = 'block';
                }, 5000);
            }
        }

        function addControlListeners() {
            document.getElementById('up').addEventListener('click', () => movePlayer(0, -1));
            document.getElementById('down').addEventListener('click', () => movePlayer(0, 1));
            document.getElementById('left').addEventListener('click', () => movePlayer(-1, 0));
            document.getElementById('right').addEventListener('click', () => movePlayer(1, 0));
        }

        function startGame() {
            generateMaze();
            renderMaze();
            addControlListeners();
        }

        startGame();
    </script>
</body>
</html>
