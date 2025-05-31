<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Country Entry Filter Tool</title>
  <style>
    :root {
      --primary-color: #3498db;
      --primary-hover: #2980b9;
      --secondary-color: #e74c3c;
      --secondary-hover: #c0392b;
      --success-color: #2ecc71;
      --success-hover: #27ae60;
      --light-gray: #ecf0f1;
      --dark-gray: #7f8c8d;
      --shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    }
    
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: #f4f6f8;
      padding: 20px;
      color: #333;
      max-width: 1200px;
      margin: 0 auto;
    }
    
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }
    
    h2 {
      margin: 0;
      color: #2c3e50;
    }
    
    .refresh-btn {
      background-color: var(--light-gray);
      color: var(--dark-gray);
      border: none;
      padding: 8px 15px;
      border-radius: 5px;
      cursor: pointer;
      font-size: 14px;
      display: flex;
      align-items: center;
      gap: 5px;
    }
    
    .refresh-btn:hover {
      background-color: #dfe6e9;
    }
    
    .input-section, .filter-section, .output-section {
      background: white;
      border-radius: 8px;
      padding: 20px;
      box-shadow: var(--shadow);
      margin-bottom: 20px;
    }
    
    .section-title {
      font-size: 18px;
      margin-top: 0;
      margin-bottom: 15px;
      color: #2c3e50;
      border-bottom: 1px solid #eee;
      padding-bottom: 10px;
    }
    
    label {
      font-weight: 600;
      display: block;
      margin-bottom: 8px;
      font-size: 14px;
    }
    
    select, textarea, input[type="file"] {
      width: 100%;
      padding: 10px;
      font-size: 14px;
      margin-bottom: 15px;
      border: 1px solid #ddd;
      border-radius: 5px;
      box-sizing: border-box;
    }
    
    select[multiple] {
      height: 200px;
    }
    
    .btn {
      padding: 8px 15px;
      font-size: 14px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      margin-right: 10px;
      margin-bottom: 10px;
      transition: background-color 0.2s;
      display: inline-flex;
      align-items: center;
      gap: 5px;
    }
    
    .btn-primary {
      background-color: var(--primary-color);
      color: white;
    }
    
    .btn-primary:hover {
      background-color: var(--primary-hover);
    }
    
    .btn-secondary {
      background-color: var(--secondary-color);
      color: white;
    }
    
    .btn-secondary:hover {
      background-color: var(--secondary-hover);
    }
    
    .btn-success {
      background-color: var(--success-color);
      color: white;
    }
    
    .btn-success:hover {
      background-color: var(--success-hover);
    }
    
    .btn-outline {
      background-color: white;
      color: var(--primary-color);
      border: 1px solid var(--primary-color);
    }
    
    .btn-outline:hover {
      background-color: var(--light-gray);
    }
    
    .btn-group {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 15px;
    }
    
    .counter {
      background: #ffffff;
      border: 1px solid #ddd;
      padding: 15px;
      margin-bottom: 20px;
      border-radius: 8px;
      display: flex;
      justify-content: space-between;
      font-weight: 600;
      box-shadow: var(--shadow);
    }
    
    .entry {
      background: white;
      border-radius: 6px;
      padding: 15px;
      box-shadow: var(--shadow);
      margin-bottom: 15px;
      white-space: pre-line;
      border-left: 4px solid var(--primary-color);
    }
    
    .two-column {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }
    
    @media (max-width: 768px) {
      .two-column {
        grid-template-columns: 1fr;
      }
    }
    
    .group-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px;
      border-bottom: 1px solid #eee;
    }
    
    .group-item:last-child {
      border-bottom: none;
    }
    
    .delete-group {
      color: var(--secondary-color);
      cursor: pointer;
      font-size: 14px;
    }
    
    .delete-group:hover {
      color: var(--secondary-hover);
    }
    
    .icon {
      width: 16px;
      height: 16px;
    }
    
    .group-countries {
      margin-top: 10px;
      font-size: 12px;
      color: #666;
      background: #f9f9f9;
      padding: 10px;
      border-radius: 5px;
      display: none;
    }
    
    .country-count {
      display: inline-block;
      margin-right: 10px;
      margin-bottom: 5px;
    }
    
    .download-btn {
      margin-top: 15px;
    }
    
    .country-filter-btn {
      width: auto;
      padding: 8px 12px;
      font-size: 13px;
      margin-top: -10px;
      margin-bottom: 15px;
    }
    
    .filter-name {
      font-weight: normal;
      color: var(--primary-color);
    }
  </style>
</head>
<body>
  <div class="header">
    <h2>Country-Based Entry Filtering Tool</h2>
    <button class="refresh-btn" onclick="clearAll()">
      <svg class="icon" viewBox="0 0 24 24" fill="none" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
      </svg>
      Refresh
    </button>
  </div>

  <div class="input-section">
    <h3 class="section-title">Input Data</h3>
    <label for="fileInput">Upload File:</label>
    <input type="file" id="fileInput" accept=".txt">
    
    <label for="manualInput">Or Paste Entries:</label>
    <textarea id="manualInput" rows="5" placeholder="Paste entries here, separated by blank lines..."></textarea>
    
    <div class="btn-group">
      <button class="btn btn-primary" onclick="loadEntries()">Load Entries</button>
      <button class="btn btn-outline" onclick="document.getElementById('manualInput').value = ''">Clear Text</button>
    </div>
  </div>

  <div class="filter-section">
    <h3 class="section-title">Filter Options</h3>
    
    <div class="two-column">
      <div>
        <label for="groupSelect">Select Group:</label>
        <select id="groupSelect">
          <option value="">-- None --</option>
        </select>
        
        <div id="groupCountries" class="group-countries"></div>
        
        <div id="userGroupsContainer" style="margin-top: 15px; display: none;">
          <label>Your Groups:</label>
          <div id="userGroupsList" style="background: var(--light-gray); padding: 10px; border-radius: 5px;"></div>
        </div>
      </div>
      
      <div>
        <label for="countrySelect">Select Countries:</label>
        <select id="countrySelect" multiple size="10"></select>
        <button class="btn btn-primary country-filter-btn" onclick="applyCountryFilter()">Filter by Selected Countries</button>
      </div>
    </div>
    
    <div class="btn-group">
      <button class="btn btn-success" onclick="copyVisibleEntries()">Copy Visible Entries</button>
      <button class="btn btn-outline" onclick="createNewGroup()">Create New Group</button>
    </div>
  </div>

  <div class="counter">
    <span>Total Entries: <span id="totalCount" style="color: var(--primary-color)">0</span></span>
    <span id="filteredLabel">Filtered Entries: <span id="filteredCount" style="color: var(--primary-color)">0</span></span>
  </div>

  <button id="downloadBtn" class="btn btn-success download-btn" onclick="downloadFilteredEntries()" style="display: none;">
    Download Filtered Entries (TXT)
  </button>

  <div class="output-section">
    <h3 class="section-title">Results</h3>
    <div id="entriesContainer"></div>
  </div>

  <script>
    // Country list with standardized names
    const countryList = [
      "Afghanistan", "Albania", "Algeria", "Andorra", "Angola", "Antigua and Barbuda", 
      "Argentina", "Armenia", "Australia", "Austria", "Azerbaijan", "Bahamas", "Bahrain", 
      "Bangladesh", "Barbados", "Belarus", "Belgium", "Belize", "Benin", "Bhutan", 
      "Bolivia", "Bosnia and Herzegovina", "Botswana", "Brazil", "Brasil", "Brunei", 
      "Bulgaria", "Burkina Faso", "Burundi", "Cabo Verde", "Cambodia", "Cameroon", 
      "Canada", "Central African Republic", "Chad", "Tchad", "Chile", "China", 
      "Colombia", "Comoros", "Congo", "Costa Rica", "Cote d'Ivoire", "Côte d'Ivoire", 
      "Cote D'Ivoire", "Ivory Coast", "Croatia", "Cuba", "Cyprus", "Czech Republic", 
      "Denmark", "Djibouti", "Dominica", "Dominican Republic", "Ecuador", "Egypt", 
      "El Salvador", "Equatorial Guinea", "Eritrea", "Estonia", "Eswatini", "Ethiopia", 
      "Fiji", "Finland", "France", "Gabon", "Gambia", "Georgia", "Germany", "Ghana", 
      "Greece", "Grenada", "Guatemala", "Guinea", "Guinea-Bissau", "Guyana", "Haiti", 
      "Honduras", "Hungary", "Iceland", "India", "Indonesia", "Iran", "Iraq", "Ireland", 
      "Israel", "Italy", "Jamaica", "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati", 
      "Korea", "South Korea", "Kuwait", "Kyrgyzstan", "Laos", "Latvia", "Lebanon", 
      "Lesotho", "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg", 
      "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta", "Marshall Islands", 
      "Mauritania", "Mauritius", "Mexico", "Micronesia", "Moldova", "Monaco", "Mongolia", 
      "Montenegro", "Morocco", "Mozambique", "Myanmar", "Burma", "Namibia", "Nauru", 
      "Nepal", "Netherlands", "New Zealand", "Nicaragua", "Niger", "Nigeria", 
      "North Macedonia", "Macedonia", "Norway", "Oman", "Pakistan", "Palau", "Palestine", 
      "Panama", "Papua New Guinea", "Paraguay", "Peru", "Philippines", "Poland", 
      "Portugal", "Qatar", "Romania", "Russia", "Rwanda", "Saint Kitts and Nevis", 
      "Saint Lucia", "Saint Vincent and the Grenadines", "Samoa", "San Marino", 
      "Sao Tome and Principe", "Saudi Arabia", "Senegal", "Serbia", "Seychelles", 
      "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands", "Somalia", 
      "South Africa", "Spain", "Sri Lanka", "Sudan", "Suriname", "Sweden", "Switzerland", 
      "Syria", "Taiwan", "Tajikistan", "Tanzania", "Thailand", "Timor-Leste", "Togo", 
      "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey", "Turkmenistan", "Tuvalu", 
      "Uganda", "Ukraine", "United Arab Emirates", "UAE", "U.A.E.", "U. A. E", "U. A. E.", 
      "United Kingdom", "UK", "U.K.", "United States", "USA", "U.S.A.", "U.S.A", "U. S. A.", 
      "U. S. A", "Uruguay", "Uzbekistan", "Vanuatu", "Vatican City", "Venezuela", 
      "Vietnam", "Viet Nam", "Yemen", "Zambia", "Zimbabwe", "Hong Kong", "Macau", "Macao"
    ];

    // Country name standardization map
    const countryMap = {
      "USA": "United States",
      "U.S.A.": "United States",
      "U.S.A": "United States",
      "U. S. A.": "United States",
      "U. S. A": "United States",
      "UK": "United Kingdom",
      "U.K.": "United Kingdom",
      "Korea": "South Korea",
      "UAE": "United Arab Emirates",
      "U.A.E.": "United Arab Emirates",
      "U. A. E": "United Arab Emirates",
      "U. A. E.": "United Arab Emirates",
      "Hongkong": "Hong Kong",
      "Ivory Coast": "Côte d'Ivoire",
      "Cote d'Ivoire": "Côte d'Ivoire",
      "Cote D'Ivoire": "Côte d'Ivoire",
      "Macau": "Macao",
      "Macedonia": "North Macedonia",
      "Burma": "Myanmar",
      "Viet Nam": "Vietnam",
      "Tchad": "Chad",
      "Brasil": "Brazil"
    };

    // Default country groups
    const defaultGroups = {
      "A - Japan Group": ["Japan", "South Korea", "Taiwan", "Thailand"],
      "B - African Group": ["Egypt", "Kenya", "Morocco", "Nigeria", "South Africa"],
      "C - Prime Group": ["Brazil", "Mexico", "Russia", "Saudi Arabia"],
      "D - European Group": ["France", "Germany", "Italy", "Spain", "UK"],
      "E - Chinese Group": ["China", "Hong Kong"],
      "F - Indian Group": ["India"],
      "G - US Group": ["United States", "Canada"],
      "H - Other Countries": ["Australia", "Argentina", "Israel"]
    };
    
    // Load user groups from localStorage or initialize empty object
    let userGroups = JSON.parse(localStorage.getItem('userGroups')) || {};
    let countryGroups = { ...defaultGroups, ...userGroups };
    let entries = '';
    let allParts = [];
    let currentFilteredEntries = [];
    let currentFilterName = '';

    // Initialize the page when loaded
    document.addEventListener('DOMContentLoaded', function() {
      populateDropdowns();
      document.getElementById('manualInput').addEventListener('input', function() {
        if (this.value.trim()) {
          entries = this.value.trim();
          allParts = entries.split(/\n\n+/);
          renderEntries(() => true);
        }
      });
    });

    // Improved country matching function
    function entryContainsCountry(entry, country) {
      const standardizedCountry = countryMap[country] || country;
      const patterns = [
        new RegExp(`\\b${standardizedCountry}\\s*$`, 'im'),
        new RegExp(`\\b${standardizedCountry}\\b`, 'im'),
        ...(country !== standardizedCountry ? [
          new RegExp(`\\b${country}\\s*$`, 'im'),
          new RegExp(`\\b${country}\\b`, 'im')
        ] : [])
      ];
      return patterns.some(pattern => pattern.test(entry));
    }

    // Count entries matching a specific country
    function countEntriesForCountry(country) {
      if (!allParts.length) return 0;
      return allParts.filter(entry => entryContainsCountry(entry, country)).length;
    }

    // Populate dropdowns with countries and groups
    function populateDropdowns() {
      const groupSelect = document.getElementById('groupSelect');
      const countrySelect = document.getElementById('countrySelect');
      const userGroupsList = document.getElementById('userGroupsList');
      
      // Clear existing options
      groupSelect.innerHTML = '<option value="">-- None --</option>';
      countrySelect.innerHTML = '';
      userGroupsList.innerHTML = '';

      // Add default groups
      for (let group in defaultGroups) {
        const option = document.createElement('option');
        option.value = group;
        option.textContent = group;
        groupSelect.appendChild(option);
      }

      // Add user groups
      let hasUserGroups = false;
      for (let group in userGroups) {
        hasUserGroups = true;
        const option = document.createElement('option');
        option.value = group;
        option.textContent = group;
        groupSelect.appendChild(option);
        
        // Add to user groups list
        const groupItem = document.createElement('div');
        groupItem.className = 'group-item';
        groupItem.innerHTML = `
          <span>${group}</span>
          <span class="delete-group" onclick="deleteGroup('${group}')">Delete</span>
        `;
        userGroupsList.appendChild(groupItem);
      }

      // Show/hide user groups section
      document.getElementById('userGroupsContainer').style.display = hasUserGroups ? 'block' : 'none';

      // Add create new group option
      const customOption = document.createElement('option');
      customOption.value = '__create__';
      customOption.textContent = '+ Create New Group';
      groupSelect.appendChild(customOption);

      // Populate country select with sorted countries
      countryList.sort().forEach(country => {
        const option = document.createElement('option');
        option.value = country;
        option.textContent = country;
        countrySelect.appendChild(option);
      });
    }

    // Render entries based on filter function
    function renderEntries(filterFn, filterName = '') {
      const container = document.getElementById('entriesContainer');
      container.innerHTML = '';
      let count = 0;
      currentFilteredEntries = [];
      currentFilterName = filterName;
      
      allParts.forEach(entry => {
        if (filterFn(entry)) {
          const div = document.createElement('div');
          div.className = 'entry';
          div.textContent = entry.trim();
          container.appendChild(div);
          count++;
          currentFilteredEntries.push(entry.trim());
        }
      });
      
      updateCounters(count);
      document.getElementById('downloadBtn').style.display = count > 0 ? 'block' : 'none';
    }

    // Update group countries display
    function updateGroupCountriesDisplay(groupName) {
      const groupCountriesDiv = document.getElementById('groupCountries');
      if (!groupName || !countryGroups[groupName]) {
        groupCountriesDiv.style.display = 'none';
        return;
      }
      
      const countries = countryGroups[groupName];
      let html = '';
      
      countries.forEach(country => {
        const count = countEntriesForCountry(country);
        html += `<span class="country-count">${country} (${count})</span>`;
      });
      
      groupCountriesDiv.innerHTML = html;
      groupCountriesDiv.style.display = 'block';
    }

    // Load entries from textarea
    function loadEntries() {
      const manualText = document.getElementById('manualInput').value;
      if (manualText.trim()) {
        entries = manualText.trim();
        allParts = entries.split(/\n\n+/);
        renderEntries(() => true);
      }
    }

    // Handle file upload
    document.getElementById('fileInput').addEventListener('change', function() {
      const file = this.files[0];
      if (!file) return;
      
      const reader = new FileReader();
      reader.onload = function(e) {
        entries = e.target.result;
        allParts = entries.split(/\n\n+/);
        renderEntries(() => true);
        document.getElementById('manualInput').value = entries;
      };
      reader.readAsText(file);
    });

    // Handle group selection change
    document.getElementById('groupSelect').addEventListener('change', function() {
      const val = this.value;
      if (val === '__create__') {
        createNewGroup();
        return;
      }
      
      updateGroupCountriesDisplay(val);
      
      if (val && countryGroups[val]) {
        renderEntries(entry => countryGroups[val].some(c => entryContainsCountry(entry, c)), val);
      } else {
        renderEntries(() => true);
      }
    });

    // Apply country filter
    function applyCountryFilter() {
      document.getElementById('groupSelect').value = '';
      document.getElementById('groupCountries').style.display = 'none';
      const selectedOptions = Array.from(document.getElementById('countrySelect').selectedOptions).map(opt => opt.value);
      const filterName = selectedOptions.length === 1 ? selectedOptions[0] : 
                        selectedOptions.length > 1 ? 'Selected Countries' : '';
      renderEntries(entry => selectedOptions.some(country => entryContainsCountry(entry, country)), filterName);
    }

    // Copy visible entries to clipboard
    function copyVisibleEntries() {
      const visibleEntries = Array.from(document.querySelectorAll('.entry')).map(div => div.textContent).join('\n\n');
      navigator.clipboard.writeText(visibleEntries).then(() => {
        alert('Copied to clipboard!');
      }).catch(err => {
        alert('Failed to copy: ' + err);
      });
    }

    // Download filtered entries
    function downloadFilteredEntries() {
      if (currentFilteredEntries.length === 0) return;
      
      const filename = currentFilterName 
        ? `${currentFilterName.replace(/[^a-z0-9]/gi, '_')}_entries.txt` 
        : 'filtered_entries.txt';
      
      const blob = new Blob([currentFilteredEntries.join('\n\n')], { type: 'text/plain' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = filename;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
    }

    // Clear all and refresh page
    function clearAll() {
      localStorage.removeItem('userGroups');
      location.reload();
    }

    // Create new country group
    function createNewGroup() {
      const groupName = prompt("Enter new group name:");
      if (!groupName) return;
      
      const selected = prompt("Enter comma-separated country names:\n(" + countryList.join(', ') + ")");
      if (!selected) return;
      
      const countryListNew = selected.split(',').map(s => s.trim()).filter(Boolean);
      userGroups[groupName] = countryListNew;
      localStorage.setItem('userGroups', JSON.stringify(userGroups));
      countryGroups = { ...defaultGroups, ...userGroups };
      populateDropdowns();
      document.getElementById('groupSelect').value = groupName;
      updateGroupCountriesDisplay(groupName);
      renderEntries(entry => countryListNew.some(c => entryContainsCountry(entry, c)), groupName;
    }

    // Delete a user group
    function deleteGroup(groupName) {
      if (confirm(`Are you sure you want to delete the group "${groupName}"?`)) {
        delete userGroups[groupName];
        localStorage.setItem('userGroups', JSON.stringify(userGroups));
        countryGroups = { ...defaultGroups, ...userGroups };
        populateDropdowns();
        renderEntries(() => true);
        document.getElementById('groupCountries').style.display = 'none';
      }
    }

    // Update counters with current filter status
    function updateCounters(filteredCount = 0) {
      document.getElementById('totalCount').textContent = allParts.length;
      document.getElementById('filteredCount').textContent = filteredCount;
      
      const filteredLabel = document.getElementById('filteredLabel');
      if (currentFilterName) {
        filteredLabel.innerHTML = `Filtered (<span class="filter-name">${currentFilterName}</span>): <span id="filteredCount" style="color: var(--primary-color)">${filteredCount}</span>`;
      } else {
        filteredLabel.innerHTML = `Filtered Entries: <span id="filteredCount" style="color: var(--primary-color)">${filteredCount}</span>`;
      }
    }
  </script>
</body>
</html>
