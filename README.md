<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard & Quiz</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.2/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        /* General Styles */
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(to bottom, #1E3C72, #2A5298);
            color: white;
            text-align: center;
        }

        /* Header */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 20px;
            background: #16213E;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .header h1 {
            font-size: 24px;
            margin: 0;
            color: #ffcc00;
            font-weight: bold;
        }

        .menu-icon {
            font-size: 24px;
            cursor: pointer;
            color: #ffcc00;
        }

        /* Hero Banner */
        .hero {
            background: #1E3C72;
            color: white;
            text-align: center;
            padding: 40px 20px;
            margin: 0;
        }

        .hero h2 {
            font-size: 28px;
            color: #ffcc00;
            margin-bottom: 15px;
            font-weight: bold;
        }

        .hero p {
            font-size: 18px;
            line-height: 1.6;
            max-width: 600px;
            margin: 0 auto 20px;
        }

        .start-btn {
            padding: 12px 30px;
            background: #ffcc00;
            color: #1E3C72;
            font-size: 18px;
            font-weight: bold;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: background 0.3s ease;
        }

        .start-btn:hover {
            background: #e6b800;
        }

        /* Flashcard Section */
        .flashcard-section {
            background: rgba(255, 255, 255, 0.1);
            padding: 30px 20px;
            margin: 20px auto;
            border-radius: 15px;
            max-width: 500px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
        }

        .flashcard-container {
            display: flex;
            justify-content: center;
            align-items: center;
            perspective: 1000px;
            position: relative;
        }

        .flashcard {
            width: 250px; /* Larger square */
            height: 250px; /* Larger square */
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s;
            cursor: pointer;
            background: #ff9800;
            border-radius: 15px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }

        .flashcard.flip {
            transform: rotateY(180deg);
        }

        .flashcard-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 22px; /* Adjusted font size */
            font-weight: bold;
            color: white;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
            box-sizing: border-box; /* Ensure padding is included in width/height */
        }

        .flashcard-front {
            background: #ff9800; /* Front background color */
        }

        .flashcard-back {
            background: #009688; /* Back background color */
            transform: rotateY(180deg);
        }

        /* Lottie Hand Animation */
        #fingerAnimation {
            position: absolute;
            width: 60px;
            height: 60px;
            bottom: -30px;
            left: 50%;
            transform: translateX(-50%);
        }

        /* विशेषता Section */
        .special-features {
            background: linear-gradient(to right, #4A90E2, #2A5298); /* Gradient background */
            color: white;
            padding: 30px 20px;
            margin: 20px 0;
            text-align: left;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }

        .special-features h3 {
            font-size: 24px;
            color: #ffcc00;
            margin-bottom: 20px;
            text-align: center;
        }

        .special-features ul {
            list-style: none;
            padding: 0;
            max-width: 600px;
            margin: 0 auto;
        }

        .special-features ul li {
            font-size: 16px;
            line-height: 1.6;
            margin-bottom: 10px;
            padding-left: 30px;
            position: relative;
        }

        .special-features ul li::before {
            content: "✔️";
            position: absolute;
            left: 0;
            color: #ffcc00;
        }

        /* फ्लैशकार्ड के फायदे Section */
        .flashcard-benefits {
            background: linear-gradient(to right, #009688, #004D40); /* Gradient background */
            color: white;
            padding: 30px 20px;
            margin: 20px 0;
            text-align: left;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }

        .flashcard-benefits h3 {
            font-size: 24px;
            color: #ffcc00;
            margin-bottom: 20px;
            text-align: center;
        }

        .flashcard-benefits ul {
            list-style: none;
            padding: 0;
            max-width: 600px;
            margin: 0 auto;
        }

        .flashcard-benefits ul li {
            font-size: 16px;
            line-height: 1.6;
            margin-bottom: 10px;
            padding-left: 30px;
            position: relative;
        }

        .flashcard-benefits ul li::before {
            content: "✔️";
            position: absolute;
            left: 0;
            color: #ffcc00;
        }

        /* Marquee Section */
        .marquee-heading {
            font-size: 24px;
            font-weight: bold;
            color: #ffcc00;
            margin: 30px 0 10px;
        }

        .marquee-container {
            overflow: hidden;
            white-space: nowrap;
            width: 100%;
            margin: 10px 0;
        }

        .marquee {
            display: inline-block;
            animation: marquee 15s linear infinite;
        }

        .marquee img {
            width: 150px;
            height: auto;
            margin: 0 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }

        @keyframes marquee {
            from { transform: translateX(100%); }
            to { transform: translateX(-100%); }
        }

        /* Floating Button */
        .floating-btn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #ffcc00;
            color: #1E3C72;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            transition: background 0.3s ease;
        }

        .floating-btn:hover {
            background: #e6b800;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .modal-content {
            background: #1E3C72;
            padding: 20px;
            border-radius: 10px;
            max-width: 400px;
            text-align: center;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
        }

        .modal-content h3 {
            font-size: 22px;
            color: #ffcc00;
            margin-bottom: 15px;
        }

        .modal-content p {
            font-size: 16px;
            line-height: 1.6;
            margin-bottom: 20px;
        }

        .modal-content button {
            padding: 10px 20px;
            background: #ffcc00;
            color: #1E3C72;
            font-size: 16px;
            font-weight: bold;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s ease;
        }

        .modal-content button:hover {
            background: #e6b800;
        }

        /* Footer */
        .footer {
            background: #0b2135;
            padding: 20px;
            margin-top: 30px;
            text-align: center;
        }

        .footer p {
            margin: 0;
            font-size: 14px;
            color: #ccc;
        }
    </style>
</head>
<body>

    <!-- Header -->
    <div class="header">
        <h1>UPPSC Flashcard & Quiz</h1>
        <i class="fa fa-bars menu-icon" onclick="toggleMenu()"></i>
    </div>

    <!-- Hero Banner -->
    <div class="hero">
        <h2>UPPSC Flashcard & Quiz</h2>
        <p>इस ऐप के माध्यम से आप हर दिन 50-60 नए शब्द और महत्वपूर्ण करंट अफेयर्स आसानी से सीख सकते हैं।  
        यह विशेष रूप से UPPSC RO/ARO सहित अन्य प्रतियोगी परीक्षाओं की तैयारी करने वाले छात्रों के लिए उपयोगी है।  
        नियमित अभ्यास से आपकी शब्दावली मजबूत होगी, करंट अफेयर्स पर पकड़ बनेगी, और परीक्षा में बेहतर अंक प्राप्त करने में सहायता मिलेगी।</p>
        <button id="startBtn" class="start-btn">शुरू करें</button>
    </div>

    <!-- Flashcard Section -->
    <div class="flashcard-section">
        <div class="flashcard-container">
            <div class="flashcard" id="flashcard">
                <div class="flashcard-face flashcard-front">अतिथि शब्द का पर्यायवाची है</div>
                <div class="flashcard-face flashcard-back">अभ्यागत, आगुन्तक, पाहुन, मेहमान, गृहागत</div>
            </div>
            <div id="fingerAnimation"></div>
        </div>
    </div>

    <!-- विशेषता Section -->
    <div class="special-features">
        <h3>📌 विशेषता</h3>
        <ul>
            <li>विगत वर्षों में पूछे गए प्रश्नों तथा अति संभावित प्रश्नों का संकलन।</li>
            <li>प्रत्येक विषय से 600+ शब्दों के अति संभावित प्रश्नों की व्यापक प्रैक्टिस (विलोम, तद्भव-तत्सम, अनेकार्थी, पर्यायवाची, विशेषण एवं विदेशी शब्द)।</li>
            <li>नियमित अभ्यास से 60/60 अंक पक्के करें।</li>
            <li>मासिक करंट अफेयर्स की प्रभावी प्रैक्टिस।</li>
        </ul>
    </div>

    <!-- Marquee Heading -->
    <div class="marquee-heading">सभी प्रमुख पुस्तक का संकलन</div>

    <!-- Marquee Book Covers -->
    <div class="marquee-container">
        <div class="marquee">
            <img src="https://raw.githubusercontent.com/prakashsharma19/entry/main/image.png" alt="Book 1">
            <img src="https://raw.githubusercontent.com/prakashsharma19/entry/main/hardev.png" alt="Book 2">
            <img src="https://raw.githubusercontent.com/prakashsharma19/entry/main/image.png" alt="Book 1">
        </div>
    </div>

    <!-- फ्लैशकार्ड के फायदे Section -->
    <div class="flashcard-benefits">
        <h3>📌 फ्लैशकार्ड के फायदे</h3>
        <ul>
            <li>याद करने में आसानी – जल्दी और लंबे समय तक याद रखें।</li>
            <li>दृश्य और मानसिक जुड़ाव – स्मरण शक्ति बढ़ती है।</li>
            <li>तेज़ पुनरावृत्ति – कठिन शब्द और करंट अफेयर्स याद रहें।</li>
            <li>कहीं भी, कभी भी अभ्यास – मोबाइल या डिजिटल पर पढ़ें।</li>
        </ul>
    </div>

    <!-- Floating Button -->
    <div class="floating-btn" onclick="openModal()">
        <i class="fas fa-plus"></i>
    </div>

    <!-- Modal -->
    <div class="modal" id="modal">
        <div class="modal-content">
            <h3>होमस्क्रीन शॉर्टकट जोड़ें</h3>
            <p>इस ऐप को अपने होमस्क्रीन पर जोड़ने के लिए:</p>
            <p><strong>Android:</strong> ब्राउज़र मेनू में "Add to Home screen" विकल्प चुनें।</p>
            <p><strong>iOS:</strong> शेयर बटन पर क्लिक करें और "Add to Home Screen" चुनें।</p>
            <button onclick="closeModal()">ठीक है</button>
        </div>
    </div>

    <!-- Footer -->
    <div class="footer">
        <p>© 2025 UPPSC Flashcard & Quiz | Contact Us</p>
    </div>

    <script>
        // Flashcard Functionality
        let card = document.getElementById("flashcard");
        let words = [
            { front: "अतिथि शब्द का पर्यायवाची है", back: "अभ्यागत, आगुन्तक, पाहुन, मेहमान, गृहागत" },
            { front: "जंगल शब्द का पर्यायवाची है", back: "दाव, अरण्य, कांतार, विपिन, अटवी, कानन, वन, बयाबान" },
            { front: "वैमनस्य", back: "सौहार्द" },
            { front: "ह्रस्व", back: "दीर्घ" }
        ];
        let index = 0;
        let isFlipped = false;

        function flipCard() {
            card.classList.toggle("flip");
            isFlipped = !isFlipped;

            setTimeout(() => {
                index = (index + 1) % words.length;
                if (!isFlipped) {
                    card.querySelector(".flashcard-front").innerText = words[index].front;
                    card.querySelector(".flashcard-back").innerText = words[index].back;
                }
            }, 300); // Adjusted flip speed
        }

        // Auto flip every 2 seconds
        setInterval(flipCard, 2000);

        // Lottie Hand Animation
        let animation = lottie.loadAnimation({
            container: document.getElementById("fingerAnimation"),
            renderer: "svg",
            loop: true,
            autoplay: true,
            path: "https://raw.githubusercontent.com/prakashsharma19/entry/main/Animation%20-%201740910253032.json"
        });

        // Reduce speed of hand clicking animation
        animation.setSpeed(0.5);

        // Modal Functionality
        let modal = document.getElementById("modal");

        function openModal() {
            modal.style.display = "flex";
        }

        function closeModal() {
            modal.style.display = "none";
        }

        // Navigate to the second page when "शुरू करें" button is clicked
        document.getElementById("startBtn").addEventListener("click", function() {
            window.location.href = "https://prakashsharma19.github.io/flashcard/";
        });
    </script>

</body>
</html>
