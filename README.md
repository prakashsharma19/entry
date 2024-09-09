<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Author and Article Search Tool</title>
  <style>
    body {
      font-family: 'Helvetica Neue', Arial, sans-serif;
      margin: 20px;
      background-color: #f0f2f5;
      color: #333;
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
      padding: 10px;
      background-color: #ffffff;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      border-radius: 8px;
    }
    .results {
      margin-top: 20px;
    }
    .result-item {
      border: 1px solid #ddd;
      padding: 20px;
      margin-bottom: 15px;
      border-radius: 8px;
      background-color: #ffffff;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .button-container {
      display: flex;
      gap: 10px;
      margin-top: 10px;
    }
    .button-container a {
      display: inline-block;
      width: 80px;
      height: 40px;
      border-radius: 8px;
      background-color: #fff;
      border: 2px solid #ccc;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      transition: all 0.2s ease;
      overflow: hidden;
    }
    .button-container a img {
      width: 100%;
      height: 100%;
      object-fit: cover; /* Ensures the image fills the button without white space */
    }
    .button-container a:hover {
      transform: translateY(-3px);
      background-color: #f9f9f9;
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
      width: 90%; /* Increased the width */
      padding: 12px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ccc;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
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
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      transition: transform 0.2s ease;
    }
    button.search-btn:hover {
      transform: translateY(-3px);
    }
    textarea {
      width: 100%;
      height: 150px;
      margin-top: 20px;
      padding: 10px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    .save-btn {
      margin-top: 10px;
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 8px 15px;
      cursor: pointer;
      border-radius: 5px;
      transition: transform 0.2s ease;
    }
    .save-btn:hover {
      transform: translateY(-3px);
    }
    .author-info {
      font-size: 14px;
      color: #666;
    }
  </style>
</head>
<body>
  <h1>Search Articles</h1>

  <div class="container">
    <div class="left">
      <input type="file" id="fileInput" accept=".pdf, .doc, .docx" onchange="loadFile(event)">
      <iframe id="documentViewer" title="PDF/Word Viewer"></iframe>
      <textarea id="textBox" placeholder="Paste or edit text here..."></textarea>
      <input type="text" id="fileNameInput" placeholder="Enter file name" />
      <button class="save-btn" onclick="saveText()">Save as .txt</button>
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
    let uploadedFileName = '';

    // Function to load PDF or Word file in the iframe (supports both .doc and .docx files)
    function loadFile(event) {
      const file = event.target.files[0];
      const viewer = document.getElementById('documentViewer');

      if (file) {
        uploadedFileName = file.name.split('.').slice(0, -1).join(''); // Get the filename without extension
        document.getElementById('fileNameInput').value = uploadedFileName; // Set the filename in the input box

        const fileURL = URL.createObjectURL(file);
        const fileType = file.type;

        if (fileType === 'application/pdf') {
          // Load PDF in iframe with zoom 100%
          viewer.src = fileURL + '#zoom=100';
        } else if (fileType === 'application/msword' || fileType === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document') {
          // For Word files, use Google Docs viewer (more reliable)
          viewer.src = `https://docs.google.com/viewer?url=${encodeURIComponent(fileURL)}&embedded=true`;
        } else {
          alert('Please upload a valid PDF, DOC, or DOCX document.');
        }
      }
    }

    // Function to save text content to a .txt file
    function saveText() {
      const textContent = document.getElementById('textBox').value;
      let fileName = document.getElementById('fileNameInput').value || 'untitled'; // Default to "untitled" if no name
      const blob = new Blob([textContent], { type: 'text/plain' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.download = `${fileName}.txt`;
      link.click();
    }

    // Helper function to extract DOI from a string
    function extractDOI(text) {
      const doiPattern = /10\.\d{4,9}\/[-._;()/:A-Z0-9]+/i;
      const match = text.match(doiPattern);
      return match ? match[0] : null;
    }

    // Function to search for author details using CrossRef API (for DOI-based search)
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      // Show loading indicator
      document.getElementById('loading').style.display = 'block';

      // Check if the query contains a DOI
      const doi = extractDOI(query);
      let crossRefUrl;

      // If a DOI is found, use it to search via CrossRef API
      if (doi) {
        crossRefUrl = `https://api.crossref.org/works/${encodeURIComponent(doi)}`;
      } else {
        // Otherwise, search using titles, authors, or other keywords via CrossRef
        const sanitizedQuery = query.replace(/[.,]/g, '').split(' ').join('+');
        crossRefUrl = `https://api.crossref.org/works?query=${encodeURIComponent(sanitizedQuery)}&rows=5`;
      }

      fetch(crossRefUrl)
        .then(response => response.json())
        .then(data => {
          // Hide loading indicator
          document.getElementById('loading').style.display = 'none';

          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';

          // Check if there are results
          let works = data.message.items || (data.message.title ? [data.message] : []);
          if (works.length > 0) {
            works.forEach(work => {
              const title = work.title ? work.title[0] : 'Untitled';
              const authors = work.author || [];
              const doiLink = work.DOI || null;

              let authorList = '';
              authors.forEach(author => {
                const name = `${author.given} ${author.family}`;
                authorList += `<div class="author-info">${name}</div>`;
              });

              // Generate buttons for DOI, Google Scholar, and ArXiv
              let doiButton = '';
              let scholarButton = '';
              let arxivButton = '';

              if (doiLink) {
                doiButton = `<a href="https://doi.org/${doiLink}" target="_blank"><img src="https://github.com/prakashsharma19/Referee/blob/main/Doi-removebg-preview.png?raw=true" alt="DOI"></a>`;
              }

              scholarButton = `<a href="https://scholar.google.com/scholar?q=${encodeURIComponent(title)}" target="_blank"><img src="https://github.com/prakashsharma19/Referee/blob/main/Google_scholar-removebg-preview.png?raw=true" alt="Google Scholar"></a>`;
              arxivButton = `<a href="https://arxiv.org/search/?query=${encodeURIComponent(title)}&searchtype=all" target="_blank"><img src="https://github.com/prakashsharma19/Referee/blob/main/ArXiv%20image.png?raw=true" alt="ArXiv"></a>`;

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  ${authorList}
                  <div class="button-container">
                    ${doiButton}
                    ${scholarButton}
                    ${arxivButton}
                  </div>
                </div>
              `;
              resultsContainer.innerHTML += resultItem;
            });
          } else {
            // No results found
            resultsContainer.innerHTML = `<p>No results found.</p>`;
          }
        })
        .catch(error => {
          console.error('Error fetching data:', error);
          alert('An error occurred while fetching data.');
          document.getElementById('loading').style.display = 'none';
        });
    }
  </script>
</body>
</html>
