# Tony.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ball and Racquet Game</title>
    <style>
        canvas {
            border: 1px solid #000;
            display: block;
            margin: auto;
        }
    </style>
</head>
<body>
    <h1>Ball and Racquet Game</h1>
    <canvas id="gameCanvas" width="600" height="400"></canvas>

    <script>
        var canvas = document.getElementById("gameCanvas");
        var context = canvas.getContext("2d");

        var ball = {
            x: 50,
            y: 50,
            radius: 10,
            speedX: 2,
            speedY: 2
        };

        var racquet = {
            x: 250,
            y: 380,
            width: 80,
            height: 10
        };

        var score = 0;

        function drawBall() {
            context.beginPath();
            context.arc(ball.x, ball.y, ball.radius, 0, 2 * Math.PI);
            context.fillStyle = "darkGray";
            context.fill();
            context.closePath();
        }

        function drawRacquet() {
            context.fillRect(racquet.x, racquet.y, racquet.width, racquet.height);
        }

        function drawScore() {
            context.font = "20px Arial";
            context.fillStyle = "black";
            context.fillText("Score: " + score, 10, 30);
        }

        function moveBall() {
            ball.x += ball.speedX;
            ball.y += ball.speedY;

            if (ball.x + ball.radius > canvas.width || ball.x - ball.radius < 0) {
                ball.speedX = -ball.speedX;
            }

            if (ball.y - ball.radius < 0) {
                ball.speedY = -ball.speedY;
            }

            if (
                ball.y + ball.radius > racquet.y &&
                ball.x > racquet.x &&
                ball.x < racquet.x + racquet.width
            ) {
                ball.speedY = -ball.speedY;
                score++;
            }

            if (ball.y + ball.radius > canvas.height) {
                alert("Game Over! Your Score: " + score);
                resetGame();
            }
        }

        function moveRacquet(e) {
            if (e.key === "ArrowLeft" && racquet.x > 0) {
                racquet.x -= 10;
            } else if (e.key === "ArrowRight" && racquet.x + racquet.width < canvas.width) {
                racquet.x += 10;
            }
        }

        function resetGame() {
            score = 0;
            ball.x = 50;
            ball.y = 50;
            ball.speedX = 2;
            ball.speedY = 2;
            racquet.x = 250;
        }

        function draw() {
            context.clearRect(0, 0, canvas.width, canvas.height);
            drawBall();
            drawRacquet();
            drawScore();
            moveBall();
            requestAnimationFrame(draw);
        }

        document.addEventListener("keydown", moveRacquet);
        draw();
    </script>
</body>
</html>
