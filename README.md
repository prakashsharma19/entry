<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Hindi & Current Affairs Quiz App</title>
    <link rel="stylesheet" href="styles.css">
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
            padding: 50px 20px;
        }
        .title {
            font-size: 32px;
            font-weight: bold;
            margin-bottom: 10px;
        }
        .description {
            font-size: 18px;
            margin-bottom: 30px;
            padding: 0 10px;
        }
        .features {
            display: flex;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
        }
        .feature-card {
            background: rgba(255, 255, 255, 0.2);
            padding: 20px;
            border-radius: 10px;
            width: 90%;
            max-width: 300px;
            transition: transform 0.3s;
        }
        .feature-card:hover {
            transform: scale(1.05);
        }
        .feature-card img {
            width: 60px;
            margin-bottom: 10px;
        }
        .feature-card h2 {
            font-size: 20px;
            margin-bottom: 10px;
        }
        .buttons {
            margin-top: 30px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .start-button, .upgrade-button {
            padding: 12px 25px;
            font-size: 16px;
            border: none;
            border-radius: 30px;
            cursor: pointer;
            transition: background 0.3s, transform 0.2s;
            width: 80%;
            max-width: 250px;
            margin-bottom: 10px;
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
        @media (min-width: 768px) {
            .features {
                flex-wrap: nowrap;
            }
            .feature-card {
                width: 250px;
            }
            .buttons {
                flex-direction: row;
                justify-content: center;
            }
            .start-button, .upgrade-button {
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
