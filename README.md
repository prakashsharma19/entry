<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UPPSC Flashcard Learning</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.9.6/lottie.min.js"></script>
</head>
<body class="bg-gradient-to-b from-blue-500 to-black text-white text-center p-5">

    <div class="max-w-sm mx-auto">
        <h1 class="text-2xl font-bold mb-2">Flashcard Learning</h1>
        <p class="text-sm mb-4">Memorize concepts easily with interactive flashcards.</p>

        <!-- Flashcard Learning Box -->
        <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg relative">
            <h3 class="text-lg font-semibold mb-2">Flashcard Learning</h3>
            <div class="w-full h-20 flex justify-center items-center perspective-1000 relative">
                <div id="flashcard" class="w-44 h-20 relative transform-style-preserve-3d transition-transform duration-700">
                    <div class="absolute w-full h-full flex justify-center items-center text-lg font-bold bg-orange-500 rounded-lg shadow-lg backface-hidden" id="front">अंगीकरण</div>
                    <div class="absolute w-full h-full flex justify-center items-center text-lg font-bold bg-teal-500 rounded-lg shadow-lg transform rotate-y-180 backface-hidden" id="back">अनंगीकरण</div>
                </div>
                <div id="fingerAnimation" class="absolute w-12 h-12 bottom-0 left-1/2 transform -translate-x-1/2"></div>
            </div>
        </div>
    </div>

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
            card.classList.toggle("rotate-y-180");
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
    </script>
</body>
</html>
