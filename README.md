<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>arXiv Author Search</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            background: #fff;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        input[type="text"], button {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            font-size: 16px;
        }
        .result {
            border: 1px solid #ddd;
            padding: 10px;
            margin-top: 10px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Search for arXiv Author</h1>
        <input type="text" id="authorName" placeholder="Enter author name">
        <button onclick="searchAuthor()">Search</button>

        <div id="results"></div>
    </div>

    <script>
        function searchAuthor() {
            const authorName = document.getElementById('authorName').value;
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = '';  // Clear previous results

            if (!authorName) {
                resultsDiv.innerHTML = '<p>Please enter an author name.</p>';
                return;
            }

            const apiUrl = `http://export.arxiv.org/api/query?search_query=au:${encodeURIComponent(authorName)}&start=0&max_results=5`;

            fetch(apiUrl)
                .then(response => response.text())
                .then(data => {
                    const parser = new DOMParser();
                    const xmlDoc = parser.parseFromString(data, "application/xml");
                    const entries = xmlDoc.getElementsByTagName('entry');

                    if (entries.length === 0) {
                        resultsDiv.innerHTML = '<p>No papers found for this author.</p>';
                        return;
                    }

                    for (let i = 0; i < entries.length; i++) {
                        const title = entries[i].getElementsByTagName('title')[0].textContent;
                        const summary = entries[i].getElementsByTagName('summary')[0].textContent;
                        const published = entries[i].getElementsByTagName('published')[0].textContent;
                        const authors = entries[i].getElementsByTagName('author');
                        const paperAuthors = [];
                        
                        for (let j = 0; j < authors.length; j++) {
                            paperAuthors.push(authors[j].getElementsByTagName('name')[0].textContent);
                        }

                        const link = entries[i].getElementsByTagName('id')[0].textContent;

                        // Create the result HTML
                        const paperDiv = document.createElement('div');
                        paperDiv.classList.add('result');
                        paperDiv.innerHTML = `
                            <h3>${title}</h3>
                            <p><strong>Authors:</strong> ${paperAuthors.join(', ')}</p>
                            <p><strong>Published:</strong> ${published}</p>
                            <p>${summary}</p>
                            <a href="${link}" target="_blank">Read Full Paper</a>
                        `;
                        resultsDiv.appendChild(paperDiv);
                    }
                })
                .catch(error => {
                    resultsDiv.innerHTML = '<p>Error fetching data. Please try again later.</p>';
                    console.error('Error:', error);
                });
        }
    </script>

</body>
</html>
