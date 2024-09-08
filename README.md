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
          // Load PDF in iframe
          viewer.src = fileURL + '#view=fit&page=last';
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

    // Function to search for author details using OpenAlex API, CrossRef API, and arXiv API
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      // Show loading indicator
      document.getElementById('loading').style.display = 'block';

      // Clean the query and split into parts to handle full citation, title, author, etc.
      const sanitizedQuery = query.replace(/[.,]/g, '').split(' ').join('+');

      // Build the OpenAlex API request URL
      const openAlexUrl = `https://api.openalex.org/works?filter=title.search:${encodeURIComponent(sanitizedQuery)}&per-page=5`;

      // Fetch author details from OpenAlex API
      fetch(openAlexUrl)
        .then(response => response.json())
        .then(data => {
          // Hide loading indicator
          document.getElementById('loading').style.display = 'none';

          // Clear previous results
          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';

          // Check if results exist
          if (data.results && data.results.length > 0) {
            data.results.forEach(work => {
              const title = work.title;
              const authors = work.authorships;
              const arxivId = work.arxiv_id || null; // Get arXiv ID if available
              const doi = work.doi || null; // Get DOI if available

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

              // Generate links for arXiv, DOI, and Google Scholar
              let arxivLink = '';
              let pdfLink = '';
              let doiLink = '';
              let scholarLink = '';
              let arxivSearchLink = `<a href="https://arxiv.org/search/?query=${encodeURIComponent(title)}&searchtype=all" target="_blank" class="fetch-btn">Search on arXiv</a>`;

              if (arxivId) {
                arxivLink = `<a href="https://arxiv.org/abs/${arxivId}" target="_blank" class="fetch-btn">View Article on arXiv</a>`;
                pdfLink = `<a href="https://arxiv.org/pdf/${arxivId}" target="_blank" class="pdf-btn">Download PDF</a>`;
              }
              if (doi) {
                doiLink = `<a href="https://doi.org/${doi}" target="_blank" class="fetch-btn">Source (DOI)</a>`;
              }

              scholarLink = `<a href="https://scholar.google.com/scholar?q=${encodeURIComponent(title)}" target="_blank" class="fetch-btn">Google Scholar</a>`;

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  ${authorList}
                  ${arxivLink}
                  ${pdfLink}
                  ${doiLink}
                  ${scholarLink}
                  ${arxivSearchLink}
                </div>
              `;
              resultsContainer.innerHTML += resultItem;
            });
          } else {
            // No results found, suggest searching on Google Scholar and arXiv
            resultsContainer.innerHTML = `
              <p>No results found in the database.</p>
              <button class="search-btn" onclick="searchOnGoogleScholar('${query}')">Try Google Scholar</button>
              <button class="search-btn" onclick="searchOnArxiv('${query}')">Try arXiv</button>
            `;
          }
        })
        .catch(error => {
          console.error('Error fetching data:', error);
          alert('An error occurred while fetching data.');
          document.getElementById('loading').style.display = 'none'; // Hide loading indicator on error
        });
    }

    // Function to search on Google Scholar when no results found
    function searchOnGoogleScholar(query) {
      const scholarUrl = `https://scholar.google.com/scholar?q=${encodeURIComponent(query)}`;
      window.open(scholarUrl, '_blank');
    }

    // Function to search on arXiv when no results found
    function searchOnArxiv(query) {
      const arxivUrl = `https://arxiv.org/search/?query=${encodeURIComponent(query)}&searchtype=all`;
      window.open(arxivUrl, '_blank');
    }
  </script>
</body>
</html>
