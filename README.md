<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>UPPSC Hindi & Current Affairs Quiz App</title>
    <style>
        /* General Reset */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* Body Styling */
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(to bottom right, #5a2db2, #2196f3);
            color: white;
            text-align: center;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        /* Header */
        header {
            max-width: 800px;
            margin-bottom: 20px;
        }

        header h1 {
            font-size: 28px;
            margin-bottom: 10px;
        }

        header p {
            font-size: 18px;
            margin-bottom: 20px;
            line-height: 1.4;
        }

        /* Container for Cards */
        .container {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            max-width: 900px;
            width: 100%;
        }

        /* Individual Card */
        .card {
            background: rgba(255, 255, 255, 0.15);
            padding: 20px;
            border-radius: 12px;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 30%; /* Ensures 3 cards stay in a row */
            min-width: 250px; /* Prevents collapsing */
        }

        .card img {
            width: 80px;
            height: 80px;
            margin-bottom: 10px;
        }

        .card h2 {
            font-size: 20px;
            margin-bottom: 8px;
        }

        .card p {
            font-size: 16px;
            line-height: 1.4;
        }

        /* Buttons */
        .buttons {
            margin-top: 30px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            align-items: center;
        }

        .btn {
            padding: 14px;
            width: 240px;
            font-size: 18px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            text-align: center;
        }

        .free {
            background-color: orange;
            color: white;
        }

        .paid {
            background-color: green;
            color: white;
        }

        /* Responsive Fixes */
        @media (max-width: 768px) {
            .container {
                flex-direction: column;
                align-items: center;
            }
            .card {
                width: 100%;
                max-width: 300px;
            }
        }
    </style>
</head>
<body>

    <header>
        <h1>UPPSC Hindi & Current Affairs Quiz App</h1>
        <p>Master UPPSC Hindi with 3000+ words and stay updated with daily Current Affairs using flashcards & quizzes.</p>
    </header>

    <section class="container">
        <div class="card">
            <img src="flashcard.png" alt="Flashcard Learning">
            <h2>Flashcard Learning</h2>
            <p>Memorize concepts easily with interactive flashcards.</p>
        </div>

        <div class="card">
            <img src="quiz.png" alt="Interactive Quizzes">
            <h2>Interactive Quizzes</h2>
            <p>Test your knowledge and track your progress.</p>
        </div>

        <div class="card">
            <img src="current-affairs.png" alt="Daily Current Affairs">
            <h2>Daily Current Affairs</h2>
            <p>Stay updated with the latest UPPSC current affairs.</p>
        </div>
    </section>

    <div class="buttons">
        <button class="btn free">Start for Free</button>
        <button class="btn paid">Upgrade for ₹99/year</button>
    </div>

</body>
</html>
