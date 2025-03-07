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

        .menu {
            display: none;
            position: absolute;
            right: 10px;
            top: 50px;
            background: #1b3a57;
            padding: 10px;
            border-radius: 5px;
        }

        .menu a {
            display: block;
            color: white;
            text-decoration: none;
            padding: 8px 0;
        }

        .menu a:hover {
            background: #3a4f69;
        }

        /* Hero Section */
        .hero {
            padding: 40px 20px;
        }

        .hero h2 {
            font-size: 28px;
            margin-bottom: 10px;
        }

        .hero p {
            font-size: 18px;
            line-height: 1.6;
        }

        /* Flashcard Flip Animation */
        .flashcard-container {
            perspective: 1000px;
            display: flex;
            justify-content: center;
            margin: 20px 0;
        }

        .flashcard {
            width: 200px;
            height: 120px;
            text-align: center;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s;
        }

        .flashcard:hover {
            transform: rotateY(180deg);
        }

        .flashcard-front, .flashcard-back {
            width: 100%;
            height: 100%;
            position: absolute;
            backface-visibility: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            font-weight: bold;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .flashcard-front {
            background: #1abc9c;
            color: white;
        }

        .flashcard-back {
            background: #f39c12;
            color: white;
            transform: rotateY(180deg);
        }

        /* Marquee Section */
        .marquee-container {
            overflow: hidden;
            white-space: nowrap;
            width: 100%;
            margin: 30px 0;
        }

        .marquee {
            display: inline-block;
            animation: marquee 10s linear infinite;
        }

        .marquee img {
            width: 120px;
            height: auto;
            margin: 0 15px;
            border-radius: 10px;
        }

        @keyframes marquee {
            from { transform: translateX(100%); }
            to { transform: translateX(-100%); }
        }

        /* Start Button */
        .start-btn {
            padding: 12px 25px;
            font-size: 20px;
            color: white;
            background: #ff5733;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: 0.3s;
        }

        .start-btn:hover {
            background: #e64c2c;
        }

        /* Benefits Section */
        .benefits {
            background: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            margin: 20px;
        }

        .benefits h3 {
            font-size: 22px;
            color: #ffcc00;
        }

        .benefits p {
            font-size: 16px;
            line-height: 1.5;
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
    <div class="menu" id="menu">
        <a href="#">About</a>
        <a href="#">Login</a>
    </div>

    <!-- Hero Section -->
    <div class="hero">
        <h2>UPPSC Flashcard & Quiz</h2>
        <p>इस ऐप के माध्यम से आप हर दिन 50-60 नए शब्द और महत्वपूर्ण करंट अफेयर्स आसानी से सीख सकते हैं। यह विशेष रूप से UPPSC RO/ARO सहित अन्य प्रतियोगी परीक्षाओं की तैयारी करने वाले छात्रों के लिए उपयोगी है। नियमित अभ्यास से आपकी शब्दावली मजबूत होगी, करंट अफेयर्स पर पकड़ बनेगी, और परीक्षा में बेहतर अंक प्राप्त करने में सहायता मिलेगी।</p>
    </div>

    <!-- Flashcard Section -->
    <div class="flashcard-container">
        <div class="flashcard">
            <div class="flashcard-front">अनुकरण</div>
            <div class="flashcard-back">Imitation</div>
        </div>
    </div>

    <!-- Marquee Book Covers -->
    <div class="marquee-container">
        <div class="marquee">
            <img src="https://raw.githubusercontent.com/prakashsharma19/entry/main/image.png" alt="Book 1">
            <img src="https://raw.githubusercontent.com/prakashsharma19/entry/main/hardev.png" alt="Book 2">
        </div>
    </div>

    <!-- Start Button -->
    <button class="start-btn">शुरू करें</button>

    <!-- Benefits Section -->
    <div class="benefits">
        <h3>फ्लैशकार्ड के फायदे</h3>
        <p>याद करने में आसानी – फ्लैशकार्ड तकनीक से शब्दों और जानकारी को जल्दी और लंबे समय तक याद रखा जा सकता है।...</p>
    </div>

    <!-- Footer -->
    <div class="footer">
        <p>© 2025 UPPSC Flashcard & Quiz | Contact Us</p>
    </div>

    <script>
        function toggleMenu() {
            var menu = document.getElementById("menu");
            menu.style.display = menu.style.display === "block" ? "none" : "block";
        }
    </script>

</body>
</html>
