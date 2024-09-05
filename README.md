<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>arXiv Search</title>
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
        input[type="text"], button, select {
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
        .pdf-viewer {
            width: 100%;
            height: 600px;
            border: 1px solid #ddd;
            margin-top: 20px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Search arXiv by Author or Title</h1>
        
        <!-- Search Options -->
        <select id="searchType">
            <option value="author">Search by Author</option>
            <option value="title">Search by Title</option>
        </select>
        
        <!-- Search Input -->
        <input type="text" id="searchQuery" placeholder="Enter author name or paper title">
        <button onclick="searchArxiv()">Search</button>

        <div id="results"></div>

        <!-- PDF Viewer -->
        <iframe id="pdfViewer" class="pdf-viewer" src="" style="display: none;"></iframe>
    </div>

    <script>
        function searchArxiv() {
            const searchType = document.getElementById('searchType').value;
            const query = document.getElementById('searchQuery').value;
            const resultsDiv = document.getElementById('results');
            const pdfViewer = document.getElementById('pdfViewer');
            resultsDiv.innerHTML = '';  // Clear previous results
            pdfViewer.style.display = 'none'; // Hide PDF viewer

            if (!query) {
                resultsDiv.innerHTML = '<p>Please enter a search query.</p>';
                return;
            }

            // Build the search query based on type (author or title)
            let searchField = '';
            if (searchType === 'author') {
                searchField = `au:${encodeURIComponent(query)}`;
            } else if (searchType === 'title') {
                searchField = `ti:${encodeURIComponent(query)}`;
            }

            // Use CORS proxy for fetching data
            const apiUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent('http://export.arxiv.org/api/query?search_query=' + searchField + '&start=0&max_results=5')}`;

            fetch(apiUrl)
                .then(response => response.text())
                .then(data => {
                    const parser = new DOMParser();
                    const xmlDoc = parser.parseFromString(data, "application/xml");
                    const entries = xmlDoc.getElementsByTagName('entry');

                    if (entries.length === 0) {
                        resultsDiv.innerHTML = '<p>No results found for this query.</p>';
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
                        const pdfLink = link.replace('abs', 'pdf');  // Generate PDF link

                        // Create the result HTML and add button to display the PDF
                        const paperDiv = document.createElement('div');
                        paperDiv.classList.add('result');
                        paperDiv.innerHTML = `
                            <h3>${title}</h3>
                            <p><strong>Authors:</strong> ${paperAuthors.join(', ')}</p>
                            <p><strong>Published:</strong> ${published}</p>
                            <p>${summary}</p>
                            <button onclick="showPdf('${pdfLink}')">View PDF</button>
                        `;
                        resultsDiv.appendChild(paperDiv);
                    }
                })
                .catch(error => {
                    resultsDiv.innerHTML = '<p>Error fetching data. Please try again later.</p>';
                    console.error('Error:', error);
                });
        }

        function showPdf(pdfUrl) {
            const pdfViewer = document.getElementById('pdfViewer');
            pdfViewer.src = pdfUrl;  // Set the PDF URL
            pdfViewer.style.display = 'block';  // Show the PDF viewer
        }
    </script>

</body>
</html>
