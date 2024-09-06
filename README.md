<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Author and Article Search Tool</title>
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
    .copy-btn, .fetch-btn, .pdf-btn {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 5px 10px;
      cursor: pointer;
      border-radius: 3px;
      margin-right: 5px;
    }
    .fetch-btn {
      background-color: #2196F3;
    }
    .pdf-btn {
      background-color: #FF5722;
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
    // Function to search for author details using OpenAlex API, CrossRef API, and arXiv API
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      // Build the OpenAlex API request URL
      const openAlexUrl = `https://api.openalex.org/works?filter=title.search:${encodeURIComponent(query)}&per-page=5`;

      // Fetch author details from OpenAlex API
      fetch(openAlexUrl)
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
              const arxivId = work.arxiv_id || null; // Get arXiv ID if available

              let authorList = '';
              authors.forEach(author => {
                const name = author.author.display_name || 'Name not available';
                const institution = author.institutions.length > 0 ? author.institutions[0].display_name : 'Institution not available';

                // ORCID ID (if available)
                const orcid = author.author.orcid || null;

                // Display author details
                const authorInfo = `<strong>Name:</strong> ${name}<br><strong>Institution:</strong> ${institution}`;
                authorList += `
                  <div class="author-info" id="author-${name}">
                    ${authorInfo}
                    <button class="copy-btn" onclick="copyToClipboard('${authorInfo}')">Copy</button>
                  </div><br>
                `;

                // Fetch more details from ORCID if available
                if (orcid) {
                  fetch(`https://pub.orcid.org/v3.0/${orcid}/record`)
                    .then(response => response.json())
                    .then(orcidData => {
                      const orcidAffiliations = orcidData.affiliations.map(aff => aff.organization.name).join(', ');
                      const orcidInfo = `<strong>ORCID:</strong> ${orcid}<br><strong>Affiliations:</strong> ${orcidAffiliations}`;
                      document.querySelector(`#author-${name}`).innerHTML += `<br>${orcidInfo}`;
                    });
                }
              });

              // Generate links for arXiv
              let arxivLink = '';
              let pdfLink = '';
              if (arxivId) {
                arxivLink = `<a href="https://arxiv.org/abs/${arxivId}" target="_blank" class="fetch-btn">View Article</a>`;
                pdfLink = `<a href="https://arxiv.org/pdf/${arxivId}" target="_blank" class="pdf-btn">Download PDF</a>`;
              }

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  ${authorList}
                  ${arxivLink}
                  ${pdfLink}
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
