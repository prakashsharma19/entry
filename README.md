<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcard Flip Animation</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: linear-gradient(135deg, #4a90e2, #9013fe);
            font-family: Arial, sans-serif;
        }

        .flashcard-container {
            display: flex;
            gap: 20px;
        }

        .flashcard {
            width: 200px;
            height: 100px;
            perspective: 1000px;
            cursor: pointer;
            position: relative;
        }

        .flashcard-inner {
            width: 100%;
            height: 100%;
            position: absolute;
            transform-style: preserve-3d;
            transition: transform 0.5s;
        }

        .flashcard.flipped .flashcard-inner {
            transform: rotateY(180deg);
        }

        .flashcard-front, .flashcard-back {
            width: 100%;
            height: 100%;
            position: absolute;
            backface-visibility: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 18px;
            font-weight: bold;
            border-radius: 10px;
        }

        .flashcard-front {
            background: #4CAF50;
            color: white;
        }

        .flashcard-back {
            background: #FF5733;
            color: white;
            transform: rotateY(180deg);
        }
    </style>
</head>
<body>

    <div class="flashcard-container">
        <!-- First Flashcard -->
        <div class="flashcard" onclick="flipCard(this)">
            <div class="flashcard-inner">
                <div class="flashcard-front">अंगीकरण</div>
                <div class="flashcard-back">अनंगीकरण</div>
            </div>
        </div>

        <!-- Second Flashcard -->
        <div class="flashcard" onclick="flipCard(this)">
            <div class="flashcard-inner">
                <div class="flashcard-front">अत्यधिक</div>
                <div class="flashcard-back">अत्यल्प</div>
            </div>
        </div>
    </div>

    <script>
        function flipCard(card) {
            card.classList.toggle('flipped');
        }
    </script>

</body>
</html>
