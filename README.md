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
      display: flex;
    }
    .left-container, .right-container {
      padding: 20px;
    }
    .left-container {
      width: 65%; /* Bigger for better readability */
      margin-right: 20px;
      border-right: 1px solid #ccc;
    }
    .right-container {
      width: 35%;
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
    .fetch-btn, .pdf-btn {
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
    .green-btn {
      background-color: #4CAF50; /* Professional green color */
      color: white;
      border: none;
      padding: 10px 15px;
      font-size: 16px;
      cursor: pointer;
      border-radius: 5px;
    }
    iframe {
      width: 100%;
      height: 800px;
      border: none;
      margin-top: 20px;
      display: none;
    }
    input[type="text"] {
      width: 80%;
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    button {
      padding: 10px 15px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <!-- Left side: for viewing PDF or Word files -->
  <div class="left-container">
    <iframe id="viewer"></iframe>
  </div>

  <!-- Right side: for search functionality -->
  <div class="right-container">
    <h1>Search Author Details by Paper Title or Name</h1>

    <div class="search-container">
      <input type="text" id="searchQuery" placeholder="Enter author name or paper title">
      <button class="green-btn" onclick="searchAuthor()">Search</button>
    </div>

    <div class="results" id="results"></div>
  </div>

  <script>
    // Function to search for author details using OpenAlex API and show results
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value.trim();
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      // Extract title or keywords from the query (attempt to focus search)
      const refinedQuery = extractTitleOrKeywords(query);
      const openAlexUrl = `https://api.openalex.org/works?filter=title.search:${encodeURIComponent(refinedQuery)}&per-page=5`;

      fetch(openAlexUrl)
        .then(response => response.json())
        .then(data => {
          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';

          if (data.results && data.results.length > 0) {
            data.results.forEach(work => {
              const title = work.title;
              const authors = work.authorships;
              const arxivId = work.arxiv_id || null;
              const doi = work.doi || null;

              let authorList = '';
              authors.forEach(author => {
                const name = author.author.display_name || '';
                const fullAffiliation = author.raw_affiliation_string || '';
                const institutions = author.institutions || [];
                const primaryInstitution = institutions.length > 0 ? institutions[0] : {};
                const institution = primaryInstitution.display_name || '';
                const country = primaryInstitution.country_code || '';
                const email = author.author.email || '';

                // Show only the available details (skip empty fields)
                const affiliationDetails = fullAffiliation || `${institution} ${country ? ', ' + country : ''}`;
                const authorInfo = `
                  ${name}${affiliationDetails ? '<br>' + affiliationDetails : ''}${email ? '<br>' + email : ''}
                `;

                authorList += `
                  <div class="author-info">
                    ${authorInfo}
                  </div><br>
                `;
              });

              let arxivLink = '';
              let pdfLink = '';
              let doiLink = '';
              let googleScholarLink = '';
              let pdfEmbed = '';

              if (arxivId) {
                arxivLink = `<a href="https://arxiv.org/abs/${arxivId}" target="_blank" class="fetch-btn">View Article</a>`;
                pdfLink = `<a href="#" class="pdf-btn" onclick="openPDF('https://arxiv.org/pdf/${arxivId}')">Open PDF</a>`;
                pdfEmbed = `<iframe src="https://arxiv.org/pdf/${arxivId}" title="PDF Viewer"></iframe>`;
              }
              if (doi) {
                doiLink = `<a href="https://doi.org/${doi}" target="_blank" class="fetch-btn">Source (DOI)</a>`;
                googleScholarLink = `<a href="https://scholar.google.com/scholar?q=${encodeURIComponent(title)}" target="_blank" class="fetch-btn">Google Scholar</a>`;
              }

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  ${authorList}
                  ${arxivLink}
                  ${pdfLink}
                  ${doiLink}
                  ${googleScholarLink}
                  ${pdfEmbed}
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

    // Function to open PDF or Word in iframe and load the last page first
    function openPDF(url) {
      const viewer = document.getElementById('viewer');
      viewer.src = url + '#page=last'; // Open last page
      viewer.style.display = 'block'; // Show the iframe
    }

    // Function to extract relevant part from a longer citation or text
    function extractTitleOrKeywords(query) {
      const titleMatch = query.match(/["“”](.*?)["“”]/); // Match text between quotes
      if (titleMatch) {
        return titleMatch[1]; // Return the title
      }

      // Fallback: return the most meaningful part (first part) of the query
      return query.split(',')[0];
    }
  </script>
</body>
</html>
