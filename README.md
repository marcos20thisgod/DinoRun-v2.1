# DinoRun-v1.1
by Marcos Cardoza 
<!DOCTYPE html>
<html>
  <head>
    <title>DinoRun</title>
    <style>
      #game {
        width: 400px;
        height: 230px;
        border: 1px solid black;
        position: relative;
        overflow: hidden;
        background-image: url('https://i.ibb.co/3YrDJky/89309798-fondo-transparente-de-pixel-art-ubicaci-n-con-bosque-nevado-por-la-noche-paisaje-para-juego.jpg');
        animation: background-scroll 02s linear infinite;
      }

      @keyframes background-scroll {
        from {
          background-position: 0 0;
        }
        to {
          background-position: -1000px 0;
        }
      }

      #dinosaur {
        width: 50px;
        height: 50px;
        position: absolute;
        bottom: 0px;
        left: 40px;
        background-image: url('https://i.ibb.co/GH7x14y/be5.gif');
        background-size: contain;
        animation: run 0.8s infinite;
        transform-origin: bottom;
      }

      .obstacle {
        width: 10px;
        height: 25px;
        position: absolute;
        bottom: 0;
        background-color: red;
      }
    </style>
  </head>
  <body>
    <h1>DinoRun</h1>
    <p>Toca la pantalla para saltar.</p>
    <p>Hola Eliezer jajaja no soy el dinosaurio de<br>Google atentamente Marcos :c</p>
    <div id="game">
      <div id="dinosaur"></div>
    </div>
    <p id="score">Score: 0</p>

    <script>
      let dinosaur = document.getElementById("dinosaur");
      let game = document.getElementById("game");
      let obstacles = [];
      let meteor = document.getElementById("meteor");
      let score = 0;

      function addObstacle() {
        let obstacle = document.createElement("div");
        obstacle.className = "obstacle";
        obstacle.style.left = game.offsetWidth + "px";
        game.appendChild(obstacle);
        obstacles.push(obstacle);
      }

      function updateObstacles() {
        for (let i = 0; i < obstacles.length; i++) {
          obstacles[i].style.left = obstacles[i].offsetLeft - 5 + "px";

          if (obstacles[i].offsetLeft < -obstacles[i].offsetWidth) {
            game.removeChild(obstacles[i]);
            obstacles.splice(i, 1);
            i--;
          }

          if (
            obstacles[i] &&
            obstacles[i].offsetLeft <= dinosaur.offsetLeft + dinosaur.offsetWidth &&
            obstacles[i].offsetLeft + obstacles[i].offsetWidth >= dinosaur.offsetLeft &&
            dinosaur.offsetTop + dinosaur.offsetHeight >=
              game.offsetHeight - obstacles[i].offsetHeight
          ) {
            endGame();
          }
        }

        // Check for collision with meteor
        if (
          dinosaur.offsetLeft + dinosaur.offsetWidth >= meteor.offsetLeft &&
          dinosaur.offsetLeft <= meteor.offsetLeft + meteor.offsetWidth &&
          dinosaur.offsetTop + dinosaur.offsetHeight >= meteor.offsetTop
        ) {
          endGame();
        }

        score++;
        document.getElementById("score").innerHTML = "Score: " + score;
      }

      let gameLoop = setInterval(function() {
        updateObstacles();
      }, 20);

      function endGame() {
        clearInterval(gameLoop);
        alert("Game over!");
      }

      function jump(height) {
        if (dinosaur.classList.contains("jump")) {
          return;
        }
        dinosaur.classList.add("jump");
        let jumpTime = height / 2;
        let jumpUp = setInterval(function() {
          let dinosaurBottom = parseInt(getComputedStyle(dinosaur).getPropertyValue("bottom"));
          if (dinosaurBottom >= height) {
            clearInterval(jumpUp);
            let fallDown = setInterval(function() {
              let dinosaurBottom = parseInt(getComputedStyle(dinosaur).getPropertyValue("bottom"));
              if (dinosaurBottom <= 0) {
                clearInterval(fallDown);
                dinosaur.classList.remove("jump");
              } else {
                dinosaur.style.bottom = (dinosaurBottom - 5) + "px";
              }
            }, 20);
          } else {
            dinosaur.style.bottom = (dinosaurBottom + 5) + "px";
          }
        }, jumpTime/15);
      }

      document.addEventListener("touchstart", function(event) {
        jump(80);
      });

      setInterval(function() {
        addObstacle();
      }, 3000);
    </script>
  </body>
</html>
