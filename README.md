const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

const paddleWidth = 10, paddleHeight = 100;
const ballSize = 10;

let player = { x: 10, y: canvas.height / 2 - paddleHeight / 2, width: paddleWidth, height: paddleHeight, dy: 0 };
let ai = { x: canvas.width - 20, y: canvas.height / 2 - paddleHeight / 2, width: paddleWidth, height: paddleHeight, dy: 2 };
let ball = { x: canvas.width / 2, y: canvas.height / 2, dx: 4, dy: 4, size: ballSize };

function drawRect(x, y, width, height, color) {
    ctx.fillStyle = color;
    ctx.fillRect(x, y, width, height);
}

function drawBall(x, y, size, color) {
    ctx.fillStyle = color;
    ctx.beginPath();
    ctx.arc(x, y, size, 0, Math.PI * 2);
    ctx.fill();
}

function clearCanvas() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
}

function movePaddle(paddle) {
    paddle.y += paddle.dy;
    if (paddle.y < 0) {
        paddle.y = 0;
    }
    if (paddle.y + paddle.height > canvas.height) {
        paddle.y = canvas.height - paddle.height;
    }
}

function moveBall() {
    ball.x += ball.dx;
    ball.y += ball.dy;

    if (ball.y <= 0 || ball.y + ball.size >= canvas.height) {
        ball.dy *= -1;
    }

    if (ball.x <= player.x + player.width && ball.y > player.y && ball.y < player.y + player.height) {
        ball.dx *= -1;
    }

    if (ball.x + ball.size >= ai.x && ball.y > ai.y && ball.y < ai.y + ai.height) {
        ball.dx *= -1;
    }

    if (ball.x <= 0 || ball.x + ball.size >= canvas.width) {
        ball.x = canvas.width / 2;
        ball.y = canvas.height / 2;
        ball.dx *= -1;
    }
}

function update() {
    clearCanvas();
    drawRect(player.x, player.y, player.width, player.height, 'white');
    drawRect(ai.x, ai.y, ai.width, ai.height, 'white');
    drawBall(ball.x, ball.y, ball.size, 'white');
    movePaddle(player);
    movePaddle(ai);
    moveBall();
    ai.y += ai.dy;
    if (ai.y <= 0 || ai.y + ai.height >= canvas.height) {
        ai.dy *= -1;
    }
    requestAnimationFrame(update);
}

function keyDown(e) {
    if (e.key === 'ArrowUp') {
        player.dy = -6;
    } else if (e.key === 'ArrowDown') {
        player.dy = 6;
    }
}

function keyUp(e) {
    if (e.key === 'ArrowUp' || e.key === 'ArrowDown') {
        player.dy = 0;
    }
}

document.addEventListener('keydown', keyDown);
document.addEventListener('keyup', keyUp);

update();
