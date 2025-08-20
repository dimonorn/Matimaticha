<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Куб собирает монетки</title>
    <style>
        body {
            margin: 0;
            padding: 20px;
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
        }
        
        #game-container {
            position: relative;
            width: 600px;
            height: 400px;
            background-color: #222;
            border: 3px solid #333;
            margin: 0 auto;
            overflow: hidden;
        }
        
        #player {
            position: absolute;
            width: 40px;
            height: 40px;
            background-color: #3498db;
            transition: transform 0.2s;
        }
        
        .coin {
            position: absolute;
            width: 20px;
            height: 20px;
            background-color: gold;
            border-radius: 50%;
            animation: pulse 1s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
        
        #score {
            text-align: center;
            font-size: 24px;
            margin: 10px;
            color: #333;
        }
        
        #controls {
            text-align: center;
            margin: 10px;
        }
        
        button {
            padding: 10px 20px;
            font-size: 16px;
            margin: 5px;
            cursor: pointer;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h1 style="text-align: center;">Куб собирает монетки</h1>
    <div id="score">Собрано монет: 0</div>
    <div id="controls">
        <button onclick="movePlayer('up')">↑ Вверх</button>
        <button onclick="movePlayer('left')">← Влево</button>
        <button onclick="movePlayer('down')">↓ Вниз</button>
        <button onclick="movePlayer('right')">→ Вправо</button>
        <button onclick="resetGame()">Новая игра</button>
    </div>
    <div id="game-container">
        <div id="player"></div>
    </div>

    <script>
        // Переменные игры
        let score = 0;
        let playerX = 280;
        let playerY = 180;
        const playerSpeed = 10;
        const gameWidth = 600;
        const gameHeight = 400;
        const playerSize = 40;
        const coinSize = 20;
        
        const player = document.getElementById('player');
        const scoreDisplay = document.getElementById('score');
        const gameContainer = document.getElementById('game-container');
        
        // Инициализация игры
        function initGame() {
            score = 0;
            playerX = 280;
            playerY = 180;
            updatePlayerPosition();
            updateScore();
            
            // Удаляем старые монетки
            document.querySelectorAll('.coin').forEach(coin => coin.remove());
            
            // Создаем новые монетки
            createCoins(5);
        }
        
        // Создание монеток
        function createCoins(count) {
            for (let i = 0; i < count; i++) {
                const coin = document.createElement('div');
                coin.className = 'coin';
                
                // Случайная позиция в пределах игрового поля
                const coinX = Math.random() * (gameWidth - coinSize);
                const coinY = Math.random() * (gameHeight - coinSize);
                
                coin.style.left = coinX + 'px';
                coin.style.top = coinY + 'px';
                
                gameContainer.appendChild(coin);
            }
        }
        
        // Движение игрока
        function movePlayer(direction) {
            switch(direction) {
                case 'up':
                    playerY = Math.max(0, playerY - playerSpeed);
                    break;
                case 'down':
                    playerY = Math.min(gameHeight - playerSize, playerY + playerSpeed);
                    break;
                case 'left':
                    playerX = Math.max(0, playerX - playerSpeed);
                    break;
                case 'right':
                    playerX = Math.min(gameWidth - playerSize, playerX + playerSpeed);
                    break;
            }
            
            updatePlayerPosition();
            checkCoinCollision();
        }
        
        // Обновление позиции игрока
        function updatePlayerPosition() {
            player.style.left = playerX + 'px';
            player.style.top = playerY + 'px';
            
            // Анимация вращения куба
            player.style.transform = `rotate(${Date.now() / 10 % 360}deg)`;
        }
        
        // Проверка столкновения с монетками
        function checkCoinCollision() {
            const coins = document.querySelectorAll('.coin');
            
            coins.forEach(coin => {
                const coinRect = {
                    left: parseInt(coin.style.left),
                    top: parseInt(coin.style.top),
                    right: parseInt(coin.style.left) + coinSize,
                    bottom: parseInt(coin.style.top) + coinSize
                };
                
                const playerRect = {
                    left: playerX,
                    top: playerY,
                    right: playerX + playerSize,
                    bottom: playerY + playerSize
                };
                
                // Проверка пересечения
                if (playerRect.right > coinRect.left && 
                    playerRect.left < coinRect.right && 
                    playerRect.bottom > coinRect.top && 
                    playerRect.top < coinRect.bottom) {
                    
                    // Собираем монетку
                    coin.remove();
                    score++;
                    updateScore();
                    
                    // Создаем новую монетку, если собрали все
                    if (document.querySelectorAll('.coin').length === 0) {
                        createCoins(3);
                    }
                }
            });
        }
        
        // Обновление счета
        function updateScore() {
            scoreDisplay.textContent = `Собрано монет: ${score}`;
        }
        
        // Сброс игры
        function resetGame() {
            initGame();
        }
        
        // Управление с клавиатуры
        document.addEventListener('keydown', (event) => {
            switch(event.key) {
                case 'ArrowUp':
                    movePlayer('up');
                    break;
                case 'ArrowDown':
                    movePlayer('down');
                    break;
                case 'ArrowLeft':
                    movePlayer('left');
                    break;
                case 'ArrowRight':
                    movePlayer('right');
                    break;
            }
        });
        
        // Запуск игры при загрузке страницы
        window.onload = initGame;
    </script>
</body>
</html># Matimaticha
Dhhdhdh
