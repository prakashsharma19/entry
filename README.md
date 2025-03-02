<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcard Flip with Finger Click Animation</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(to bottom, #3a7bd5, #00d2ff);
            text-align: center;
            color: white;
            padding: 20px;
        }
        .container {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            flex-wrap: wrap;
            margin-top: 50px;
        }
        .card {
            width: 250px;
            height: 150px;
            background: rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(10px);
            border-radius: 10px;
            padding: 20px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        .flashcard-container {
            position: relative;
            width: 200px;
            height: 100px;
            perspective: 1000px;
        }
        .flashcard {
            width: 100%;
            height: 100%;
            position: absolute;
            backface-visibility: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 20px;
            font-weight: bold;
            background: #00897b;
            color: white;
            border-radius: 10px;
            transition: transform 0.6s;
        }
        .flashcard.back {
            background: #d32f2f;
            transform: rotateY(180deg);
        }
        .flip {
            transform: rotateY(180deg);
        }
        .finger-animation {
            position: absolute;
            bottom: -40px;
            left: 50%;
            transform: translateX(-50%);
            width: 50px;
            height: 50px;
        }
    </style>
</head>
<body>

    <h1>UPPSC Hindi & Current Affairs Quiz App</h1>
    <p>Master UPPSC Hindi with 3000+ words and stay updated with daily Current Affairs using flashcards & quizzes.</p>

    <div class="container">
        <!-- Flashcard Learning Box -->
        <div class="card">
            <h3>Flashcard Learning</h3>
            <p>Memorize concepts easily with interactive flashcards.</p>
            <div class="flashcard-container">
                <div class="flashcard front">अंगीकरण</div>
                <div class="flashcard back">अनंगीकरण</div>
                <div class="finger-animation" id="fingerAnimation"></div>
            </div>
        </div>

        <!-- Interactive Quizzes Box -->
        <div class="card">
            <h3>Interactive Quizzes</h3>
            <p>Test your knowledge and track your progress.</p>
        </div>

        <!-- Daily Current Affairs Box -->
        <div class="card">
            <h3>Daily Current Affairs</h3>
            <p>Stay updated with the latest UPPSC current affairs.</p>
        </div>
    </div>

    <script>
        const frontCard = document.querySelector('.flashcard.front');
        const backCard = document.querySelector('.flashcard.back');
        let flipped = false;

        function flipCard() {
            if (flipped) {
                frontCard.style.transform = "rotateY(0deg)";
                backCard.style.transform = "rotateY(180deg)";
            } else {
                frontCard.style.transform = "rotateY(180deg)";
                backCard.style.transform = "rotateY(360deg)";
            }
            flipped = !flipped;
        }

        // Load Finger Click Animation
        const animation = lottie.loadAnimation({
            container: document.getElementById("fingerAnimation"),
            renderer: "svg",
            loop: true,
            autoplay: true,
            path: "Animation - 1740910253032.json" // Your JSON animation file
        });

        // Flip the card every 2.5 seconds
        setInterval(flipCard, 2500);
    </script>

</body>
</html>
