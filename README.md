<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Text Formatter</title>
<style>
    body {
        font-family: 'Times New Roman', Times, serif;
        font-size: 16px;
        line-height: 1.6;
        margin: 0;
        background-color: #ADD8E6; /* Light blue background color */
        color: #333; /* Darker text color */
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    .container {
        width: 80%;
        max-width: 800px;
        padding: 20px;
        background-color: #fff; /* White background for main content */
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Subtle shadow for container */
    }
    #text-area {
        width: 100%;
        height: 200px;
        font-family: 'Times New Roman', Times, serif;
        font-size: 16px;
        padding: 10px;
        box-sizing: border-box;
        margin-bottom: 10px;
    }
    #fixed-text {
        margin-top: 10px;
        padding: 10px;
        border: 1px solid #ccc;
        background-color: #f9f9f9;
        border-radius: 8px;
    }
    a.email-link {
        text-decoration: underline;
        color: blue;
    }
    .copy-button {
        display: inline-block;
        padding: 10px 20px;
        background-color: #4CAF50;
        color: white;
        text-align: center;
        font-size: 16px;
        cursor: pointer;
        border: none;
        border-radius: 4px;
        margin-top: 10px;
        text-decoration: none;
        transition: background-color 0.3s ease, transform 0.1s ease;
    }
    .copy-button:hover {
        background-color: #45a049;
        transform: scale(1.05);
    }
    .footer {
        text-align: center;
        margin-top: 20px;
        font-style: italic;
        color: #777;
    }
</style>
</head>
<body>
    <div class="container">
        <h1 style="text-align: center;">Text Formatter</h1>
        <p style="text-align: center;">Paste your text below and click "Fix" to format:</p>
        <textarea id="text-area" placeholder="Paste your text here..."></textarea>
        <br>
        <button class="copy-button" onclick="formatText()">Fix</button>
        <div id="fixed-text"></div>
        <button class="copy-button" onclick="copyToClipboard()">Copy Result</button>
    </div>

    <div class="footer">
        <p>This page is developed by Prakash.</p>
    </div>

    <script>
        function formatText() {
            var textarea = document.getElementById('text-area');
            var fixedText = document.getElementById('fixed-text');
            var text = textarea.value.trim();

            // Replace email addresses with links and add paragraph breaks
            var formattedText = text.replace(/\b[\w\.-]+@[\w\.-]+\.\w{2,}\b/g, function(match) {
                return '<a href="mailto:' + match + '" class="email-link">' + match + '</a><br><br>';
            });

            // Apply Times New Roman font and font size 16px
            fixedText.innerHTML = '<p style="font-family: \'Times New Roman\', Times, serif; font-size: 16px;">' + formattedText + '</p>';
        }

        function copyToClipboard() {
            var textToCopy = document.getElementById('fixed-text').innerText;

            // Create a temporary textarea to copy the text
            var tempTextArea = document.createElement('textarea');
            tempTextArea.value = textToCopy;
            document.body.appendChild(tempTextArea);

            // Select and copy the text
            tempTextArea.select();
            document.execCommand('copy');

            // Remove the temporary textarea
            document.body.removeChild(tempTextArea);

            // Alert copied text (optional)
            alert('Copied to clipboard!');
        }
    </script>
</body>
</html>
