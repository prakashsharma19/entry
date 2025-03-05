<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard Learning</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Hind:wght@500&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Hind', Arial, sans-serif;
            background: linear-gradient(to bottom, #4a90e2, #000000);
            text-align: center;
            color: white;
            margin: 0;
            padding: 20px;
        }

        .container {
            width: 90%;
            max-width: 700px;
            margin: auto;
        }

        h1 {
            font-size: 32px;
            margin-bottom: 10px;
        }

        p {
            font-size: 18px;
            margin-bottom: 20px;
        }

        .card-box {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            position: relative;
        }

        .flashcard-container {
            width: 100%;
            height: 200px;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
            position: relative;
        }

        .flashcard {
            width: 250px;
            height: 200px;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s;
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
            font-size: 22px;
            font-weight: bold;
            text-align: center;
            color: white;
            font-family: 'Hind', Arial, sans-serif;
            background: #ff9800;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            padding: 10px;
        }

        .flashcard-back {
            background: #009688;
            transform: rotateY(180deg);
        }

        .hand-animation {
            position: absolute;
            width: 80px;
            height: 80px;
            bottom: -50px;
            left: 50%;
            transform: translateX(-50%);
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
        </div>
    </div>

    <script>
        let flashcards = [
            {
                id: "flashcard1",
                words: [
                    { front: "अतिथि शब्द का पर्यायवाची है", back: "अभ्यागत, आगुन्तक, पाहुन, मेहमान, गृहागत" },
                    { front: "जंगल शब्द का पर्यायवाची है", back: "दाव, अरण्य, कांतार, विपिन, अटवी, कानन, वन, बयाबान" },
                    { front: "वैमनस्य", back: "सौहार्द" },
                    { front: "ह्रस्व", back: "दीर्घ" },
                    { front: "व्यष्टि", back: "समष्टि" }
                ]
            }
        ];

        flashcards.forEach((cardData) => {
            let card = document.getElementById(cardData.id);
            let frontFace = card.querySelector(".flashcard-front");
            let backFace = card.querySelector(".flashcard-back");
            let index = 0;

            function updateCardText() {
                frontFace.innerText = cardData.words[index].front;
                backFace.innerText = cardData.words[index].back;
            }

            updateCardText();

            card.addEventListener("click", function () {
                card.classList.toggle("flip");
            });

            setInterval(() => {
                card.classList.remove("flip");
                setTimeout(() => {
                    index = (index + 1) % cardData.words.length;
                    updateCardText();
                }, 300);
            }, 4000);
        });

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
