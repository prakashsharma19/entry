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
            margin: 0;
            padding: 0;
            background: linear-gradient(to bottom, #4a90e2, #000000);
            color: white;
            line-height: 1.6;
        }

        a {
            color: white;
            text-decoration: none;
        }

        a:hover {
            text-decoration: underline;
        }

        /* Header */
        header {
            background: rgba(0, 0, 0, 0.7);
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 1000;
        }

        header h1 {
            margin: 0;
            font-size: 24px;
        }

        nav ul {
            list-style: none;
            margin: 0;
            padding: 0;
            display: flex;
        }

        nav ul li {
            margin-left: 20px;
        }

        nav ul li a {
            font-size: 16px;
        }

        /* Main Content */
        .container {
            width: 90%;
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }

        h2 {
            font-size: 22px;
            margin-bottom: 20px;
        }

        .card-box {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        /* Flashcard Container */
        .flashcard-container {
            width: 100%;
            height: 220px;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
            position: relative;
        }

        .flashcard {
            width: 200px;
            height: 200px;
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
        .hand-animation {
            position: absolute;
            width: 50px;
            height: 50px;
            bottom: -20px;
            left: 50%;
            transform: translateX(-50%);
        }

        /* Footer */
        footer {
            background: rgba(0, 0, 0, 0.7);
            text-align: center;
            padding: 10px 0;
            margin-top: 40px;
        }

        footer p {
            margin: 0;
            font-size: 14px;
        }

        /* Mobile Responsiveness */
        @media (max-width: 768px) {
            header {
                flex-direction: column;
                text-align: center;
            }

            nav ul {
                flex-direction: column;
                margin-top: 10px;
            }

            nav ul li {
                margin: 10px 0;
            }

            .flashcard {
                width: 150px;
                height: 150px;
            }

            .flashcard-face {
                font-size: 18px;
            }
        }
    </style>
</head>
<body>

    <!-- Header -->
    <header>
        <h1>Flashcard Learning</h1>
        <nav>
            <ul>
                <li><a href="#">Home</a></li>
                <li><a href="#">About</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </nav>
    </header>

    <!-- Main Content -->
    <div class="container">
        <h2>Memorize Concepts Easily</h2>

        <!-- Flashcard Learning Box -->
        <div class="card-box">
            <h3>Flashcard Learning</h3>
            <div class="flashcard-container">
                <div class="flashcard" id="flashcard1">
                    <div class="flashcard-face flashcard-front">अंगीकरण</div>
                    <div class="flashcard-face flashcard-back">अनंगीकरण</div>
                </div>
                <div class="hand-animation" id="hand1"></div>
            </div>
        </div>

        <!-- Synonym Learning Box -->
        <div class="card-box">
            <h3>Synonym Learning</h3>
            <div class="flashcard-container">
                <div class="flashcard" id="flashcard2">
                    <div class="flashcard-face flashcard-front">विशाल</div>
                    <div class="flashcard-face flashcard-back">विराट</div>
                </div>
                <div class="hand-animation" id="hand2"></div>
            </div>
        </div>

        <!-- Current Affairs Box -->
        <div class="card-box">
            <h3>Current Affairs</h3>
            <div class="flashcard-container">
                <div class="flashcard" id="flashcard3">
                    <div class="flashcard-face flashcard-front">G20 2023 का अध्यक्ष कौन था?</div>
                    <div class="flashcard-face flashcard-back">भारत</div>
                </div>
                <div class="hand-animation" id="hand3"></div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer>
        <p>&copy; 2023 Flashcard Learning. All rights reserved.</p>
    </footer>

    <script>
        let flashcards = [
            {
                id: "flashcard1",
                words: [
                    { front: "अंगीकरण", back: "अनंगीकरण" },
                    { front: "न्याय", back: "अन्याय" },
                    { front: "वैमनस्य शब्द का विलोम है", back: "सौहार्द" },
                    { front: "ह्रस्व शब्द का विलोम है", back: "दीर्घ" },
                    { front: "व्यष्टि शब्द का विलोम है", back: "समष्टि" }
                ]
            },
            {
                id: "flashcard2",
                words: [
                    { front: "विशाल", back: "विराट" },
                    { front: "तेज", back: "गति" },
                    { front: "अतिथि शब्द का पर्यायवाची है", back: "अभ्यागत, आगुन्तक, पाहुन, मेहमान, गृहागत" },
                    { front: "जंगल शब्द का पर्यायवाची है", back: "दाव, अरण्य, कांतार, विपिन, अटवी, कानन, वन, बयाबान" }
                ]
            },
            {
                id: "flashcard3",
                words: [
                    { front: "G20 2023 का अध्यक्ष कौन था?", back: "भारत" },
                    { front: "यूनेस्को मुख्यालय कहाँ है?", back: "पेरिस" }
                ]
            }
        ];

        flashcards.forEach((cardData) => {
            let card = document.getElementById(cardData.id);
            let index = 0;
            let isFlipped = false;

            function flipCard() {
                card.classList.toggle("flip");
                isFlipped = !isFlipped;

                setTimeout(() => {
                    index = (index + 1) % cardData.words.length;
                    if (!isFlipped) {
                        card.querySelector(".flashcard-front").innerText = cardData.words[index].front;
                        card.querySelector(".flashcard-back").innerText = cardData.words[index].back;
                    }
                }, 500);
            }

            setInterval(flipCard, 3000);
        });

        // Load Lottie Hand Animation for each card
        function loadHandAnimation(id) {
            return lottie.loadAnimation({
                container: document.getElementById(id),
                renderer: "svg",
                loop: true,
                autoplay: true,
                path: "https://raw.githubusercontent.com/prakashsharma19/entry/main/Animation%20-%201740910253032.json"
            });
        }

        loadHandAnimation("hand1").setSpeed(0.5);
        loadHandAnimation("hand2").setSpeed(0.5);
        loadHandAnimation("hand3").setSpeed(0.5);

    </script>

</body>
</html>