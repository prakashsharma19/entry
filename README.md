<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Article Search</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      display: flex;
      height: 100vh;
    }
    .container {
      display: flex;
      width: 100%;
      height: 100%;
    }
    .pdf-viewer {
      flex: 2;
      padding: 20px;
      background: #fff;
      border-radius: 5px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      overflow: hidden;
      position: relative;
    }
    .pdf-viewer iframe {
      width: 100%;
      height: 100%;
      border: none;
    }
    .search-container {
      flex: 1;
      padding: 20px;
      background: #fff;
      border-radius: 5px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .search-container input[type="text"] {
      width: calc(100% - 20px);
      padding: 10px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ccc;
      margin-bottom: 10px;
    }
    .search-container button {
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 5px;
      border: none;
      background-color: #2196F3;
      color: white;
      cursor: pointer;
    }
    .file-upload-container {
      text-align: center;
      margin: 20px 0;
    }
    .file-upload-container input[type="file"] {
      font-size: 16px;
      padding: 5px;
    }
    .file-upload-container button {
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 5px;
      border: none;
      background-color: #FFC107;
      color: white;
      cursor: pointer;
    }
    .results {
      margin-top: 20px;
    }
    .result-item {
      border: 1px solid #ddd;
      padding: 20px;
      margin-bottom: 20px;
      border-radius: 10px;
      background-color: white;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .author-info {
      margin-bottom: 10px;
    }
    .copy-btn, .fetch-btn, .pdf-btn, .upload-btn {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 8px 12px;
      cursor: pointer;
      border-radius: 3px;
      margin-right: 5px;
      text-decoration: none;
      display: inline-block;
      text-align: center;
    }
    .fetch-btn {
      background-color: #2196F3;
    }
    .pdf-btn {
      background-color: #FF5722;
    }
    .upload-btn {
      background-color: #FFC107;
    }
    .links-container {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-top: 10px;
    }
    .loading-indicator {
      display: none;
      text-align: center;
      margin-top: 20px;
      font-size: 18px;
      color: #2196F3;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="pdf-viewer" id="pdfViewer">
      <!-- PDF will be embedded here -->
    </div>

    <div class="search-container">
      <h1>Search Author Details by Paper Title or Name</h1>
      <input type="text" id="searchQuery" placeholder="Enter author name or paper title">
      <button onclick="searchAuthor()">Search</button>

      <div class="file-upload-container">
        <input type="file" id="fileInput" accept=".pdf">
        <button class="upload-btn" onclick="handleFile()">Upload PDF</button>
      </div>

      <div class="results" id="results"></div>
      <div class="loading-indicator" id="loadingIndicator">Loading...</div>
    </div>
  </div>

  <script>
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      const loadingIndicator = document.getElementById('loadingIndicator');
      loadingIndicator.style.display = 'block';

      const openAlexUrl = `https://api.openalex.org/works?filter=title.search:${encodeURIComponent(query)}&per-page=5`;

      fetch(openAlexUrl)
        .then(response => response.json())
        .then(data => {
          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';
          loadingIndicator.style.display = 'none';

          if (data.results && data.results.length > 0) {
            data.results.forEach(work => {
              const title = work.title;
              const authors = work.authorships;
              const arxivId = work.arxiv_id || null;
              const doi = work.doi || null;

              let authorList = '';
              authors.forEach(author => {
                const name = author.author.display_name || 'Name not available';
                const fullAffiliation = author.raw_affiliation_string || '';
                const institutions = author.institutions || [];
                const primaryInstitution = institutions.length > 0 ? institutions[0] : {};
                const institution = primaryInstitution.display_name || 'Institution not available';
                const country = primaryInstitution.country_code || 'Country not available';
                const email = author.author.email || 'Email not available';
                const affiliationDetails = fullAffiliation || `${institution}, ${country}`;

                const authorInfo = `
                  <div class="author-info">
                    ${name}<br>
                    ${affiliationDetails}<br>
                    ${country}<br>
                    ${email}<br>
                  </div>
                `;

                authorList += `
                  <div id="author-${name}">
                    ${authorInfo}
                    <button class="copy-btn" onclick="copyToClipboard('${name}, ${affiliationDetails}, ${country}, ${email}')">Copy</button>
                  </div><br>
                `;
              });

              let arxivLink = '';
              let pdfLink = '';
              let doiLink = '';
              let pdfEmbed = '';
              let arxivHomeLink = '';

              if (arxivId) {
                arxivLink = `<a href="https://arxiv.org/abs/${arxivId}" target="_blank" class="fetch-btn">View Article on arXiv</a>`;
                pdfLink = `<a href="https://arxiv.org/pdf/${arxivId}" target="_blank" class="pdf-btn">Download PDF</a>`;
                arxivHomeLink = `<a href="https://arxiv.org" target="_blank" class="fetch-btn">Visit arXiv.org</a>`;
                pdfEmbed = `<iframe src="https://arxiv.org/pdf/${arxivId}" title="PDF Viewer"></iframe>`;
              }
              if (doi) {
                doiLink = `<a href="https://doi.org/${doi}" target="_blank" class="fetch-btn">Source (DOI)</a>`;
              }

              const googleScholarLink = `<a href="https://scholar.google.com/scholar?q=${encodeURIComponent(authors[0].author.display_name)}" target="_blank" class="fetch-btn">Recent Articles on Google Scholar</a>`;

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  ${authorList}
                  <div class="links-container">
                    ${doiLink}
                    ${arxivLink}
                    ${arxivHomeLink}
                    ${googleScholarLink}
                  </div>
                  ${pdfLink}
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
          loadingIndicator.style.display = 'none';
          alert('An error occurred while fetching data.');
        });
    }

    function copyToClipboard(text) {
      const tempInput = document.createElement('textarea');
      tempInput.value = text;
      document.body.appendChild(tempInput);
      tempInput.select();
      document.execCommand('copy');
      document.body.removeChild(tempInput);
      alert('Copied to clipboard!');
    }

    function handleFile() {
      const fileInput = document.getElementById('fileInput');
      const file = fileInput.files[0];
      if (file && file.type === 'application/pdf') {
        const fileURL = URL.createObjectURL(file);
        document.getElementById('pdfViewer').innerHTML = `
          <iframe src="${fileURL}" type="application/pdf" width="100%" height="100%"></iframe>
        `;
      } else {
        alert('Please upload a valid PDF file.');
      }
    }

    // Enable text drag and drop from PDF to search input
    document.getElementById('pdfViewer').addEventListener('dragstart', function (event) {
      const text = window.getSelection().toString();
      if (text) {
        event.dataTransfer.setData('text/plain', text);
      }
    });

    document.getElementById('searchQuery').addEventListener('drop', function (event) {
      event.preventDefault();
      const text = event.dataTransfer.getData('text/plain');
      if (text) {
        document.getElementById('searchQuery').value = text;
      }
    });

    document.getElementById('searchQuery').addEventListener('dragover', function (event) {
      event.preventDefault();
    });
  </script>
</body>
</html>
