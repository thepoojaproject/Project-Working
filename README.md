
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LinkVault - Website Link Manager</title>
    <link rel="icon" type="image/x-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🔗</text></svg>">
    <style>
        :root {
            --primary: #4361ee;
            --secondary: #3a0ca3;
            --accent: #7209b7;
            --light: #f8f9fa;
            --dark: #212529;
            --success: #4cc9f0;
            --danger: #f72585;
            --warning: #f8961e;
            --border-radius: 8px;
            --box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            color: var(--dark);
            line-height: 1.6;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
        }
        
        h1 {
            color: var(--primary);
            margin-bottom: 10px;
        }
        
        .tagline {
            color: var(--secondary);
            font-size: 1.1rem;
        }
        
        .app-container {
            display: grid;
            grid-template-columns: 1fr 2fr;
            gap: 20px;
        }
        
        @media (max-width: 768px) {
            .app-container {
                grid-template-columns: 1fr;
            }
        }
        
        .form-section, .links-section {
            background: white;
            padding: 25px;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--secondary);
        }
        
        input, select, textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            font-size: 1rem;
            transition: border 0.3s;
        }
        
        input:focus, select:focus, textarea:focus {
            border-color: var(--primary);
            outline: none;
            box-shadow: 0 0 0 2px rgba(67, 97, 238, 0.2);
        }
        
        .input-error {
            border-color: var(--danger);
        }
        
        .error-message {
            color: var(--danger);
            font-size: 0.85rem;
            margin-top: 5px;
            display: none;
        }
        
        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s;
        }
        
        button:hover {
            background: var(--secondary);
            transform: translateY(-2px);
        }
        
        button:disabled {
            background: #cccccc;
            cursor: not-allowed;
            transform: none;
        }
        
        .btn-danger {
            background: var(--danger);
        }
        
        .btn-danger:hover {
            background: #d1145a;
        }
        
        .btn-warning {
            background: var(--warning);
        }
        
        .btn-warning:hover {
            background: #e07c0c;
        }
        
        .btn-secondary {
            background: #6c757d;
        }
        
        .btn-secondary:hover {
            background: #5a6268;
        }
        
        .links-list {
            max-height: 600px;
            overflow-y: auto;
        }
        
        .link-item {
            padding: 15px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: background-color 0.2s;
        }
        
        .link-item:hover {
            background-color: #f8f9fa;
        }
        
        .link-item:last-child {
            border-bottom: none;
        }
        
        .link-info h3 {
            margin-bottom: 5px;
            color: var(--primary);
        }
        
        .link-url {
            color: var(--accent);
            text-decoration: none;
            font-size: 0.9rem;
            word-break: break-all;
        }
        
        .link-url:hover {
            text-decoration: underline;
        }
        
        .link-category {
            display: inline-block;
            background: var(--success);
            color: white;
            padding: 3px 8px;
            border-radius: 20px;
            font-size: 0.8rem;
            margin-top: 5px;
        }
        
        .link-actions {
            display: flex;
            gap: 10px;
        }
        
        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: #6c757d;
        }
        
        .empty-state i {
            font-size: 3rem;
            margin-bottom: 15px;
            color: #dee2e6;
        }
        
        .search-box {
            margin-bottom: 20px;
        }
        
        .stats {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
            text-align: center;
        }
        
        .stat-item {
            padding: 15px;
        }
        
        .stat-number {
            font-size: 2rem;
            font-weight: bold;
            color: var(--primary);
        }
        
        .stat-label {
            font-size: 0.9rem;
            color: #6c757d;
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            padding: 20px;
            color: #6c757d;
            font-size: 0.9rem;
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            background: var(--success);
            color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            transform: translateX(150%);
            transition: transform 0.3s ease;
            z-index: 1000;
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        .notification.error {
            background: var(--danger);
        }
        
        .notification.warning {
            background: var(--warning);
        }
        
        .filter-options {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        
        .filter-btn {
            background: #e9ecef;
            color: var(--dark);
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s;
        }
        
        .filter-btn.active {
            background: var(--primary);
            color: white;
        }
        
        .utility-buttons {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 2000;
            align-items: center;
            justify-content: center;
        }
        
        .modal-content {
            background: white;
            padding: 30px;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            width: 90%;
            max-width: 500px;
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .modal-close {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: #6c757d;
        }
        
        /* Heartbeat Animation */
        .heartbeat {
            display: inline-block;
            position: relative;
            animation: heartbeat 1.5s infinite;
        }
        
        @keyframes heartbeat {
            0% {
                transform: scale(1);
            }
            5% {
                transform: scale(1.1);
            }
            10% {
                transform: scale(1);
            }
            15% {
                transform: scale(1.2);
            }
            50% {
                transform: scale(1);
            }
            100% {
                transform: scale(1);
            }
        }
        
        .signature {
            font-size: 1rem;
            font-weight: 600;
            color: var(--accent);
            margin-top: 10px;
        }
        
        .sort-options {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            flex-wrap: wrap;
        }
        
        .sort-btn {
            background: #e9ecef;
            color: var(--dark);
            border: none;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s;
        }
        
        .sort-btn.active {
            background: var(--accent);
            color: white;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>LinkVault</h1>
            <p class="tagline">Store and organize all your website links in one place</p>
        </header>
        
        <div class="app-container">
            <section class="form-section">
                <h2 id="form-title">Add New Link</h2>
                <form id="linkForm">
                    <input type="hidden" id="editId" value="">
                    
                    <div class="form-group">
                        <label for="linkTitle">Title</label>
                        <input type="text" id="linkTitle" placeholder="Enter website title" required>
                        <div class="error-message" id="titleError">Title is required</div>
                    </div>
                    
                    <div class="form-group">
                        <label for="linkUrl">URL</label>
                        <input type="url" id="linkUrl" placeholder="https://example.com" required>
                        <div class="error-message" id="urlError">Please enter a valid URL</div>
                    </div>
                    
                    <div class="form-group">
                        <label for="linkCategory">Category</label>
                        <select id="linkCategory">
                            <option value="work">Work</option>
                            <option value="personal">Personal</option>
                            <option value="entertainment">Entertainment</option>
                            <option value="education">Education</option>
                            <option value="shopping">Shopping</option>
                            <option value="other">Other</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label for="linkDescription">Description (Optional)</label>
                        <textarea id="linkDescription" rows="3" placeholder="Add a brief description"></textarea>
                    </div>
                    
                    <button type="submit" id="submitBtn">Save Link</button>
                    <button type="button" id="cancelEditBtn" style="display: none;" class="btn-secondary">Cancel Edit</button>
                </form>
                
                <div class="stats">
                    <div class="stat-item">
                        <div class="stat-number" id="totalLinks">0</div>
                        <div class="stat-label">Total Links</div>
                    </div>
                    <div class="stat-item">
                        <div class="stat-number" id="categoriesCount">0</div>
                        <div class="stat-label">Categories</div>
                    </div>
                </div>
                
                <div class="utility-buttons">
                    <button type="button" id="exportBtn" class="btn-secondary">Export Links</button>
                    <button type="button" id="importBtn" class="btn-secondary">Import Links</button>
                    <button type="button" id="clearAllBtn" class="btn-danger">Clear All</button>
                </div>
            </section>
            
            <section class="links-section">
                <div class="search-box">
                    <input type="text" id="searchInput" placeholder="Search your links...">
                </div>
                
                <div class="filter-options">
                    <button class="filter-btn active" data-category="all">All</button>
                    <button class="filter-btn" data-category="work">Work</button>
                    <button class="filter-btn" data-category="personal">Personal</button>
                    <button class="filter-btn" data-category="entertainment">Entertainment</button>
                    <button class="filter-btn" data-category="education">Education</button>
                    <button class="filter-btn" data-category="shopping">Shopping</button>
                    <button class="filter-btn" data-category="other">Other</button>
                </div>
                
                <div class="sort-options">
                    <button class="sort-btn active" data-sort="newest">Newest First</button>
                    <button class="sort-btn" data-sort="oldest">Oldest First</button>
                    <button class="sort-btn" data-sort="title">Title A-Z</button>
                </div>
                
                <div class="links-list" id="linksList">
                    <!-- Links will be dynamically added here -->
                    <div class="empty-state">
                        <i>🔗</i>
                        <h3>No links saved yet</h3>
                        <p>Add your first website link using the form on the left</p>
                    </div>
                </div>
            </section>
        </div>
        
        <footer>
            <p class="signature">Made with <span class="heartbeat">💖</span> By Armeen</p>
        </footer>
    </div>

    <div class="notification" id="notification">Link added successfully!</div>
    
    <!-- Import Modal -->
    <div class="modal" id="importModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Import Links</h2>
                <button class="modal-close" id="closeImportModal">&times;</button>
            </div>
            <p>Paste your exported links JSON data:</p>
            <textarea id="importData" rows="10" style="width: 100%; margin: 15px 0;"></textarea>
            <div class="error-message" id="importError" style="display: none;">Invalid JSON format</div>
            <button type="button" id="confirmImportBtn" class="btn-warning">Import</button>
        </div>
    </div>

    <script>
        // Sample initial data
        let links = JSON.parse(localStorage.getItem('links')) || [
            {
                id: 1,
                title: "Google",
                url: "https://www.google.com",
                category: "work",
                description: "Search engine",
                dateAdded: new Date('2023-01-01').getTime()
            },
            {
                id: 2,
                title: "YouTube",
                url: "https://www.youtube.com",
                category: "entertainment",
                description: "Video sharing platform",
                dateAdded: new Date('2023-01-02').getTime()
            },
            {
                id: 3,
                title: "GitHub",
                url: "https://www.github.com",
                category: "work",
                description: "Code hosting platform",
                dateAdded: new Date('2023-01-03').getTime()
            }
        ];

        // DOM Elements
        const linkForm = document.getElementById('linkForm');
        const linksList = document.getElementById('linksList');
        const searchInput = document.getElementById('searchInput');
        const totalLinksEl = document.getElementById('totalLinks');
        const categoriesCountEl = document.getElementById('categoriesCount');
        const notification = document.getElementById('notification');
        const filterButtons = document.querySelectorAll('.filter-btn');
        const sortButtons = document.querySelectorAll('.sort-btn');
        const exportBtn = document.getElementById('exportBtn');
        const importBtn = document.getElementById('importBtn');
        const clearAllBtn = document.getElementById('clearAllBtn');
        const importModal = document.getElementById('importModal');
        const closeImportModal = document.getElementById('closeImportModal');
        const confirmImportBtn = document.getElementById('confirmImportBtn');
        const importData = document.getElementById('importData');
        const importError = document.getElementById('importError');
        const formTitle = document.getElementById('form-title');
        const submitBtn = document.getElementById('submitBtn');
        const cancelEditBtn = document.getElementById('cancelEditBtn');
        const editId = document.getElementById('editId');

        // Current filter and sort state
        let currentFilter = 'all';
        let currentSearchTerm = '';
        let currentSort = 'newest';

        // Initialize the app
        function init() {
            renderLinks();
            updateStats();
            
            // Event listeners
            linkForm.addEventListener('submit', handleFormSubmit);
            searchInput.addEventListener('input', handleSearch);
            
            // Filter buttons
            filterButtons.forEach(button => {
                button.addEventListener('click', () => {
                    // Update active state
                    filterButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    
                    // Set current filter
                    currentFilter = button.dataset.category;
                    applyFilters();
                });
            });
            
            // Sort buttons
            sortButtons.forEach(button => {
                button.addEventListener('click', () => {
                    // Update active state
                    sortButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    
                    // Set current sort
                    currentSort = button.dataset.sort;
                    applyFilters();
                });
            });
            
            // Utility buttons
            exportBtn.addEventListener('click', exportLinks);
            importBtn.addEventListener('click', () => importModal.style.display = 'flex');
            clearAllBtn.addEventListener('click', clearAllLinks);
            closeImportModal.addEventListener('click', () => importModal.style.display = 'none');
            confirmImportBtn.addEventListener('click', importLinks);
            cancelEditBtn.addEventListener('click', cancelEdit);
            
            // Close modal when clicking outside
            window.addEventListener('click', (e) => {
                if (e.target === importModal) {
                    importModal.style.display = 'none';
                }
            });
        }

        // Handle form submission
        function handleFormSubmit(e) {
            e.preventDefault();
            
            const title = document.getElementById('linkTitle').value.trim();
            const url = document.getElementById('linkUrl').value.trim();
            const category = document.getElementById('linkCategory').value;
            const description = document.getElementById('linkDescription').value.trim();
            const id = editId.value;
            
            // Validate inputs
            if (!validateForm(title, url)) {
                return;
            }
            
            // Check for duplicate URL (excluding the current one being edited)
            if (isDuplicateUrl(url, id)) {
                showError('urlError', 'This URL already exists in your links');
                return;
            }
            
            if (id) {
                // Update existing link
                updateLink(id, title, url, category, description);
            } else {
                // Create new link object
                const newLink = {
                    id: Date.now(), // Simple ID generation
                    title,
                    url,
                    category,
                    description,
                    dateAdded: Date.now()
                };
                
                // Add to links array
                links.push(newLink);
                
                // Show notification
                showNotification('Link added successfully!');
            }
            
            // Save to localStorage
            saveToLocalStorage();
            
            // Re-render the links list
            renderLinks();
            updateStats();
            
            // Reset form
            resetForm();
        }

        // Validate form inputs
        function validateForm(title, url) {
            let isValid = true;
            
            // Validate title
            if (!title) {
                showError('titleError', 'Title is required');
                isValid = false;
            } else {
                hideError('titleError');
            }
            
            // Validate URL
            if (!url) {
                showError('urlError', 'URL is required');
                isValid = false;
            } else if (!isValidUrl(url)) {
                showError('urlError', 'Please enter a valid URL');
                isValid = false;
            } else {
                hideError('urlError');
            }
            
            return isValid;
        }

        // Check if URL is valid
        function isValidUrl(string) {
            try {
                new URL(string);
                return true;
            } catch (_) {
                return false;
            }
        }

        // Check for duplicate URL
        function isDuplicateUrl(url, excludeId = null) {
            return links.some(link => {
                if (excludeId && link.id == excludeId) return false;
                return link.url.toLowerCase() === url.toLowerCase();
            });
        }

        // Show error message
        function showError(elementId, message) {
            const element = document.getElementById(elementId);
            element.textContent = message;
            element.style.display = 'block';
            document.getElementById(elementId.replace('Error', '')).classList.add('input-error');
        }

        // Hide error message
        function hideError(elementId) {
            const element = document.getElementById(elementId);
            element.style.display = 'none';
            document.getElementById(elementId.replace('Error', '')).classList.remove('input-error');
        }

        // Update existing link
        function updateLink(id, title, url, category, description) {
            const index = links.findIndex(link => link.id == id);
            if (index !== -1) {
                links[index] = {
                    ...links[index],
                    title,
                    url,
                    category,
                    description
                };
                showNotification('Link updated successfully!');
            }
        }

        // Edit a link
        function editLink(id) {
            const link = links.find(link => link.id == id);
            if (link) {
                document.getElementById('linkTitle').value = link.title;
                document.getElementById('linkUrl').value = link.url;
                document.getElementById('linkCategory').value = link.category;
                document.getElementById('linkDescription').value = link.description || '';
                editId.value = link.id;
                
                // Update UI for edit mode
                formTitle.textContent = 'Edit Link';
                submitBtn.textContent = 'Update Link';
                cancelEditBtn.style.display = 'inline-block';
                
                // Scroll to form
                document.querySelector('.form-section').scrollIntoView({ behavior: 'smooth' });
            }
        }

        // Cancel edit mode
        function cancelEdit() {
            resetForm();
        }

        // Reset form to default state
        function resetForm() {
            linkForm.reset();
            editId.value = '';
            formTitle.textContent = 'Add New Link';
            submitBtn.textContent = 'Save Link';
            cancelEditBtn.style.display = 'none';
            
            // Clear any error messages
            hideError('titleError');
            hideError('urlError');
        }

        // Render links to the DOM
        function renderLinks(linksToRender = null) {
            const linksToShow = linksToRender || sortLinks(links);
            
            if (linksToShow.length === 0) {
                linksList.innerHTML = `
                    <div class="empty-state">
                        <i>🔍</i>
                        <h3>No links found</h3>
                        <p>Try adjusting your search or filter criteria</p>
                    </div>
                `;
                return;
            }
            
            linksList.innerHTML = linksToShow.map(link => `
                <div class="link-item">
                    <div class="link-info">
                        <h3>${link.title}</h3>
                        <a href="${link.url}" target="_blank" class="link-url">${link.url}</a>
                        <div class="link-category">${link.category}</div>
                        ${link.description ? `<p>${link.description}</p>` : ''}
                        <small>Added: ${new Date(link.dateAdded).toLocaleDateString()}</small>
                    </div>
                    <div class="link-actions">
                        <button class="btn-warning" onclick="editLink(${link.id})">Edit</button>
                        <button class="btn-danger" onclick="deleteLink(${link.id})">Delete</button>
                    </div>
                </div>
            `).join('');
        }

        // Sort links based on current sort option
        function sortLinks(linksToSort) {
            const sortedLinks = [...linksToSort];
            
            switch (currentSort) {
                case 'newest':
                    return sortedLinks.sort((a, b) => b.dateAdded - a.dateAdded);
                case 'oldest':
                    return sortedLinks.sort((a, b) => a.dateAdded - b.dateAdded);
                case 'title':
                    return sortedLinks.sort((a, b) => a.title.localeCompare(b.title));
                default:
                    return sortedLinks;
            }
        }

        // Delete a link
        function deleteLink(id) {
            if (confirm("Are you sure you want to delete this link?")) {
                links = links.filter(link => link.id !== id);
                saveToLocalStorage();
                applyFilters();
                updateStats();
                showNotification('Link deleted successfully!');
            }
        }

        // Handle search
        function handleSearch() {
            currentSearchTerm = searchInput.value.toLowerCase();
            applyFilters();
        }

        // Apply both search and filter
        function applyFilters() {
            let filteredLinks = links;
            
            // Apply category filter
            if (currentFilter !== 'all') {
                filteredLinks = filteredLinks.filter(link => link.category === currentFilter);
            }
            
            // Apply search filter
            if (currentSearchTerm) {
                filteredLinks = filteredLinks.filter(link => 
                    link.title.toLowerCase().includes(currentSearchTerm) ||
                    link.url.toLowerCase().includes(currentSearchTerm) ||
                    link.category.toLowerCase().includes(currentSearchTerm) ||
                    (link.description && link.description.toLowerCase().includes(currentSearchTerm))
                );
            }
            
            // Apply sorting
            filteredLinks = sortLinks(filteredLinks);
            
            renderLinks(filteredLinks);
        }

        // Update statistics
        function updateStats() {
            totalLinksEl.textContent = links.length;
            
            // Count unique categories
            const categories = [...new Set(links.map(link => link.category))];
            categoriesCountEl.textContent = categories.length;
        }

        // Save to localStorage
        function saveToLocalStorage() {
            localStorage.setItem('links', JSON.stringify(links));
        }

        // Export links as JSON
        function exportLinks() {
            const dataStr = JSON.stringify(links, null, 2);
            const dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);
            
            const exportFileDefaultName = 'linkvault-backup.json';
            
            const linkElement = document.createElement('a');
            linkElement.setAttribute('href', dataUri);
            linkElement.setAttribute('download', exportFileDefaultName);
            linkElement.click();
            
            showNotification('Links exported successfully!');
        }

        // Import links from JSON
        function importLinks() {
            try {
                const importedLinks = JSON.parse(importData.value);
                
                if (!Array.isArray(importedLinks)) {
                    throw new Error('Invalid data format');
                }
                
                // Validate each link has required fields
                for (const link of importedLinks) {
                    if (!link.title || !link.url || !link.category) {
                        throw new Error('Some links are missing required fields');
                    }
                }
                
                // Add imported links (avoid duplicates by URL)
                let importedCount = 0;
                importedLinks.forEach(importedLink => {
                    if (!isDuplicateUrl(importedLink.url)) {
                        // Generate new ID and date for imported links
                        links.push({
                            ...importedLink,
                            id: Date.now() + Math.random(),
                            dateAdded: Date.now()
                        });
                        importedCount++;
                    }
                });
                
                saveToLocalStorage();
                renderLinks();
                updateStats();
                importModal.style.display = 'none';
                importData.value = '';
                
                showNotification(`Successfully imported ${importedCount} links!`);
            } catch (error) {
                importError.textContent = error.message || 'Invalid JSON format';
                importError.style.display = 'block';
            }
        }

        // Clear all links
        function clearAllLinks() {
            if (confirm("Are you sure you want to delete ALL links? This action cannot be undone.")) {
                links = [];
                saveToLocalStorage();
                renderLinks();
                updateStats();
                showNotification('All links have been deleted!', 'warning');
            }
        }

        // Show notification
        function showNotification(message, type = 'success') {
            notification.textContent = message;
            notification.className = 'notification';
            notification.classList.add(type);
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // Initialize the app when DOM is loaded
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
