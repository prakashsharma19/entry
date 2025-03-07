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
            background: #1E3C72; /* Dark Blue */
            color: white;
            text-align: left;
            padding: 30px 20px;
            margin: 0; /* Edge-to-Edge */
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
            margin-top: 20px;
        }

        .flashcard {
            width: 250px;
            height: 150px;
            background: white;
            color: black;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            font-weight: bold;
            position: relative;
            cursor: pointer;
            transform-style: preserve-3d;
            transition: transform 0.6s;
            text-align: center;
            padding: 10px;
        }

        .flashcard.flip {
            transform: rotateY(180deg);
        }

        .flashcard .front, .flashcard .back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .flashcard .back {
            transform: rotateY(180deg);
            background: #FFD700;
        }

        /* Start Button */
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

        /* Benefits Section */
        .benefits {
            background: #3e4a61;
            color: white;
            padding: 20px;
            margin: 0; /* Edge-to-Edge */
            text-align: left;
        }

        .benefits h3 {
            font-size: 22px;
            color: #ffcc00;
            padding-left: 20px;
        }

        .benefits ul {
            list-style: square;
            padding-left: 40px;
        }

        .benefits ul li {
            font-size: 16px;
            line-height: 1.5;
            margin-bottom: 8px;
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
        <button class="start-btn" onclick="startFlashcards()">शुरू करें</button>
    </div>

    <!-- Flashcard Section -->
    <div class="flashcard-container">
        <div class="flashcard" onclick="flipCard()">
            <div class="front"></div>
            <div class="back"></div>
        </div>
    </div>

    <button class="start-btn" onclick="nextFlashcard()">अगला</button>

    <!-- Benefits Section -->
    <div class="benefits">
        <h3>📌 फ्लैशकार्ड के फायदे</h3>
        <ul>
            <li>📖 याद करने में आसानी – जल्दी और लंबे समय तक याद रखें।</li>
            <li>🧠 दृश्य और मानसिक जुड़ाव – स्मरण शक्ति बढ़ती है।</li>
            <li>⏳ तेज़ पुनरावृत्ति – कठिन शब्द और करंट अफेयर्स याद रहें।</li>
            <li>📱 कहीं भी, कभी भी अभ्यास – मोबाइल या डिजिटल पर पढ़ें।</li>
        </ul>
    </div>

    <!-- Footer -->
    <div class="footer">
        <p>© 2025 UPPSC Flashcard & Quiz | Contact Us</p>
    </div>

    <script>
        const flashcards = [
            { word: "न्याय", meaning: "सही और गलत का निर्णय" },
            { word: "संविधान", meaning: "देश के नियमों का संकलन" },
            { word: "प्रशासन", meaning: "शासन व्यवस्था" },
            { word: "आर्थिक", meaning: "वित्तीय स्थिति से संबंधित" },
            { word: "पर्यावरण", meaning: "प्राकृतिक परिवेश" }
        ];

        let currentIndex = 0;

        function startFlashcards() {
            currentIndex = 0;
            showFlashcard();
        }

        function showFlashcard() {
            const flashcard = document.querySelector(".flashcard");
            const front = flashcard.querySelector(".front");
            const back = flashcard.querySelector(".back");

            front.textContent = flashcards[currentIndex].word;
            back.textContent = flashcards[currentIndex].meaning;
        }

        function flipCard() {
            document.querySelector(".flashcard").classList.toggle("flip");
        }

        function nextFlashcard() {
            currentIndex = (currentIndex + 1) % flashcards.length;
            showFlashcard();
            document.querySelector(".flashcard").classList.remove("flip");
        }

        // Initialize the first flashcard
        showFlashcard();
    </script>

</body>
</html>
