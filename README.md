<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fireworks</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>
  <style>
    body {
      margin: 0;
      background: radial-gradient(circle, #020111, #000000);
      overflow: hidden;
      height: 100vh;
      cursor: pointer;
      font-family: "Arial", sans-serif;
      color: white;
      text-align: center;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
    }

    .instructions {
      font-size: 2rem;
      font-weight: bold;
      z-index: 10;
      color: #ff9671;
      text-shadow: 1px -1px 7px rgb(142 142 142 / 80%);
    }

    .projectile {
      position: absolute;
      width: 12px;
      height: 12px;
      background: white;
      border-radius: 50%;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.8),
        0 0 30px rgba(255, 255, 255, 0.6), 0 0 40px rgba(255, 255, 255, 0.4);
    }

    .particule {
      position: absolute;
      font-size: 2rem;
      font-family: "Cursive", serif;
      font-weight: bold;
      color: white;
      transform: scale(0);
      opacity: 0;
    }

    .sparkle {
      position: absolute;
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: white;
      transform: scale(0);
      opacity: 0;
    }
  </style>
</head>
<body>
  <div class="instructions">Click the Firework!</div>
  <script>
    const colors = [
      "#ff6f91",
      "#ff9671",
      "#ffc75f",
      "#f9f871",
      "#ff4c4c",
      "#ffcc00"
    ];
    const letters = "I LOVE YOU";
    let letterIndex = 0;

    function getRandomLetter() {
      const letter = letters.charAt(letterIndex);
      letterIndex = (letterIndex + 1) % letters.length;
      return letter;
    }

    function createFirework(x, y) {
      const launchHeight =
        Math.random() * (window.innerHeight / 4) + window.innerHeight / 4;
      const projectile = document.createElement("div");
      projectile.classList.add("projectile");
      document.body.appendChild(projectile);
      projectile.style.left = `${x}px`;
      projectile.style.top = `${y}px`;

      anime({
        targets: projectile,
        translateY: -launchHeight,
        duration: 1200,
        easing: "easeOutQuad",
        complete: () => {
          projectile.remove();
          createBurst(x, y - launchHeight);
        }
      });
    }

    function createBurst(x, y) {
      const numLetters = 15;
      const numSparkles = 50;

      // Letters
      for (let i = 0; i < numLetters; i++) {
        createParticle(x, y, false);
      }

      // Sparkles
      for (let i = 0; i < numSparkles; i++) {
        createParticle(x, y, true);
      }
    }

    function createParticle(x, y, isSparkle) {
      const el = document.createElement("div");
      el.classList.add(isSparkle ? "sparkle" : "particule");
      const instruction = document.querySelector('.instructions').style.display = 'none';

      if (!isSparkle) {
        el.textContent = getRandomLetter();
        el.style.color = colors[Math.floor(Math.random() * colors.length)];
      } else {
        el.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
      }

      el.style.left = `${x}px`;
      el.style.top = `${y}px`;
      document.body.appendChild(el);

      animateParticle(el, isSparkle);
    }

    function animateParticle(el, isSparkle) {
      const angle = Math.random() * Math.PI * 2;
      const distance = anime.random(100, 200);
      const duration = anime.random(1200, 2000);
      const fallDistance = anime.random(20, 80);
      const scale = isSparkle ? Math.random() * 0.5 + 0.5 : Math.random() * 1 + 0.5;

      anime
        .timeline({
          targets: el,
          easing: "easeOutCubic",
          duration: duration,
          complete: () => el.remove()
        })
        .add({
          translateX: Math.cos(angle) * distance,
          translateY: Math.sin(angle) * distance,
          scale: [0, scale],
          opacity: [1, 0.9]
        })
        .add({
          translateY: `+=${fallDistance}px`,
          opacity: [0.9, 0],
          easing: "easeInCubic",
          duration: duration / 2
        });
    }

    document.addEventListener("click", (e) => {
      createFirework(e.clientX, e.clientY);
    });

    window.onload = function () {
      const centerX = window.innerWidth / 2;
      const centerY = window.innerHeight / 2;
      createFirework(centerX, centerY);
    };
  </script>
</body>
</html>
