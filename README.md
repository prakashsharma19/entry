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
            overflow: auto; /* Enable scroll for locking */
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
        button.green {
            background-color: #32CD32; /* Green color for Fix button */
            color: white;
        }
        button.red {
            background-color: #FF6347;
            color: white;
            position: absolute;
            top: 10px;
            right: 20px;
        }
        button.lock {
            background-color: #696969;
            color: white;
            margin-right: 10px;
        }
        button:hover {
            opacity: 0.8;
        }
        #cutTextDisplay {
            color: green;
            font-weight: bold;
            font-size: 24px; /* Larger font size */
            margin-left: 10px;
        }
        .blinking-cursor::after {
            content: '|';
            animation: blink-animation 1s steps(5, start) infinite;
            font-weight: bold;
            color: red;
        }
        @keyframes blink-animation {
            to {
                visibility: hidden;
            }
        }
        #scrollLockNotice {
            display: none;
            color: red;
            font-weight: bold;
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: white;
            padding: 10px;
            border: 1px solid red;
            border-radius: 5px;
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
    <button class="green" onclick="cleanText()">Fix Text</button>
    <label for="breakLineToggle">Break Line</label>
    <input type="checkbox" id="breakLineToggle" />
    <label for="autoCutToggle">Automatic Cut</label>
    <input type="checkbox" id="autoCutToggle" />
    <span id="loading">Processing, please wait...</span>
    <span id="cutTextDisplay"></span>
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
        <button class="blue" onclick="toggleFullScreen()">Full Screen</button>
        <button class="lock" id="lockButton" onclick="toggleLock()">Lock</button>
    </div>
    <div id="outputContainer" contenteditable="true"></div>

    <button class="red" onclick="deleteAll()">Delete All</button>

    <div id="scrollLockNotice">Scrolling is locked, unlock first.</div>

    <script>
        let lockActive = false;
        let originalOverflow = '';

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

                // Apply break line if toggle is on
                if (document.getElementById("breakLineToggle").checked) {
                    inputText = inputText.replace(/,\s*(?!and\b)/g, ',<br>');
                }

                // Save cleaned text in memory (local storage)
                localStorage.setItem('outputText', inputText);

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

        // Function to delete all text in both input and output boxes
        function deleteAll() {
            document.getElementById("textInput").value = '';
            document.getElementById("outputContainer").innerHTML = '';
            localStorage.removeItem('outputText'); // Clear from memory
        }

        // Automatic cut functionality
        document.getElementById('autoCutToggle').addEventListener('change', function() {
            if (this.checked) {
                document.addEventListener('mousemove', autoCut);
                document.getElementById('textInput').classList.add('blinking-cursor'); // Only cursor blinks
            } else {
                document.removeEventListener('mousemove', autoCut);
                document.getElementById('textInput').classList.remove('blinking-cursor');
            }
        });

        // Store cut text in localStorage
        function saveCutText(text) {
            localStorage.setItem('cutText', text);
            document.getElementById('cutTextDisplay').innerText = text;
        }

        // Auto cut function that cuts text until the next comma or end of line
        function autoCut(event) {
            const cursorPosition = event.clientX;
            const inputText = document.getElementById('textInput').value;

            // Find the next comma or end of line
            let textUntilComma = inputText.match(/^.*?,/) || inputText;
            textUntilComma = textUntilComma[0];

            // Cut text and remove it from the textarea
            document.getElementById('textInput').value = inputText.replace(textUntilComma, '');
            saveCutText(textUntilComma);

            // Optionally, you can store the updated text in localStorage
            localStorage.setItem('inputText', document.getElementById('textInput').value);
        }

        // Faster selection when auto-cut is off
        document.getElementById('autoCutToggle').addEventListener('change', function() {
            if (!this.checked) {
                document.addEventListener('keydown', handleQuickSelection);
            } else {
                document.removeEventListener('keydown', handleQuickSelection);
            }
        });

        function handleQuickSelection(e) {
            if ((e.ctrlKey || e.shiftKey) && (e.key === 'ArrowRight' || e.key === 'ArrowLeft')) {
                e.preventDefault();
                let inputText = document.getElementById('textInput').value;
                let cursorPos = document.getElementById('textInput').selectionStart;
                let nextComma = inputText.indexOf(',', cursorPos);
                let nextPeriod = inputText.indexOf('.', cursorPos);
                let endPos = Math.min(nextComma, nextPeriod) === -1 ? inputText.length : Math.min(nextComma, nextPeriod);

                document.getElementById('textInput').setSelectionRange(cursorPos, endPos);
            }
        }

        // Load saved cut text and input text on page load
        window.onload = function() {
            const savedText = localStorage.getItem('inputText');
            const cutText = localStorage.getItem('cutText');
            if (savedText) {
                document.getElementById('textInput').value = savedText;
            }
            if (cutText) {
                document.getElementById('cutTextDisplay').innerText = cutText;
            }
        };

        // Full screen functionality
        function toggleFullScreen() {
            if (!document.fullscreenElement) {
                document.documentElement.requestFullscreen();
            } else {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                }
            }
        }

        // Lock button functionality (Locks all buttons and scrolling)
        function toggleLock() {
            let lockButton = document.getElementById('lockButton');
            lockActive = !lockActive;
            if (lockActive) {
                lockButton.textContent = 'Unlock';
                document.querySelectorAll('button').forEach(btn => btn.disabled = true);
                lockButton.disabled = false; // Keep lock button enabled
                originalOverflow = document.body.style.overflow;
                document.body.style.overflow = 'hidden'; // Lock scrolling
            } else {
                lockButton.textContent = 'Lock';
                document.querySelectorAll('button').forEach(btn => btn.disabled = false);
                document.body.style.overflow = originalOverflow; // Unlock scrolling
            }
        }

        // Show a message if scrolling is attempted while locked
        window.addEventListener('scroll', function() {
            if (lockActive) {
                document.getElementById('scrollLockNotice').style.display = 'block';
                setTimeout(() => {
                    document.getElementById('scrollLockNotice').style.display = 'none';
                }, 2000);
            }
        });
    </script>
</body>
</html>
