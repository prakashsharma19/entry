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
            font-size: 18px; /* Large text by default */
            border-radius: 5px;
            border: 1px solid #ccc;
            margin-bottom: 20px;
            resize: vertical;
        }
        textarea#removeInput {
            width: 100%;
            height: 30px;
            padding: 5px;
            font-size: 16px;
            border-radius: 5px;
            border: 1px solid #ccc;
            margin-bottom: 10px;
            resize: none;
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
            font-size: 18px; /* Large text by default */
            font-family: 'Times New Roman', serif;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
            background-color: #1E90FF;
            color: white;
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
        #loading {
            display: none;
            color: red;
        }
    </style>
</head>
<body>
    <h2>Entry Workspace</h2>
    <textarea id="textInput" placeholder="Paste your text here..."></textarea>
    <br>

    <textarea id="removeInput" placeholder="Enter text to remove"></textarea>
    <br>
    <label><input type="checkbox" id="sortCheckbox"> Sort the text</label>
    <br><br>

    <div class="toolbar">
        <label for="sortOrder">Drag to Reorder the Sorting Fields:</label>
        <ul id="sortable">
            <li class="sortable-item" data-type="name">Name</li>
            <li class="sortable-item" data-type="department">Department</li>
            <li class="sortable-item" data-type="university">University</li>
            <li class="sortable-item" data-type="institute">Institute</li>
            <li class="sortable-item" data-type="country">Country</li>
            <li class="sortable-item" data-type="email">Email</li>
        </ul>
    </div>

    <button onclick="cleanText()">Fix Text</button>
    <span id="loading">Processing, please wait...</span>
    <br><br>

    <div id="outputContainer" contenteditable="true"></div>
    <br>
    <button onclick="deleteAll()">Delete All</button>

    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.min.js"></script>
    <script>
        $(function() {
            $("#sortable").sortable();
        });

        // Helper function to clean special characters
        function removeDiacritics(str) {
            return str.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
        }

        // Function to clean and format the text
        function cleanText() {
            document.getElementById("loading").style.display = "inline"; // Show loading indicator
            setTimeout(() => {
                let inputText = document.getElementById("textInput").value;
                let removeText = document.getElementById("removeInput").value;

                // Remove specified text
                if (removeText) {
                    let removeRegex = new RegExp(removeText, 'gi');
                    inputText = inputText.replace(removeRegex, '');
                }

                // If sort checkbox is selected
                if (document.getElementById('sortCheckbox').checked) {
                    inputText = sortTextByCategory(inputText);
                }

                // Convert special characters to regular text
                inputText = removeDiacritics(inputText);

                // Remove unwanted full stops
                inputText = inputText.replace(/\.\s*\./g, '.');

                // Preserve paragraph spacing
                inputText = inputText.replace(/\n/g, '<br>');

                // Save cleaned text in memory (local storage)
                localStorage.setItem('outputText', inputText);

                // Output cleaned text in the editable div
                document.getElementById("outputContainer").innerHTML = inputText;

                document.getElementById("loading").style.display = "none"; // Hide loading indicator
            }, 1000); // Simulate processing time
        }

        // Function to sort text by category (Name, Department, University, Institute, Country, Email)
        function sortTextByCategory(inputText) {
            let lines = inputText.split(','); // Split by commas for now, can be customized for different text formats.
            
            let sortedData = {
                name: '',
                department: '',
                university: '',
                institute: '',
                country: '',
                email: '',
                others: ''
            };

            // Identify each part based on keywords or structure
            lines.forEach(line => {
                if (line.match(/@/)) {
                    sortedData.email = line.trim();
                } else if (line.toLowerCase().includes('department')) {
                    sortedData.department = line.trim();
                } else if (line.toLowerCase().includes('university')) {
                    sortedData.university = line.trim();
                } else if (line.toLowerCase().includes('institute')) {
                    sortedData.institute = line.trim();
                } else if (line.toLowerCase().includes('india') || line.toLowerCase().includes('usa')) {
                    sortedData.country = line.trim();
                } else if (!sortedData.name) {
                    sortedData.name = line.trim();
                } else {
                    sortedData.others += line.trim() + ', ';
                }
            });

            // Get user-specified sorting order from the draggable list
            let sortedOrder = $("#sortable").sortable("toArray", { attribute: "data-type" });

            // Build the final sorted text
            let finalText = '';
            sortedOrder.forEach(type => {
                if (sortedData[type]) {
                    finalText += sortedData[type] + '\n';
                }
            });

            return finalText.trim();
        }

        // Load saved text on page load
        window.onload = function() {
            if (localStorage.getItem('outputText')) {
                document.getElementById("outputContainer").innerHTML = localStorage.getItem('outputText');
            }
        };

        // Function to delete all text in both input and output boxes
        function deleteAll() {
            document.getElementById("textInput").value = '';
            document.getElementById("outputContainer").innerHTML = '';
            localStorage.removeItem('outputText'); // Clear from memory
        }
    </script>
</body>
</html>
