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
  </style>
</head>
<body>
  <h1>Search Author Details by Paper Title or Name</h1>

  <div class="container">
    <div class="left">
      <h2>Document Viewer</h2>
      <input type="file" id="fileInput" accept=".pdf, .docx" onchange="loadFile(event)">
      <iframe id="documentViewer" title="PDF/Word Viewer"></iframe>
    </div>

    <div class="right">
      <div class="search-container">
        <input type="text" id="searchQuery" placeholder="Enter author name or paper title">
        <button onclick="searchAuthor()">Search</button>
      </div>
      <p class="loading" id="loading">Loading...</p>
      <div class="results" id="results"></div>
    </div>
  </div>

  <script>
    // Function to search for author details using OpenAlex API, CrossRef API, and arXiv API
    function searchAuthor() {
      const query = document.getElementById('searchQuery').value;
      if (!query) {
        alert('Please enter a search query');
        return;
      }

      // Show loading indicator
      document.getElementById('loading').style.display = 'block';

      // Clean the query by removing special characters (like commas and full stops)
      const sanitizedQuery = query.replace(/[.,]/g, '');

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
                const name = author.author.display_name || 'Name not available';

                // Use raw affiliation string if available
                const fullAffiliation = author.raw_affiliation_string || '';

                // Fallback to construct address from available fields
                const institutions = author.institutions || [];
                const primaryInstitution = institutions.length > 0 ? institutions[0] : {};
                const institution = primaryInstitution.display_name || 'Institution not available';
                const country = primaryInstitution.country_code || 'Country not available';
                const email = author.author.email || 'Email not available'; // Assuming email is available

                // Construct the full affiliation in a simplified format, prioritizing raw affiliation
                const affiliationDetails = fullAffiliation || `${institution}, ${country}`;

                // Direct format display
                const authorInfo = `
                  ${name}<br>
                  ${affiliationDetails}<br>
                  ${email}<br>
                `;

                // Adding a copy button for each author info
                authorList += `
                  <div class="author-info" id="author-${name}">
                    ${authorInfo}
                    <button class="copy-btn" onclick="copyToClipboard('${name}, ${affiliationDetails}, ${email}')">Copy</button>
                  </div><br>
                `;
              });

              // Generate links for arXiv and DOI
              let arxivLink = '';
              let pdfLink = '';
              let doiLink = '';
              let pdfEmbed = '';

              if (arxivId) {
                arxivLink = `<a href="https://arxiv.org/abs/${arxivId}" target="_blank" class="fetch-btn">View Article</a>`;
                pdfLink = `<a href="https://arxiv.org/pdf/${arxivId}" target="_blank" class="pdf-btn">Download PDF</a>`;
                // Embed the PDF in an iframe
                pdfEmbed = `<iframe src="https://arxiv.org/pdf/${arxivId}" title="PDF Viewer"></iframe>`;
              }
              if (doi) {
                doiLink = `<a href="https://doi.org/${doi}" target="_blank" class="fetch-btn">Source (DOI)</a>`;
                // Add Google Scholar link
                doiLink += ` <a href="https://scholar.google.com/scholar?q=${encodeURIComponent(title)}" target="_blank" class="fetch-btn">Google Scholar</a>`;
              }

              const resultItem = `
                <div class="result-item">
                  <h3>${title}</h3>
                  ${authorList}
                  ${arxivLink}
                  ${pdfLink}
                  ${doiLink}
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
          document.getElementById('loading').style.display = 'none'; // Hide loading indicator on error
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

    // Function to load PDF or Word file in the iframe (Last page first functionality)
    function loadFile(event) {
      const file = event.target.files[0];
      const viewer = document.getElementById('documentViewer');

      if (file) {
        const fileURL = URL.createObjectURL(file);
        const fileType = file.type;

        if (fileType === 'application/pdf') {
          // Load PDF in iframe (starting with the last page)
          viewer.src = fileURL + '#view=fit&page=last';
        } else if (fileType === 'application/vnd.openxmlformats-officedocument.wordprocessingml.document') {
          // For Word files, use Microsoft Office viewer
          viewer.src = `https://view.officeapps.live.com/op/embed.aspx?src=${encodeURIComponent(fileURL)}`;
        } else {
          alert('Please upload a valid PDF or Word document.');
        }
      }
    }
  </script>
</body>
</html>
