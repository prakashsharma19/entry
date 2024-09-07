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
    // Function to search for author details using OpenAlex API and arXiv API via PHP
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      // Show loading indicator
      document.getElementById('loading').style.display = 'block';

      // Clean the query
      const sanitizedQuery = query.replace(/[.,]/g, '').split(' ').join('+');

      // Fetch author details from OpenAlex API
      const openAlexUrl = `https://api.openalex.org/works?filter=title.search:${encodeURIComponent(sanitizedQuery)}&per-page=5`;

      fetch(openAlexUrl)
        .then(response => response.json())
        .then(data => {
          document.getElementById('loading').style.display = 'none';

          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';

          // Check if OpenAlex has results
          if (data.results && data.results.length > 0) {
            data.results.forEach(work => {
              const title = work.title;
              const authors = work.authorships;
              const arxivId = work.arxiv_id || null;
              const doi = work.doi || null;

              let authorList = '';
              authors.forEach(author => {
                const name = author.author.display_name || '';
                const affiliation = author.raw_affiliation_string || '';
                const email = author.author.email || '';
                authorList += `<div>${name} ${affiliation ? ' - ' + affiliation : ''} ${email ? '<br>' + email : ''}</div><br>`;
              });

              let arxivLink = '';
              let pdfLink = '';
              let doiLink = '';

              if (arxivId) {
                arxivLink = `<a href="https://arxiv.org/abs/${arxivId}" target="_blank" class="fetch-btn">View on arXiv</a>`;
                pdfLink = `<a href="https://arxiv.org/pdf/${arxivId}" target="_blank" class="pdf-btn">Download PDF</a>`;
              }
              if (doi) {
                doiLink = `<a href="https://doi.org/${doi}" target="_blank" class="fetch-btn">DOI Link</a>`;
              }

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  ${authorList}
                  ${arxivLink}
                  ${pdfLink}
                  ${doiLink}
                </div>
              `;
              resultsContainer.innerHTML += resultItem;
            });
          } else {
            resultsContainer.innerHTML = `<p>No results found in OpenAlex.</p>`;
          }
        })
        .catch(error => {
          console.error('Error fetching OpenAlex data:', error);
          alert('An error occurred while fetching data from OpenAlex.');
          document.getElementById('loading').style.display = 'none';
        });

      // Fetch articles from arXiv API via PHP
      const arxivApiUrl = `arxiv.php?query=${encodeURIComponent(sanitizedQuery)}`;

      fetch(arxivApiUrl)
        .then(response => response.text())
        .then(data => {
          const parser = new DOMParser();
          const xmlDoc = parser.parseFromString(data, "application/xml");
          const entries = xmlDoc.getElementsByTagName("entry");

          if (entries.length > 0) {
            const entry = entries[0];
            const title = entry.getElementsByTagName("title")[0].textContent;
            const pdfLink = entry.getElementsByTagName("id")[0].textContent.replace('/abs/', '/pdf/') + '.pdf';

            const resultItem = `
              <div class="result-item">
                <h3>${title}</h3>
                <a href="${pdfLink}" target="_blank" class="pdf-btn">Download PDF from arXiv</a>
              </div>
            `;
            document.getElementById('results').innerHTML += resultItem;
          }
        })
        .catch(error => {
          console.error('Error fetching arXiv data:', error);
          alert('An error occurred while fetching data from arXiv.');
        });
    }

    function searchOnGoogleScholar(query) {
      const scholarUrl = `https://scholar.google.com/scholar?q=${encodeURIComponent(query)}`;
      window.open(scholarUrl, '_blank');
    }

    // Function to load PDF or Word file in the iframe (Google Docs/OneDrive for Word files)
    function loadFile(event) {
      const file = event.target.files[0];
      const viewer = document.getElementById('documentViewer');

      if (file) {
        const fileURL = URL.createObjectURL(file);
        const fileType = file.type;

        if (fileType === 'application/pdf') {
          viewer.src = fileURL + '#view=fit&page=last';
        } else if (fileType === 'application/msword' || fileType === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document') {
          // Option 1: Google Docs Viewer (only for public URLs)
          // viewer.src = `https://docs.google.com/viewer?url=${encodeURIComponent(fileURL)}&embedded=true`;

          // Option 2: Alert for Word files suggesting upload to OneDrive or Google Drive
          alert('For Word files, please upload them to a cloud service (like Google Drive or OneDrive) for viewing.');
          
          // Option 3: Embed OneDrive document (example URL, adjust accordingly)
          // viewer.src = "https://onedrive.live.com/embed?cid=YOURCID&resid=YOURRESID&authkey=YOURAUTHKEY&em=2";
        } else {
          alert('Please upload a valid PDF, DOC, or DOCX document.');
        }
      }
    }
  </script>
</body>
</html>
