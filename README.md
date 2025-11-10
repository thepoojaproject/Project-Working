<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Bouncing Ball Animation</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: radial-gradient(circle at center, #0f2027, #203a43, #2c5364);
      overflow: hidden;
    }

    .container {
      position: relative;
      width: 400px;
      height: 400px;
      border: 3px solid #fff;
      border-radius: 10px;
      overflow: hidden;
      background: rgba(255, 255, 255, 0.05);
      box-shadow: 0 0 30px rgba(255,255,255,0.2);
    }

    .ball {
      position: absolute;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      background: radial-gradient(circle at 30% 30%, #ff416c, #ff4b2b);
      box-shadow: 0 0 20px rgba(255, 75, 43, 0.8);
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="ball" id="ball"></div>
  </div>

  <script>
    const ball = document.getElementById('ball');
    const container = document.querySelector('.container');

    let x = 50;
    let y = 50;
    let dx = 3;
    let dy = 2;
    const gravity = 0.3;
    const bounce = 0.8;

    function animate() {
      const cw = container.clientWidth;
      const ch = container.clientHeight;
      const bw = ball.clientWidth;
      const bh = ball.clientHeight;

      dy += gravity; // add gravity
      x += dx;
      y += dy;

      // Bounce from floor
      if (y + bh > ch) {
        y = ch - bh;
        dy *= -bounce;
      }

      // Bounce from top
      if (y < 0) {
        y = 0;
        dy *= -bounce;
      }

      // Bounce from sides
      if (x + bw > cw) {
        x = cw - bw;
        dx *= -1;
      }

      if (x < 0) {
        x = 0;
        dx *= -1;
      }

      ball.style.left = x + 'px';
      ball.style.top = y + 'px';

      requestAnimationFrame(animate);
    }

    animate();
  </script>
</body>
</html>
