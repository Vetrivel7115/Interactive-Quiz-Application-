<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Interactive Quiz App</title>
  <style>
    :root {
      --main-bg: #fefefe;
      --primary: #007bff;
      --secondary: #f5f7fa;
      --correct: #28a745;
      --wrong: #dc3545;
      --text-dark: #333;
      --shadow: rgba(0, 0, 0, 0.1);
    }

    * {
      box-sizing: border-box;
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      padding: 0;
      background: linear-gradient(135deg, #dff3ff, #fceefc);
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }

    .quiz-container {
      background-color: var(--main-bg);
      width: 90%;
      max-width: 600px;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 8px 20px var(--shadow);
      animation: fadeIn 0.6s ease;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    h1 {
      text-align: center;
      color: var(--text-dark);
      margin-bottom: 20px;
    }

    .question {
      font-size: 1.2rem;
      font-weight: 600;
      margin-bottom: 20px;
      color: var(--text-dark);
    }

    .option {
      background-color: var(--secondary);
      border: none;
      padding: 15px;
      width: 100%;
      margin: 10px 0;
      font-size: 1rem;
      border-radius: 10px;
      cursor: pointer;
      transition: 0.3s;
    }

    .option:hover {
      background-color: #e0e7ff;
    }

    .option.correct {
      background-color: var(--correct);
      color: white;
    }

    .option.wrong {
      background-color: var(--wrong);
      color: white;
    }

    #next-btn {
      background-color: var(--primary);
      color: white;
      padding: 12px 25px;
      font-size: 1rem;
      border: none;
      border-radius: 8px;
      margin-top: 20px;
      cursor: pointer;
      display: none;
      transition: background-color 0.3s;
    }

    #next-btn:hover {
      background-color: #0056b3;
    }

    #score {
      margin-top: 25px;
      font-size: 1.2rem;
      font-weight: bold;
      color: var(--text-dark);
      text-align: center;
    }

    @media screen and (max-width: 480px) {
      .quiz-container {
        padding: 20px;
      }

      .option {
        font-size: 0.95rem;
      }

      #next-btn {
        width: 100%;
      }
    }
  </style>
</head>
<body>

  <div class="quiz-container">
    <h1>Quiz App</h1>
    <div class="question" id="question"></div>
    <div id="options"></div>
    <button id="next-btn">Next</button>
    <div id="score"></div>
  </div>

  <script>
    const quizData = [
      {
        question: "What is the capital of France?",
        options: ["London", "Paris", "Rome", "Berlin"],
        answer: "Paris"
      },
      {
        question: "Which language runs in a web browser?",
        options: ["Java", "C", "Python", "JavaScript"],
        answer: "JavaScript"
      },
      {
        question: "Who is the founder of Microsoft?",
        options: ["Steve Jobs", "Bill Gates", "Elon Musk", "Jeff Bezos"],
        answer: "Bill Gates"
      }
    ];

    let currentQuestion = 0;
    let score = 0;

    const questionEl = document.getElementById("question");
    const optionsEl = document.getElementById("options");
    const nextBtn = document.getElementById("next-btn");
    const scoreEl = document.getElementById("score");

    function loadQuestion() {
      resetState();
      const current = quizData[currentQuestion];
      questionEl.innerText = `Q${currentQuestion + 1}. ${current.question}`;

      current.options.forEach(option => {
        const button = document.createElement("button");
        button.innerText = option;
        button.classList.add("option");
        button.addEventListener("click", () => selectAnswer(button, current.answer));
        optionsEl.appendChild(button);
      });
    }

    function selectAnswer(button, correctAnswer) {
      const isCorrect = button.innerText === correctAnswer;
      if (isCorrect) {
        button.classList.add("correct");
        score++;
      } else {
        button.classList.add("wrong");
        [...optionsEl.children].forEach(btn => {
          if (btn.innerText === correctAnswer) btn.classList.add("correct");
        });
      }

      Array.from(optionsEl.children).forEach(btn => btn.disabled = true);
      nextBtn.style.display = "inline-block";
    }

    function resetState() {
      optionsEl.innerHTML = "";
      nextBtn.style.display = "none";
      scoreEl.innerText = "";
    }

    nextBtn.addEventListener("click", () => {
      currentQuestion++;
      if (currentQuestion < quizData.length) {
        loadQuestion();
      } else {
        showScore();
      }
    });

    function showScore() {
      questionEl.innerText = "Quiz Completed!";
      optionsEl.innerHTML = "";
      nextBtn.style.display = "none";
      scoreEl.innerText = `🎉 Your Score: ${score} / ${quizData.length}`;
    }

    loadQuestion();
  </script>

</body>
</html>
