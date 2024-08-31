<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        textarea {
            width: 100%;
            margin-bottom: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
        .error {
            color: red;
        }
        .success {
            color: green;
        }
        #results {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Document Proofreader</h1>
    <textarea id="documentInput" rows="20" placeholder="Paste your document here..."></textarea>
    <br>
    <button onclick="proofreadDocument()">Check Document</button>
    <div id="results"></div>

    <script>
        function proofreadDocument() {
            const content = document.getElementById('documentInput').value;
            let results = document.getElementById('results');
            results.innerHTML = ''; // Clear previous results

            // Validate numbering sequences
            let numberingErrors = validateNumbering(content);

            // Validate citations and references
            let { citations, references, citationErrors } = validateCitationsAndReferences(content);

            // Display results
            if (numberingErrors.length === 0 && citationErrors.length === 0) {
                results.innerHTML = '<p class="success">No errors found! The document looks good.</p>';
            } else {
                if (numberingErrors.length > 0) {
                    results.innerHTML += '<h3>Numbering Errors:</h3>';
                    numberingErrors.forEach(error => {
                        results.innerHTML += `<p class="error">${error}</p>`;
                    });
                }

                if (citationErrors.length > 0) {
                    results.innerHTML += '<h3>Citation/Reference Errors:</h3>';
                    citationErrors.forEach(error => {
                        results.innerHTML += `<p class="error">${error}</p>`;
                    });
                }
            }
        }

        function validateNumbering(content) {
            const lines = content.split('\n');
            let errors = [];
            let sectionNumber = 0;
            let subsectionNumber = 0;

            lines.forEach(line => {
                let sectionMatch = line.match(/^(\d+)\./);
                let subsectionMatch = line.match(/^(\d+)\.(\d+)\./);

                if (sectionMatch) {
                    let currentSection = parseInt(sectionMatch[1]);
                    if (currentSection !== sectionNumber + 1) {
                        errors.push(`Section ${currentSection} is out of order. Expected ${sectionNumber + 1}.`);
                    }
                    sectionNumber = currentSection;
                    subsectionNumber = 0; // Reset subsection number
                } else if (subsectionMatch) {
                    let currentSection = parseInt(subsectionMatch[1]);
                    let currentSubsection = parseInt(subsectionMatch[2]);

                    if (currentSection !== sectionNumber) {
                        errors.push(`Subsection ${currentSection}.${currentSubsection} belongs to section ${currentSection} but current section is ${sectionNumber}.`);
                    } else if (currentSubsection !== subsectionNumber + 1) {
                        errors.push(`Subsection ${currentSection}.${currentSubsection} is out of order. Expected ${sectionNumber}.${subsectionNumber + 1}.`);
                    }
                    subsectionNumber = currentSubsection;
                }
            });

            return errors;
        }

        function validateCitationsAndReferences(content) {
            let citationPattern = /\((.*?)\)\s*\[(\d+)\]/g;
            let referencePattern = /\[(\d+)\]\s*(.*)/g;

            let citations = {};
            let references = {};

            let citationErrors = [];

            let match;

            while ((match = citationPattern.exec(content)) !== null) {
                let author = match[1];
                let citationNumber = parseInt(match[2]);
                citations[citationNumber] = author;
            }

            while ((match = referencePattern.exec(content)) !== null) {
                let referenceNumber = parseInt(match[1]);
                let referenceContent = match[2];
                references[referenceNumber] = referenceContent;
            }

            for (let number in citations) {
                if (!references[number]) {
                    citationErrors.push(`Citation [${number}] by ${citations[number]} not found in references.`);
                }
            }

            return { citations, references, citationErrors };
        }
    </script>
</body>
</html>
