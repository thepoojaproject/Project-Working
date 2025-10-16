
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neelam Bookself - Your Digital Notebook</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        :root {
            --primary: #2c3e50;
            --secondary: #34495e;
            --accent: #3498db;
            --light: #ecf0f1;
            --dark: #2c3e50;
            --paper: #fefefe;
            --paper-shadow: #ddd;
            --ink: #333;
            --bg-gradient-start: #f5f7fa;
            --bg-gradient-end: #e4e8ed;
        }

        /* Dark Mode Variables */
        [data-theme="dark"] {
            --primary: #1a2530;
            --secondary: #2c3e50;
            --accent: #4aa3df;
            --light: #34495e;
            --dark: #ecf0f1;
            --paper: #1e1e1e;
            --paper-shadow: #333;
            --ink: #eee;
            --bg-gradient-start: #121212;
            --bg-gradient-end: #1e1e1e;
        }

        body {
            background-color: #f5f7fa;
            color: var(--ink);
            line-height: 1.6;
            background-image: linear-gradient(to bottom, var(--bg-gradient-start) 0%, var(--bg-gradient-end) 100%);
            min-height: 100vh;
            transition: background-color 0.3s ease, color 0.3s ease;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }

        /* Header Styles */
        header {
            background-color: var(--primary);
            color: white;
            padding: 15px 0;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            font-size: 24px;
            font-weight: 700;
        }

        .logo i {
            color: var(--accent);
        }

        .search-bar {
            flex: 1;
            max-width: 500px;
            margin: 0 20px;
            position: relative;
        }

        .search-bar input {
            width: 100%;
            padding: 10px 20px;
            border-radius: 4px;
            border: none;
            font-size: 16px;
            background: rgba(255, 255, 255, 0.9);
            transition: all 0.3s ease;
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
        }

        [data-theme="dark"] .search-bar input {
            background: rgba(0, 0, 0, 0.5);
            color: white;
        }

        .search-bar input:focus {
            outline: none;
            background: white;
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.2);
        }

        .nav-links {
            display: flex;
            gap: 20px;
            align-items: center;
        }

        .nav-links a {
            color: white;
            text-decoration: none;
            font-weight: 500;
            transition: all 0.3s ease;
            padding: 5px 10px;
            border-radius: 3px;
        }

        .nav-links a:hover {
            background-color: rgba(255, 255, 255, 0.1);
        }

        .btn {
            padding: 8px 16px;
            border-radius: 4px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            border: none;
            font-size: 14px;
        }

        .btn-primary {
            background-color: var(--accent);
            color: white;
        }

        .btn-primary:hover {
            background-color: #2980b9;
        }

        .btn-outline {
            background-color: transparent;
            border: 1px solid white;
            color: white;
        }

        .btn-outline:hover {
            background-color: white;
            color: var(--primary);
        }

        /* Main Content */
        .main-content {
            display: flex;
            margin: 30px 0;
            gap: 30px;
        }

        /* Sidebar */
        .sidebar {
            width: 250px;
            background-color: var(--paper);
            border-radius: 5px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            padding: 20px;
            height: fit-content;
        }

        .sidebar-section {
            margin-bottom: 25px;
        }

        .sidebar-section h3 {
            font-size: 16px;
            margin-bottom: 15px;
            color: var(--primary);
            border-bottom: 1px solid var(--paper-shadow);
            padding-bottom: 8px;
        }

        .sidebar-links {
            list-style: none;
        }

        .sidebar-links li {
            margin-bottom: 10px;
        }

        .sidebar-links a {
            color: var(--ink);
            text-decoration: none;
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 8px 5px;
            border-radius: 3px;
            transition: all 0.3s ease;
        }

        .sidebar-links a:hover {
            background-color: var(--light);
        }

        .sidebar-links a.active {
            background-color: var(--accent);
            color: white;
        }

        .category-tags {
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }

        .tag {
            background-color: var(--light);
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .tag:hover {
            background-color: var(--accent);
            color: white;
        }

        /* Notebook Area */
        .notebook {
            flex: 1;
            background-color: var(--paper);
            border-radius: 5px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }

        .notebook-header {
            background-color: var(--secondary);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .notebook-title {
            font-size: 20px;
            font-weight: 600;
        }

        .notebook-actions {
            display: flex;
            gap: 10px;
        }

        .notebook-content {
            padding: 30px;
            min-height: 500px;
            border-left: 1px solid var(--paper-shadow);
            border-right: 1px solid var(--paper-shadow);
        }

        .story-meta {
            display: flex;
            gap: 20px;
            margin-bottom: 25px;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--paper-shadow);
            color: #666;
            font-size: 14px;
        }

        [data-theme="dark"] .story-meta {
            color: #aaa;
        }

        .story-content {
            line-height: 1.8;
            font-size: 16px;
        }

        .story-content p {
            margin-bottom: 20px;
        }

        /* Editor Styles */
        .editor-toolbar {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .editor-btn {
            padding: 8px 12px;
            background: var(--light);
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s ease;
        }

        .editor-btn:hover {
            background: var(--accent);
            color: white;
        }

        #story-title-input {
            width: 100%;
            padding: 10px;
            font-size: 20px;
            font-weight: 600;
            border: 1px solid var(--paper-shadow);
            border-radius: 4px;
            margin-bottom: 20px;
            background: var(--paper);
            color: var(--ink);
        }

        [data-theme="dark"] #story-title-input {
            background: #333;
            border-color: #444;
            color: #eee;
        }

        #story-editor {
            width: 100%;
            min-height: 400px;
            padding: 15px;
            border: 1px solid var(--paper-shadow);
            border-radius: 4px;
            font-size: 16px;
            line-height: 1.8;
            background: var(--paper);
            color: var(--ink);
            resize: vertical;
        }

        [data-theme="dark"] #story-editor {
            background: #333;
            border-color: #444;
            color: #eee;
        }

        #story-editor:focus {
            outline: none;
            border-color: var(--accent);
        }

        .notebook-footer {
            background-color: var(--light);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-top: 1px solid var(--paper-shadow);
        }

        .action-buttons {
            display: flex;
            gap: 10px;
        }

        .action-btn {
            display: flex;
            align-items: center;
            gap: 5px;
            padding: 8px 15px;
            background-color: white;
            border: 1px solid var(--paper-shadow);
            border-radius: 4px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 14px;
        }

        [data-theme="dark"] .action-btn {
            background-color: #333;
            border-color: #444;
            color: var(--ink);
        }

        .action-btn:hover {
            background-color: var(--accent);
            color: white;
            border-color: var(--accent);
        }

        /* Stories Grid */
        .stories-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 25px;
            margin-bottom: 40px;
        }

        .story-card {
            background: var(--paper);
            border-radius: 5px;
            overflow: hidden;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            cursor: pointer;
            border: 1px solid var(--paper-shadow);
        }

        .story-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .story-header {
            padding: 15px;
            border-bottom: 1px solid var(--paper-shadow);
        }

        .story-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 5px;
            color: var(--dark);
        }

        .story-author {
            display: flex;
            align-items: center;
            gap: 8px;
            color: #666;
            font-size: 14px;
        }

        [data-theme="dark"] .story-author {
            color: #aaa;
        }

        .author-avatar {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            background-color: var(--accent);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 12px;
            font-weight: bold;
        }

        .story-excerpt {
            padding: 15px;
            color: #555;
            font-size: 14px;
            line-height: 1.6;
        }

        [data-theme="dark"] .story-excerpt {
            color: #ccc;
        }

        .story-footer {
            padding: 10px 15px;
            background-color: var(--light);
            display: flex;
            justify-content: space-between;
            color: #666;
            font-size: 12px;
        }

        [data-theme="dark"] .story-footer {
            color: #aaa;
        }

        .story-stat {
            display: flex;
            align-items: center;
            gap: 5px;
        }

        /* Section Titles */
        .section-title {
            font-size: 24px;
            margin: 30px 0 20px;
            color: var(--dark);
            position: relative;
            padding-bottom: 10px;
        }

        .section-title:after {
            content: '';
            display: block;
            width: 50px;
            height: 3px;
            background: var(--accent);
            margin-top: 8px;
            border-radius: 2px;
        }

        /* Footer */
        footer {
            background: var(--primary);
            color: white;
            padding: 40px 0 20px;
            margin-top: 50px;
        }

        .footer-content {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 30px;
            margin-bottom: 30px;
        }

        .footer-column h3 {
            font-size: 16px;
            margin-bottom: 15px;
            position: relative;
            padding-bottom: 8px;
        }

        .footer-column h3:after {
            content: '';
            position: absolute;
            left: 0;
            bottom: 0;
            width: 30px;
            height: 2px;
            background: var(--accent);
        }

        .footer-links {
            list-style: none;
        }

        .footer-links li {
            margin-bottom: 10px;
        }

        .footer-links a {
            color: #bdc3c7;
            text-decoration: none;
            transition: all 0.3s ease;
            font-size: 14px;
        }

        .footer-links a:hover {
            color: white;
        }

        .copyright {
            text-align: center;
            padding-top: 20px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            color: #bdc3c7;
            font-size: 14px;
        }

        /* Heartbeat Animation */
        .heartbeat {
            display: inline-block;
            animation: heartbeat 1.5s ease-in-out infinite;
        }

        @keyframes heartbeat {
            0% {
                transform: scale(1);
            }
            14% {
                transform: scale(1.3);
            }
            28% {
                transform: scale(1);
            }
            42% {
                transform: scale(1.3);
            }
            70% {
                transform: scale(1);
            }
        }

        /* Responsive */
        @media (max-width: 900px) {
            .main-content {
                flex-direction: column;
            }
            
            .sidebar {
                width: 100%;
            }
            
            .header-content {
                flex-direction: column;
                gap: 15px;
            }
            
            .search-bar {
                max-width: 100%;
                margin: 10px 0;
            }
        }

        @media (max-width: 400px) {
            .stories-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header>
        <div class="container">
            <div class="header-content">
                <div class="logo">
                    <i class="fas fa-book"></i>
                    <span>Neelam Bookself</span>
                </div>
                <div class="search-bar" role="search">
                    <input type="text" placeholder="Search stories, authors..." aria-label="Search stories or authors">
                </div>
                <div class="nav-links">
                    <a href="#">Library</a>
                    <a href="#">My Stories</a>
                    <a href="#">Write</a>
                    <button class="btn btn-outline">Sign In</button>
                    <button class="btn btn-primary">Sign Up</button>
                </div>
            </div>
        </div>
    </header>

    <div class="container">
        <!-- Main Content -->
        <div class="main-content">
            <!-- Sidebar -->
            <div class="sidebar">
                <div class="sidebar-section">
                    <h3>My Notebooks</h3>
                    <ul class="sidebar-links">
                        <li><a href="#" data-view="library"><i class="fas fa-book" aria-hidden="true"></i> All Stories</a></li>
                        <li><a href="#" data-view="favorites"><i class="fas fa-heart" aria-hidden="true"></i> Favorites</a></li>
                        <li><a href="#" data-view="recent"><i class="fas fa-history" aria-hidden="true"></i> Recently Read</a></li>
                        <li><a href="#" data-view="downloaded"><i class="fas fa-download" aria-hidden="true"></i> Downloaded</a></li>
                    </ul>
                </div>
                
                <div class="sidebar-section">
                    <h3>Categories</h3>
                    <div class="category-tags">
                        <div class="tag">Romance</div>
                        <div class="tag">Fantasy</div>
                        <div class="tag">Sci-Fi</div>
                        <div class="tag">Mystery</div>
                        <div class="tag">Horror</div>
                        <div class="tag">Adventure</div>
                        <div class="tag">Fan Fiction</div>
                        <div class="tag">Teen Fiction</div>
                    </div>
                </div>
                
                <div class="sidebar-section">
                    <h3>Writing Tools</h3>
                    <ul class="sidebar-links">
                        <li><a href="#" class="active" data-view="editor"><i class="fas fa-edit" aria-hidden="true"></i> New Story</a></li>
                        <li><a href="#" data-view="stats"><i class="fas fa-chart-bar" aria-hidden="true"></i> Stats</a></li>
                        <li><a href="#" data-view="comments"><i class="fas fa-comments" aria-hidden="true"></i> Comments</a></li>
                        <li><a href="#" data-view="settings"><i class="fas fa-cog" aria-hidden="true"></i> Settings</a></li>
                    </ul>
                </div>
            </div>
            
            <!-- Notebook Area -->
            <div class="notebook">
                <div class="notebook-header">
                    <div class="notebook-title" id="notebook-title">New Story</div>
                    <div class="notebook-actions">
                        <button id="save-story" class="btn btn-primary"><i class="fas fa-save"></i> Save Draft</button>
                    </div>
                </div>
                
                <div class="notebook-content">
                    <!-- Editor View -->
                    <div id="editor-view">
                        <input type="text" id="story-title-input" placeholder="Enter story title..." value="Untitled Story">
                        
                        <div class="editor-toolbar">
                            <button class="editor-btn" data-command="bold"><i class="fas fa-bold"></i></button>
                            <button class="editor-btn" data-command="italic"><i class="fas fa-italic"></i></button>
                            <button class="editor-btn" data-command="underline"><i class="fas fa-underline"></i></button>
                            <button class="editor-btn" data-command="insertUnorderedList"><i class="fas fa-list-ul"></i></button>
                            <button class="editor-btn" data-command="insertOrderedList"><i class="fas fa-list-ol"></i></button>
                            <button class="editor-btn" data-command="createLink"><i class="fas fa-link"></i></button>
                        </div>
                        
                        <div id="story-editor" contenteditable="true" placeholder="Start writing your story here...">
                            <p>Once upon a time...</p>
                        </div>
                    </div>
                    
                    <!-- Reader View (hidden by default) -->
                    <div id="reader-view" style="display: none;">
                        <div class="story-meta">
                            <span><i class="fas fa-user" aria-hidden="true"></i> You</span>
                            <span><i class="fas fa-calendar" aria-hidden="true"></i> Draft</span>
                            <span><i class="fas fa-eye" aria-hidden="true"></i> 0 reads</span>
                        </div>
                        
                        <div class="story-content" id="preview-content">
                            <!-- Preview loads here -->
                        </div>
                    </div>
                </div>
                
                <div class="notebook-footer">
                    <div class="action-buttons">
                        <button id="preview-story" class="action-btn"><i class="fas fa-eye"></i> Preview</button>
                        <button class="action-btn" aria-label="Like story"><i class="far fa-heart"></i> Like</button>
                        <button class="action-btn" aria-label="Comment on story"><i class="far fa-comment"></i> Comment</button>
                    </div>
                    <div class="reading-options">
                        <button class="action-btn" aria-label="Adjust font"><i class="fas fa-font"></i> Font</button>
                        <button id="dark-mode-toggle" class="action-btn" aria-label="Toggle dark mode"><i class="fas fa-moon"></i> Dark Mode</button>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- More Stories -->
        <h2 class="section-title">Recommended Stories</h2>
        <div class="stories-grid">
            <!-- Story 1 -->
            <div class="story-card" data-title="The Godess" data-author="Armeen Sheikh" data-excerpt="A psychological thriller that will keep you guessing until the very end. Detective Miller finds himself trapped in a cat and mouse game with a serial killer who always seems one step ahead." data-views="18.7K" data-likes="2.8K" data-comments="342">
                <div class="story-header">
                    <h3 class="story-title">The Godess</h3>
                    <div class="story-author">
                        <div class="author-avatar">MR</div>
                        <span>Armeen Sheikh</span>
                    </div>
                </div>
                <div class="story-excerpt">
लखनऊ की 22 वर्षीय खूबसूरत आर्मीन शेख को कॉलेज में अमीर, ब्राह्मण युवक विक्रमादित्य सिंह 'विक्की' से प्यार हो जाता है। उनका अंतर्जातीय प्रेम उनके रूढ़िवादी परिवारों के लिए अस्वीकार्य था। गुप्त मुलाकातों और समाज सुधार पर बहस से शुरू हुआ उनका रिश्ता, जल्द ही परिवार की इज़्ज़त और मर्यादा के विवाद में बदल जाता है। पकड़े जाने पर, परिवारों द्वारा दोनों पर ज़बरदस्त दबाव डाला जाता है। भागने की उनकी कोशिश नाकाम हो जाती है जब आर्मीन के भाई उन्हें पकड़वा देते हैं। विक्की को शहर से बाहर भेज दिया जाता है और आर्मीन का जबरन निकाह कर दिया जाता है। शादी के बाद भी आर्मीन का दिल विक्की में ही रहता है। विक्की लौटता है, एक आखिरी मुलाकात होती है, लेकिन समाज की दीवारें उन्हें अलग रखती हैं। विक्की की रहस्यमय परिस्थितियों में मौत हो जाती है, और आर्मीन समाज के नियमों की बलि चढ़कर, जीवन भर विक्की को याद करती हुई 'गुनाहों की देवी' बनकर रह जाती है। यह कहानी सवाल उठाती है कि गुनाह प्यार है या समाज का अंधापन।                </div>
                <div class="story-footer">
                    <div class="story-stat">
                        <i class="far fa-eye"></i>
                        <span>18.7K</span>
                    </div>
                    <div class="story-stat">
                        <i class="far fa-heart"></i>
                        <span>2.8K</span>
                    </div>
                    <div class="story-stat">
                        <i class="far fa-comment"></i>
                        <span>342</span>
                    </div>
                </div>
            </div>

            <!-- Story 2 -->
            <div class="story-card" data-title="Love in Paris" data-author="Amelia Laurent" data-excerpt="A romantic tale set in the city of love, where fate brings two souls together. When American journalist Alex meets French artist Claire, their worlds collide in the most unexpected way." data-views="32.1K" data-likes="5.4K" data-comments="892">
                <div class="story-header">
                    <h3 class="story-title">Love in Paris</h3>
                    <div class="story-author">
                        <div class="author-avatar">AL</div>
                        <span>Amelia Laurent</span>
                    </div>
                </div>
                <div class="story-excerpt">
                    A romantic tale set in the city of love, where fate brings two souls together. When American journalist Alex meets French artist Claire, their worlds collide in the most unexpected way.
                </div>
                <div class="story-footer">
                    <div class="story-stat">
                        <i class="far fa-eye"></i>
                        <span>32.1K</span>
                    </div>
                    <div class="story-stat">
                        <i class="far fa-heart"></i>
                        <span>5.4K</span>
                    </div>
                    <div class="story-stat">
                        <i class="far fa-comment"></i>
                        <span>892</span>
                    </div>
                </div>
            </div>

            <!-- Story 3 -->
            <div class="story-card" data-title="The Forgotten Kingdom" data-author="Thomas Wright" data-excerpt="An epic fantasy adventure in a world of magic, dragons, and ancient prophecies. A young farm boy discovers he is the last heir to a kingdom long thought destroyed." data-views="41.3K" data-likes="7.1K" data-comments="1.2K">
                <div class="story-header">
                    <h3 class="story-title">The Forgotten Kingdom</h3>
                    <div class="story-author">
                        <div class="author-avatar">TW</div>
                        <span>Thomas Wright</span>
                    </div>
                </div>
                <div class="story-excerpt">
                    An epic fantasy adventure in a world of magic, dragons, and ancient prophecies. A young farm boy discovers he is the last heir to a kingdom long thought destroyed.
                </div>
                <div class="story-footer">
                    <div class="story-stat">
                        <i class="far fa-eye"></i>
                        <span>41.3K</span>
                    </div>
                    <div class="story-stat">
                        <i class="far fa-heart"></i>
                        <span>7.1K</span>
                    </div>
                    <div class="story-stat">
                        <i class="far fa-comment"></i>
                        <span>1.2K</span>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer>
        <div class="container">
            <div class="footer-content">
                <div class="footer-column">
                    <h3>Neelam Bookself</h3>
                    <ul class="footer-links">
                        <li><a href="#">About Us</a></li>
                        <li><a href="#">Careers</a></li>
                        <li><a href="#">Press</a></li>
                        <li><a href="#">Blog</a></li>
                    </ul>
                </div>
                <div class="footer-column">
                    <h3>Explore</h3>
                    <ul class="footer-links">
                        <li><a href="#">Library</a></li>
                        <li><a href="#">Genres</a></li>
                        <li><a href="#">Community</a></li>
                        <li><a href="#">Events</a></li>
                    </ul>
                </div>
                <div class="footer-column">
                    <h3>Write</h3>
                    <ul class="footer-links">
                        <li><a href="#">Start Writing</a></li>
                        <li><a href="#">Writing Tips</a></li>
                        <li><a href="#">Writer Resources</a></li>
                        <li><a href="#">Publishing</a></li>
                    </ul>
                </div>
                <div class="footer-column">
                    <h3>Support</h3>
                    <ul class="footer-links">
                        <li><a href="#">Help Center</a></li>
                        <li><a href="#">Contact Us</a></li>
                        <li><a href="#">Privacy Policy</a></li>
                        <li><a href="#">Terms of Service</a></li>
                    </ul>
                </div>
            </div>
            <div class="copyright">
                <p>Made in <span class="heartbeat">❤️</span> By Armeen</p>
            </div>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Elements
            const sidebarLinks = document.querySelectorAll('.sidebar-links a');
            const notebookTitle = document.getElementById('notebook-title');
            const editorView = document.getElementById('editor-view');
            const readerView = document.getElementById('reader-view');
            const storyTitleInput = document.getElementById('story-title-input');
            const storyEditor = document.getElementById('story-editor');
            const previewContent = document.getElementById('preview-content');
            const previewBtn = document.getElementById('preview-story');
            const saveBtn = document.getElementById('save-story');
            const editorBtns = document.querySelectorAll('.editor-btn');

            // Load saved draft
            const savedTitle = localStorage.getItem('draftTitle');
            const savedContent = localStorage.getItem('draftContent');
            if (savedTitle) storyTitleInput.value = savedTitle;
            if (savedContent) storyEditor.innerHTML = savedContent;

            // Update title in header
            storyTitleInput.addEventListener('input', function() {
                notebookTitle.textContent = this.value || 'New Story';
            });

            // Formatting buttons
            editorBtns.forEach(btn => {
                btn.addEventListener('click', function(e) {
                    e.preventDefault();
                    const command = this.dataset.command;
                    if (command === 'createLink') {
                        const url = prompt('Enter URL:');
                        if (url) document.execCommand(command, false, url);
                    } else {
                        document.execCommand(command, false, null);
                    }
                    storyEditor.focus();
                });
            });

            // Preview
            previewBtn.addEventListener('click', function() {
                if (editorView.style.display !== 'none') {
                    // Switch to preview
                    previewContent.innerHTML = storyEditor.innerHTML;
                    notebookTitle.textContent = storyTitleInput.value || 'Preview';
                    editorView.style.display = 'none';
                    readerView.style.display = 'block';
                    this.innerHTML = '<i class="fas fa-edit"></i> Edit';
                } else {
                    // Back to editor
                    editorView.style.display = 'block';
                    readerView.style.display = 'none';
                    this.innerHTML = '<i class="fas fa-eye"></i> Preview';
                }
            });

            // Save draft
            saveBtn.addEventListener('click', function() {
                localStorage.setItem('draftTitle', storyTitleInput.value);
                localStorage.setItem('draftContent', storyEditor.innerHTML);
                alert('Draft saved locally!');
            });

            // Sidebar navigation
            sidebarLinks.forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    sidebarLinks.forEach(l => l.classList.remove('active'));
                    this.classList.add('active');
                    
                    const view = this.dataset.view;
                    if (view === 'editor') {
                        // Show editor
                        notebookTitle.textContent = storyTitleInput.value || 'New Story';
                        editorView.style.display = 'block';
                        readerView.style.display = 'none';
                        previewBtn.innerHTML = '<i class="fas fa-eye"></i> Preview';
                    } else {
                        // Placeholder for other views
                        notebookTitle.textContent = this.textContent.trim();
                        editorView.style.display = 'none';
                        readerView.style.display = 'block';
                        previewContent.innerHTML = `<p>This section (${view}) is under construction. Switch back to New Story to write.</p>`;
                    }
                });
            });

            // Story cards load into reader
            const storyCards = document.querySelectorAll('.story-card');
            storyCards.forEach(card => {
                card.addEventListener('click', function() {
                    notebookTitle.textContent = this.dataset.title;
                    document.querySelector('.story-meta').innerHTML = `
                        <span><i class="fas fa-user"></i> ${this.dataset.author}</span>
                        <span><i class="fas fa-calendar"></i> Published: Today</span>
                        <span><i class="fas fa-eye"></i> ${this.dataset.views} reads</span>
                    `;
                    previewContent.innerHTML = `<p>${this.dataset.excerpt}</p><p>(Full story would load here...)</p>`;
                    editorView.style.display = 'none';
                    readerView.style.display = 'block';
                    // Update sidebar active
                    sidebarLinks.forEach(l => l.classList.remove('active'));
                });
            });

            // Dark Mode Toggle
            const darkModeToggle = document.getElementById('dark-mode-toggle');
            const body = document.body;
            darkModeToggle.addEventListener('click', function() {
                if (body.getAttribute('data-theme') === 'dark') {
                    body.removeAttribute('data-theme');
                    localStorage.setItem('theme', 'light');
                    this.innerHTML = '<i class="fas fa-moon"></i> Dark Mode';
                } else {
                    body.setAttribute('data-theme', 'dark');
                    localStorage.setItem('theme', 'dark');
                    this.innerHTML = '<i class="fas fa-sun"></i> Light Mode';
                }
            });

            // Load theme
            if (localStorage.getItem('theme') === 'dark') {
                body.setAttribute('data-theme', 'dark');
                darkModeToggle.innerHTML = '<i class="fas fa-sun"></i> Light Mode';
            }

            // Other actions
            document.querySelectorAll('.action-btn').forEach(btn => {
                if (!btn.id || btn.id !== 'dark-mode-toggle') {
                    btn.addEventListener('click', function(e) {
                        if (this.closest('.reading-options') && btn.id !== 'dark-mode-toggle') return;
                        if (btn.id === 'preview-story') return;
                        alert('Action: ' + this.textContent.trim());
                    });
                }
            });

            // Prevent default on links
            document.querySelectorAll('a').forEach(a => {
                a.addEventListener('click', e => {
                    if (!a.href || a.href === '#') e.preventDefault();
                });
            });
        });
    </script>
</body>
</html>
