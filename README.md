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
            font-size: 18px;
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
            font-family: 'Times New Roman', serif;
            white-space: pre-wrap;
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
        #cutTextDisplay {
            color: green;
            font-weight: bold;
            margin-left: 10px;
        }
        .blinking-cursor {
            animation: blink-animation 1s steps(5, start) infinite;
            font-weight: bold;
            color: red;
        }
        @keyframes blink-animation {
            to {
                visibility: hidden;
            }
        }
    </style>
</head>
<body>
    <h2>Entry Workspace</h2>
    <textarea id="textInput" placeholder="Paste your text here..."></textarea>
    <br>
    <button onclick="cleanText()">Fix Text</button>
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
    </div>
    <div id="outputContainer" contenteditable="true"></div>

    <button class="red" onclick="deleteAll()">Delete All</button>

    <script>
        function removeDiacritics(str) {
            return str.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        }

        function cleanText() {
            document.getElementById("loading").style.display = "inline";
            setTimeout(() => {
                let inputText = document.getElementById("textInput").value;
                inputText = inputText.replace(/Corresponding author/gi, '');
                inputText = inputText.replace(/View the author's ORCID record/gi, '');
                inputText = inputText.replace(/https?:\/\/\S+/g, '');
                inputText = inputText.replace(/\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}\b/gi, function(email) {
                    return '<a href="mailto:' + email + '">' + email + '</a>';
                });
                inputText = removeDiacritics(inputText);
                inputText = inputText.replace(/\.\s*\./g, '.');
                inputText = inputText.replace(/\n/g, '<br>');
                if (document.getElementById("breakLineToggle").checked) {
                    inputText = inputText.replace(/,\s*(?!and\b)/g, ',<br>');
                }
                localStorage.setItem('outputText', inputText);
                document.getElementById("outputContainer").innerHTML = inputText;
                document.getElementById("loading").style.display = "none";
            }, 1000);
        }

        function execCommand(command) {
            document.execCommand(command, false, null);
        }

        function changeTextSize() {
            let fontSize = document.getElementById("fontSize").value;
            document.getElementById("outputContainer").style.fontSize = fontSize + "px";
        }

        function changeFontFamily() {
            let fontFamily = document.getElementById("fontFamily").value;
            document.getElementById("outputContainer").style.fontFamily = fontFamily;
        }

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

        function deleteAll() {
            document.getElementById("textInput").value = '';
            document.getElementById("outputContainer").innerHTML = '';
            localStorage.removeItem('outputText');
        }

        document.getElementById('autoCutToggle').addEventListener('change', function() {
            if (this.checked) {
                document.addEventListener('keydown', autoCutOnKey);
                document.getElementById('outputContainer').classList.add('blinking-cursor');
            } else {
                document.removeEventListener('keydown', autoCutOnKey);
                document.getElementById('outputContainer').classList.remove('blinking-cursor');
            }
        });

        function saveCutText(text) {
            localStorage.setItem('cutText', text);
            document.getElementById('cutTextDisplay').innerText = `Cut text: ${text}`;
        }

        function autoCutOnKey(event) {
            if (event.key === 'ArrowUp' || event.key === 'ArrowDown') {
                let outputContainer = document.getElementById('outputContainer');
                let currentText = outputContainer.innerText;
                let match = currentText.match(/.*?,/);
                if (match) {
                    let cutText = match[0];
                    outputContainer.innerText = currentText.replace(cutText, '');
                    saveCutText(cutText);
                    localStorage.setItem('outputText', outputContainer.innerText);
                }
            }
        }

        window.onload = function() {
            const savedText = localStorage.getItem('outputText');
            const cutText = localStorage.getItem('cutText');
            if (savedText) {
                document.getElementById('outputContainer').innerText = savedText;
            }
            if (cutText) {
                document.getElementById('cutTextDisplay').innerText = `Cut text: ${cutText}`;
            }
        };
    </script>
</body>
</html>
