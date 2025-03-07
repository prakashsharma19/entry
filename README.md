<!DOCTYPE html>
<html lang="hi">
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
            background: rgba(0, 0, 0, 0.7);
            padding: 10px 20px;
        }

        .menu-icon {
            font-size: 24px;
            cursor: pointer;
        }

        .menu {
            position: fixed;
            top: 0;
            right: -200px;
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

        .hero {
            padding: 50px 20px;
            background: rgba(255, 255, 255, 0.1);
        }

        .flashcard-container {
            margin: 20px auto;
        }

        .flashcard {
            width: 200px;
            height: 100px;
            transform-style: preserve-3d;
            transition: transform 1s;
            margin: auto;
        }

        .flashcard.flip {
            transform: rotateY(180deg);
        }

        .book-marquee {
            overflow: hidden;
            white-space: nowrap;
            padding: 20px 0;
        }

        .book-marquee img {
            width: 100px;
            height: auto;
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
            background: rgba(255, 255, 255, 0.2);
        }

        .footer {
            background: rgba(0, 0, 0, 0.7);
            padding: 10px;
        }
    </style>
</head>
<body>

    <div class="header">
        <h1>ऐप का नाम</h1>
        <div class="menu-icon" onclick="toggleMenu()">&#9776;</div>
    </div>

    <div class="menu" id="menu">
        <a href="#">About</a>
        <a href="#">Login</a>
    </div>

    <div class="hero">
        <h2>ऐप का विवरण</h2>
        <p>यहाँ पर ऐप की खासियतों का वर्णन करें।</p>
    </div>

    <div class="flashcard-container">
        <div class="flashcard" id="flashcard">
            <div class="flashcard-face flashcard-front">अंगीकरण</div>
            <div class="flashcard-face flashcard-back">अनंगीकरण</div>
        </div>
    </div>

    <div class="book-marquee">
        <img src="https://github.com/prakashsharma19/entry/blob/main/image.png" alt="Book Cover">
        <img src="https://github.com/prakashsharma19/entry/blob/main/image.png" alt="Book Cover">
        <img src="https://github.com/prakashsharma19/entry/blob/main/image.png" alt="Book Cover">
    </div>

    <button onclick="startApp()">शुरू करें</button>

    <div class="benefits">
        <h2>फ्लैशकार्ड के फायदे</h2>
        <p>फ्लैशकार्ड्स के माध्यम से शब्द याद रखना अधिक प्रभावी होता है...</p>
    </div>

    <div class="footer">
        <p>&copy; 2025 UPPSC Flashcard Learning | संपर्क करें</p>
    </div>

    <script>
        function toggleMenu() {
            let menu = document.getElementById("menu");
            if (menu.style.right === "0px") {
                menu.style.right = "-200px";
            } else {
                menu.style.right = "0px";
            }
        }

        let card = document.getElementById("flashcard");
        card.addEventListener("click", function() {
            card.classList.toggle("flip");
        });

        function startApp() {
            alert("ऐप शुरू हो रहा है...");
        }
    </script>
</body>
</html>
