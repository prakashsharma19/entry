<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Entry Workspace</title>
    <style>
        body {
            font-family: Arial, sans-serif;
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
            font-size: 14px;
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
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #45a049;
        }
        #outputContainer {
            display: block;
            font-size: 14px;
        }
        .toolbar {
            margin-bottom: 10px;
        }
        .toolbar button {
            padding: 5px;
            margin-right: 5px;
            border: none;
            background-color: #ddd;
            cursor: pointer;
        }
        .toolbar button:hover {
            background-color: #ccc;
        }
    </style>
</head>
<body>
    <h2>Entry Workspace</h2>
    <textarea id="textInput" placeholder="Paste your text here..."></textarea>
    <br>
    <button onclick="cleanText()">Fix Text</button>
    <br><br>
    <div class="toolbar">
        <button onclick="execCommand('bold')">Bold</button>
        <button onclick="execCommand('italic')">Italic</button>
        <button onclick="execCommand('underline')">Underline</button>
    </div>
    <div id="outputContainer" contenteditable="true"></div>

    <script>
        // Helper function to clean special characters
        function removeDiacritics(str) {
            return str.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        }

        // Function to clean the text
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

            // Convert special characters to regular text
            inputText = removeDiacritics(inputText);

            // Preserve paragraph spacing
            inputText = inputText.replace(/\n/g, '<br>');

            // Output cleaned text in the editable div
            document.getElementById("outputContainer").innerHTML = inputText;
        }

        // Function to handle text formatting (bold, italic, underline)
        function execCommand(command) {
            document.execCommand(command, false, null);
        }
    </script>
</body>
</html>
