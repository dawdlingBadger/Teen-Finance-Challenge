# Teen Budgeting Challenge
This is an interactive game for teenagers to learn about budgeting and finance.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Teen Budgeting Challenge</title>
    <style>
        /* Basic styling */
        body {
            font-family: Arial, sans-serif;
            background-color: #b3e3f6; /* Light blue background */
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }

        /* Background money pattern */
        body::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url('/path/to/background-image.png'); /* Replace with actual path to background image */
            background-size: cover;
            opacity: 0.3;
            z-index: -1;
        }

        /* Centered container */
        .container {
            text-align: center;
            width: 60%;
            max-width: 500px;
            padding: 20px;
            border-radius: 10px;
            background-color: #d9ead3; /* Light green for entry box */
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.2);
        }

        /* Username input box */
        .username-box {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
        }

        .username-box input {
            font-size: 1.2em;
            padding: 10px;
            border: none;
            border-radius: 5px;
            outline: none;
            width: 100%;
            margin-left: 10px;
        }

        /* Start button */
        .start-button {
            padding: 15px 30px;
            font-size: 1.5em;
            color: #fff;
            background-color: #8e7cc3; /* Purple button */
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        /* Game area */
        .game-level {
            display: none;
        }

        .mcq {
            margin: 20px 0;
        }

        .mcq input {
            margin-right: 10px;
        }

        .progress {
            font-size: 1.2em;
            color: #333;
        }

        /* Level complete button */
        .next-level {
            padding: 10px 20px;
            font-size: 1.2em;
            color: #fff;
            background-color: #8e7cc3;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Username Input Section -->
        <div class="username-box">
            <img src="user-icon.png" alt="User Icon" width="40">
            <input type="text" id="username" placeholder="Type your username">
        </div>
        <button class="start-button" onclick="startGame()">Start</button>

        <!-- Game Levels -->
        <div id="game-levels">
            <div class="game-level" id="level1">
                <h2>Level 1: Budget Basics</h2>
                <p>Answer 20 questions to unlock the next level!</p>
                <div id="mcq-container1" class="mcq"></div>
                <button class="next-level" onclick="nextLevel(2)">Next Level</button>
            </div>
            <!-- Additional levels here -->
        </div>

        <div class="progress">Progress: <span id="level-progress">0</span>%</div>
    </div>

    <script>
        let currentLevel = 1;
        let questionsAnswered = 0;
        const totalQuestions = 20;

        function startGame() {
            document.querySelector('.container').style.display = 'none';
            document.getElementById('game-levels').style.display = 'block';
            loadMCQs(1);
        }

        function loadMCQs(level) {
            const container = document.getElementById(`mcq-container${level}`);
            container.innerHTML = '';
            for (let i = 1; i <= totalQuestions; i++) {
                container.innerHTML += `
                    <div class="mcq">
                        <label>Question ${i} (Placeholder for level ${level}):</label>
                        <input type="radio" name="q${i}" value="A"> A<br>
                        <input type="radio" name="q${i}" value="B"> B<br>
                        <input type="radio" name="q${i}" value="C"> C<br>
                    </div>
                `;
            }
            document.getElementById(`level${level}`).style.display = 'block';
        }

        function nextLevel(nextLevel) {
            if (questionsAnswered >= totalQuestions) {
                document.getElementById(`level${currentLevel}`).style.display = 'none';
                currentLevel = nextLevel;
                questionsAnswered = 0;
                document.getElementById('level-progress').innerText = (currentLevel - 1) * 20;
                if (document.getElementById(`level${nextLevel}`)) {
                    loadMCQs(nextLevel);
                } else {
                    alert("Congratulations! You've completed the game.");
                }
            } else {
                alert("Complete all questions before advancing!");
            }
        }

        // Dummy MCQ answer check (for demo purposes)
        document.addEventListener('input', function(e) {
            if (e.target.type === 'radio') {
                questionsAnswered++;
                if (questionsAnswered === totalQuestions) {
                    document.querySelector('.next-level').style.display = 'inline';
                }
            }
        });
    </script>
</body>
</html>
