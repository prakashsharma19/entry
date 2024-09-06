<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Author Details Search Tool</title>
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
  <h1>Search Author Details by Paper Title or Name</h1>

  <div class="search-container">
    <input type="text" id="searchQuery" placeholder="Enter author name or paper title">
    <button onclick="searchAuthor()">Search</button>
  </div>

  <div class="results" id="results"></div>

  <script>
    // Function to search for author details using OpenAlex API
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      // Build the OpenAlex API request URL
      const url = `https://api.openalex.org/works?filter=title.search:${encodeURIComponent(query)}&per-page=5`;

      // Fetch author details from OpenAlex API
      fetch(url)
        .then(response => response.json())
        .then(data => {
          // Clear previous results
          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';

          // Check if results exist
          if (data.results && data.results.length > 0) {
            data.results.forEach(work => {
              const title = work.title;
              const authors = work.authorships;

              let authorList = '';
              authors.forEach(author => {
                const name = author.author.display_name || 'Name not available';
                const institution = author.institutions.length > 0 ? author.institutions[0].display_name : 'Institution not available';

                const authorInfo = `<strong>Name:</strong> ${name}<br><strong>Institution:</strong> ${institution}`;
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
          alert('An error occurred while fetching data.');
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
