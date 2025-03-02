<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcard Flip with Hand Animation</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #3498db;
        }
        .flashcard-container {
            position: relative;
            width: 200px;
            height: 120px;
            perspective: 1000px;
        }
        .flashcard {
            width: 100%;
            height: 100%;
            position: absolute;
            transform-style: preserve-3d;
            transition: transform 0.8s;
        }
        .flashcard.flipped {
            transform: rotateY(180deg);
        }
        .flashcard .front, .flashcard .back {
            position: absolute;
            width: 100%;
            height: 100%;
            background: white;
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 20px;
            font-weight: bold;
            backface-visibility: hidden;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        .flashcard .back {
            background: #2ecc71;
            transform: rotateY(180deg);
        }
        #fingerAnimation {
            position: absolute;
            width: 80px;
            height: 80px;
            bottom: -40px;
            left: 50%;
            transform: translateX(-50%);
        }
    </style>
</head>
<body>

    <div class="flashcard-container">
        <div class="flashcard" id="flashcard">
            <div class="front">अंगीकरण</div>
            <div class="back">अनंगीकरण</div>
        </div>
        <div id="fingerAnimation"></div>
    </div>

    <script>
        // Load Lottie Animation (Hand Clicking)
        const animation = lottie.loadAnimation({
            container: document.getElementById("fingerAnimation"),
            renderer: "svg",
            loop: true,
            autoplay: true,
            path: "https://raw.githubusercontent.com/prakashsharma19/entry/main/Animation%20-%201740910253032.json"
        });

        // Auto Flip Flashcard Every 2 Seconds
        let isFlipped = false;
        setInterval(() => {
            const flashcard = document.getElementById("flashcard");
            isFlipped = !isFlipped;
            flashcard.classList.toggle("flipped", isFlipped);
        }, 2000);
    </script>

</body>
</html>
