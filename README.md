<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Elsevier API Search</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        input[type="text"], button {
            width: calc(100% - 22px);
            padding: 10px;
            margin: 10px 0;
            font-size: 16px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #007bff;
            color: #fff;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .result {
            border: 1px solid #ddd;
            padding: 15px;
            margin-top: 10px;
            border-radius: 4px;
            background: #f9f9f9;
        }
        .loading {
            text-align: center;
            margin-top: 20px;
            font-size: 18px;
            color: #007bff;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Search Elsevier Articles</h1>
        
        <!-- Search Input -->
        <input type="text" id="searchQuery" placeholder="Enter search term">
        <button onclick="searchElsevier()">Search</button>
        <div id="loading" class="loading" style="display: none;">Loading...</div>
        <div id="results"></div>
    </div>

    <script>
        function searchElsevier() {
            const query = document.getElementById('searchQuery').value;
            const resultsDiv = document.getElementById('results');
            const loadingIndicator = document.getElementById('loading');
            resultsDiv.innerHTML = '';  // Clear previous results
            loadingIndicator.style.display = 'block'; // Show loading indicator

            if (!query) {
                resultsDiv.innerHTML = '<p>Please enter a search query.</p>';
                loadingIndicator.style.display = 'none'; // Hide loading indicator
                return;
            }

            // Build the search query
            const apiKey = '1e696708ab7dc6a923779f7cfc51cb21'; // Your Elsevier API key
            const searchUrl = `https://api.elsevier.com/content/search/scopus?query=${encodeURIComponent(query)}&apiKey=${apiKey}`;

            fetch(searchUrl, {
                headers: {
                    'Accept': 'application/json'
                }
            })
                .then(response => response.json())
                .then(data => {
                    if (!data || !data['search-results'] || !data['search-results']['entry']) {
                        resultsDiv.innerHTML = '<p>No results found for this query.</p>';
                        loadingIndicator.style.display = 'none'; // Hide loading indicator
                        return;
                    }

                    const entries = data['search-results']['entry'];

                    for (let i = 0; i < entries.length; i++) {
                        const title = entries[i]['dc:title'] || 'No title available';
                        const authors = entries[i]['dc:creator'] || ['No authors available'];
                        const published = entries[i]['prism:coverDate'] || 'No publication date available';
                        const link = entries[i]['prism:url'] || '#';

                        // Create the result HTML with direct link to the article
                        const paperDiv = document.createElement('div');
                        paperDiv.classList.add('result');
                        paperDiv.innerHTML = `
                            <h3>${title}</h3>
                            <p><strong>Authors:</strong> ${authors.join(', ')}</p>
                            <p><strong>Published:</strong> ${published}</p>
                            <p><a href="${link}" target="_blank">Read Full Article</a></p>
                        `;
                        resultsDiv.appendChild(paperDiv);
                    }

                    loadingIndicator.style.display = 'none'; // Hide loading indicator
                })
                .catch(error => {
                    resultsDiv.innerHTML = '<p>Error fetching data. Please try again later.</p>';
                    loadingIndicator.style.display = 'none'; // Hide loading indicator
                    console.error('Error:', error);
                });
        }
    </script>

</body>
</html>
