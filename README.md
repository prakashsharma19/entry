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
        .collapsible {
            cursor: pointer;
            padding: 10px;
            font-size: 18px;
            background-color: #5cb85c;
            color: white;
            border: none;
            border-radius: 5px;
            text-align: center;
            outline: none;
            margin-bottom: 10px;
            transition: background-color 0.2s ease;
        }
        .collapsible:hover {
            background-color: #4cae4c;
        }
        .content {
            display: none;
            padding: 0 18px;
            background-color: #f9f9f9;
            margin-bottom: 20px;
            border-radius: 8px;
            border: 1px solid #ccc;
        }
        .content ul {
            padding-left: 20px;
        }
        #text-area, #delete-text-area {
            width: 100%;
            height: 100px;
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
            white-space: pre-wrap; /* Preserve formatting */
            word-wrap: break-word;
            font-family: 'Arial', sans-serif;
            font-size: 16px;
        }
        a.email-link {
            text-decoration: underline;
            color: blue;
        }
        .button-group {
            text-align: center;
            margin-top: 20px;
        }
        .button {
            padding: 15px 25px;
            background-color: #5cb85c;
            color: white;
            font-size: 18px;
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
                font-size: 16px;
                padding: 10px 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Fix Entries PPH</h1>
        <p>Paste your text below and click "Fix" to format and normalize special characters:</p>

        <!-- Collapsible button -->
        <button class="collapsible">How to Use</button>

        <!-- Collapsible content -->
        <div class="content">
            <h2>How to Use:</h2>
            <ul>
                <li><strong>Paste Text:</strong> Enter your text into the provided text area.</li>
                <li><strong>Click "Fix":</strong> Press the "Fix" button to clean and format the text.</li>
                <li><strong>View Results:</strong> The processed text will appear below in the output area.</li>
                <li><strong>Copy Text:</strong> Use the "Copy Result" button to copy the cleaned-up text to your clipboard.</li>
                <li><strong>Delete Text:</strong> Enter any text to delete from the processed text, then press "Delete Text".</li>
            </ul>

            <h2>Functionality:</h2>
            <ul>
                <li><strong>Remove Links:</strong> Automatically detects and removes URLs starting with <code>http://</code>, <code>https://</code>, or <code>www.</code></li>
                <li><strong>Email Conversion:</strong> Converts email addresses into clickable "mailto" links.</li>
                <li><strong>Character Normalization:</strong> Removes special characters and diacritics to standardize the text.</li>
                <li><strong>Copy to Clipboard:</strong> Allows easy copying of the processed text with a single click.</li>
                <li><strong>Delete Specific Text:</strong> Allows the user to remove specific text from the processed output.</li>
            </ul>
            <p>Developed by <strong>Prakash</strong>, this tool simplifies text cleanup for various purposes.</p>
        </div>

        <textarea id="text-area" placeholder="Paste your text here..."></textarea>
        <div class="button-group">
            <button class="button" onclick="formatText()">Fix</button>
            <button class="button" onclick="copyToClipboard()">Copy Result</button>
        </div>

        <h2>Delete Specific Text:</h2>
        <textarea id="delete-text-area" placeholder="Enter text to delete from processed text..."></textarea>
        <button class="button" onclick="deleteText()">Delete Text</button>

        <div id="fixed-text"></div>
    </div>

    <div class="footer">
        <p>This page is developed by Prakash.</p>
    </div>

    <script>
        // Collapsible functionality
        document.addEventListener("DOMContentLoaded", function () {
            var collapsible = document.querySelector(".collapsible");
            var content = document.querySelector(".content");

            collapsible.addEventListener("click", function () {
                // Toggle the content visibility
                if (content.style.display === "block") {
                    content.style.display = "none";
                } else {
                    content.style.display = "block";
                }
            });
        });

        let processedText = ''; // Variable to hold the processed text

        async function formatText() {
            var textarea = document.getElementById('text-area');
            var fixedText = document.getElementById('fixed-text');
            var text = textarea.value;

            // Asynchronous text formatting to avoid blocking UI
            await new Promise(resolve => setTimeout(resolve, 0));

            // Remove links starting with http, https, www
            text = text.replace(/https?:\/\/[^\s]+|www\.[^\s]+/g, '');

            // Replace email addresses with links
            var formattedText = text.replace(/\b[\w\.-]+@[\w\.-]+\.\w{2,}\b/g, function (match) {
                return '<a href="mailto:' + match + '" class="email-link">' + match + '</a>';
            });

            // Replace newlines with a single <br> to avoid extra spacing
            formattedText = formattedText.replace(/\n/g, '<br>');

            // Normalize special characters by removing diacritics
            var tempDiv = document.createElement('div');
            tempDiv.innerHTML = formattedText;
            tempDiv.innerHTML = tempDiv.innerHTML.normalize("NFD").replace(/[\u0300-\u036f]/g, "");

            // Remove unnecessary spaces
            tempDiv.innerHTML = tempDiv.innerHTML.replace(/\s+/g, ' ').trim();

            // Update the processed text variable
            processedText = tempDiv.innerHTML;

            // Update the fixed text area
            fixedText.innerHTML = processedText;
        }

        function deleteText() {
            var deleteTextarea = document.getElementById('delete-text-area');
            var fixedText = document.getElementById('fixed-text');
            var textToDelete = deleteTextarea.value;

            // Remove the specified text from the processed text
            processedText = processedText.replace(new RegExp(textToDelete, 'gi'), '').replace(/\s+/g, ' ').trim();

            // Update the fixed text area
            fixedText.innerHTML = processedText;
            deleteTextarea.value = ''; // Clear the delete input field
        }

        function copyToClipboard() {
            var fixedText = document.getElementById('fixed-text');
            navigator.clipboard.writeText(fixedText.innerText).then(function () {
                alert("Copied to clipboard!");
            }, function (err) {
                alert("Failed to copy text: ", err);
            });
        }
    </script>
</body>
</html>
