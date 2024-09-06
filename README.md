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
    // Function to search articles by author name or title
    function searchArticles() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }
      
      // Fetching articles from arXiv
      fetch(`https://export.arxiv.org/api/query?search_query=all:${query}&start=0&max_results=5`)
        .then(response => response.text())
        .then(data => {
          // Parsing the XML response from arXiv
          const parser = new DOMParser();
          const xmlDoc = parser.parseFromString(data, "text/xml");
          const entries = xmlDoc.getElementsByTagName("entry");

          // Clear previous results
          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';

          // Display results
          for (let i = 0; i < entries.length; i++) {
            const entry = entries[i];
            const title = entry.getElementsByTagName('title')[0].textContent;
            const authors = entry.getElementsByTagName('author');
            const published = entry.getElementsByTagName('published')[0].textContent;
            const institution = entry.getElementsByTagName('arxiv:affiliation')[0]?.textContent || 'Institution not available';

            let authorList = '';
            for (let j = 0; j < authors.length; j++) {
              const name = authors[j].getElementsByTagName('name')[0].textContent;
              const email = authors[j].getElementsByTagName('arxiv:email')[0]?.textContent || 'Email not available';
              const authorInfo = `<strong>Name:</strong> ${name}<br><strong>Email:</strong> ${email}<br><strong>Institution:</strong> ${institution}`;
              authorList += `
                <div class="author-info">
                  ${authorInfo}
                  <button class="copy-btn" onclick="copyToClipboard('${authorInfo}')">Copy</button>
                </div><br>
              `;
            }

            const resultItem = `
              <div class="result-item">
                <h3>${title}</h3>
                <p><strong>Published:</strong> ${published}</p>
                ${authorList}
              </div>
            `;
            resultsContainer.innerHTML += resultItem;
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
