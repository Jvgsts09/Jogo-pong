// Definindo as variáveis globais
let canvas;
let canvasContext;
let ballX = 50;
let ballSpeedX = 10;
let ballY = 50;
let ballSpeedY = 4;

let paddle1Y = 250;
let paddle2Y = 250;
const PADDLE_HEIGHT = 100;
const PADDLE_THICKNESS = 10;

// Função para calcular a posição do mouse
function calculateMousePos(evt) {
    let rect = canvas.getBoundingClientRect();
    let root = document.documentElement;
    let mouseX = evt.clientX - rect.left - root.scrollLeft;
    let mouseY = evt.clientY - rect.top - root.scrollTop;
    return {
        x: mouseX,
        y: mouseY
    };
}

// Função para resetar a bola
function ballReset() {
    ballX = canvas.width / 2;
    ballY = canvas.height / 2;
    ballSpeedX = -ballSpeedX;
}

// Função principal para desenhar elementos na tela
function drawEverything() {
    // Desenha o fundo
    colorRect(0, 0, canvas.width, canvas.height, 'black');

    // Desenha a bola
    colorCircle(ballX, ballY, 10, 'white');

    // Desenha a raquete do jogador 1
    colorRect(0, paddle1Y, PADDLE_THICKNESS, PADDLE_HEIGHT, 'white');

    // Desenha a raquete do jogador 2
    colorRect(canvas.width - PADDLE_THICKNESS, paddle2Y, PADDLE_THICKNESS, PADDLE_HEIGHT, 'white');
}

// Função para desenhar um retângulo colorido
function colorRect(leftX, topY, width, height, drawColor) {
    canvasContext.fillStyle = drawColor;
    canvasContext.fillRect(leftX, topY, width, height);
}

// Função para desenhar um círculo colorido
function colorCircle(centerX, centerY, radius, drawColor) {
    canvasContext.fillStyle = drawColor;
    canvasContext.beginPath();
    canvasContext.arc(centerX, centerY, radius, 0, Math.PI * 2, true);
    canvasContext.fill();
}

// Função para atualizar a posição dos elementos
function moveEverything() {
    ballX += ballSpeedX;
    ballY += ballSpeedY;

    if (ballX < 0) {
        if (ballY > paddle1Y && ballY < paddle1Y + PADDLE_HEIGHT) {
            ballSpeedX = -ballSpeedX;
        } else {
            ballReset();
        }
    }
    if (ballX > canvas.width) {
        if (ballY > paddle2Y && ballY < paddle2Y + PADDLE_HEIGHT) {
            ballSpeedX = -ballSpeedX;
        } else {
            ballReset();
        }
    }

    if (ballY < 0 || ballY > canvas.height) {
        ballSpeedY = -ballSpeedY;
    }
}

// Função para controlar a raquete do jogador 1
function player1Move(evt) {
    let mousePos = calculateMousePos(evt);
    paddle1Y = mousePos.y - (PADDLE_HEIGHT / 2);
}

// Função principal de inicialização do jogo
window.onload = function() {
    canvas = document.getElementById('gameCanvas');
    canvasContext = canvas.getContext('2d');

    const framesPerSecond = 30;
    setInterval(function() {
        moveEverything();
        drawEverything();
    }, 1000 / framesPerSecond);

    canvas.addEventListener('mousemove', player1Move);
};# Jogo-pong
