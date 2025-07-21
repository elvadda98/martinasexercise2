<!DOCTYPE html>
<html>
<head>
    <title>Vocabulary Typing Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 700px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f9fc;
            line-height: 1.6;
        }
        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 10px;
        }
        .word-bank {
            background-color: #e8f4f8;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            text-align: center;
        }
        .word-bank span {
            display: inline-block;
            background-color: #3498db;
            color: white;
            padding: 5px 10px;
            margin: 5px;
            border-radius: 4px;
            font-weight: bold;
        }
        .sentence {
            background-color: white;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 15px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            position: relative;
        }
        input {
            padding: 8px;
            border: 2px solid #3498db;
            border-radius: 4px;
            width: 120px;
            font-size: 16px;
        }
        .check-btn {
            background-color: #2ecc71;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            margin-left: 10px;
        }
        .check-btn:hover {
            background-color: #27ae60;
        }
        .hint-btn {
            background-color: #f39c12;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
            margin-left: 10px;
        }
        .hint-btn:hover {
            background-color: #e67e22;
        }
        .feedback {
            margin-top: 8px;
            font-style: italic;
            display: none;
        }
        .correct {
            color: #27ae60;
        }
        .incorrect {
            color: #e74c3c;
        }
        #score {
            text-align: center;
            font-size: 18px;
            margin: 20px 0;
            font-weight: bold;
        }
        #restart {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        #restart:hover {
            background-color: #2980b9;
        }
        .hint-text {
            color: #7f8c8d;
            font-size: 14px;
            margin-top: 5px;
            display: none;
        }
        .correct-answer {
            font-weight: bold;
            color: #27ae60;
        }
    </style>
</head>
<body>
    <h1>✍️ Vocabulary Typing Challenge</h1>
    
    <div class="word-bank">
        <h3>Word Bank:</h3>
        <span>catch up</span>
        <span>thousand</span>
        <span>steps</span>
        <span>cave</span>
        <span>usually</span>
        <span>rare</span>
        <span>despite</span>
    </div>
    
    <div id="score">Score: 0/20</div>
    
    <div id="sentences-container">
        <!-- Sentences will be added by JavaScript -->
    </div>
    
    <button id="restart">Restart Game</button>

    <script>
        // Vocabulary words and sentences
        const wordBank = ["catch up", "thousand", "steps", "cave", "usually", "rare", "despite"];
        
        const sentences = [
            { text: "I take about ten ______ to walk from my bed to the bathroom.", answer: "steps", hint: "What do you walk with?" },
            { text: "______ the cold weather, we decided to go camping.", answer: "despite", hint: "Means 'even though'" },
            { text: "Blue diamonds are extremely ______ and valuable.", answer: "rare", hint: "Opposite of common" },
            { text: "The hikers explored a dark ______ in the mountains.", answer: "cave", hint: "Where bats live" },
            { text: "She ______ drinks coffee in the morning.", answer: "usually", hint: "Means 'most often'" },
            { text: "After vacation, I had to ______ on all my missed work.", answer: "catch up", hint: "What you do when behind" },
            { text: "The book cost over a ______ dollars!", answer: "thousand", hint: "1,000" },
            { text: "He counted every ______ as he climbed the tower.", answer: "step", hint: "Part of walking" },
            { text: "______ feeling tired, she finished the marathon.", answer: "despite", hint: "Similar to 'although'" },
            { text: "Finding a seashell this big is ______.", answer: "rare", hint: "Not common" },
            { text: "Bats ______ live in dark places like caves.", answer: "usually", hint: "Most of the time" },
            { text: "We need to ______ with the group ahead of us.", answer: "catch up", hint: "Reach others" },
            { text: "The museum had over a ______ artifacts.", answer: "thousand", hint: "Number after 999" },
            { text: "The ______ walls were covered in ancient paintings.", answer: "cave", hint: "Underground space" },
            { text: "______ her fear of spiders, she entered the basement.", answer: "despite", hint: "Means 'regardless of'" },
            { text: "It's ______ to see snow in this warm climate.", answer: "rare", hint: "Unusual" },
            { text: "I ______ wake up at 7 AM on weekdays.", answer: "usually", hint: "My normal routine" },
            { text: "Run faster to ______ to the leader!", answer: "catch up", hint: "What you do when behind" },
            { text: "The recipe requires a ______ grains of salt.", answer: "thousand", hint: "1,000" },
            { text: "Watch your ______ on these slippery stairs.", answer: "steps", hint: "What you take when walking" }
        ];

        let score = 0;
        
        // Initialize the game
        function initGame() {
            score = 0;
            document.getElementById("score").textContent = "Score: 0/20";
            const container = document.getElementById("sentences-container");
            container.innerHTML = "";
            
            // Create sentence elements
            sentences.forEach((sentence, index) => {
                const sentenceDiv = document.createElement("div");
                sentenceDiv.className = "sentence";
                
                const parts = sentence.text.split("______");
                
                sentenceDiv.innerHTML = `
                    <p>${index + 1}. ${parts[0]}<input type="text" id="answer-${index}" data-answer="${sentence.answer.toLowerCase()}">${parts[1]}</p>
                    <button class="check-btn" onclick="checkAnswer(${index})">Check</button>
                    <button class="hint-btn" onclick="showHint(${index})">Hint</button>
                    <div class="hint-text" id="hint-${index}">${sentence.hint}</div>
                    <div class="feedback" id="feedback-${index}"></div>
                `;
                
                container.appendChild(sentenceDiv);
            });
        }
        
        // Show hint
        function showHint(index) {
            const hint = document.getElementById(`hint-${index}`);
            hint.style.display = "block";
        }
        
        // Check answer
        function checkAnswer(index) {
            const input = document.getElementById(`answer-${index}`);
            const feedback = document.getElementById(`feedback-${index}`);
            const userAnswer = input.value.trim().toLowerCase();
            const correctAnswer = input.dataset.answer;
            
            if (userAnswer === correctAnswer) {
                feedback.innerHTML = "✓ <span class='correct-answer'>Correct!</span>";
                feedback.className = "feedback correct";
                feedback.style.display = "block";
                input.style.borderColor = "#27ae60";
                score++;
                document.getElementById("score").textContent = `Score: ${score}/20`;
            } else {
                feedback.innerHTML = `✗ Incorrect. The correct answer is: <span class='correct-answer'>${correctAnswer}</span>`;
                feedback.className = "feedback incorrect";
                feedback.style.display = "block";
                input.style.borderColor = "#e74c3c";
            }
        }
        
        // Restart game
        document.getElementById("restart").onclick = initGame;
        
        // Start the game
        initGame();
    </script>
</body>
</html>
