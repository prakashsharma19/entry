<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Auto-Flipping Flashcard</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #4a90e2;
        }
        
        .card-container {
            position: relative;
            width: 200px;
            height: 120px;
            perspective: 1000px;
        }

        .card {
            width: 100%;
            height: 100%;
            position: absolute;
            transform-style: preserve-3d;
            transition: transform 1s;
        }

        .card.flip {
            transform: rotateY(180deg);
        }

        .card-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            font-weight: bold;
            color: white;
            background: #ff9800;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .card-back {
            background: #009688;
            transform: rotateY(180deg);
        }

        /* Animated Finger */
        .finger {
            position: absolute;
            bottom: -60px;
            left: 50%;
            transform: translateX(-50%);
            width: 40px;
            height: 40px;
            background-image: url('https://cdn-icons-png.flaticon.com/512/109/109617.png'); /* Finger pointing icon */
            background-size: cover;
            animation: tap 2s infinite;
        }

        @keyframes tap {
            0%, 100% {
                transform: translateX(-50%) translateY(0);
            }
            50% {
                transform: translateX(-50%) translateY(-10px);
            }
        }
    </style>
</head>
<body>

    <div class="card-container">
        <div class="card" id="flashcard">
            <div class="card-face card-front">अंगीकरण</div>
            <div class="card-face card-back">अनंगीकरण</div>
        </div>
        <div class="finger"></div>
    </div>

    <script>
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
                    card.querySelector(".card-front").innerText = words[index].front;
                    card.querySelector(".card-back").innerText = words[index].back;
                }
            }, 500);
        }

        setInterval(flipCard, 2000);
    </script>

</body>
</html>
