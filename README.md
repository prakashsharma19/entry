<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>UPPSC Hindi & Current Affairs Quiz App</title>
    <style>
        /* General Styles */
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background: linear-gradient(to bottom right, #5a2db2, #2196f3);
            color: white;
            margin: 0;
            padding: 20px;
        }

        /* Header Styling */
        header h1 {
            font-size: 22px;
            margin-bottom: 5px;
        }

        header p {
            font-size: 16px;
            margin-bottom: 20px;
        }

        /* Card Container */
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
            max-width: 400px;
            margin: 0 auto;
        }

        /* Individual Cards */
        .card {
            background: rgba(255, 255, 255, 0.15);
            padding: 15px;
            border-radius: 10px;
            width: 90%;
            max-width: 280px;
            text-align: center;
        }

        .card img {
            width: 100px;
            height: auto;
            margin-bottom: 10px;
        }

        /* Buttons */
        .buttons {
            margin-top: 20px;
        }

        .btn {
            padding: 12px;
            width: 200px;
            font-size: 16px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
        }

        .free {
            background-color: orange;
            color: white;
        }

        .paid {
            background-color: green;
            color: white;
        }

        /* Responsive Layout */
        @media (min-width: 768px) {
            header h1 {
                font-size: 28px;
            }

            .container {
                flex-direction: row;
                justify-content: center;
                max-width: 900px;
            }

            .card {
                max-width: 250px;
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
