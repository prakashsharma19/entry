<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcards & Quiz</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f7fc;
            color: #333;
        }
        header {
            position: fixed;
            top: 0;
            width: 100%;
            background: linear-gradient(90deg, #007bff, #6610f2);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
            font-size: 20px;
        }
        .menu-icon {
            font-size: 26px;
            cursor: pointer;
        }
        .menu {
            display: none;
            position: absolute;
            right: 15px;
            top: 60px;
            background: white;
            color: black;
            border-radius: 8px;
            padding: 10px;
            box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
        }
        .menu a {
            display: block;
            padding: 8px 12px;
            text-decoration: none;
            color: black;
            border-radius: 5px;
        }
        .menu a:hover {
            background-color: #007bff;
            color: white;
        }
        .hero {
            margin-top: 80px;
            text-align: center;
            padding: 60px 20px;
            background: linear-gradient(120deg, #6a11cb, #2575fc);
            color: white;
            border-radius: 0 0 50px 50px;
        }
        .hero p {
            font-size: 22px;
            max-width: 600px;
            margin: 0 auto;
            line-height: 1.6;
        }
        .flashcards {
            text-align: center;
            padding: 60px 20px;
        }
        .flashcard-container {
            width: 100%;
            max-width: 400px;
            margin: auto;
            perspective: 1000px;
            position: relative;
        }
        .flashcard {
            width: 180px;
            height: 80px;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 1s;
            margin: auto;
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
        footer {
            text-align: center;
            padding: 20px;
            background: #343a40;
            color: white;
            font-size: 14px;
            margin-top: 30px;
        }
        @media (max-width: 768px) {
            header {
                font-size: 18px;
                padding: 15px;
            }
            .hero p {
                font-size: 18px;
            }
        }
    </style>
</head>
<body>
    <header>
        <div>UPPSC Flashcards & Quiz</div>
        <div class="menu-icon" onclick="toggleMenu()">☰</div>
        <div class="menu" id="menu">
            <a href="#">About</a>
            <a href="#">Login</a>
        </div>
    </header>
    <section class="hero">
        <p>यह ऐप यूपीपीएससी आरओ/एआरओ उम्मीदवारों के लिए है जो तेजी से हिंदी सीखना चाहते हैं और शब्दों को लंबे समय तक याद रखना चाहते हैं।</p>
    </section>
    <section class="flashcards">
        <h2>Flashcard Learning</h2>
        <div class="flashcard-container">
            <div class="flashcard" id="flashcard">
                <div class="flashcard-face flashcard-front">अंगीकरण</div>
                <div class="flashcard-face flashcard-back">अनंगीकरण</div>
            </div>
            <div id="fingerAnimation"></div>
        </div>
    </section>
    <footer>
        &copy; 2025 UPPSC Flashcards & Quiz
    </footer>
    <script>
        function toggleMenu() {
            var menu = document.getElementById('menu');
            menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
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
