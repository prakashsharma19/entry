<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Entry Workspace</title>
    <style>
        body {
            font-family: Arial, sans-serif;
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
            font-size: 16px;
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
            font-size: 18px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #45a049;
        }
        .toolbar {
            margin-bottom: 10px;
        }
        .toolbar select, .toolbar button {
            padding: 5px;
            margin-right: 5px;
            border: none;
            background-color: #ddd;
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
        <button onclick="execCommand('bold')">Bold</button>
        <button onclick="execCommand('italic')">Italic</button>
        <button onclick="execCommand('underline')">Underline</button>
        <label for="fontSize">Text Size: </label>
        <select id="fontSize" onchange="changeTextSize()">
            <option value="14">Small</option>
            <option value="18" selected>Medium</option>
            <option value="24">Large</option>
            <option value="32">X-Large</option>
        </select>
        <label for="fontFamily">Font: </label>
        <select id="fontFamily" onchange="changeFontFamily()">
            <option value="Arial" selected>Arial</option>
            <option value="Times New Roman">Times New Roman</option>
            <option value="Courier New">Courier New</option>
            <option value="Georgia">Georgia</option>
        </select>
        <button onclick="copyToClipboard()">Copy to Clipboard</button>
    </div>
    <div id="outputContainer" contenteditable="true"></div>

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

                // Remove 'Corresponding author' text
                inputText = inputText.replace(/Corresponding author/gi, '');

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

                // Output cleaned text in the editable div
                document.getElementById("outputContainer").innerHTML = inputText;

                document.getElementById("loading").style.display = "none"; // Hide loading indicator
            }, 1000); // Simulate processing time
        }

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
