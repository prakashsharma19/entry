<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>UPPSC Flashcards & Quiz</title>
  <style>
    /* General Styles */
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f4f4f9;
      color: #333;
    }

    /* Header */
    .header {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      background-color: #2c3e50;
      color: #fff;
      padding: 10px 20px;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
      z-index: 1000;
    }

    .header-content {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .header h1 {
      margin: 0;
      font-size: 24px;
    }

    .hamburger {
      display: none;
      flex-direction: column;
      cursor: pointer;
    }

    .hamburger span {
      width: 25px;
      height: 3px;
      background-color: #fff;
      margin: 4px 0;
    }

    .nav-menu ul {
      list-style: none;
      margin: 0;
      padding: 0;
      display: flex;
    }

    .nav-menu ul li {
      margin-left: 20px;
    }

    .nav-menu ul li a {
      color: #fff;
      text-decoration: none;
      font-size: 16px;
    }

    /* Hero Banner */
    .hero-banner {
      margin-top: 70px;
      padding: 50px 20px;
      background-color: #3498db;
      color: #fff;
      text-align: center;
    }

    .hero-content p {
      font-size: 18px;
      margin: 0;
    }

    /* Flashcard Section */
    .flashcard-section {
      padding: 50px 20px;
      background-color: #fff;
      text-align: center;
    }

    /* Footer */
    .footer {
      background-color: #2c3e50;
      color: #fff;
      text-align: center;
      padding: 20px;
      position: relative;
      bottom: 0;
      width: 100%;
    }

    /* Responsive Styles */
    @media (max-width: 768px) {
      .hamburger {
        display: flex;
      }

      .nav-menu {
        display: none;
        position: absolute;
        top: 60px;
        right: 20px;
        background-color: #2c3e50;
        padding: 10px;
        border-radius: 5px;
      }

      .nav-menu.active {
        display: block;
      }

      .nav-menu ul {
        flex-direction: column;
      }

      .nav-menu ul li {
        margin: 10px 0;
      }
    }
  </style>
</head>
<body>
  <!-- Header -->
  <header class="header">
    <div class="header-content">
      <h1>UPPSC Flashcards & Quiz</h1>
      <div class="hamburger" id="hamburger">
        <span></span>
        <span></span>
        <span></span>
      </div>
      <nav class="nav-menu" id="nav-menu">
        <ul>
          <li><a href="#">About</a></li>
          <li><a href="#">Login</a></li>
        </ul>
      </nav>
    </div>
  </header>

  <!-- Hero Banner -->
  <section class="hero-banner">
    <div class="hero-content">
      <p>यह ऐप यूपीपीएससी आरओ/एआरओ उम्मीदवारों के लिए है जो तेजी से हिंदी सीखना चाहते हैं और शब्दों को लंबे समय तक याद रखना चाहते हैं।</p>
    </div>
  </section>

  <!-- Flashcard Section (Placeholder) -->
  <section class="flashcard-section">
    <!-- Content will be added later -->
  </section>

  <!-- Footer -->
  <footer class="footer">
    <p>&copy; 2023 UPPSC Flashcards & Quiz. All rights reserved.</p>
  </footer>

  <script>
    // Hamburger Menu Toggle
    const hamburger = document.getElementById('hamburger');
    const navMenu = document.getElementById('nav-menu');

    hamburger.addEventListener('click', () => {
      navMenu.classList.toggle('active');
    });
  </script>
</body>
</html>
