<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard Learning</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(to bottom, #283048, #859398); /* Professional gradient */
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
            font-size: 24px;
            margin-bottom: 15px;
        }

        p {
            font-size: 16px;
            margin-bottom: 20px;
        }

        .card-box {
            background: rgba(255, 255, 255, 0.15); /* Glass effect */
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 15px;
            position: relative;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
        }

        /* Flashcard Container */
        .flashcard-container {
            width: 100%;
            height: 120px;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
            position: relative;
        }

        .flashcard {
            width: 220px;
            height: 100px;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.8s ease-in-out;
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
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }

        .flashcard-back {
            background: #009688;
            transform: rotateY(180deg);
        }

        /* Lottie Hand Animation */
        #fingerAnimation {
            position: absolute;
            width: 60px;
            height: 60px;
            bottom: -35px;
            left: 50%;
            transform: translateX(-50%);
        }

    </style>
</head>
<body>

    <div class="container">
        <h1>Flashcard Learning</h1>
        <p>Memorize concepts easily with interactive flashcards.</p>

        <!-- Flashcard Learning Box -->
        <div class="card-box">
            <h3>Interactive Flashcard</h3>
            <div class="flashcard-container">
                <div class="flashcard" id="flashcard">
                    <div class="flashcard-face flashcard-front">अंगीकरण</div>
                    <div class="flashcard-face flashcard-back">अनंगीकरण</div>
                </div>
                <div id="fingerAnimation"></div>
            </div>
        </div>
    </div>

    <script>
        let card = document.getElementById("flashcard");
        let words = [
            { front: "अंगीकरण", back: "अनंगीकरण" },
            { front: "अत्यधिक", back: "अत्यल्प" },
            { front: "ज्ञान", back: "अज्ञान" }
        ];
        let index = 0;
        let isFlipped = false;

        function flipCard() {
            card.classList.toggle("flip");
            isFlipped = !isFlipped;

            setTimeout(() => {
                if (!isFlipped) {
                    index = (index + 1) % words.length;
                    card.querySelector(".flashcard-front").innerText = words[index].front;
                    card.querySelector(".flashcard-back").innerText = words[index].back;
                }
            }, 500);
        }

        // Auto flip every 2.5 seconds
        setInterval(flipCard, 2500);

        // Load Lottie Hand Animation
        let animation = lottie.loadAnimation({
            container: document.getElementById("fingerAnimation"),
            renderer: "svg",
            loop: true,
            autoplay: true,
            path: "https://raw.githubusercontent.com/prakashsharma19/entry/main/Animation%20-%201740910253032.json"
        });

        animation.setSpeed(0.7); // Slower animation speed
    </script>

</body>
</html>
