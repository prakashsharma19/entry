<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard Learning</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(to bottom, #1e3c72, #2a5298);
            text-align: center;
            color: white;
            margin: 0;
            padding: 0;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(0, 0, 0, 0.6);
            padding: 10px 20px;
        }

        .menu-icon {
            font-size: 24px;
            cursor: pointer;
        }

        .menu {
            position: fixed;
            top: 0;
            left: -200px;
            width: 200px;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            padding-top: 60px;
            transition: 0.3s;
        }

        .menu a {
            display: block;
            color: white;
            padding: 10px;
            text-decoration: none;
        }

        .menu a:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        .container {
            width: 90%;
            max-width: 400px;
            margin: auto;
            padding: 20px;
        }

        .card-box {
            background: rgba(255, 255, 255, 0.15);
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
        }

        .flashcard-container {
            width: 100%;
            height: 100px;
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
        }

        .flashcard {
            width: 200px;
            height: 100px;
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

        .footer {
            background: rgba(0, 0, 0, 0.6);
            padding: 10px;
            position: fixed;
            width: 100%;
            bottom: 0;
        }
    </style>
</head>
<body>

    <div class="header">
        <div class="menu-icon" onclick="toggleMenu()">&#9776;</div>
        <h1>Flashcard Learning</h1>
    </div>

    <div class="menu" id="menu">
        <a href="#">About</a>
        <a href="#">Login</a>
    </div>

    <div class="container">
        <p>Memorize concepts easily with interactive flashcards.</p>
        <div class="card-box">
            <h3>Flashcard Learning</h3>
            <div class="flashcard-container">
                <div class="flashcard" id="flashcard">
                    <div class="flashcard-face flashcard-front">अंगीकरण</div>
                    <div class="flashcard-face flashcard-back">अनंगीकरण</div>
                </div>
            </div>
        </div>
    </div>

    <div class="footer">
        <p>&copy; 2025 UPPSC Flashcard Learning</p>
    </div>

    <script>
        function toggleMenu() {
            let menu = document.getElementById("menu");
            if (menu.style.left === "0px") {
                menu.style.left = "-200px";
            } else {
                menu.style.left = "0px";
            }
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
