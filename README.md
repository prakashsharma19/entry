<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Hindi & Current Affairs Quiz App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #6a11cb, #2575fc);
            color: white;
            text-align: center;
            margin: 0;
            padding: 0;
        }
        .container {
            padding: 30px 5%;
            max-width: 1200px;
            margin: auto;
        }
        .title {
            font-size: 22px;
            font-weight: bold;
            margin-bottom: 10px;
        }
        .description {
            font-size: 16px;
            margin-bottom: 20px;
        }
        .features {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }
        .feature-card {
            background: rgba(255, 255, 255, 0.2);
            padding: 15px;
            border-radius: 10px;
            width: 90%;
            max-width: 350px;
            text-align: center;
            transition: transform 0.3s;
        }
        .feature-card:hover {
            transform: scale(1.05);
        }
        .feature-card img {
            width: 50px;
            height: auto;
            margin-bottom: 10px;
        }
        .feature-card h2 {
            font-size: 18px;
            margin-bottom: 5px;
        }
        .buttons {
            margin-top: 25px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            align-items: center;
        }
        .start-button, .upgrade-button {
            padding: 12px 18px;
            font-size: 16px;
            border: none;
            border-radius: 30px;
            cursor: pointer;
            width: 90%;
            max-width: 280px;
            transition: background 0.3s, transform 0.2s;
        }
        .start-button {
            background: #ff9800;
            color: white;
        }
        .upgrade-button {
            background: #4caf50;
            color: white;
        }
        .start-button:hover, .upgrade-button:hover {
            transform: scale(1.05);
        }

        /* Larger screens adjustments */
        @media (min-width: 768px) {
            .features {
                flex-direction: row;
                justify-content: center;
                flex-wrap: wrap;
            }
            .feature-card {
                width: 30%;
            }
            .buttons {
                flex-direction: row;
                justify-content: center;
            }
            .start-button, .upgrade-button {
                width: auto;
                margin: 0 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="title">UPPSC Hindi & Current Affairs Quiz App</h1>
        <p class="description">Master UPPSC Hindi with 3000+ words and stay updated with daily Current Affairs using flashcards & quizzes.</p>
        
        <div class="features">
            <div class="feature-card">
                <img src="flashcard.png" alt="Flashcard">
                <h2>Flashcard Learning</h2>
                <p>Memorize concepts easily with interactive flashcards.</p>
            </div>
            <div class="feature-card">
                <img src="quiz.png" alt="Quiz">
                <h2>Interactive Quizzes</h2>
                <p>Test your knowledge and track your progress.</p>
            </div>
            <div class="feature-card">
                <img src="current-affairs.png" alt="Current Affairs">
                <h2>Daily Current Affairs</h2>
                <p>Stay updated with latest UPPSC current affairs.</p>
            </div>
        </div>

        <div class="buttons">
            <button class="start-button">Start for Free</button>
            <button class="upgrade-button">Upgrade for ₹99/year</button>
        </div>
    </div>
</body>
</html>
