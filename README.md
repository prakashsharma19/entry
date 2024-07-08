<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Fix Entries</title>
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
        overflow: hidden;
    }
    .container {
        width: 80%;
        max-width: 1200px;
        padding: 20px;
        background-color: #fff; /* White background for main content */
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* Subtle shadow for container */
        text-align: center; /* Center align the content */
    }
    .logo {
        max-width: 150px;
        margin-bottom: 20px;
    }
    #text-area, #fixed-text {
        width: calc(100% - 110px);
        height: 300px;
        font-family: 'Times New Roman', Times, serif;
        font-size: 16px;
        padding: 10px;
        box-sizing: border-box;
        margin-bottom: 10px;
        border: 1px solid #ccc;
        border-radius: 8px;
    }
    #fixed-text {
        overflow-y: scroll; /* Add a scroll bar */
        background-color: #f9f9f9;
    }
    a.email-link {
        text-decoration: underline;
        color: blue;
    }
    .copy-button, .fix-button {
        display: inline-block;
        padding: 10px 20px;
        background-color: #4CAF50;
        color: white;
        text-align: center;
        font-size: 16px;
        cursor: pointer;
        border: none;
        border-radius: 4px;
        text-decoration: none;
        transition: background-color 0.3s ease, transform 0.1s ease;
        margin-left: 10px;
    }
    .copy-button:hover, .fix-button:hover {
        background-color: #45a049;
        transform: scale(1.05);
    }
    .footer {
        text-align: center;
        margin-top: 20px;
        font-style: italic;
        color: #777;
    }
    .button-container {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
</style>
</head>
<body>
    <div class="container">
        <img src="logo.png" alt="Logo" class="logo">
        <h1>Fix Entries</h1>
        <p>Paste your text below and click "Fix" to format:</p>
        <div style="display: flex; justify-content: space-between;">
            <textarea id="text-area" placeholder="Paste your text here..."></textarea>
            <div class="button-container">
                <button class="fix-button" onclick="formatText()">Fix</button>
                <button class="copy-button" onclick="copyToClipboard()">Copy Result</button>
            </div>
        </div>
        <div id="fixed-text" contenteditable="true"></div>
    </div>

    <div class="footer">
        <p>This page is developed by Prakash.</p>
    </div>

    <script>
        function formatText() {
            var textarea = document.getElementById('text-area');
            var fixedText = document.getElementById('fixed-text');
            var text = textarea.value.trim();

            // Split text into lines
            var lines = text.split('\n');
            var formattedText = '';

            for (var i = 0; i < lines.length; i++) {
                // Trim the line to remove extra spaces
                var line = lines[i].trim();
                // Check if the line contains an email
                if (/\b[\w\.-]+@[\w\.-]+\.\w{2,}\b/.test(line)) {
                    // Replace email with a link
                    line = line.replace(/\b[\w\.-]+@[\w\.-]+\.\w{2,}\b/g, function(match) {
                        return '<a href="mailto:' + match + '" class="email-link">' + match + '</a>';
                    });
                }
                formattedText += line + '<br>';
            }

            // Update the fixed text box with the formatted text
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
