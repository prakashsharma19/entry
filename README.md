<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard & Quiz</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(to bottom, #1E3C72, #2A5298);
            color: white;
            text-align: center;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            background: #16213E;
        }

        .header h1 {
            font-size: 22px;
            margin: 0;
            color: #fff;
            padding-left: 10px;
        }

        .menu-icon {
            font-size: 24px;
            cursor: pointer;
            padding-right: 10px;
        }

        .hero {
            background: #1E3C72;
            color: white;
            text-align: left;
            padding: 30px 20px;
        }

        .hero h2 {
            font-size: 26px;
            color: #ffcc00;
            margin-bottom: 15px;
        }

        .hero p {
            font-size: 18px;
            line-height: 1.6;
        }

        .start-btn {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background: #ffcc00;
            color: black;
            font-size: 18px;
            font-weight: bold;
            border: none;
            cursor: pointer;
            border-radius: 5px;
        }

        .flashcard-container {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-top: 20px;
            perspective: 1000px;
            position: relative;
        }

        .flashcard {
            width: 180px;
            height: 100px;
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

    <div class="header">
        <h1>UPPSC Flashcard & Quiz</h1>
        <i class="fa fa-bars menu-icon"></i>
    </div>

    <div class="hero">
        <h2>UPPSC Flashcard & Quiz</h2>
        <p>इस ऐप के माध्यम से आप हर दिन 50-60 नए शब्द और महत्वपूर्ण करंट अफेयर्स आसानी से सीख सकते हैं।</p>
        <button class="start-btn">शुरू करें</button>
    </div>

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
