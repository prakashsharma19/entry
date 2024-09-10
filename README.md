<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Entry Workspace</title>
    <style>
        body {
            font-family: 'Times New Roman', serif;
            margin: 20px;
            background-color: #f4f4f9;
        }
        h2 {
            color: #333;
        }
        textarea#textInput {
            width: 100%;
            height: 100px;
            padding: 10px;
            font-size: 18px; /* Large text by default */
            border-radius: 5px;
            border: 1px solid #ccc;
            margin-bottom: 20px;
            resize: vertical;
        }
        div#outputContainer {
            width: 100%;
            height: 400px;
            padding: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
            background-color: #fff;
            overflow-y: auto;
            color: black;
            font-size: 18px; /* Large text by default */
            font-family: 'Times New Roman', serif;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button.blue {
            background-color: #1E90FF;
            color: white;
        }
        button.red {
            background-color: #FF6347;
            color: white;
            position: absolute;
            top: 10px;
            right: 20px;
        }
        button:hover {
            opacity: 0.8;
        }
        .toolbar {
            margin-bottom: 10px;
        }
        .toolbar select, .toolbar button {
            padding: 5px;
            margin-right: 5px;
            border: none;
            cursor: pointer;
        }
        .toolbar select:hover, .toolbar button:hover {
            background-color: #ccc;
        }
        #loading {
            display: none;
            color: red;
        }
    </style>
</head>
<body>
    <h2>Entry Workspace</h2>
    <textarea id="textInput" placeholder="Paste your text here..."></textarea>
    <br>
    <button onclick="cleanText()">Fix Text</button>
    <span id="loading">Processing, please wait...</span>
    <br><br>
    <div class="toolbar">
        <button class="blue" onclick="execCommand('bold')">Bold</button>
        <button class="blue" onclick="execCommand('italic')">Italic</button>
        <button class="blue" onclick="execCommand('underline')">Underline</button>
        <label for="fontSize">Text Size: </label>
        <select id="fontSize" onchange="changeTextSize()">
            <option value="14">Small</option>
            <option value="18" selected>Large</option>
            <option value="24">X-Large</option>
            <option value="32">XX-Large</option>
        </select>
        <label for="fontFamily">Font: </label>
        <select id="fontFamily" onchange="changeFontFamily()">
            <option value="Times New Roman" selected>Times New Roman</option>
            <option value="Arial">Arial</option>
            <option value="Courier New">Courier New</option>
            <option value="Georgia">Georgia</option>
        </select>
        <button class="blue" onclick="copyToClipboard()">Copy to Clipboard</button>
    </div>
    <div id="outputContainer" contenteditable="true"></div>

    <button class="red" onclick="deleteAll()">Delete All</button>

    <script>
        // Helper function to clean special characters
        function removeDiacritics(str) {
            return str.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        }

        // Function to clean the text
        function cleanText() {
            document.getElementById("loading").style.display = "inline"; // Show loading indicator
            setTimeout(() => {
                let inputText = document.getElementById("textInput").value;

                // Remove 'Corresponding author' and 'View the author\'s ORCID record' texts
                inputText = inputText.replace(/Corresponding author/gi, '');
                inputText = inputText.replace(/View the author's ORCID record/gi, '');

                // Remove links
                inputText = inputText.replace(/https?:\/\/\S+/g, '');

                // Convert email addresses to mailto links
                inputText = inputText.replace(/\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}\b/gi, function(email) {
                    return '<a href="mailto:' + email + '">' + email + '</a>';
                });

                // Convert special characters to regular text
                inputText = removeDiacritics(inputText);

                // Remove unwanted full stops
                inputText = inputText.replace(/\.\s*\./g, '.');

                // Preserve paragraph spacing
                inputText = inputText.replace(/\n/g, '<br>');

                // Save cleaned text in memory (local storage)
                localStorage.setItem('outputText', inputText);

                // Output cleaned text in the editable div
                document.getElementById("outputContainer").innerHTML = inputText;

                document.getElementById("loading").style.display = "none"; // Hide loading indicator
            }, 1000); // Simulate processing time
        }

        // Load saved text on page load
        window.onload = function() {
            if (localStorage.getItem('outputText')) {
                document.getElementById("outputContainer").innerHTML = localStorage.getItem('outputText');
            }
        };

        // Function to handle text formatting (bold, italic, underline)
        function execCommand(command) {
            document.execCommand(command, false, null);
        }

        // Function to change text size in the result box
        function changeTextSize() {
            let fontSize = document.getElementById("fontSize").value;
            document.getElementById("outputContainer").style.fontSize = fontSize + "px";
        }

        // Function to change font family in the result box
        function changeFontFamily() {
            let fontFamily = document.getElementById("fontFamily").value;
            document.getElementById("outputContainer").style.fontFamily = fontFamily;
        }

        // Function to copy cleaned text to clipboard
        function copyToClipboard() {
            let outputContainer = document.getElementById("outputContainer");
            let range = document.createRange();
            range.selectNodeContents(outputContainer);
            let selection = window.getSelection();
            selection.removeAllRanges();
            selection.addRange(range);
            document.execCommand("copy");
            alert("Text copied to clipboard!");
        }

        // Function to delete all text in both input and output boxes
        function deleteAll() {
            document.getElementById("textInput").value = '';
            document.getElementById("outputContainer").innerHTML = '';
            localStorage.removeItem('outputText'); // Clear from memory
        }

        // Function to jump to email link using F11 key
        document.addEventListener('keydown', function (e) {
            if (e.key === 'F11') {
                let emailLinks = document.querySelectorAll('#outputContainer a[href^="mailto:"]');
                if (emailLinks.length > 0) {
                    emailLinks[0].focus();
                    emailLinks[0].click();
                }
                e.preventDefault();
            }
        });
    </script>
</body>
</html>
