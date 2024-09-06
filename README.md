<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Research Article Search</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }
    .search-container {
      margin-bottom: 20px;
    }
    .results {
      margin-top: 20px;
    }
    .result-item {
      border: 1px solid #ccc;
      padding: 15px;
      margin-bottom: 15px;
      border-radius: 5px;
    }
    .copy-btn {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 5px 10px;
      cursor: pointer;
      border-radius: 3px;
    }
  </style>
</head>
<body>
  <h1>Research Article and Author Details</h1>

  <div class="search-container">
    <input type="text" id="searchQuery" placeholder="Enter author name or article title">
    <button onclick="searchArticles()">Search</button>
  </div>

  <div class="results" id="results"></div>

  <script>
    const apiKey = '1e696708ab7dc6a923779f7cfc51cb21'; // Elsevier API key

    // Function to search articles by author name or title
    function searchArticles() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }
      
      // Fetching articles from Elsevier API (ScienceDirect)
      fetch(`https://api.elsevier.com/content/search/sciencedirect?query=${encodeURIComponent(query)}&apiKey=${apiKey}`)
        .then(response => response.json())
        .then(data => {
          // Clear previous results
          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';

          // Check if results exist
          if (data && data['search-results'] && data['search-results'].entry.length > 0) {
            // Display results
            data['search-results'].entry.forEach(entry => {
              const title = entry['dc:title'];
              const authors = entry['authors'] ? entry['authors']['author'] : [];
              const publicationName = entry['prism:publicationName'] || 'Publication not available';
              const publicationDate = entry['prism:coverDate'] || 'Date not available';

              let authorList = '';
              authors.forEach(author => {
                const name = author['$'] || 'Name not available';
                const affiliation = author['affiliation'] || 'Institution not available';
                const email = 'Email not available'; // Elsevier API does not provide email in search results

                const authorInfo = `<strong>Name:</strong> ${name}<br><strong>Institution:</strong> ${affiliation}`;
                authorList += `
                  <div class="author-info">
                    ${authorInfo}
                    <button class="copy-btn" onclick="copyToClipboard('${authorInfo}')">Copy</button>
                  </div><br>
                `;
              });

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  <p><strong>Publication:</strong> ${publicationName}</p>
                  <p><strong>Published:</strong> ${publicationDate}</p>
                  ${authorList}
                </div>
              `;
              resultsContainer.innerHTML += resultItem;
            });
          } else {
            resultsContainer.innerHTML = '<p>No results found.</p>';
          }
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    }

    // Function to copy text to clipboard
    function copyToClipboard(text) {
      const tempInput = document.createElement('textarea');
      tempInput.value = text;
      document.body.appendChild(tempInput);
      tempInput.select();
      document.execCommand('copy');
      document.body.removeChild(tempInput);
      alert('Copied to clipboard!');
    }
  </script>
</body>
</html>
