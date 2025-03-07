<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcard Animation</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(to bottom, #4a90e2, #000000);
            text-align: center;
            color: white;
            margin: 0;
            padding: 20px;
        }

        .flashcard-container {
            width: 100%;
            height: 100px;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
            position: relative;
        }

        .flashcard {
            width: 180px;
            height: 80px;
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

        /* Lottie Hand Animation */
        #fingerAnimation {
            position: absolute;
            width: 50px;
            height: 50px;
            bottom: -20px;
            left: 50%;
            transform: translateX(-50%);
        }
    </style>
</head>
<body>
    <h1>फ्लैशकार्ड एनिमेशन</h1>
    <div class="flashcard-container">
        <div class="flashcard" id="flashcard">
            <div class="flashcard-face flashcard-front">अंगीकरण</div>
            <div class="flashcard-face flashcard-back">अनंगीकरण</div>
        </div>
        <div id="fingerAnimation"></div>
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

        let animation = lottie.loadAnimation({
            container: document.getElementById("fingerAnimation"),
            renderer: "svg",
            loop: true,
            autoplay: true,
            path: "https://raw.githubusercontent.com/prakashsharma19/entry/main/Animation%20-%201740910253032.json"
        });
        animation.setSpeed(0.5);
    </script>
</body>
</html>
