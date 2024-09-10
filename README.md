<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text Cleaner</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        textarea {
            width: 100%;
            height: 300px;
            padding: 10px;
            margin-bottom: 20px;
            font-size: 14px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <h2>Text Cleaner Tool</h2>
    <textarea id="textInput" placeholder="Paste your text here..."></textarea>
    <br>
    <button onclick="cleanText()">Fix Text</button>
    <br><br>
    <h3>Cleaned Text:</h3>
    <textarea id="outputText" readonly></textarea>

    <script>
        function cleanText() {
            let inputText = document.getElementById("textInput").value;
            
            // Remove 'Corresponding author' text
            inputText = inputText.replace(/Corresponding author/gi, '');
            
            // Remove links
            inputText = inputText.replace(/https?:\/\/\S+/g, '');
            
            // Convert email addresses to mailto links
            inputText = inputText.replace(/\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}\b/gi, function(email) {
                return '<a href="mailto:' + email + '">' + email + '</a>';
            });
            
            // Remove special characters
            inputText = inputText.replace(/[^\w\s]/gi, '');

            // Output cleaned text
            document.getElementById("outputText").value = inputText;
        }
    </script>
</body>
</html>
