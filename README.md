<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mnemonic Tool</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f8ff;
            margin: 0;
        }

        .container {
            text-align: center;
            width: 50%;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 10px;
            background-color: #fff;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        textarea {
            width: 100%;
            height: 200px;
            padding: 10px;
            margin-bottom: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
            resize: vertical;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }

        #results {
            margin-top: 20px;
            text-align: left;
        }

        h2 {
            margin-bottom: 10px;
        }

        .mnemonic-list {
            list-style: none;
            padding: 0;
        }

        .mnemonic-list li {
            margin-bottom: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Mnemonic and Keywords Generator</h1>
        <textarea id="inputText" placeholder="Paste your text here..."></textarea>
        <button onclick="generateMnemonics()">Generate Mnemonics</button>
        <div id="results">
            <h2>Results</h2>
            <p><strong>Mnemonics:</strong></p>
            <ul id="mnemonics" class="mnemonic-list"></ul>
            <p><strong>Keywords:</strong> <span id="keywords"></span></p>
        </div>
    </div>

    <script>
        function generateMnemonics() {
            const text = document.getElementById('inputText').value;

            if (text.trim() === "") {
                alert("Please paste some text!");
                return;
            }

            const phrases = extractPhrases(text);
            const mnemonics = generateUniqueMnemonics(phrases);
            const keywords = phrases.map(phrase => phrase.split(' ')[0]).join(', ');

            displayResults(mnemonics, keywords);
        }

        function extractPhrases(text) {
            // Split the text into sentences
            const sentences = text.match(/[^\.!\?]+[\.!\?]+/g);

            // Extract meaningful phrases from sentences
            const phrases = sentences.map(sentence => {
                const words = sentence.split(' ');
                // Take the first few words of each sentence as a phrase
                return words.slice(0, 3).join(' ');
            });

            return phrases;
        }

        function generateUniqueMnemonics(phrases) {
            // Generate mnemonics from meaningful phrases
            return phrases.map(phrase => {
                const words = phrase.split(' ');
                // Create a mnemonic from the first letter of each word in the phrase
                return words.map(word => word.charAt(0).toUpperCase()).join('');
            });
        }

        function displayResults(mnemonics, keywords) {
            const mnemonicsList = document.getElementById('mnemonics');
            mnemonicsList.innerHTML = '';

            mnemonics.forEach(mnemonic => {
                const listItem = document.createElement('li');
                listItem.textContent = mnemonic;
                mnemonicsList.appendChild(listItem);
            });

            document.getElementById('keywords').textContent = keywords;
        }
    </script>
</body>
</html>
