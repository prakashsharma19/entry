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
    .container {
      display: flex;
    }
    .left {
      flex: 2;
      padding-right: 20px;
    }
    .right {
      flex: 1;
      padding-left: 20px;
      border-left: 2px solid #ccc;
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
      padding: 8px 15px;
      cursor: pointer;
      border-radius: 5px;
      margin-right: 5px;
    }
    .pdf-btn {
      background-color: #FF5722;
    }
    iframe {
      width: 100%;
      height: 600px;
      border: none;
      margin-top: 20px;
    }
    .loading {
      display: none;
      margin-top: 10px;
      font-size: 18px;
      color: #888;
    }
    h1 {
      text-align: center;
    }
    input[type="text"] {
      width: 70%;
      padding: 8px;
      font-size: 16px;
      border-radius: 4px;
      border: 1px solid #ccc;
    }
    button {
      padding: 10px;
      font-size: 16px;
      cursor: pointer;
    }
    input[type="file"] {
      padding: 8px;
      font-size: 16px;
      margin-top: 10px;
    }
    button.search-btn {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <h1>Search Articles</h1>

  <div class="container">
    <div class="left">
      <input type="file" id="fileInput" accept=".pdf, .doc, .docx" onchange="loadFile(event)">
      <iframe id="documentViewer" title="PDF/Word Viewer"></iframe>
    </div>

    <div class="right">
      <div class="search-container">
        <input type="text" id="searchQuery" placeholder="Enter author name, paper title, or full citation">
        <button class="search-btn" onclick="searchAuthor()">Search</button>
      </div>
      <p class="loading" id="loading">Loading...</p>
      <div class="results" id="results"></div>
    </div>
  </div>

  <script>
    // Function to search for articles using OpenAlex API and arXiv API
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      // Show loading indicator
      document.getElementById('loading').style.display = 'block';

      // Clear previous results
      const resultsContainer = document.getElementById('results');
      resultsContainer.innerHTML = '';

      // Fetch data from arXiv first
      fetchArxivData(query)
        .then((arxivFound) => {
          // If no arXiv results are found, fetch from OpenAlex
          if (!arxivFound) {
            fetchOpenAlexData(query);
          }
        });
    }

    // Function to fetch data from arXiv API
    function fetchArxivData(query) {
      const arxivUrl = `http://export.arxiv.org/api/query?search_query=all:${encodeURIComponent(query)}&start=0&max_results=5`;

      return fetch(arxivUrl)
        .then(response => response.text())
        .then(str => new window.DOMParser().parseFromString(str, "text/xml"))
        .then(data => {
          const entries = data.getElementsByTagName('entry');
          const resultsContainer = document.getElementById('results');

          // If no arXiv results found, return false
          if (entries.length === 0) {
            return false;
          }

          for (let i = 0; i < entries.length; i++) {
            const entry = entries[i];
            const title = entry.getElementsByTagName('title')[0].textContent;
            const summary = entry.getElementsByTagName('summary')[0].textContent;
            const link = entry.getElementsByTagName('link')[0].getAttribute('href');
            const pdfLink = link.replace('/abs/', '/pdf/');

            const resultItem = `
              <div class="result-item">
                <h3>${title}</h3>
                <p>${summary}</p>
                <a href="${link}" target="_blank" class="fetch-btn">View Article (arXiv)</a>
                <a href="${pdfLink}" target="_blank" class="pdf-btn">Download PDF</a>
              </div>
            `;
            resultsContainer.innerHTML += resultItem;
          }

          // Hide loading indicator after arXiv data is fetched
          document.getElementById('loading').style.display = 'none';

          // Return true as arXiv results were found
          return true;
        })
        .catch(error => {
          console.error('Error fetching data from arXiv:', error);
          document.getElementById('loading').style.display = 'none';
          return false;
        });
    }

    // Function to fetch data from OpenAlex API
    function fetchOpenAlexData(query) {
      const openAlexUrl = `https://api.openalex.org/works?filter=title.search:${encodeURIComponent(query)}&per-page=5`;

      fetch(openAlexUrl)
        .then(response => response.json())
        .then(data => {
          const resultsContainer = document.getElementById('results');

          if (data.results && data.results.length > 0) {
            data.results.forEach(work => {
              const title = work.title;
              const authors = work.authorships;
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

                // Only display fields that are available
                const affiliationDetails = fullAffiliation || `${institution}${country ? ', ' + country : ''}`;
                const emailDisplay = email ? `<br>${email}<br>` : '';

                if (name || affiliationDetails || email) {
                  authorList += `
                    <div class="author-info">
                      ${name ? name + '<br>' : ''}
                      ${affiliationDetails ? affiliationDetails + '<br>' : ''}
                      ${emailDisplay}
                    </div><br>
                  `;
                }
              });

              // Generate DOI and Google Scholar buttons
              let doiLink = '';

              if (doi) {
                doiLink = `<a href="https://doi.org/${doi}" target="_blank" class="fetch-btn">Source (DOI)</a>`;
                doiLink += ` <a href="https://scholar.google.com/scholar?q=${encodeURIComponent(title)}" target="_blank" class="fetch-btn">Google Scholar</a>`;
              }

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  ${authorList}
                  ${doiLink}
                </div>
              `;
              resultsContainer.innerHTML += resultItem;
            });
          } else {
            // No results found
            resultsContainer.innerHTML = `<p>No results found in the OpenAlex database.</p>`;
          }

          // Hide loading indicator after OpenAlex data is fetched
          document.getElementById('loading').style.display = 'none';
        })
        .catch(error => {
          console.error('Error fetching data from OpenAlex:', error);
          document.getElementById('loading').style.display = 'none';
        });
    }

    // Function to load PDF or Word file in the iframe (supports both .doc and .docx files)
    function loadFile(event) {
      const file = event.target.files[0];
      const viewer = document.getElementById('documentViewer');

      if (file) {
        const fileURL = URL.createObjectURL(file);
        const fileType = file.type;

        if (fileType === 'application/pdf') {
          // Load PDF in iframe (starting with the last page)
          viewer.src = fileURL + '#view=fit&page=last';
        } else if (fileType === 'application/msword' || fileType === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document') {
          // For Word files, use Google Docs viewer (more reliable)
          viewer.src = `https://docs.google.com/viewer?url=${encodeURIComponent(fileURL)}&embedded=true`;
        } else {
          alert('Please upload a valid PDF, DOC, or DOCX document.');
        }
      }
    }
  </script>
</body>
</html>
