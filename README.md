<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Word Count Analyzer</title>
    <style>
        :root {
            --primary: #6366f1;
            --primary-light: #818cf8;
            --primary-dark: #4f46e5;
            --secondary: #64748b;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --light: #f1f5f9;
            --dark: #1e293b;
            --text: #334155;
            --border-radius: 0.5rem;
            --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            --transition: all 0.3s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        }

        body {
            background-color: #f8fafc;
            color: var(--text);
            line-height: 1.6;
            min-height: 100vh;
            padding: 1rem;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 1.5rem;
        }

        .app-header {
            text-align: center;
            margin-bottom: 2rem;
        }

        .app-title {
            font-size: 2.5rem;
            font-weight: 700;
            color: var(--primary-dark);
            margin-bottom: 0.5rem;
        }

        .app-subtitle {
            color: var(--secondary);
            font-size: 1.1rem;
        }

        .main-panel {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1.5rem;
        }

        @media (min-width: 1024px) {
            .main-panel {
                grid-template-columns: 1fr 1fr;
            }
        }

        .card {
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            transition: var(--transition);
        }

        .card:hover {
            box-shadow: var(--shadow-lg);
        }

        .card-header {
            padding: 1.25rem 1.5rem;
            background-color: var(--primary);
            color: white;
            font-weight: 600;
            font-size: 1.25rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .card-header .icon {
            font-size: 1.25rem;
        }

        .card-body {
            padding: 1.5rem;
        }

        .input-area {
            width: 100%;
            min-height: 300px;
            padding: 1rem;
            border: 1px solid #cbd5e1;
            border-radius: 0.375rem;
            font-size: 1rem;
            resize: vertical;
            transition: var(--transition);
        }

        .input-area:focus {
            outline: none;
            border-color: var(--primary-light);
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.2);
        }

        .button-container {
            display: flex;
            gap: 0.75rem;
            margin-top: 1rem;
        }

        .btn {
            padding: 0.625rem 1.25rem;
            border: none;
            border-radius: 0.375rem;
            font-weight: 500;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .btn-primary {
            background-color: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background-color: var(--primary-dark);
        }

        .btn-secondary {
            background-color: var(--secondary);
            color: white;
        }

        .btn-secondary:hover {
            background-color: #4b5563;
        }

        .btn-outline {
            background-color: transparent;
            border: 1px solid var(--secondary);
            color: var(--secondary);
        }

        .btn-outline:hover {
            background-color: #f1f5f9;
        }

        .stats-container {
            margin-top: 1.5rem;
        }

        .stat-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 1rem;
        }

        @media (min-width: 640px) {
            .stat-grid {
                grid-template-columns: repeat(3, 1fr);
            }
        }

        .stat-card {
            background-color: #f8fafc;
            padding: 1.25rem;
            border-radius: 0.375rem;
            text-align: center;
            transition: var(--transition);
        }

        .stat-card:hover {
            transform: translateY(-3px);
            box-shadow: var(--shadow);
        }

        .stat-value {
            font-size: 1.875rem;
            font-weight: 700;
            line-height: 1.2;
            margin-bottom: 0.25rem;
        }

        .stat-label {
            color: var(--secondary);
            font-size: 0.875rem;
            font-weight: 500;
        }

        .words-stat {
            color: var(--primary);
        }

        .chars-stat {
            color: var(--success);
        }

        .sentences-stat {
            color: var(--warning);
        }

        .paragraph-stat {
            color: var(--danger);
        }

        .readtime-stat {
            color: var(--primary-dark);
        }

        .progress-container {
            margin-top: 1.5rem;
        }

        .progress-item {
            margin-bottom: 1rem;
        }

        .progress-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 0.5rem;
        }

        .progress-label {
            font-weight: 500;
        }

        .progress-value {
            font-weight: 600;
        }

        .progress-bar {
            height: 0.5rem;
            background-color: #e2e8f0;
            border-radius: 999px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            border-radius: 999px;
            transition: width 0.5s ease;
        }

        .grade-excellent {
            background-color: var(--success);
        }

        .grade-good {
            background-color: var(--primary);
        }

        .grade-average {
            background-color: var(--warning);
        }

        .grade-poor {
            background-color: var(--danger);
        }

        .word-table-container {
            margin-top: 1.5rem;
            overflow-x: auto;
        }

        .word-table {
            width: 100%;
            border-collapse: collapse;
        }

        .word-table th,
        .word-table td {
            padding: 0.75rem 1rem;
            text-align: left;
            border-bottom: 1px solid #e2e8f0;
        }

        .word-table th {
            background-color: #f1f5f9;
            font-weight: 600;
            color: var(--secondary);
        }

        .word-table tr:hover {
            background-color: #f8fafc;
        }

        .chart-container {
            height: 250px;
            margin-top: 1.5rem;
        }

        .tabs {
            display: flex;
            margin-bottom: 1rem;
            border-bottom: 1px solid #e2e8f0;
        }

        .tab-item {
            padding: 0.75rem 1rem;
            cursor: pointer;
            border-bottom: 2px solid transparent;
            font-weight: 500;
            transition: var(--transition);
        }

        .tab-item:hover {
            color: var(--primary);
        }

        .tab-item.active {
            color: var(--primary);
            border-bottom-color: var(--primary);
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .score-container {
            display: flex;
            align-items: center;
            gap: 1rem;
            margin-top: 1rem;
        }

        .score-circle {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            font-weight: 700;
            color: white;
        }

        .score-details {
            flex: 1;
        }

        .score-title {
            font-weight: 600;
            margin-bottom: 0.25rem;
        }

        .score-description {
            color: var(--secondary);
            font-size: 0.875rem;
        }

        .tooltip {
            position: relative;
            display: inline-block;
            cursor: help;
        }

        .tooltip .tooltip-text {
            visibility: hidden;
            width: 200px;
            background-color: #334155;
            color: white;
            text-align: center;
            border-radius: 6px;
            padding: 8px;
            position: absolute;
            z-index: 1;
            bottom: 125%;
            left: 50%;
            margin-left: -100px;
            opacity: 0;
            transition: opacity 0.3s;
            font-size: 0.75rem;
            box-shadow: var(--shadow);
        }

        .tooltip .tooltip-text::after {
            content: "";
            position: absolute;
            top: 100%;
            left: 50%;
            margin-left: -5px;
            border-width: 5px;
            border-style: solid;
            border-color: #334155 transparent transparent transparent;
        }

        .tooltip:hover .tooltip-text {
            visibility: visible;
            opacity: 1;
        }

        .feature-list {
            list-style-type: none;
            margin-top: 1rem;
        }

        .feature-item {
            padding: 0.5rem 0;
            border-bottom: 1px solid #e2e8f0;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .feature-icon {
            color: var(--primary);
        }

        .source-text {
            margin-top: 1rem;
            padding: 1rem;
            background-color: #f8fafc;
            border-radius: 0.375rem;
            font-size: 0.875rem;
            max-height: 150px;
            overflow-y: auto;
            white-space: pre-wrap;
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        #keywords-container {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
            margin-top: 1rem;
        }

        .keyword-tag {
            background-color: #e0e7ff;
            color: var(--primary-dark);
            padding: 0.375rem 0.75rem;
            border-radius: 999px;
            font-size: 0.875rem;
            font-weight: 500;
        }

        .alert {
            padding: 1rem;
            border-radius: var(--border-radius);
            margin-top: 1rem;
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }

        .alert-warning {
            background-color: #fef3c7;
            color: #92400e;
            border-left: 4px solid #f59e0b;
        }

        .alert-info {
            background-color: #dbeafe;
            color: #1e40af;
            border-left: 4px solid #3b82f6;
        }

        .suggestions-container {
            margin-top: 1rem;
        }
        
        .suggestion-item {
            background-color: #f1f5f9;
            padding: 0.75rem 1rem;
            border-radius: 0.375rem;
            margin-bottom: 0.5rem;
        }
        
        .suggestion-header {
            display: flex;
            justify-content: space-between;
            font-weight: 500;
            margin-bottom: 0.25rem;
        }
        
        .suggestion-type {
            color: var(--primary);
        }
        
        .file-input {
            display: none;
        }
        
        .drop-zone {
            border: 2px dashed #cbd5e1;
            border-radius: var(--border-radius);
            padding: 2rem;
            text-align: center;
            margin-bottom: 1rem;
            transition: var(--transition);
        }
        
        .drop-zone:hover, .drop-zone.dragover {
            border-color: var(--primary);
            background-color: #f1f5f9;
        }
        
        .drop-zone-text {
            color: var(--secondary);
            margin-bottom: 0.5rem;
        }
    </style>
</head>
<body>
    <div class="container">
        <header class="app-header">
            <h1 class="app-title">Advanced Word Count Analyzer</h1>
            <p class="app-subtitle">Get detailed insights and statistics about your text</p>
        </header>

        <main class="main-panel">
            <div class="card">
                <div class="card-header">
                    <span>Input Text</span>
                    <span class="icon">📝</span>
                </div>
                <div class="card-body">
                    <div class="drop-zone" id="drop-zone">
                        <p class="drop-zone-text">Drop text file here or click to upload</p>
                        <button class="btn btn-outline" id="file-select-btn">Select File</button>
                        <input type="file" id="file-input" class="file-input" accept=".txt,.doc,.docx,.pdf,.md">
                    </div>
                    
                    <textarea id="input-text" class="input-area" placeholder="Type or paste your text here..."></textarea>
                    
                    <div class="button-container">
                        <button id="analyze-btn" class="btn btn-primary">
                            <span>Analyze Text</span>
                        </button>
                        <button id="clear-btn" class="btn btn-secondary">
                            <span>Clear</span>
                        </button>
                        <button id="copy-results-btn" class="btn btn-outline">
                            <span>Copy Results</span>
                        </button>
                    </div>
                    
                    <div id="sample-text-container" class="alert alert-info" style="margin-top: 1rem">
                        <div>
                            <strong>Need sample text?</strong> 
                            <button id="sample-text-btn" class="btn btn-outline" style="padding: 0.25rem 0.5rem; margin-left: 0.5rem;">Insert Sample</button>
                        </div>
                    </div>
                </div>
            </div>

            <div class="card">
                <div class="card-header">
                    <span>Text Analysis Results</span>
                    <span class="icon">📊</span>
                </div>
                <div class="card-body">
                    <div id="results-container">
                        <div id="empty-state">
                            <p>Enter text and click "Analyze Text" to see detailed statistics.</p>
                            <ul class="feature-list">
                                <li class="feature-item">
                                    <span class="feature-icon">✓</span>
                                    <span>Word, character, and sentence counts</span>
                                </li>
                                <li class="feature-item">
                                    <span class="feature-icon">✓</span>
                                    <span>Reading and speaking time estimates</span>
                                </li>
                                <li class="feature-item">
                                    <span class="feature-icon">✓</span>
                                    <span>Readability scoring</span>
                                </li>
                                <li class="feature-item">
                                    <span class="feature-icon">✓</span>
                                    <span>Top keywords and frequency analysis</span>
                                </li>
                                <li class="feature-item">
                                    <span class="feature-icon">✓</span>
                                    <span>Writing style suggestions</span>
                                </li>
                            </ul>
                        </div>
                        
                        <div id="analysis-results" style="display: none;">
                            <div class="tabs">
                                <div class="tab-item active" data-tab="basic-stats">Basic Stats</div>
                                <div class="tab-item" data-tab="readability">Readability</div>
                                <div class="tab-item" data-tab="word-analysis">Word Analysis</div>
                                <div class="tab-item" data-tab="suggestions">Suggestions</div>
                            </div>
                            
                            <div id="basic-stats" class="tab-content active">
                                <div class="stat-grid">
                                    <div class="stat-card">
                                        <div class="stat-value words-stat" id="word-count">0</div>
                                        <div class="stat-label">Words</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value chars-stat" id="char-count">0</div>
                                        <div class="stat-label">Characters</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value chars-stat" id="char-no-spaces-count">0</div>
                                        <div class="stat-label">Chars (no spaces)</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value sentences-stat" id="sentence-count">0</div>
                                        <div class="stat-label">Sentences</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value paragraph-stat" id="paragraph-count">0</div>
                                        <div class="stat-label">Paragraphs</div>
                                    </div>
                                    <div class="stat-card">
                                        <div class="stat-value readtime-stat" id="read-time">0</div>
                                        <div class="stat-label">Reading Time</div>
                                    </div>
                                </div>
                                
                                <div class="progress-container">
                                    <div class="progress-item">
                                        <div class="progress-header">
                                            <span class="progress-label">Average Word Length</span>
                                            <span class="progress-value" id="avg-word-length">0</span>
                                        </div>
                                        <div class="progress-bar">
                                            <div class="progress-fill" id="avg-word-bar" style="width: 0%;"></div>
                                        </div>
                                    </div>
                                    <div class="progress-item">
                                        <div class="progress-header">
                                            <span class="progress-label">Average Sentence Length</span>
                                            <span class="progress-value" id="avg-sentence-length">0</span>
                                        </div>
                                        <div class="progress-bar">
                                            <div class="progress-fill" id="avg-sentence-bar" style="width: 0%;"></div>
                                        </div>
                                    </div>
                                </div>
                                
                                <div class="score-container">
                                    <div class="score-circle" id="diversity-score-circle">85</div>
                                    <div class="score-details">
                                        <div class="score-title">Vocabulary Diversity Score</div>
                                        <div class="score-description">Measures how varied your vocabulary is in this text. Higher scores indicate less repetition and a richer vocabulary.</div>
                                    </div>
                                </div>
                            </div>
                            
                            <div id="readability" class="tab-content">
                                <div class="score-container">
                                    <div class="score-circle" id="readability-score-circle">0</div>
                                    <div class="score-details">
                                        <div class="score-title">Flesch Reading Ease</div>
                                        <div class="score-description">
                                            <span id="readability-description">Higher scores indicate text that is easier to read.</span>
                                            <span class="tooltip">ⓘ
                                                <span class="tooltip-text">
                                                    90-100: Very Easy (5th grade)<br>
                                                    80-89: Easy (6th grade)<br>
                                                    70-79: Fairly Easy (7th grade)<br>
                                                    60-69: Standard (8-9th grade)<br>
                                                    50-59: Fairly Difficult (10-12th grade)<br>
                                                    30-49: Difficult (College)<br>
                                                    0-29: Very Difficult (College Graduate)
                                                </span>
                                            </span>
                                        </div>
                                    </div>
                                </div>
                                
                                <div class="progress-container">
                                    <div class="progress-item">
                                        <div class="progress-header">
                                            <span class="progress-label">Flesch-Kincaid Grade Level</span>
                                            <span class="progress-value" id="fk-grade">0</span>
                                        </div>
                                        <div class="progress-bar">
                                            <div class="progress-fill" id="fk-grade-bar" style="width: 0%;"></div>
                                        </div>
                                    </div>
                                    <div class="progress-item">
                                        <div class="progress-header">
                                            <span class="progress-label">Gunning Fog Index</span>
                                            <span class="progress-value tooltip" id="gunning-fog">
                                                0
                                                <span class="tooltip-text">Estimates the years of formal education needed to understand the text on first reading.</span>
                                            </span>
                                        </div>
                                        <div class="progress-bar">
                                            <div class="progress-fill" id="gunning-fog-bar" style="width: 0%;"></div>
                                        </div>
                                    </div>
                                    <div class="progress-item">
                                        <div class="progress-header">
                                            <span class="progress-label">Coleman-Liau Index</span>
                                            <span class="progress-value" id="coleman-liau">0</span>
                                        </div>
                                        <div class="progress-bar">
                                            <div class="progress-fill" id="coleman-liau-bar" style="width: 0%;"></div>
                                        </div>
                                    </div>
                                    <div class="progress-item">
                                        <div class="progress-header">
                                            <span class="progress-label">SMOG Index</span>
                                            <span class="progress-value tooltip" id="smog-index">
                                                0
                                                <span class="tooltip-text">Simple Measure of Gobbledygook - estimates the years of education needed to understand a piece of writing.</span>
                                            </span>
                                        </div>
                                        <div class="progress-bar">
                                            <div class="progress-fill" id="smog-bar" style="width: 0%;"></div>
                                        </div>
                                    </div>
                                </div>
                                
                                <div class="alert alert-warning">
                                    <span id="readability-recommendation">Enter text to get readability recommendations.</span>
                                </div>
                            </div>
                            
                            <div id="word-analysis" class="tab-content">
                                <h3>Top Keywords</h3>
                                <div id="keywords-container"></div>
                                
                                <div class="word-table-container">
                                    <table class="word-table">
                                        <thead>
                                            <tr>
                                                <th>Word</th>
                                                <th>Count</th>
                                                <th>Frequency</th>
                                            </tr>
                                        </thead>
                                        <tbody id="word-frequency-table">
                                            <!-- Word frequency data will be inserted here -->
                                        </tbody>
                                    </table>
                                </div>
                                
                                <h3 style="margin-top: 1.5rem">Word Length Distribution</h3>
                                <div id="word-length-chart" class="chart-container">
                                    <canvas></canvas>
                                </div>
                            </div>
                            
                            <div id="suggestions" class="tab-content">
                                <div id="suggestions-container" class="suggestions-container">
                                    <p>Writing style suggestions will appear here after analysis.</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // DOM Elements
            const inputText = document.getElementById('input-text');
            const analyzeBtn = document.getElementById('analyze-btn');
            const clearBtn = document.getElementById('clear-btn');
            const copyResultsBtn = document.getElementById('copy-results-btn');
            const resultsContainer = document.getElementById('results-container');
            const emptyState = document.getElementById('empty-state');
            const analysisResults = document.getElementById('analysis-results');
            const tabItems = document.querySelectorAll('.tab-item');
            const tabContents = document.querySelectorAll('.tab-content');
            const sampleTextBtn = document.getElementById('sample-text-btn');
            const fileInput = document.getElementById('file-input');
            const fileSelectBtn = document.getElementById('file-select-btn');
            const dropZone = document.getElementById('drop-zone');
            
            // Sample text for demo purposes
            const sampleText = `The Advanced Word Count Analyzer is a powerful tool designed to provide comprehensive text analysis. It gives you detailed insights about your writing, including word count, character count, sentence structure, readability metrics, and vocabulary diversity.\n\nThis tool is perfect for writers, students, and professionals who need to analyze their text for various purposes. You can use it to optimize your content for specific reading levels, improve readability, and ensure your message is communicated effectively.\n\nWith features like readability scoring, keyword analysis, and writing style suggestions, you can fine-tune your text to meet your specific goals. Whether you're writing an academic paper, a blog post, or a professional document, this analyzer helps you understand and improve your writing.\n\nThe user-friendly interface makes it easy to paste your text and get immediate results. Try it now by analyzing this sample text or input your own content to see detailed statistics and suggestions!`;
            
            // Event Listeners
            analyzeBtn.addEventListener('click', analyzeText);
            clearBtn.addEventListener('click', clearText);
            copyResultsBtn.addEventListener('click', copyResults);
            sampleTextBtn.addEventListener('click', insertSampleText);
            fileSelectBtn.addEventListener('click', () => fileInput.click());
            fileInput.addEventListener('change', handleFileSelect);
            
            // Tab switching
            tabItems.forEach(tab => {
                tab.addEventListener('click', () => {
                    tabItems.forEach(t => t.classList.remove('active'));
                    tabContents.forEach(c => c.classList.remove('active'));
                    
                    tab.classList.add('active');
                    const tabContent = document.getElementById(tab.getAttribute('data-tab'));
                    tabContent.classList.add('active');
                });
            });
            
            // File drag and drop
            dropZone.addEventListener('dragover', (e) => {
                e.preventDefault();
                dropZone.classList.add('dragover');
            });
            
            dropZone.addEventListener('dragleave', () => {
                dropZone.classList.remove('dragover');
            });
            
            dropZone.addEventListener('drop', (e) => {
                e.preventDefault();
                dropZone.classList.remove('dragover');
                
                if (e.dataTransfer.files.length) {
                    handleFiles(e.dataTransfer.files);
                }
            });
            
            // Functions
            function handleFileSelect(e) {
                const files = e.target.files;
                handleFiles(files);
            }
            
            function handleFiles(files) {
                const file = files[0];
                const reader = new FileReader();
                
                reader.onload = function(e) {
                    inputText.value = e.target.result;
                };
                
                reader.readAsText(file);
            }
            
            function insertSampleText() {
                inputText.value = sampleText;
            }
            
            function clearText() {
                inputText.value = '';
                showEmptyState();
            }
            
            function copyResults() {
                // Create a text representation of all results
                let resultText = "ADVANCED WORD COUNT ANALYSIS\n\n";
                
                // Basic Stats
                resultText += "BASIC STATISTICS\n";
                resultText += "----------------\n";
                resultText += `Words: ${document.getElementById('word-count').textContent}\n`;
                resultText += `Characters: ${document.getElementById('char-count').textContent}\n`;
                resultText += `Characters (no spaces): ${document.getElementById('char-no-spaces-count').textContent}\n`;
