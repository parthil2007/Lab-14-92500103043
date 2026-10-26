<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Brain Test Quiz</title>

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
          rel="stylesheet">

    <style>
        body {
            background: linear-gradient(135deg, #e7d8c9, #c8ad91);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: Arial, sans-serif;
        }

        .quiz-box {
            background: #fffaf3;
            width: 100%;
            max-width: 600px;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px #4e342e30;
            border-top: 5px solid #795548;
        }

        .option-btn {
            width: 100%;
            text-align: left;
            margin: 8px 0;
            padding: 12px;
            border: 1px solid #d6c2ad;
            border-radius: 8px;
            background: #fffdf9;
            color: #4e342e;
        }

        .option-btn:hover:not(:disabled) {
            background: #efebe5;
            border-color: #795548;
        }

        .correct {
            background: #d1fae5;
            border-color: #059669;
        }

        .wrong {
            background: #fee2e2;
            border-color: #dc2626;
        }

        .btn-next {
            background: #795548;
            color: white;
        }

        .btn-next:hover {
            background: #4e342e;
            color: white;
        }
    </style>
</head>

<body>
    <div class="container p-3">
        <div class="quiz-box mx-auto">

            <h2 class="text-center fw-bold mb-2" style="color:#5d4037;">
                ☕ Brain Test Quiz
            </h2>

            <p class="text-center text-muted">
                Think smart. Test your knowledge!
            </p>

            <div id="quizArea">
                <div class="d-flex justify-content-between mb-2">
                    <span id="questionNumber">Question 1 of 5</span>
                    <span>Score: <b id="score">0</b></span>
                </div>

                <div class="progress mb-4">
                    <div class="progress-bar" id="progressBar"
                         style="width:20%; background:#795548;">
                    </div>
                </div>

                <h5 id="question" class="fw-bold mb-3"></h5>
                <div id="options"></div>

                <p id="feedback" class="mt-3 fw-bold"></p>

                <button id="nextBtn" class="btn btn-next w-100 mt-2"
                        onclick="nextQuestion()" disabled>
                    Next Question
                </button>
            </div>

            <div id="resultArea" class="text-center" style="display:none;">
                <h2 class="fw-bold" style="color:#795548;">
                    Quiz Completed!
                </h2>

                <h4 class="mt-4">Your Final Score</h4>
                <h1 id="finalScore" class="fw-bold" style="color:#5d4037;"></h1>
                <p id="resultMessage" class="fs-5"></p>

                <button class="btn btn-next w-100 mt-3" onclick="restartQuiz()">
                    Try Again
                </button>
            </div>

        </div>
    </div>



    <script>
const questions = [
    {
        question: "Which language is used to structure a webpage?",
        options: ["CSS", "HTML", "Java", "Python"],
        answer: 1
    },
    {
        question: "Which language is used to style a webpage?",
        options: ["HTML", "C", "CSS", "SQL"],
        answer: 2
    },
    {
        question: "Which keyword declares a variable in JavaScript?",
        options: ["let", "print", "display", "echo"],
        answer: 0
    },
    {
        question: "Which company developed Java?",
        options: ["Microsoft", "Google", "Sun Microsystems", "Apple"],
        answer: 2
    },
    {
        question: "Which Bootstrap class creates a primary button?",
        options: ["btn-danger", "btn-primary", "btn-success", "btn-warning"],
        answer: 1
    }
];

let currentQuestion = 0;
let score = 0;
let answered = false;

function showQuestion() {
    answered = false;

    const q = questions[currentQuestion];

    document.getElementById("questionNumber").textContent =
        `Question ${currentQuestion + 1} of ${questions.length}`;

    document.getElementById("score").textContent = score;
    document.getElementById("question").textContent = q.question;

    document.getElementById("progressBar").style.width =
        ((currentQuestion + 1) / questions.length * 100) + "%";

    document.getElementById("feedback").textContent = "";
    document.getElementById("nextBtn").disabled = true;
    document.getElementById("nextBtn").textContent = "Next Question";

    const optionsDiv = document.getElementById("options");
    optionsDiv.innerHTML = "";

    q.options.forEach((option, index) => {
        const button = document.createElement("button");

        button.textContent = String.fromCharCode(65 + index) + ". " + option;
        button.className = "option-btn";
        button.onclick = () => checkAnswer(index, button);

        optionsDiv.appendChild(button);
    });
}

function checkAnswer(selected, button) {
    if (answered) return;

    answered = true;

    const correct = questions[currentQuestion].answer;
    const buttons = document.querySelectorAll(".option-btn");

    buttons.forEach(btn => btn.disabled = true);

    if (selected === correct) {
        score++;
        button.classList.add("correct");

        document.getElementById("feedback").textContent = "Correct answer!";
        document.getElementById("feedback").style.color = "green";
    } else {
        button.classList.add("wrong");
        buttons[correct].classList.add("correct");

        document.getElementById("feedback").textContent =
            "Wrong answer! The correct answer is " +
            questions[currentQuestion].options[correct] + ".";

        document.getElementById("feedback").style.color = "red";
    }

    document.getElementById("score").textContent = score;
    document.getElementById("nextBtn").disabled = false;

    if (currentQuestion === questions.length - 1) {
        document.getElementById("nextBtn").textContent = "Show Final Result";
    }
}

function nextQuestion() {
    if (!answered) return;

    currentQuestion++;

    if (currentQuestion < questions.length) {
        showQuestion();
    } else {
        showResult();
    }
}

function showResult() {
    document.getElementById("quizArea").style.display = "none";
    document.getElementById("resultArea").style.display = "block";

    document.getElementById("finalScore").textContent =
        score + " / " + questions.length;

    const percentage = (score / questions.length) * 100;
    let message = "";

    if (percentage === 100) {
        message = "Excellent! You got every answer correct!";
    } else if (percentage >= 60) {
        message = "Good job! Keep learning and improving.";
    } else {
        message = "Keep practicing! You can try again.";
    }

    document.getElementById("resultMessage").textContent = message;
}

function restartQuiz() {
    currentQuestion = 0;
    score = 0;

    document.getElementById("quizArea").style.display = "block";
    document.getElementById("resultArea").style.display = "none";

    showQuestion();
}

showQuestion();
</script>

</body>
</html>
