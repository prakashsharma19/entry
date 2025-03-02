<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Quiz App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #4a90e2;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            display: flex;
            gap: 20px;
        }

        .card-box {
            width: 260px;
            height: 200px;
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            color: white;
            position: relative;
        }

        /* Flashcard Inside "Flashcard Learning" */
        .flashcard-container {
            width: 100%;
            height: 100px;
            perspective: 1000px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .flashcard {
            width: 90%;
            height: 100%;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 1s;
        }

        .flashcard.flip {
            transform: rotateY(180deg);
        }

        .flashcard-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 20px;
            font-weight: bold;
            color: white;
            background: #ff9800;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .flashcard-back {
            background: #009688;
            transform: rotateY(180deg);
        }

        /* Other Card Styles */
        .other-card {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            text-align: center;
            color: white;
        }
    </style>
</head>
<body>

    <div class="container">
        <!-- Flashcard Learning Box -->
        <div class="card-box">
            <h3>Flashcard Learning</h3>
            <p>Memorize concepts easily with interactive flashcards.</p>
            <div class="flashcard-container">
                <div class="flashcard" id="flashcard">
                    <div class="flashcard-face flashcard-front">अंगीकरण</div>
                    <div class="flashcard-face flashcard-back">अनंगीकरण</div>
                </div>
            </div>
        </div>

        <!-- Interactive Quizzes Box -->
        <div class="card-box">
            <h3>Interactive Quizzes</h3>
            <p>Test your knowledge and track your progress.</p>
        </div>

        <!-- Daily Current Affairs Box -->
        <div class="card-box">
            <h3>Daily Current Affairs</h3>
            <p>Stay updated with the latest UPPSC current affairs.</p>
        </div>
    </div>

    <script>
        let card = document.getElementById("flashcard");
        let words = [
            { front: "अंगीकरण", back: "अनंगीकरण" },
            { front: "अत्यधिक", back: "अत्यल्प" }
        ];
        let index = 0;
        let isFlipped = false;

        function flipCard() {
            card.classList.toggle("flip");
            isFlipped = !isFlipped;
            
            setTimeout(() => {
                index = (index + 1) % words.length;
                if (!isFlipped) {
                    card.querySelector(".flashcard-front").innerText = words[index].front;
                    card.querySelector(".flashcard-back").innerText = words[index].back;
                }
            }, 500);
        }

        setInterval(flipCard, 2000);
    </script>

</body>
</html>
