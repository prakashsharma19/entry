<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fix Entries PPH</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            font-size: 16px;
            line-height: 1.6;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            color: #333;
        }
        .container {
            max-width: 1200px;
            margin: 40px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
        }
        h1 {
            font-size: 28px;
            text-align: center;
            color: #444;
            margin-bottom: 20px;
        }
        p {
            font-size: 18px;
            text-align: center;
            color: #666;
        }
        #text-area {
            width: 100%;
            height: 200px;
            font-family: 'Arial', sans-serif;
            font-size: 16px;
            padding: 15px;
            margin-bottom: 20px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 6px;
            resize: none;
        }
        #fixed-text {
            margin-top: 20px;
            padding: 20px;
            border: 1px solid #ccc;
            background-color: #f9f9f9;
            border-radius: 8px;
            min-height: 300px;
            overflow-y: auto;
            white-space: pre-wrap;
            word-wrap: break-word;
            font-family: 'Arial', sans-serif;
            font-size: 16px;
            line-height: 0.6; /* Default line spacing */
        }
        a.email-link {
            text-decoration: underline;
            color: blue;
        }
        .button-group, .toolbar, .find-replace-section {
            text-align: center;
            margin-top: 20px;
        }
        .button {
            padding: 10px 20px;
            background-color: #5cb85c;
            color: white;
            font-size: 16px;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            margin: 5px;
            transition: background-color 0.2s, transform 0.2s;
        }
        .button:hover {
            background-color: #4cae4c;
        }
        .button:active {
            transform: scale(0.95);
        }
        .toolbar select, .toolbar input {
            padding: 8px;
            font-size: 16px;
            margin-right: 10px;
        }
        .find-replace-section input {
            padding: 8px;
            font-size: 16px;
            margin-right: 10px;
        }
        .footer {
            text-align: center;
            margin-top: 20px;
            font-style: italic;
            color: #999;
        }
        @media (max-width: 768px) {
            .container {
                padding: 10px;
            }
            .button {
                font-size: 14px;
                padding: 8px 15px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Fix Entries PPH</h1>
        <p>Paste your text below and click "Fix" to format and normalize special characters:</p>
        <textarea id="text-area" placeholder="Paste your text here..."></textarea>

        <div class="toolbar">
            <!-- Formatting Options -->
            <label for="font-family">Font:</label>
            <select id="font-family">
                <option value="Arial">Arial</option>
                <option value="Times New Roman">Times New Roman</option>
                <option value="Courier New">Courier New</option>
                <option value="Georgia">Georgia</option>
            </select>

            <label for="font-size">Size:</label>
            <input type="number" id="font-size" value="16" min="10" max="36" /> px

            <label for="line-spacing">Line Spacing:</label>
            <input type="number" id="line-spacing" value="0.6" step="0.1" min="0.5" max="3" />
        </div>

        <div class="find-replace-section">
            <h3>Find and Replace</h3>
            <input type="text" id="find-text" placeholder="Find..." />
            <input type="text" id="replace-text" placeholder="Replace with..." />
            <button class="button" onclick="findAndReplace()">Replace</button>
            <button class="button" onclick="removeText()">Remove</button>
        </div>

        <div class="button-group">
            <button class="button" onclick="formatText()">Fix</button>
            <button class="button" onclick="applyFormatting()">Apply Formatting</button>
            <button class="button" onclick="copyToClipboard()">Copy Result</button>
        </div>

        <div id="fixed-text" contenteditable="true"></div>
    </div>

    <div class="footer">
        <p>This page is developed by Prakash.</p>
    </div>

    <script>
        // Load saved formatting settings from localStorage
        document.addEventListener('DOMContentLoaded', () => {
            const savedFontFamily = localStorage.getItem('fontFamily');
            const savedFontSize = localStorage.getItem('fontSize');
            const savedLineSpacing = localStorage.getItem('lineSpacing');

            if (savedFontFamily) document.getElementById('font-family').value = savedFontFamily;
            if (savedFontSize) document.getElementById('font-size').value = savedFontSize;
            if (savedLineSpacing) document.getElementById('line-spacing').value = savedLineSpacing;

            applyFormatting(); // Apply saved settings on load
        });

        async function formatText() {
            var textarea = document.getElementById('text-area');
            var fixedText = document.getElementById('fixed-text');
            var text = textarea.value.trim();

            // Asynchronous text formatting to avoid blocking UI
            await new Promise(resolve => setTimeout(resolve, 0));

            // Replace email addresses with links and remove other hyperlinks
            var formattedText = text.replace(/\b[\w\.-]+@[\w\.-]+\.\w{2,}\b/g, function(match) {
                return '<a href="mailto:' + match + '" class="email-link">' + match + '</a>';
            }).replace(/https?:\/\/[^\s]+/g, '');  // Remove non-email URLs

            // Normalize special characters by removing diacritics
            var tempDiv = document.createElement('div');
            tempDiv.innerHTML = formattedText;
            tempDiv.innerHTML = tempDiv.innerHTML.normalize("NFD").replace(/[\u0300-\u036f]/g, "");

            // Make the processed text editable and update the content
            fixedText.innerHTML = tempDiv.innerHTML;
        }

        function applyFormatting() {
            var fixedText = document.getElementById('fixed-text');

            // Get selected values from the toolbar
            var fontFamily = document.getElementById('font-family').value;
            var fontSize = document.getElementById('font-size').value + 'px';
            var lineSpacing = document.getElementById('line-spacing').value;

            // Apply the formatting to the processed text area
            fixedText.style.fontFamily = fontFamily;
            fixedText.style.fontSize = fontSize;
            fixedText.style.lineHeight = lineSpacing;

            // Save formatting options to localStorage for future use
            localStorage.setItem('fontFamily', fontFamily);
            localStorage.setItem('fontSize', document.getElementById('font-size').value);
            localStorage.setItem('lineSpacing', lineSpacing);
        }

        function findAndReplace() {
            var fixedText = document.getElementById('fixed-text');
            var findText = document.getElementById('find-text').value;
            var replaceText = document.getElementById('replace-text').value;

            fixedText.innerHTML = fixedText.innerHTML.replace(new RegExp(findText, 'g'), replaceText);
        }

        function removeText() {
            var fixedText = document.getElementById('fixed-text');
            var findText = document.getElementById('find-text').value;

            fixedText.innerHTML = fixedText.innerHTML.replace(new RegExp(findText, 'g'), '');
        }

        function copyToClipboard() {
            var fixedText = document.getElementById('fixed-text');

            // Create a range and select the fixedText div
            var range = document.createRange();
            range.selectNodeContents(fixedText);

            // Remove previous selection and add new range
            var selection = window.getSelection();
            selection.removeAllRanges();
            selection.addRange(range);

            // Execute the copy command
            document.execCommand('copy');

            // Remove the range and alert (optional)
            selection.removeAllRanges();
            alert('Copied to clipboard!');
        }
    </script>
</body>
</html>
