<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dino Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game">
        <div id="dino"></div>
        <div id="cactus"></div>
        <div id="score">Score: 0</div>
    </div>
    <script src="script.js"></script>
</body>
</html>
body {
    margin: 0;
    padding: 0;
    background-color: #f7f7f7;
    overflow: hidden;
}

#game {
    position: relative;
    width: 100vw;
    height: 100vh;
    background-color: #e0e0e0;
    overflow: hidden;
}

#dino {
    position: absolute;
    bottom: 20px;
    left: 50px;
    width: 50px;
    height: 50px;
    background-color: #000;
}

#cactus {
    position: absolute;
    bottom: 20px;
    right: -50px;
    width: 50px;
    height: 50px;
    background-color: #3e3e3e;
}

#score {
    position: absolute;
    top: 20px;
    left: 20px;
    font-size: 24px;
    color: #000;
}const dino = document.getElementById('dino');
const cactus = document.getElementById('cactus');
const scoreElement = document.getElementById('score');

let isJumping = false;
let score = 0;
let cactusSpeed = 5;

document.addEventListener('keydown', (event) => {
    if (event.code === 'Space' && !isJumping) {
        jump();
    }
});

function jump() {
    isJumping = true;
    let jumpHeight = 0;
    const jumpInterval = setInterval(() => {
        if (jumpHeight >= 100) {
            clearInterval(jumpInterval);
            fall();
        } else {
            jumpHeight += 10;
            dino.style.bottom = `${20 + jumpHeight}px`;
        }
    }, 20);
}

function fall() {
    const fallInterval = setInterval(() => {
        let currentBottom = parseInt(dino.style.bottom);
        if (currentBottom <= 20) {
            clearInterval(fallInterval);
            isJumping = false;
            dino.style.bottom = '20px';
        } else {
            dino.style.bottom = `${currentBottom - 10}px`;
        }
    }, 20);
}

function moveCactus() {
    let cactusPosition = window.innerWidth;
    const cactusInterval = setInterval(() => {
        if (cactusPosition < -50) {
            cactusPosition = window.innerWidth;
            score++;
            scoreElement.textContent = `Score: ${score}`;
            cactusSpeed += 0.5; // Increase speed over time
        } else {
            cactusPosition -= cactusSpeed;
            cactus.style.right = `${window.innerWidth - cactusPosition}px`;
        }

        // Collision detection
        const dinoRect = dino.getBoundingClientRect();
        const cactusRect = cactus.getBoundingClientRect();
        if (!(dinoRect.right < cactusRect.left || 
              dinoRect.left > cactusRect.right || 
              dinoRect.bottom < cactusRect.top || 
              dinoRect.top > cactusRect.bottom)) {
            alert('Game Over');
            location.reload();
        }
    }, 20);
}

moveCactus();
