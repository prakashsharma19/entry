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
        margin: 20px;
        background-color: #f3f3f3; /* Light background color */
        color: #333; /* Darker text color */
    }
    .container {
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
        background-color: #fff; /* White background for main content */
        border-radius: 8px;
        box-shadow: 0 0 10px rgba(0,0,0,0.1); /* Subtle shadow for container */
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
    }
    .copy-button:hover {
        background-color: #45a049;
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
        <h1>Text Formatter</h1>
        <p>Paste your text below and click "Fix" to format:</p>
        <textarea id="text-area" placeholder="Paste your text here..."></textarea>
        <br>
        <button onclick="formatText()">Fix</button>
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

            // Apply Times New Roman font and font size 12px
            fixedText.innerHTML = '<p style="font-family: \'Times New Roman\', Times, serif; font-size: 16px;">' + formattedText + '</p>';
        }

        function copyToClipboard() {
            var fixedText = document.getElementById('fixed-text');

            // Create a range and select the fixedText div
            var range = document.createRange();
            range.selectNode(fixedText);

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
