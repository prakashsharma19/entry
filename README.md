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
        textarea#textInput, textarea#removeText {
            width: 100%;
            padding: 10px;
            font-size: 18px;
            border-radius: 5px;
            border: 1px solid #ccc;
            margin-bottom: 10px;
            resize: vertical;
        }
        textarea#textInput {
            height: 100px;
        }
        textarea#removeText {
            height: 40px;
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
        .sort-section {
            margin-top: 10px;
            margin-bottom: 10px;
        }
        .draggable-item {
            cursor: move;
            padding: 5px;
            border: 1px solid #ccc;
            margin: 5px 0;
            background-color: #f9f9f9;
        }
    </style>
</head>
<body>
    <h2>Entry Workspace</h2>

    <!-- Main input box -->
    <textarea id="textInput" placeholder="Paste your text here..."></textarea>

    <!-- Smaller input box for removing text -->
    <textarea id="removeText" placeholder="Enter text to remove..."></textarea>
    <button class="blue" onclick="removeText()">Remove</button>

    <!-- Toggle for sorting -->
    <div class="sort-section">
        <label for="sortToggle">Enable Sorting: </label>
        <input type="checkbox" id="sortToggle">
    </div>

    <!-- Fix button -->
    <button class="blue" onclick="cleanText()">Fix Text</button>
    <span id="loading">Processing, please wait...</span>
    
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

    <!-- Output container -->
    <div id="outputContainer" contenteditable="true"></div>

    <button class="red" onclick="deleteAll()">Delete All</button>

    <script>
        // Helper function to clean special characters
        function removeDiacritics(str) {
            return str.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        }

        // Function to remove specific text from the output
        function removeText() {
            let removeString = document.getElementById("removeText").value;
            let outputText = document.getElementById("outputContainer").innerHTML;

            if (removeString.trim()) {
                let regex = new RegExp(removeString, 'gi');
                outputText = outputText.replace(regex, '');
                document.getElementById("outputContainer").innerHTML = outputText;
            }
        }

        // Function to clean and optionally sort the text
        function cleanText() {
            document.getElementById("loading").style.display = "inline"; // Show loading indicator

            setTimeout(() => {
                let inputText = document.getElementById("textInput").value;

                // Remove unwanted text
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

                // Preserve paragraph spacing
                inputText = inputText.replace(/\n/g, '<br>');

                // Save cleaned text
                document.getElementById("outputContainer").innerHTML = inputText;

                // Check if sorting is enabled
                if (document.getElementById("sortToggle").checked) {
                    sortText();
                }

                document.getElementById("loading").style.display = "none"; // Hide loading indicator
            }, 1000); // Simulate processing time
        }

        // Function to sort the text into a specific format
        function sortText() {
            const categories = [
                { label: 'Name', text: '' },
                { label: 'Department', text: '' },
                { label: 'Institute', text: '' },
                { label: 'University', text: '' },
                { label: 'Country', text: '' },
                { label: 'Email', text: '' }
            ];

            const outputText = document.getElementById("outputContainer").innerHTML;
            const lines = outputText.split('<br>');

            // Sort text into categories (simple example, can be extended)
            for (let i = 0; i < lines.length; i++) {
                if (lines[i].includes('@')) {
                    categories[5].text = lines[i]; // Email
                } else if (lines[i].includes('University')) {
                    categories[3].text = lines[i]; // University
                } else if (lines[i].includes('Department')) {
                    categories[1].text = lines[i]; // Department
                } else if (lines[i].includes('Institute')) {
                    categories[2].text = lines[i]; // Institute
                } else if (lines[i].trim().split(' ').length <= 3) {
                    categories[0].text = lines[i]; // Name
                } else {
                    categories[4].text = lines[i]; // Country
                }
            }

            // Build sorted output
            let sortedText = '';
            categories.forEach(category => {
                if (category.text) {
                    sortedText += category.text + '<br>';
                }
            });

            // Update output
            document.getElementById("outputContainer").innerHTML = sortedText;
        }

        // Other existing functions for text formatting, etc.
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

        window.onload = function() {
            if (localStorage.getItem('outputText')) {
                document.getElementById("outputContainer").innerHTML = localStorage.getItem('outputText');
            }
        };
    </script>
</body>
</html>
