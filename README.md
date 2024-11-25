<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Finance Simulator Game</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #F0F8FF;
            color: #333;
            margin: 0;
            padding: 0;
        }

        #game-container {
            display: none;
            background-color: #FFFBF0;
            padding: 20px;
            border-radius: 10px;
            width: 80%;
            margin: auto;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        #intro {
            text-align: center;
            padding: 50px;
            background-color: #FFD700;
            border-radius: 10px;
            width: 60%;
            margin: auto;
        }

        h1 {
            color: #0066cc;
        }

        h2 {
            font-size: 1.5em;
        }

        #city {
            width: 100%;
            height: 300px;
            background: url('https://img.freepik.com/free-vector/flat-cityscape-illustration_23-2148784730.jpg') no-repeat center center;
            background-size: cover;
            animation: city-animation 5s infinite alternate;
        }

        #mother {
            position: absolute;
            bottom: 50px;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 150px;
            background-color: #F08080;
            border-radius: 10px;
            display: none;
        }

        #popup {
            position: fixed;
            top: 20%;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 20px;
            border-radius: 10px;
            display: none;
        }

        #next-button {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            font-size: 1.2em;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        #next-button:hover {
            background-color: #45a049;
        }

        @keyframes city-animation {
            0% { background-position: 0 0; }
            100% { background-position: 100% 0; }
        }
    </style>
</head>
<body>

    <!-- Intro Section -->
    <div id="intro">
        <h1>Welcome to the Finance Simulator!</h1>
        <p>Enter your name to start your financial adventure.</p>
        <input type="text" id="username" placeholder="Enter your name" />
        <button onclick="startGame()">Start Game</button>
    </div>

    <!-- Game Section -->
    <div id="game-container">
        <h2 id="game-title">Stage 1: Budgeting</h2>
        <div id="city"></div>
        <div id="mother"></div>

        <div id="story-content"></div>
        <button id="next-button" onclick="nextStage()">Next</button>
    </div>

    <!-- Popup for Financial Terms -->
    <div id="popup">
        <p id="popup-text"></p>
        <button onclick="closePopup()">Close</button>
    </div>

    <script>
        let playerName = "";
        let currentStage = 0;
        let money = 1000;  // Player's starting money

        const stages = [
            {
                title: "Welcome to the Game!",
                content: `Hi, ${playerName}! You’re about to embark on a financial adventure. Your goal is to manage your money wisely and learn key financial concepts to achieve financial success.`,
                next: "Your mother gives you your first money. Let’s start learning about budgeting!"
            },
            {
                title: "Stage 1: Budgeting",
                content: `Your mother gives you $500. Now, you have to make decisions on how to spend it. You have to pay for rent, groceries, and entertainment. How will you budget?`,
                next: "You have spent $200 on rent and $100 on groceries. How much money is left? Is it enough to save?",
                popups: [
                    {
                        term: "Budgeting",
                        definition: "Budgeting is the process of creating a plan to spend your money wisely. It helps you track your expenses and savings."
                    }
                ],
                question: "How much money do you have left after paying for rent and groceries? (Type the number)",
                answer: 200
            },
            {
                title: "Stage 2: Saving",
                content: "Now that you have a basic budget, you need to save some of your remaining money for the future. How much would you save?",
                next: "You decided to save $100. Let’s see what happens next!",
                popups: [
                    {
                        term: "Saving",
                        definition: "Saving is putting money aside for future use. It helps you prepare for emergencies or big purchases."
                    }
                ],
                question: "How much money did you save? (Type the number)",
                answer: 100
            },
            {
                title: "Stage 3: Investing",
                content: "Now you’ve saved some money! It’s time to invest. Will you invest in stocks, bonds, or a business? Make your choice wisely!",
                next: "You invested in stocks and made a profit! Let’s see how much you earned.",
                popups: [
                    {
                        term: "Investing",
                        definition: "Investing is using money to make more money. This can include stocks, bonds, and real estate."
                    }
                ],
                question: "How much profit did you make from your investment? (Type the number)",
                answer: 150
            },
            {
                title: "Stage 4: Debt Management",
                content: "Uh-oh! You had an emergency and had to borrow money to cover it. Now you need to manage your debt. How will you pay it off?",
                next: "You paid off the debt on time! Good job!",
                popups: [
                    {
                        term: "Debt Management",
                        definition: "Debt management is about managing borrowed money responsibly, including paying back loans on time and avoiding high-interest debt."
                    }
                ],
                question: "How much of your debt did you manage to pay off? (Type the number)",
                answer: 500
            },
            {
                title: "Stage 5: Financial Independence",
                content: "You’ve learned the key financial concepts. Now it’s time to become financially independent. Will you retire early or start a business?",
                next: "You chose to retire early and enjoy your life. Congratulations!",
                popups: [
                    {
                        term: "Financial Independence",
                        definition: "Financial independence means having enough income from your investments to cover your living expenses without relying on a job."
                    }
                ],
                question: "What will you choose to do? (Type 'retire' or 'business')",
                answer: "retire"
            }
        ];

        function startGame() {
            playerName = document.getElementById("username").value;
            if (playerName) {
                document.getElementById("intro").style.display = "none";
                document.getElementById("game-container").style.display = "block";
                document.getElementById("game-title").innerText = `Stage 1: Budgeting`;
                document.getElementById("mother").style.display = "block";
                loadStage(currentStage);
            } else {
                alert("Please enter your name!");
            }
        }

        function loadStage(stageIndex) {
            const stage = stages[stageIndex];
            document.getElementById("story-content").innerHTML = `
                <h3>${stage.title}</h3>
                <p>${stage.content}</p>
            `;
            if (stage.popups) {
                stage.popups.forEach(popup => {
                    showPopup(popup);
                });
            }
        }

        function showPopup(popup) {
            const popupElement = document.getElementById("popup");
            document.getElementById("popup-text").innerText = `${popup.term}: ${popup.definition}`;
            popupElement.style.display = "block";
        }

        function closePopup() {
            document.getElementById("popup").style.display = "none";
        }

        function nextStage() {
            const stage = stages[currentStage];
            const playerAnswer = prompt(stage.question);
            if (parseInt(playerAnswer) === stage.answer || playerAnswer.toLowerCase() === stage.answer.toLowerCase()) {
                alert("Correct! You've completed this stage.");
                currentStage++;
                if (currentStage < stages.length) {
                    loadStage(currentStage);
                } else {
                    alert("Congratulations! You've completed all the stages.");
                }
            } else {
                alert("Incorrect answer. Try again.");
            }
        }
    </script>

</body>
</html>
