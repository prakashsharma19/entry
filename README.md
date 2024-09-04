<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PPH-Editors</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        #inputText {
            width: 100%;
            height: 100px;
            margin-bottom: 10px;
        }
        #outputText {
            width: 100%;
            height: 100px;
            margin-bottom: 10px;
            background-color: #f0f0f0;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
        }
    </style>
</head>
<body>
    <h1>PPH-Editors</h1>
    <textarea id="inputText" placeholder="Paste the text here..."></textarea>
    <button onclick="processText()">Process Text</button>
    <textarea id="outputText" readonly></textarea>
    <button onclick="copyToClipboard()">Copy to Clipboard</button>

    <script>
        function processText() {
            const inputText = document.getElementById('inputText').value;
            const lines = inputText.split('\n');
            
            // Extract information
            let name = lines[0] || '';
            let department = '';
            let college = '';
            let address = '';
            let email = '';
            
            lines.forEach(line => {
                if (line.startsWith('Department')) {
                    department = line.split(',')[0];
                } else if (line.includes('University') || line.includes('College')) {
                    college = line;
                } else if (line.includes('@')) {
                    email = line;
                } else if (line.match(/^[A-Za-z]/) && !line.includes('@')) {
                    address = line;
                }
            });

            // Format output
            const formattedText = [
                name,
                department,
                college,
                address,
                email
            ].filter(line => line).join('\n');

            document.getElementById('outputText').value = formattedText;
        }

        function copyToClipboard() {
            const outputText = document.getElementById('outputText');
            outputText.select();
            document.execCommand('copy');
            alert('Copied to clipboard!');
        }
    </script>
</body>
</html>
