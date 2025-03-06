<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard App</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
    <style>
        .flashcard {
            transform-style: preserve-3d;
            transition: transform 0.7s;
        }
        .flashcard.flip {
            transform: rotateY(180deg);
        }
        .flashcard-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
        }
        .flashcard-back {
            transform: rotateY(180deg);
        }
    </style>
</head>
<body class="bg-gradient-to-b from-blue-500 to-black text-white text-center p-5">
    <!-- Header -->
    <header class="fixed top-0 left-0 w-full bg-gray-900 text-white p-4 flex justify-between items-center shadow-lg z-10">
        <h1 class="text-xl font-bold">UPPSC Flashcard App</h1>
        <button id="menuButton" class="text-white text-xl">☰</button>
    </header>
    
    <!-- Sidebar Menu -->
    <div id="sidebar" class="fixed top-0 right-0 h-full bg-gray-800 text-white w-48 p-4 hidden">
        <button id="closeButton" class="text-white text-xl mb-4">✖</button>
        <ul class="space-y-2">
            <li><a href="#" class="block p-2 hover:bg-gray-700">लॉगिन</a></li>
            <li><a href="#" class="block p-2 hover:bg-gray-700">हमारे बारे में</a></li>
        </ul>
    </div>
    
    <div class="mt-16 max-w-sm mx-auto">
        <h2 class="text-2xl font-bold mb-2">फ्लैशकार्ड द्वारा सीखें</h2>
        <p class="text-sm mb-4">अब सीखना और भी आसान और प्रभावी हो गया है!</p>

        <!-- Flashcard Learning Box -->
        <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg relative">
            <h3 class="text-lg font-semibold mb-2">फ्लैशकार्ड अभ्यास</h3>
            <div class="w-full h-20 flex justify-center items-center perspective-1000 relative">
                <div id="flashcard" class="flashcard w-44 h-20 relative">
                    <div class="flashcard-face flex justify-center items-center text-lg font-bold bg-orange-500 rounded-lg shadow-lg" id="front">अंगीकरण</div>
                    <div class="flashcard-face flashcard-back flex justify-center items-center text-lg font-bold bg-teal-500 rounded-lg shadow-lg" id="back">अनंगीकरण</div>
                </div>
                <div id="fingerAnimation" class="absolute w-12 h-12 bottom-0 left-1/2 transform -translate-x-1/2"></div>
            </div>
        </div>
        
        <!-- Start Practice Button -->
        <button class="mt-4 bg-yellow-500 text-black px-6 py-2 rounded-lg font-bold hover:bg-yellow-600 transition">अभ्यास शुरू करें</button>
    </div>

    <!-- Footer -->
    <footer class="mt-8 py-4 bg-gray-900 text-white text-center">
        © 2025 UPPSC Flashcard App. सभी अधिकार सुरक्षित।
    </footer>

    <script>
        let card = document.getElementById("flashcard");
        let frontText = document.getElementById("front");
        let backText = document.getElementById("back");
        let words = [
            { front: "अंगीकरण", back: "अनंगीकरण" },
            { front: "अत्यधिक", back: "अत्यल्प" }
        ];
        let index = 0;
        let isFlipped = false;

        function flipCard() {
            card.classList.toggle("flip");
            isFlipped = !isFlipped;
            setTimeout(() => {
                index = (index + 1) % words.length;
                if (!isFlipped) {
                    frontText.innerText = words[index].front;
                    backText.innerText = words[index].back;
                }
            }, 500);
        }
        setInterval(flipCard, 2000);
        
        let animation = lottie.loadAnimation({
            container: document.getElementById("fingerAnimation"),
            renderer: "svg",
            loop: true,
            autoplay: true,
            path: "https://raw.githubusercontent.com/prakashsharma19/entry/main/Animation%20-%201740910253032.json"
        });
        animation.setSpeed(0.5);

        // Sidebar Menu Functionality
        document.getElementById("menuButton").addEventListener("click", function() {
            document.getElementById("sidebar").classList.toggle("hidden");
        });
        document.getElementById("closeButton").addEventListener("click", function() {
            document.getElementById("sidebar").classList.add("hidden");
        });
    </script>
</body>
</html>
