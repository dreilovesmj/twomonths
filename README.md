[index.html](https://github.com/user-attachments/files/34452576/index.html)
<!doctype html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Aventura Romántica de Aniversario</title>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&amp;family=Fraunces:wght@500;600;700&amp;display=swap" rel="stylesheet">
  <style>
    :root {
      --base: #F2EDE4;
      --deep: #6C548C;
      --accent: #7158A6;
      --pale: #E3DCF2;
      --bright: #C1B3F2;
    }
    * { box-sizing: border-box; }
    html, body { margin: 0; width: 100%; min-height: 100vh; }
    body { font-family: 'DM Sans', sans-serif; color: var(--deep); background: var(--base); }
    button { cursor: pointer; }
    button:focus-visible { outline: 3px solid var(--accent); outline-offset: 4px; }
    
    .screen {
      display: none;
      min-height: 100vh;
      width: 100%;
      padding: clamp(20px, 5vw, 64px);
      align-items: center;
      justify-content: center;
      position: relative;
      overflow: hidden;
    }
    .screen.active { display: flex; animation: arrive .5s ease both; }
    @keyframes arrive { from { opacity: 0; transform: translateY(12px); } to { opacity: 1; transform: translateY(0); } }
    .screen:before {
      content: "";
      position: absolute;
      width: 55vw; height: 55vw;
      border: 1px solid var(--pale);
      border-radius: 50%;
      top: -32vw; right: -15vw;
      pointer-events: none;
    }
    .inner { position: relative; width: 100%; max-width: 850px; margin: auto; text-align: center; }
    h1, h2 { font-family: 'Fraunces', serif; line-height: 1.12; }
    h1 { font-size: clamp(2.5rem, 7vw, 5rem) !important; }
    h2 { font-size: clamp(2rem, 5vw, 3.6rem) !important; }
    .eyebrow { letter-spacing: .24em; text-transform: uppercase; font-size: .76rem; font-weight: 700; }
    .subtitle { line-height: 1.7; max-width: 620px; margin: 18px auto 26px; }
    
    .primary, .secondary, .option, .direction, .level {
      border-radius: 999px;
      min-height: 48px;
      padding: 12px 26px;
      font-weight: 700;
      transition: transform .18s, background .18s;
    }
    .primary { background: var(--accent); color: var(--base); border: 2px solid var(--accent); }
    .primary:hover, .secondary:hover, .option:hover, .direction:hover, .level:hover { transform: translateY(-2px); }
    .secondary, .option, .direction, .level { background: var(--pale); color: var(--deep); border: 2px solid var(--bright); }
    .primary:disabled, .option:disabled { cursor: not-allowed; opacity: .65; }
    
    .photo-placeholder {
      border: 2px dashed var(--accent);
      background: var(--pale);
      color: var(--deep);
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 20px;
      font-weight: 600;
    }
    .full-placeholder { position: absolute; inset: 0; width: 100%; height: 100%; border: 0; border-radius: 0; }
    .image-screen:before { display: none; }
    .image-screen .inner { z-index: 1; padding-top: min(40vw, 280px); }
    .image-screen .image-overlay { position: absolute; inset: 0; background: linear-gradient(0deg, var(--base), transparent 70%); pointer-events: none; }
    
    .panel { background: var(--base); border: 1px solid var(--bright); box-shadow: 12px 12px 0 var(--pale); border-radius: 26px; padding: clamp(20px, 4vw, 42px); }
    .choices { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 12px; max-width: 620px; margin: 28px auto; }
    .option { border-radius: 16px; width: 100%; text-align: left; }
    .error-box { margin: 20px auto 0; padding: 20px; border-radius: 18px; background: var(--pale); max-width: 490px; }
    
    .levels { display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; margin: 22px 0; }
    .level[aria-pressed="true"] { background: var(--accent); color: var(--base); border-color: var(--accent); }
    
    .memory-grid { display: grid; grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 10px; max-width: 570px; margin: 20px auto; }
    .memory-card {
      height: clamp(70px, 12vw, 112px);
      border: 2px solid var(--accent);
      border-radius: 12px;
      background: var(--pale);
      color: var(--deep);
      font-family: 'Fraunces', serif;
      font-size: clamp(1.5rem, 4vw, 2.6rem);
      font-weight: 700;
      transition: transform .25s, background .25s;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .memory-card.flipped, .memory-card.matched { background: var(--bright); }
    .memory-card.matched { opacity: .65; cursor: default; }
    
    .status { min-height: 32px; font-weight: 700; margin: 12px auto; }
    iframe { max-width: 570px; display: block; margin: 20px auto; border-radius: 12px; }
    .hidden { display: none !important; }
    
    @media(max-width: 580px) {
      .screen { padding: 20px 16px; }
      .choices { grid-template-columns: 1fr; }
      .panel { box-shadow: 6px 6px 0 var(--pale); }
      .image-screen .inner { padding-top: 45vw; }
    }
  </style>
</head>
<body class="w-full">
  <main class="w-full">
    
    <!-- PANTALLA 1: INICIO -->
    <section id="screen-1" class="screen image-screen active" aria-label="Inicio">
      <div class="photo-placeholder full-placeholder">Espacio para imagen de portada</div>
      <div class="image-overlay"></div>
      <div class="inner">
        <p class="eyebrow">UNA AVENTURA PARA NOSOTRAS</p>
        <h1 class="my-5">Nuestra historia, otra vez</h1>
        <button class="primary btn-next" type="button" data-next="2">Comenzar</button>
      </div>
    </section>

    <!-- PANTALLA 2: PLAYLIST -->
    <section id="screen-2" class="screen" aria-label="Playlist">
      <div class="inner">
        <p class="eyebrow">ANTES DE EMPEZAR</p>
        <h2 class="mt-4">Ponle música a este momento</h2>
        <p class="subtitle">Este juego está acompañado de una playlist especial para nosotras. Dale al play y sigamos juntas.</p>
        
        <iframe src="https://open.spotify.com/embed/playlist/43e8RIaxnRQkmiBvEyk5ui?utm_source=generator" width="100%" height="352" frameborder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy" title="Playlist especial de Spotify"></iframe>
        
        <div style="margin-bottom: 20px;">
          <a href="https://open.spotify.com/playlist/43e8RIaxnRQkmiBvEyk5ui" target="_blank" rel="noopener noreferrer" class="secondary" style="display: inline-block; text-decoration: none;">
            🎵 Abrir Playlist en App de Spotify
          </a>
        </div>

        <button class="primary btn-next" type="button" data-next="3">Continuar</button>
      </div>
    </section>

    <!-- PANTALLA 3: CUESTIONARIO -->
    <section id="screen-3" class="screen" aria-label="Cuestionario">
      <div class="inner panel">
        <p class="eyebrow">RECUERDOS COMPARTIDOS</p>
        <h2 class="mt-3">¿Qué tanto recuerdas?</h2>
        <p id="question-progress" class="status">Pregunta 1 de 3</p>

        <!-- Pregunta 1 -->
        <div id="question-1" class="question-block">
          <h3 class="text-xl font-bold">¿Cuándo nos conocimos?</h3>
          <div class="choices">
            <button class="option" type="button" data-correct="false">6 de agosto</button>
            <button class="option" type="button" data-correct="false">6 de septiembre</button>
            <button class="option" type="button" data-correct="true">7 de agosto</button>
            <button class="option" type="button" data-correct="false">5 de agosto</button>
          </div>
        </div>

        <!-- Pregunta 2 -->
        <div id="question-2" class="question-block hidden">
          <h3 class="text-xl font-bold">¿Dónde fue nuestra primera cita?</h3>
          <div class="choices">
            <button class="option" type="button" data-correct="false">En el MALI</button>
            <button class="option" type="button" data-correct="true">En el MAC</button>
            <button class="option" type="button" data-correct="false">En el malecón</button>
            <button class="option" type="button" data-correct="false">En la playa</button>
          </div>
        </div>

        <!-- Pregunta 3 -->
        <div id="question-3" class="question-block hidden">
          <h3 class="text-xl font-bold">¿Cuál fue la primera comida que compartimos?</h3>
          <div class="choices">
            <button class="option" type="button" data-correct="true">Pizza</button>
            <button class="option" type="button" data-correct="false">Hamburguesa</button>
            <button class="option" type="button" data-correct="false">Sushi</button>
            <button class="option" type="button" data-correct="false">Tacos</button>
          </div>
        </div>

        <!-- Mensaje de Error -->
        <div id="quiz-error" class="error-box hidden">
          <p class="font-bold">¡Ups! Esa no es la respuesta correcta.</p>
          <button id="btn-retry-quiz" class="secondary mt-3" type="button">Reintentar</button>
        </div>

        <button id="quiz-next-screen" class="primary btn-next hidden mt-4" type="button" data-next="4">Siguiente Juego</button>
      </div>
    </section>

    <!-- PANTALLA 4: JUEGO DE MEMORIA -->
    <section id="screen-4" class="screen" aria-label="Juego de Memoria">
      <div class="inner panel">
        <p class="eyebrow">DESAFÍO DE MEMORIA</p>
        <h2 class="mt-3">Encuentra las parejas</h2>
        
        <div class="levels">
          <button class="level" type="button" data-level="easy" aria-pressed="true">Fácil (4)</button>
          <button class="level" type="button" data-level="medium" aria-pressed="false">Medio (8)</button>
          <button class="level" type="button" data-level="hard" aria-pressed="false">Difícil (12)</button>
        </div>

        <button id="btn-start-memory" class="primary mb-4" type="button">Iniciar juego</button>

        <div id="memory-board" class="memory-grid"></div>
        <p id="memory-status" class="status"></p>
      </div>
    </section>

  </main>

  <script>
    document.addEventListener('DOMContentLoaded', () => {
      // Navegación entre pantallas
      document.querySelectorAll('.btn-next').forEach(btn => {
        btn.addEventListener('click', () => {
          const nextScreenId = btn.getAttribute('data-next');
          document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
          const nextScreen = document.getElementById(`screen-${nextScreenId}`);
          if (nextScreen) nextScreen.classList.add('active');
        });
      });

      // --- LÓGICA DEL CUESTIONARIO ---
      let currentQuestion = 1;
      const totalQuestions = 3;
      const quizErrorBox = document.getElementById('quiz-error');
      const quizRetryBtn = document.getElementById('btn-retry-quiz');
      const quizNextBtn = document.getElementById('quiz-next-screen');
      const progressText = document.getElementById('question-progress');

      function setupQuestionListeners() {
        document.querySelectorAll('.question-block').forEach((qBlock) => {
          const options = qBlock.querySelectorAll('.option');
          options.forEach(opt => {
            opt.addEventListener('click', () => {
              const isCorrect = opt.getAttribute('data-correct') === 'true';
              
              // Deshabilitar temporalmente los botones de la pregunta actual
              options.forEach(b => b.disabled = true);

              if (isCorrect) {
                opt.style.background = '#86efac'; // Verde suave
                setTimeout(() => {
                  qBlock.classList.add('hidden');
                  currentQuestion++;

                  if (currentQuestion <= totalQuestions) {
                    document.getElementById(`question-${currentQuestion}`).classList.remove('hidden');
                    progressText.textContent = `Pregunta ${currentQuestion} de ${totalQuestions}`;
                  } else {
                    progressText.textContent = "¡Completaste el cuestionario!";
                    quizNextBtn.classList.remove('hidden');
                  }
                }, 700);
              } else {
                opt.style.background = '#fca5a5'; // Rojo suave
                quizErrorBox.classList.remove('hidden');
              }
            });
          });
        });
      }

      // CORRECCIÓN: Botón Reintentar habilita nuevamente las opciones
      quizRetryBtn.addEventListener('click', () => {
        quizErrorBox.classList.add('hidden');
        const activeBlock = document.getElementById(`question-${currentQuestion}`);
        if (activeBlock) {
          const options = activeBlock.querySelectorAll('.option');
          options.forEach(opt => {
            opt.disabled = false;
            opt.style.background = 'var(--pale)';
          });
        }
      });

      setupQuestionListeners();

      // --- LÓGICA DEL JUEGO DE MEMORIA ---
      const symbolsPool = ['❤️', '🌸', '✨', '🍕', '🐱', '🎵'];
      let selectedDifficulty = 'easy';
      let cards = [];
      let flippedCards = [];
      let matchedPairs = 0;
      let totalPairs = 2;

      const levelsBtns = document.querySelectorAll('.level');
      const startMemoryBtn = document.getElementById('btn-start-memory');
      const memoryBoard = document.getElementById('memory-board');
      const memoryStatus = document.getElementById('memory-status');

      // Selección de nivel
      levelsBtns.forEach(btn => {
        btn.addEventListener('click', () => {
          levelsBtns.forEach(b => b.setAttribute('aria-pressed', 'false'));
          btn.setAttribute('aria-pressed', 'true');
          selectedDifficulty = btn.getAttribute('data-level');
          initMemoryGame();
        });
      });

      startMemoryBtn.addEventListener('click', () => {
        initMemoryGame();
      });

      function initMemoryGame() {
        memoryBoard.innerHTML = '';
        flippedCards = [];
        matchedPairs = 0;
        memoryStatus.textContent = '';

        if (selectedDifficulty === 'easy') totalPairs = 2; // 4 cartas
        else if (selectedDifficulty === 'medium') totalPairs = 4; // 8 cartas
        else if (selectedDifficulty === 'hard') totalPairs = 6; // 12 cartas

        const symbols = symbolsPool.slice(0, totalPairs);
        cards = [...symbols, ...symbols].sort(() => Math.random() - 0.5);

        cards.forEach((symbol, index) => {
          const cardEl = document.createElement('button');
          cardEl.className = 'memory-card';
          cardEl.type = 'button';
          cardEl.dataset.symbol = symbol;
          cardEl.dataset.index = index;
          cardEl.textContent = '?';

          cardEl.addEventListener('click', () => handleCardClick(cardEl));
          memoryBoard.appendChild(cardEl);
        });
      }

      function handleCardClick(card) {
        if (flippedCards.length === 2 || card.classList.contains('flipped') || card.classList.contains('matched')) {
          return;
        }

        card.classList.add('flipped');
        card.textContent = card.dataset.symbol;
        flippedCards.push(card);

        if (flippedCards.length === 2) {
          const [card1, card2] = flippedCards;
          if (card1.dataset.symbol === card2.dataset.symbol) {
            card1.classList.add('matched');
            card2.classList.add('matched');
            flippedCards = [];
            matchedPairs++;

            if (matchedPairs === totalPairs) {
              memoryStatus.textContent = '🎉 ¡Felicidades! Encontraste todas las parejas.';
            }
          } else {
            setTimeout(() => {
              card1.classList.remove('flipped');
              card2.classList.remove('flipped');
              card1.textContent = '?';
              card2.textContent = '?';
              flippedCards = [];
            }, 900);
          }
        }
      }

      // Inicialización por defecto
      initMemoryGame();
    });
  </script>
</body>
</html>
