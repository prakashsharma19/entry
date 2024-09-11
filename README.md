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
            overflow: auto;
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
            white-space: pre-wrap; /* Preserve spaces */
        }
        button {
            padding: 5px 10px;
            font-size: 14px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            margin-right: 5px;
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
        }
        button.lock.locked {
            background-color: red; /* Red when locked */
        }
        button:hover {
            opacity: 0.8;
        }
        #cutTextDisplay {
            color: green;
            font-weight: bold;
            font-size: 24px;
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
        /* Icons for Bold, Italic, Underline */
        .toolbar button {
            font-size: 18px;
            padding: 5px;
        }
        .toolbar .icon-button {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 20px;
            margin-right: 5px;
        }
        .toolbar .icon-button:hover {
            opacity: 0.8;
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
        <button class="icon-button" onclick="execCommand('bold')" title="Bold"><b>B</b></button>
        <button class="icon-button" onclick="execCommand('italic')" title="Italic"><i>I</i></button>
        <button class="icon-button" onclick="execCommand('underline')" title="Underline"><u>U</u></button>
        <button class="lock" id="lockButton" onclick="toggleLock()">Lock</button>
    </div>
    <div id="outputContainer" contenteditable="true"></div>
    <button class="red" onclick="deleteAll()">Delete All</button>
    <br>
    <button class="blue" onclick="toggleFullScreen()">Full Screen</button>

    <div id="scrollLockNotice">Scrolling is Locked, Unlock first.</div>

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

        // Auto cut functionality
        document.getElementById('autoCutToggle').addEventListener('change', function() {
            if (this.checked) {
                document.addEventListener('keydown', autoCutWithArrowKeys);
            } else {
                document.removeEventListener('keydown', autoCutWithArrowKeys);
            }
        });

        // Store cut text in localStorage
        function saveCutText(text) {
            localStorage.setItem('cutText', text);
            document.getElementById('cutTextDisplay').innerText = text;
        }

        // Auto cut function that cuts text until the next comma or period (on arrow key press)
        function autoCutWithArrowKeys(e) {
            if (['ArrowRight', 'ArrowLeft', 'ArrowUp', 'ArrowDown'].includes(e.key)) {
                let outputContainer = document.getElementById("outputContainer");
                let selectedText = window.getSelection().toString();
                if (!selectedText) {
                    let content = outputContainer.innerText;
                    let cursorPosition = window.getSelection().anchorOffset;

                    let regex = /([^,.\n]*[,.])/g; // Regex to match text till comma or period

                    let matches = [];
                    let match;
                    while ((match = regex.exec(content)) !== null) {
                        matches.push(match);
                    }

                    // Find match based on current cursor position
                    for (let i = 0; i < matches.length; i++) {
                        if (matches[i].index >= cursorPosition) {
                            // Remove the full line till comma or period
                            content = content.replace(matches[i][0], '');
                            saveCutText(matches[i][0]);
                            break;
                        }
                    }

                    // Update the output container with the modified content
                    outputContainer.innerText = content;

                    e.preventDefault();
                }
            }
        }

        // Lock button functionality (Locks all buttons and scrolling)
        function toggleLock() {
            let lockButton = document.getElementById('lockButton');
            lockActive = !lockActive;
            if (lockActive) {
                lockButton.classList.add('locked');
                lockButton.textContent = 'Unlock';
                document.querySelectorAll('button').forEach(btn => btn.disabled = true);
                lockButton.disabled = false; // Keep lock button enabled
                originalOverflow = document.body.style.overflow;
                document.body.style.overflow = 'hidden'; // Lock scrolling
            } else {
                lockButton.classList.remove('locked');
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

        // Function to delete all text in both input and output boxes
        function deleteAll() {
            document.getElementById("textInput").value = '';
            document.getElementById("outputContainer").innerHTML = '';
            localStorage.removeItem('outputText'); // Clear from memory
        }

        // Load saved cut text and input text on page load
        window.onload = function() {
            const savedText = localStorage.getItem('outputText');
            const cutText = localStorage.getItem('cutText');
            if (savedText) {
                document.getElementById('outputContainer').innerText = savedText;
            }
            if (cutText) {
                document.getElementById('cutTextDisplay').innerText = cutText;
            }
        };
    </script>
</body>
</html>
