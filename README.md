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
            height: 180px;
            background: rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(10px);
            border-radius: 10px;
            padding: 20px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
        }
        /* Flashcard Flip Container */
        .flashcard-container {
            position: relative;
            width: 180px;
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
            font-size: 18px;
            font-weight: bold;
            background: #00897b;
            color: white;
            border-radius: 10px;
            transition: transform 0.6s ease-in-out;
        }
        .flashcard.back {
            background: #d32f2f;
            transform: rotateY(180deg);
        }
        .flipped .front {
            transform: rotateY(180deg);
        }
        .flipped .back {
            transform: rotateY(360deg);
        }
        /* Finger Animation */
        .finger-animation {
            position: absolute;
            bottom: -50px; /* Adjusted for visibility */
            left: 50%;
            transform: translateX(-50%);
            width: 80px; /* Increased size */
            height: 80px;
            pointer-events: none;
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
            <div class="flashcard-container" id="flashcardContainer">
                <div class="flashcard front">अंगीकरण</div>
                <div class="flashcard back">अनंगीकरण</div>
            </div>
            <div class="finger-animation" id="fingerAnimation"></div>
        </div>
    </div>

    <script>
        const flashcardContainer = document.getElementById("flashcardContainer");
        let flipped = false;

        function flipCard() {
            flipped = !flipped;
            flashcardContainer.classList.toggle("flipped", flipped);
        }

        // Load Finger Click Animation
        const animation = lottie.loadAnimation({
            container: document.getElementById("fingerAnimation"),
            renderer: "svg",
            loop: true,
            autoplay: true,
            path: "Animation - 1740910253032.json" // Make sure this file is accessible
        });

        // Debugging - Check if animation is loading
        animation.addEventListener("data_ready", function () {
            console.log("Lottie animation loaded successfully!");
        });

        animation.addEventListener("error", function () {
            console.error("Lottie animation failed to load. Check file path.");
        });

        // Simulate clicking animation every 2.5 seconds before flipping
        setInterval(() => {
            setTimeout(flipCard, 1000); // Flip the card after a short delay
        }, 2500);
    </script>

</body>
</html>
