<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard & Quiz</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
    <style>
        /* General Styles */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(to bottom, #1E3C72, #2A5298);
            color: white;
            text-align: center;
        }

        /* Header */
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

        /* Hero Banner */
        .hero {
            background: #1E3C72;
            color: white;
            text-align: left;
            padding: 30px 20px;
            margin: 0;
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

        /* Flashcard Section */
        .flashcard-container {
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            margin-top: 20px;
        }

        .flashcard {
            width: 200px;
            height: 250px;
            background: white;
            color: black;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 22px;
            font-weight: bold;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s;
            cursor: pointer;
        }

        .flashcard.flip {
            transform: rotateY(180deg);
        }

        .flashcard .front,
        .flashcard .back {
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            backface-visibility: hidden;
        }

        .flashcard .back {
            transform: rotateY(180deg);
            background: #ffcc00;
            color: black;
        }

        .next-btn {
            margin-top: 15px;
            padding: 10px 20px;
            background: #ffcc00;
            color: black;
            font-size: 18px;
            font-weight: bold;
            border: none;
            cursor: pointer;
            border-radius: 5px;
        }

        /* Footer */
        .footer {
            background: #0b2135;
            padding: 15px;
            margin-top: 20px;
        }

        .footer p {
            margin: 0;
            font-size: 14px;
        }
    </style>
</head>
<body>

    <!-- Header -->
    <div class="header">
        <h1>UPPSC Flashcard & Quiz</h1>
        <i class="fa fa-bars menu-icon" onclick="toggleMenu()"></i>
    </div>

    <!-- Hero Banner -->
    <div class="hero">
        <h2>UPPSC Flashcard & Quiz</h2>
        <p>इस ऐप के माध्यम से आप हर दिन 50-60 नए शब्द और महत्वपूर्ण करंट अफेयर्स आसानी से सीख सकते हैं।
        यह विशेष रूप से UPPSC RO/ARO सहित अन्य प्रतियोगी परीक्षाओं की तैयारी करने वाले छात्रों के लिए उपयोगी है।
        नियमित अभ्यास से आपकी शब्दावली मजबूत होगी, करंट अफेयर्स पर पकड़ बनेगी, और परीक्षा में बेहतर अंक प्राप्त करने में सहायता मिलेगी।</p>
        <button class="start-btn">शुरू करें</button>
    </div>

    <!-- Flashcard Section -->
    <div class="flashcard-container">
        <div class="flashcard" onclick="flipCard()">
            <div class="front" id="front-text">शब्द</div>
            <div class="back" id="back-text">अर्थ</div>
        </div>
        <button class="next-btn" onclick="nextCard()">अगला</button>
    </div>

    <!-- Footer -->
    <div class="footer">
        <p>© 2025 UPPSC Flashcard & Quiz | Contact Us</p>
    </div>

    <script>
        const flashcards = [
            { front: "सफलता", back: "Achievement" },
            { front: "ज्ञान", back: "Knowledge" },
            { front: "शिक्षा", back: "Education" },
            { front: "अनुभव", back: "Experience" }
        ];

        let currentIndex = 0;
        const frontText = document.getElementById("front-text");
        const backText = document.getElementById("back-text");
        const flashcard = document.querySelector(".flashcard");

        function flipCard() {
            flashcard.classList.toggle("flip");
        }

        function nextCard() {
            flashcard.classList.remove("flip");
            currentIndex = (currentIndex + 1) % flashcards.length;
            frontText.textContent = flashcards[currentIndex].front;
            backText.textContent = flashcards[currentIndex].back;
        }
    </script>
</body>
</html>
