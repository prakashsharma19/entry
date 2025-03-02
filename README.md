<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Hindi & Current Affairs Quiz App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(to bottom, #4a90e2, #000000);
            text-align: center;
            color: white;
            margin: 0;
            padding: 20px;
        }

        .container {
            width: 90%;
            max-width: 400px;
            margin: auto;
        }

        h1 {
            font-size: 22px;
            margin-bottom: 10px;
        }

        p {
            font-size: 14px;
            margin-bottom: 20px;
        }

        .card-box {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 15px;
            position: relative;
        }

        /* Flashcard Container */
        .flashcard-container {
            width: 100%;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
        }

        .flashcard {
            width: 150px;
            height: 60px;
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
            font-size: 18px;
            font-weight: bold;
            color: white;
            background: #ff9800;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
        }

        .flashcard-back {
            background: #009688;
            transform: rotateY(180deg);
        }

        /* Button Styles */
        .btn {
            display: block;
            width: 100%;
            padding: 10px;
            font-size: 16px;
            font-weight: bold;
            color: white;
            border: none;
            border-radius: 5px;
            margin-top: 10px;
            cursor: pointer;
        }

        .btn-free {
            background: #ff9800;
        }

        .btn-upgrade {
            background: #28a745;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>UPPSC Hindi & Current Affairs Quiz App</h1>
        <p>Master UPPSC Hindi with 3000+ words and stay updated with daily Current Affairs using flashcards & quizzes.</p>

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
            <p>Stay updated with latest UPPSC current affairs.</p>
        </div>

        <!-- Buttons -->
        <button class="btn btn-free">Start for Free</button>
        <button class="btn btn-upgrade">Upgrade for ₹99/year</button>
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
