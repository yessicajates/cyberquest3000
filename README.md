# cyberquest3000
STEM day activity

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>CyberQuest 3000</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background-color: #1e1e2f; color: #fff; }
    .hidden { display: none; }
    .container { max-width: 600px; margin: 0 auto; }
    button { padding: 10px 20px; margin-top: 10px; cursor: pointer; background-color: #3c99dc; border: none; color: white; border-radius: 5px; margin-right: 10px; }
    .timer { font-size: 24px; margin: 10px 0; color: #ffcc00; }
    input { padding: 8px; width: 100%; margin-top: 5px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>CyberQuest 3000</h1>
    <p id="welcome">You are trapped in a computer game! Solve all puzzles to escape.</p>
    <div class="timer" id="timer">Time Left: 45:00</div>
    <div id="score">Score: 0</div>
    <div id="puzzle"></div>
    <input type="text" id="answer" class="hidden" placeholder="Enter your answer" />
    <button onclick="submitAnswer()" class="hidden" id="submitBtn">Submit</button>
    <button onclick="getHint()" class="hidden" id="hintBtn">Hint (-2 mins)</button>
    <button onclick="startGame()" id="startBtn">Start Game</button>
    <p id="feedback"></p>
  </div>

  <script>
    const puzzles = [
      { q: "Level 1: Bug Hunt - How many mistakes are in the code?", a: "5", hint: "Look for logic and syntax issues." },
      { q: "Level 2: Pixel Puzzle - What letter is revealed?", a: "T", hint: "Check the coordinates on the grid." },
      { q: "Level 3: Encryption - What is the hidden number?", a: "2", hint: "Reverse the Caesar cipher." },
      { q: "Level 4: Logic Gate Lock - How many TRUE outputs?", a: "1", hint: "Think about AND/OR gate truth tables." },
      { q: "Level 5: Number Dungeon - Which prime fits the riddle?", a: "13", hint: "It's between 10 and 20." },
      { q: "Level 6: Data Mine - Average coins collected?", a: "25", hint: "Use the mean formula." },
      { q: "Final Portal - Enter the code (e.g. 1-2-3-4-5-7)", a: "1-2-3-4-5-7", hint: "Sort the numbers you've used." }
    ];

    let current = 0;
    let timeLeft = 45 * 60;
    let timerInterval;
    let score = 0;

    function startGame() {
      document.getElementById("startBtn").classList.add("hidden");
      document.getElementById("answer").classList.remove("hidden");
      document.getElementById("submitBtn").classList.remove("hidden");
      document.getElementById("hintBtn").classList.remove("hidden");
      nextPuzzle();
      timerInterval = setInterval(updateTimer, 1000);
    }

    function updateTimer() {
      timeLeft--;
      const mins = Math.floor(timeLeft / 60);
      const secs = timeLeft % 60;
      document.getElementById("timer").textContent = `Time Left: ${String(mins).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
      if (timeLeft <= 0) {
        clearInterval(timerInterval);
        document.getElementById("puzzle").textContent = "Time is up! The portal has closed!";
        document.getElementById("answer").classList.add("hidden");
        document.getElementById("submitBtn").classList.add("hidden");
        document.getElementById("hintBtn").classList.add("hidden");
      }
    }

    function nextPuzzle() {
      if (current < puzzles.length) {
        document.getElementById("puzzle").textContent = puzzles[current].q;
        document.getElementById("answer").value = "";
        document.getElementById("feedback").textContent = "";
        updateScore();
      } else {
        clearInterval(timerInterval);
        document.getElementById("puzzle").textContent = "Congratulations! You escaped the game!";
        document.getElementById("answer").classList.add("hidden");
        document.getElementById("submitBtn").classList.add("hidden");
        document.getElementById("hintBtn").classList.add("hidden");
      }
    }

    function submitAnswer() {
      const userAnswer = document.getElementById("answer").value.trim().toUpperCase();
      const correctAnswer = puzzles[current].a.toUpperCase();
      if (userAnswer === correctAnswer) {
        score += 10;
        current++;
        nextPuzzle();
      } else {
        document.getElementById("feedback").textContent = "Incorrect! Try again or ask for a hint.";
      }
    }

    function getHint() {
      if (timeLeft > 120) {
        timeLeft -= 120;
        document.getElementById("feedback").textContent = `Hint: ${puzzles[current].hint}`;
      } else {
        document.getElementById("feedback").textContent = "Not enough time left for a hint!";
      }
    }

    function updateScore() {
      document.getElementById("score").textContent = `Score: ${score}`;
    }
  </script>
</body>
</html>

