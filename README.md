<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard & Quiz</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(to bottom, #1e3c72, #2a5298);
            color: white;
            margin: 0;
            padding: 0;
            text-align: center;
        }
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            background: rgba(0, 0, 0, 0.3);
        }
        .menu {
            display: none;
            position: absolute;
            top: 50px;
            right: 10px;
            background: rgba(255, 255, 255, 0.9);
            padding: 10px;
            border-radius: 5px;
        }
        .menu a {
            display: block;
            padding: 8px;
            color: black;
            text-decoration: none;
        }
        .hero {
            padding: 20px;
        }
        .flashcard-container {
            margin: 20px auto;
            width: 200px;
            height: 100px;
            perspective: 1000px;
        }
        .flashcard {
            width: 100%;
            height: 100%;
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
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }
        .flashcard-back {
            background: #009688;
            transform: rotateY(180deg);
        }
        .marquee {
            white-space: nowrap;
            overflow: hidden;
            position: relative;
            margin: 20px 0;
        }
        .marquee img {
            width: 100px;
            margin: 0 10px;
            display: inline-block;
            animation: marquee 10s linear infinite;
        }
        @keyframes marquee {
            from { transform: translateX(100%); }
            to { transform: translateX(-100%); }
        }
        .benefits {
            padding: 20px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 10px;
            margin: 20px;
        }
        .footer {
            padding: 15px;
            background: rgba(0, 0, 0, 0.3);
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="header">
        <h2>UPPSC Flashcard & Quiz</h2>
        <div>
            <button onclick="toggleMenu()">☰</button>
            <div class="menu" id="menu">
                <a href="#">About</a>
                <a href="#">Login</a>
            </div>
        </div>
    </div>
    <div class="hero">
        <h1>UPPSC Flashcard & Quiz</h1>
        <p>इस ऐप के माध्यम से आप हर दिन 50-60 नए शब्द और महत्वपूर्ण करंट अफेयर्स आसानी से सीख सकते हैं।...</p>
    </div>
    <div class="flashcard-container">
        <div class="flashcard" id="flashcard">
            <div class="flashcard-face flashcard-front">अंगीकरण</div>
            <div class="flashcard-face flashcard-back">अनंगीकरण</div>
        </div>
    </div>
    <div class="marquee">
        <img src="https://github.com/prakashsharma19/entry/blob/main/image.png">
        <img src="https://github.com/prakashsharma19/entry/blob/main/hardev.png">
    </div>
    <button>शुरू करें</button>
    <div class="benefits">
        <h3>फ्लैशकार्ड के फायदे</h3>
        <p>याद करने में आसानी – फ्लैशकार्ड तकनीक से शब्दों और जानकारी को जल्दी और लंबे समय तक याद रखा जा सकता है।...</p>
    </div>
    <div class="footer">
        <p>&copy; 2025 UPPSC Flashcard & Quiz | Contact Us</p>
    </div>
    <script>
        function toggleMenu() {
            let menu = document.getElementById("menu");
            menu.style.display = menu.style.display === "block" ? "none" : "block";
        }
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
    </script>
</body>
</html>
