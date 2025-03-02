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
            max-width: 600px;
            margin-bottom: 20px;
        }

        header h1 {
            font-size: 24px;
            margin-bottom: 10px;
        }

        header p {
            font-size: 16px;
            margin-bottom: 20px;
            line-height: 1.4;
        }

        /* Card Container */
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
            width: 100%;
            max-width: 400px;
        }

        /* Individual Card */
        .card {
            background: rgba(255, 255, 255, 0.15);
            padding: 20px;
            border-radius: 12px;
            width: 100%;
            max-width: 300px;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .card img {
            width: 80px;
            height: 80px;
            margin-bottom: 10px;
        }

        .card h2 {
            font-size: 18px;
            margin-bottom: 8px;
        }

        .card p {
            font-size: 14px;
            line-height: 1.4;
        }

        /* Buttons */
        .buttons {
            margin-top: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            width: 100%;
            align-items: center;
        }

        .btn {
            padding: 12px;
            width: 220px;
            font-size: 16px;
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

        /* Responsive Layout */
        @media (min-width: 768px) {
            .container {
                flex-direction: row;
                justify-content: center;
                max-width: 800px;
                flex-wrap: wrap;
            }

            .card {
                max-width: 260px;
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
