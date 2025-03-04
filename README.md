<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Quiz & Flashcards</title>
    <style>
        /* Global Styling */
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: linear-gradient(to bottom, #3b82f6, #111827);
            color: white;
            margin: 0;
            padding: 0;
        }

        /* Header Styling */
        .header {
            padding: 20px;
        }

        h1 {
            font-size: 28px;
            text-transform: uppercase;
            margin-bottom: 5px;
        }

        p {
            font-size: 16px;
            margin-bottom: 20px;
        }

        /* Container Styling */
        .container {
            width: 80%;
            max-width: 600px;
            margin: auto;
            padding: 20px;
        }

        /* Flashcard Section */
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
            font-weight: bold;
        }

        .flashcard {
            background-color: #f59e0b;
            color: white;
            font-size: 20px;
            padding: 15px;
            border-radius: 8px;
            display: inline-block;
            cursor: pointer;
            transition: transform 0.2s, background-color 0.3s;
            position: relative;
        }

        .flashcard:hover {
            background-color: #d97706;
        }

        .flashcard:active {
            transform: scale(0.95);
        }

        /* Hand Click Animation */
        .hand {
            width: 40px;
            position: absolute;
            left: 50%;
            bottom: -50px;
            transform: translateX(-50%);
            animation: click-animation 1.5s infinite;
        }

        @keyframes click-animation {
            0%, 100% { bottom: -50px; opacity: 1; }
            50% { bottom: -40px; opacity: 0.7; }
        }

        /* Button Section */
        .buttons {
            margin-top: 20px;
        }

        .btn {
            display: inline-block;
            background-color: #10b981;
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            text-decoration: none;
            font-size: 18px;
            margin: 5px;
            transition: background-color 0.3s;
        }

        .btn:hover {
            background-color: #059669;
        }

        .btn-upgrade {
            background-color: #f59e0b;
        }

        .btn-upgrade:hover {
            background-color: #d97706;
        }
    </style>
</head>
<body>

    <!-- Header Section -->
    <div class="header">
        <h1>UPPSC Hindi & Current Affairs Quiz App</h1>
        <p>Master UPPSC Hindi with 3000+ words and stay updated with daily Current Affairs using Flashcards & Quizzes.</p>
    </div>

    <!-- Flashcard Container -->
    <div class="container">
        <div class="card">
            <div class="card-title">Flashcard Learning</div>
            <div class="flashcard" onclick="showAnswer(this)">अंगीकरण</div>
            <img src="hand-icon.png" class="hand" alt="Click Animation">
        </div>

        <div class="card">
            <div class="card-title">Synonym Learning</div>
            <div class="flashcard" onclick="showAnswer(this)">विशाल</div>
            <img src="hand-icon.png" class="hand" alt="Click Animation">
        </div>

        <div class="card">
            <div class="card-title">Current Affairs</div>
            <div class="flashcard" onclick="showAnswer(this)">G20 2023 का अध्यक्ष कौन था?</div>
            <img src="hand-icon.png" class="hand" alt="Click Animation">
        </div>
    </div>

    <!-- Buttons Section -->
    <div class="buttons">
        <a href="#" class="btn">Start for Free</a>
        <a href="#" class="btn btn-upgrade">Upgrade for ₹99/year</a>
    </div>

    <script>
        function showAnswer(element) {
            if (element.innerText === "अंगीकरण") {
                element.innerText = "अपनाना या सम्मिलित करना";
            } else if (element.innerText === "विशाल") {
                element.innerText = "बड़ा, व्यापक";
            } else if (element.innerText === "G20 2023 का अध्यक्ष कौन था?") {
                element.innerText = "भारत";
            } else {
                location.reload(); // Reset back to original state
            }
        }
    </script>

</body>
</html>
