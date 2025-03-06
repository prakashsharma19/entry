<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard Learning</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        /* General Styles */
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(to bottom, #4a90e2, #000000);
            color: white;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            width: 90%;
            max-width: 1200px;
            text-align: center;
            padding: 20px;
        }

        h1 {
            font-size: 36px;
            font-weight: bold;
            margin-bottom: 10px;
            color: #ffffff;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        p {
            font-size: 18px;
            color: #e0e0e0;
            margin-bottom: 30px;
        }

        .card-box {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            margin-bottom: 30px;
        }

        .card-box h3 {
            font-size: 24px;
            margin-bottom: 20px;
            color: #ffffff;
        }

        /* Flashcard Container */
        .flashcard-container {
            width: 100%;
            height: 400px;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
            position: relative;
        }

        .flashcard {
            width: 300px;
            height: 300px;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 1s;
            cursor: pointer;
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
            font-size: 24px;
            font-weight: bold;
            text-align: center;
            color: white;
            background: #ff9800;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
            padding: 20px;
        }

        .flashcard-back {
            background: #009688;
            transform: rotateY(180deg);
        }

        /* Hand Animation */
        .hand-animation {
            position: absolute;
            width: 100px;
            height: 100px;
            bottom: -50px;
            left: 50%;
            transform: translateX(-50%);
        }

        /* Button Styles */
        .controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }

        .controls button {
            background: #ff9800;
            border: none;
            border-radius: 8px;
            padding: 10px 20px;
            font-size: 16px;
            color: white;
            cursor: pointer;
            transition: background 0.3s ease;
        }

        .controls button:hover {
            background: #e68900;
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            h1 {
                font-size: 28px;
            }

            p {
                font-size: 16px;
            }

            .flashcard {
                width: 250px;
                height: 250px;
            }

            .flashcard-face {
                font-size: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Flashcard Learning</h1>
        <p>Memorize concepts easily with interactive flashcards.</p>

        <div class="card-box">
            <h3>Flashcard Learning</h3>
            <div class="flashcard-container">
                <div class="flashcard" id="flashcard1">
                    <div class="flashcard-face flashcard-front"></div>
                    <div class="flashcard-face flashcard-back"></div>
                </div>
                <div class="hand-animation" id="hand1"></div>
            </div>
            <div class="controls">
                <button id="prevBtn">Previous</button>
                <button id="nextBtn">Next</button>
            </div>
        </div>
    </div>

    <script>
        // Hand Animation
        function loadHandAnimation(id) {
            return lottie.loadAnimation({
                container: document.getElementById(id),
                renderer: "svg",
                loop: true,
                autoplay: true,
                path: "https://raw.githubusercontent.com/prakashsharma19/entry/main/Animation%20-%201740910253032.json"
            });
        }

        let hand1 = loadHandAnimation("hand1");
        hand1.setSpeed(0.3);
    </script>
</body>
</html>