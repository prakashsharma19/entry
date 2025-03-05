<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard Learning</title>
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

        .container {
            width: 90%;
            max-width: 600px;
            margin: auto;
        }

        h1 {
            font-size: 26px;
            margin-bottom: 10px;
        }

        p {
            font-size: 16px;
            margin-bottom: 20px;
        }

        .card-box {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            position: relative;
        }

        /* Flashcard Styling */
        .flashcard-container {
            width: 100%;
            height: 150px;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
            position: relative;
        }

        .flashcard {
            width: 150px;
            height: 150px;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.3s ease-in-out;
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
            border-radius: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
            padding: 10px;
            text-align: center;
        }

        .flashcard-back {
            background: #009688;
            transform: rotateY(180deg);
        }

        /* Lottie Hand Animation */
        .hand-animation {
            position: absolute;
            width: 80px;
            height: 80px;
            bottom: -30px;
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
            let front = card.querySelector(".flashcard-front");
            let back = card.querySelector(".flashcard-back");
            let index = 0;
            let isFlipped = false;

            function updateCard() {
                front.innerText = cardData.words[index].front;
                back.innerText = cardData.words[index].back;
            }

            updateCard();

            function flipCard() {
                card.classList.add("flip");
                isFlipped = true;

                setTimeout(() => {
                    card.classList.remove("flip");
                    isFlipped = false;
                    index = (index + 1) % cardData.words.length;
                    updateCard();
                }, 2000); // Card stays flipped for 2 seconds
            }

            // Load Lottie Hand Animation and sync with flip
            function loadHandAnimation(id) {
                let animation = lottie.loadAnimation({
                    container: document.getElementById(id),
                    renderer: "svg",
                    loop: true,
                    autoplay: true,
                    path: "https://raw.githubusercontent.com/prakashsharma19/entry/main/Animation%20-%201740910253032.json"
                });

                animation.setSpeed(0.4); // Slowing down hand animation

                setInterval(() => {
                    flipCard();
                }, 3000); // Flip every 3 seconds in sync with the hand click
            }

            loadHandAnimation("hand1");
        });

    </script>

</body>
</html>
