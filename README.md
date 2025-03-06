<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcards & Quiz</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f8f9fa;
        }
        header {
            position: fixed;
            top: 0;
            width: 100%;
            background-color: #007bff;
            color: white;
            padding: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
        }
        .menu-icon {
            font-size: 24px;
            cursor: pointer;
        }
        .menu {
            display: none;
            position: absolute;
            right: 15px;
            top: 50px;
            background-color: white;
            color: black;
            border: 1px solid #ddd;
            padding: 10px;
            box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.2);
        }
        .menu a {
            display: block;
            padding: 5px 0;
            text-decoration: none;
            color: black;
        }
        .hero {
            margin-top: 70px;
            text-align: center;
            padding: 50px;
            background-color: #e9ecef;
        }
        .flashcards {
            text-align: center;
            padding: 50px;
        }
        footer {
            text-align: center;
            padding: 15px;
            background-color: #343a40;
            color: white;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
    </style>
</head>
<body>
    <header>
        <div>UPPSC Flashcards & Quiz</div>
        <div class="menu-icon" onclick="toggleMenu()">☰</div>
        <div class="menu" id="menu">
            <a href="#">About</a>
            <a href="#">Login</a>
        </div>
    </header>
    <section class="hero">
        <p>यह ऐप यूपीपीएससी आरओ/एआरओ उम्मीदवारों के लिए है जो तेजी से हिंदी सीखना चाहते हैं और शब्दों को लंबे समय तक याद रखना चाहते हैं।</p>
    </section>
    <section class="flashcards">
        <h2>Flashcards Section</h2>
        <!-- Content to be added later -->
    </section>
    <footer>
        &copy; 2025 UPPSC Flashcards & Quiz
    </footer>
    <script>
        function toggleMenu() {
            var menu = document.getElementById('menu');
            menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
        }
    </script>
</body>
</html>
