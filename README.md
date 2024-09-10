<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Author and Article Search Tool with ORCID API</title>
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
      display: flex;
      align-items: center;
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
      width: 75%;
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
    button.search-btn {
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 5px;
      margin-left: 10px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      transition: transform 0.2s ease;
    }
    button.search-btn:hover {
      transform: translateY(-3px);
    }
    .author-line {
      font-size: 14px;
      color: #333;
    }
  </style>
</head>
<body>
  <h1>Search Articles and Author Details</h1>

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

      <div class="search-container">
        <input type="text" id="orcidQuery" placeholder="Enter ORCID iD (e.g., 0000-0002-1825-0097)">
        <button class="search-btn" onclick="searchOrcid()">Search ORCID</button>
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
          // Use Office Online viewer for Word documents
          viewer.src = `https://view.officeapps.live.com/op/embed.aspx?src=${encodeURIComponent(fileURL)}`;
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
      const doiPattern = /10\.\d{4,9}\/[-._;()/:A-Z0-9]+/i;
      const doi = query.match(doiPattern) ? query.match(doiPattern)[0] : null;

      let crossRefUrl;

      if (doi) {
        crossRefUrl = `https://api.crossref.org/works/${encodeURIComponent(doi)}`;
      } else {
        const sanitizedQuery = query.replace(/[.,]/g, '').split(' ').join('+');
        crossRefUrl = `https://api.crossref.org/works?query=${encodeURIComponent(sanitizedQuery)}&rows=5`;
      }

      fetch(crossRefUrl)
        .then(response => response.json())
        .then(data => {
          document.getElementById('loading').style.display = 'none';

          const resultsContainer = document.getElementById('results');
          resultsContainer.innerHTML = '';

          let works = data.message.items || (data.message.title ? [data.message] : []);
          if (works.length > 0) {
            works.forEach(work => {
              const title = work.title ? work.title[0] : 'Untitled';
              const authors = work.author || [];
              const doiLink = work.DOI || null;

              const authorNames = authors.map(author => `${author.given} ${author.family}`).join(', ');
              const authorLinks = authors.map(author => 
                `<a href="https://scholar.google.com/scholar?q=${encodeURIComponent(author.given + ' ' + author.family)}" target="_blank">${author.given} ${author.family}</a>`
              ).join(', ');

              const doiButton = doiLink ? `<a href="https://doi.org/${doiLink}" target="_blank">DOI</a>` : '';
              const resultItem = `
                <div class="result-item">
                  <h3><a href="https://www.google.com/search?q=${encodeURIComponent(title)}" target="_blank">${title}</a></h3>
                  <p class="author-line">by: ${authorLinks}</p>
                  <div>${doiButton}</div>
                </div>
              `;
              resultsContainer.innerHTML += resultItem;
            });
          } else {
            resultsContainer.innerHTML = `<p>No results found.</p>`;
          }
        })
        .catch(error => {
          console.error('Error fetching data:', error);
          alert('An error occurred while fetching data.');
          document.getElementById('loading').style.display = 'none';
        });
    }

    // Function to search for author details using ORCID API
    function searchOrcid() {
      const orcidId = document.getElementById('orcidQuery').value.trim();
      if (!orcidId) {
        alert('Please enter an ORCID iD');
        return;
      }

      const orcidUrl = `https://pub.orcid.org/v3.0/${encodeURIComponent(orcidId)}`;

      fetch(orcidUrl, {
        headers: {
          'Accept': 'application/json'  // ORCID API requires this header for JSON responses
        }
      })
      .then(response => response.json())
      .then(data => {
        const resultsContainer = document.getElementById('results');
        resultsContainer.innerHTML = '';  // Clear previous results

        const name = data['person']['name']['given-names']['value'] + ' ' + data['person']['name']['family-name']['value'];
        const email = data['person']['emails']['email'][0]?.email || 'Email not public';
        const address = data['person']['addresses']['address'][0]?.city || 'Address not available';

        const resultItem = `
          <div class="result-item">
            <h3>${name}</h3>
            <p><strong>Email:</strong> ${email}</p>
            <p><strong>Address:</strong> ${address}</p>
          </div>
        `;
        resultsContainer.innerHTML += resultItem;
      })
      .catch(error => {
        console.error('Error fetching data:', error);
        alert('An error occurred while fetching ORCID data.');
      });
    }
  </script>
</body>
</html>
