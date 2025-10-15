<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LinkVault - Website Link Manager</title>
    <link rel="icon" type="image/x-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🔗</text></svg>">
    <style>
        /* Yeh CSS same hai, no change */
        :root {
            --primary: #4361ee;
            --secondary: #3a0ca3;
            --accent: #7209b7;
            --light: #f8f9fa;
            --dark: #212529;
            --success: #4cc9f0;
            --danger: #f72585;
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
            margin-bottom: ,8px;
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
        
        button {
            background: var(--primary);
            color: whiteonial;
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
        
        .btn-danger {
            background: var(--danger);
        }
        
        .btn-danger:hover {
            background: #d1145a;
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
        
        .filter-options {
            display: flex;
            gap:  Mian 10px;
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
                <h2>Add New Link</h2>
                <form id="linkForm">
                    <div class="form-group">
                        <label for="linkTitle">Title</label>
                        <input type="text" id="linkTitle" placeholder="Enter website title" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="linkUrl">URL</label>
                        <input type="url" id="linkUrl" placeholder="https://example.com" required>
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
                    
                    <button type="submit">Save Link</button>
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
                stats>
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
            <p class="signature">Made in <span class="heartbeat">❤️</span> By Armeen</p>
            <p>Links are permanently saved in your browser! No delete option to prevent loss.</p>
        </footercontainers>
    </div>

    <div class="notification" id="notification">Link added successfully!</div>

    <script>
        // Sample initial data - Yeh sirf first time ke liye, baad mein localStorage se load hoga
        let links = JSON.parse(localStorage.getItem('links')) || [
            {
                id: 1,
                title: "Google",
                url: "https://www.google.com",
                category: "work",
                description: "Search engine"
            },
            {
                id: 2,
                title: "YouTube",
                url: "https://www.youtube.com",
                category: "entertainment",
                description: "Video sharing platform"
            },
            {
                id: 3,
                title: "GitHub",
                url: "https://www.github.com",
                category: "work",
                description: "Code hosting platform"
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

        // Current filter state
        let currentFilter = 'all';
        let currentSearchTerm = '';

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
                    filterButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    currentFilter = button.dataset.category;
                    applyFilters();
                });
            });
        }

        // Handle form submission - Add karke auto save
        function handleFormSubmit(e) {
            e.preventDefault();
            
            const title = document.getElementById('linkTitle').value;
            const url = document.getElementById('linkUrl').value;
            const category = document.getElementById('linkCategory').value;
            const description = document.getElementById('linkDescription').value;
            
            // URL ko fix karo agar https na ho
            let fixedUrl = url;
            if (!/^https?:\/\//i.test(fixedUrl)) {
                fixedUrl = 'https://' + fixedUrl;
            }
            // Create new link object
            const newLink = {
                id: Date.now(),
                title,
                url: fixedUrl,
                category,
                description
            };
            
            // Add to links array
            links.push(newLink);
            
            // Auto save to localStorage (permanent save)
            saveToLocalStorage();
            
            // Re-render
            renderLinks();
            updateStats();
            
            // Show notification
            showNotification('Link added and saved permanently!');
            
            // Reset form
            linkForm.reset();
        }

        // Render links to the DOM (delete button remove kar diya)
        function renderLinks(linksToRender = null) {
            const linksToShow = linksToRender || links;
            
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
                    </div>
                    <!-- Delete button removed to prevent deletion -->
                </div>
            `).join('');
        }

        // Handle search
        function handleSearch() {
            currentSearchTerm = searchInput.value.toLowerCase();
            applyFilters();
        }

        // Apply filters
        function applyFilters() {
            let filteredLinks = links;
            
            if (currentFilter !== 'all') {
                filteredLinks = filteredLinks.filter(link => link.category === currentFilter);
            }
            
            if (currentSearchTerm) {
                filteredLinks = filteredLinks.filter(link => 
                    link.title.toLowerCase().includes(currentSearchTerm) ||
                    link.url.toLowerCase().includes(currentSearchTerm) ||
                    link.category.toLowerCase().includes(currentSearchTerm) ||
                    (link.description && link.description.toLowerCase().includes(currentSearchTerm))
                );
            }
            
            renderLinks(filteredLinks);
        }

        // Update statistics
        function updateStats() {
            totalLinksEl.textContent = links.length;
            const categories = [...new Set(links.map(link => link.category))];
            categoriesCountEl.textContent = categories.length;
        }

        // Auto save to localStorage - Permanent storage
        function saveToLocalStorage() {
            localStorage.setItem('links', JSON.stringify(links));
            // Optional backup
            localStorage.setItem('linksBackup', JSON.stringify(links)); // Agar backup chahiye
            console.log('Links saved permanently to browser storage!');
        }

        // Show notification
        function showNotification(message) {
            notification.textContent = message;
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }

        // On load, auto restore from localStorage
        document.addEventListener('DOMContentLoaded', () => {
            // Agar localStorage mein data hai to load, warna sample use karo lekin override mat
            const savedLinks = localStorage.getItem('links');
            if (savedLinks) {
                links = JSON.parse(savedLinks);
            }
            init();
            showNotification('Links loaded from permanent storage!');
        });
    </script>
</body>
</2html>
