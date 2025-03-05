<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcard Learning</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: linear-gradient(to bottom, #3b82f6, #111827);
            color: white;
            margin: 0;
            padding: 0;
        }
        h1 {
            font-size: 28px;
            margin-top: 20px;
            text-transform: uppercase;
        }
        p {
            font-size: 16px;
            margin-bottom: 20px;
        }
        .container {
            width: 80%;
            max-width: 600px;
            margin: auto;
            padding: 20px;
        }
        .card {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
        }
        .card-title {
            font-size: 20px;
            margin-bottom: 10px;
        }
        .flashcard {
            background-color: #f59e0b;
            color: white;
            font-size: 20px;
            padding: 15px;
            border-radius: 8px;
            display: inline-block;
            cursor: pointer;
            transition: transform 0.2s;
            position: relative;
        }
        .flashcard:active {
            transform: scale(0.95);
        }
        .hand {
            width: 40px;
            position: absolute;
            left: 50%;
            bottom: -50px;
            transform: translateX(-50%);
            animation: click-animation 1.5s infinite;
        }
        @keyframes click-animation {
            0%, 100% { bottom: -50px; }
            50% { bottom: -40px; }
        }
    </style>
</head>
<body>

    <h1>UPPSC Hindi & Current Affairs Quiz App</h1>
    <p>Master UPPSC Hindi with 3000+ words and stay updated with daily Current Affairs using Flashcards & Quizzes.</p>

    <div class="container">
        <div class="card">
            <div class="card-title">Flashcard Learning</div>
            <div class="flashcard">अंगीकरण</div>
            <img src="hand-icon.png" class="hand" alt="Click Animation">
        </div>

        <div class="card">
            <div class="card-title">Synonym Learning</div>
            <div class="flashcard">विशाल</div>
            <img src="hand-icon.png" class="hand" alt="Click Animation">
        </div>

        <div class="card">
            <div class="card-title">Current Affairs</div>
            <div class="flashcard">G20 2023 का अध्यक्ष कौन था?</div>
            <img src="hand-icon.png" class="hand" alt="Click Animation">
        </div>
    </div>

</body>
</html>
