
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Word Counter & Text Analyzer
</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            color: #333;
            margin: 0;
            padding: 0;
        }

        .container1 {
            margin: 50px auto;
            width: 90%;
            max-width: 800px;
        }

        .output-box {
            background: linear-gradient(145deg, #e6e6e6, #ffffff);
            padding: 20px;
            border-radius: 20px;
            box-shadow: 6px 6px 12px rgba(0, 0, 0, 0.1), -6px -6px 12px rgba(255, 255, 255, 0.7);
        }

        .output-grid {
            display: grid;
            gap: 15px;
        }

        .output-item {
            background: linear-gradient(145deg, #f0f0f0, #dcdcdc);
            padding: 10px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 6px 6px 12px rgba(0, 0, 0, 0.1), -6px -6px 12px rgba(255, 255, 255, 0.7);
            color: #333;
        }

        .output-item:hover {
            background: linear-gradient(145deg, #dcdcdc, #f0f0f0);
            box-shadow: 8px 8px 14px rgba(0, 0, 0, 0.2), -8px -8px 14px rgba(255, 255, 255, 0.9);
        }

        textarea {
            height: 300px;
            padding: 10px;
            border: 2px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
            color: #555;
            outline: none;
            width: 100%;
            box-shadow: 0 0 1px #000;
            resize: none;
        }

        textarea:focus {
            box-shadow: 8px 8px 16px rgba(0, 0, 0, 0.2), -8px -8px 16px rgba(255, 255, 255, 0.9);
        }

        .button-container {
            display: flex;
            justify-content: flex-start;
            gap: 10px;
            margin-bottom: 10px;
        }

        .button-container button {
            background: linear-gradient(145deg, #f0f0f0, #dcdcdc);
            border: none;
            padding: 10px 20px;
            border-radius: 15px;
            font-size: 16px;
            font-weight: bold;
            color: #333;
            cursor: pointer;
            box-shadow: 6px 6px 12px rgba(0, 0, 0, 0.1), -6px -6px 12px rgba(255, 255, 255, 0.7);
            transition: all 0.2s ease-in-out;
        }

        .button-container button:hover {
            background: linear-gradient(145deg, #dcdcdc, #f0f0f0);
            box-shadow: 8px 8px 14px rgba(0, 0, 0, 0.2), -8px -8px 14px rgba(255, 255, 255, 0.9);
            transform: scale(1.05);
        }

        .dark-mode {
            background-color: #121212;
            color: #e0e0e0;
        }

        .dark-mode .output-box, .dark-mode textarea, .dark-mode button {
            background: linear-gradient(145deg, #1e1e1e, #101010);
            color: #e0e0e0;
            box-shadow: 6px 6px 12px rgba(0, 0, 0, 0.7), -6px -6px 12px rgba(255, 255, 255, 0.1);
        }

        .dark-mode .output-item {
            background: linear-gradient(145deg, #1e1e1e, #101010);
            color: #e0e0e0;
        }

        .toggle-dark-mode {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(145deg, #dcdcdc, #f0f0f0);
            border: none;
            padding: 10px 20px;
            border-radius: 15px;
            font-size: 16px;
            font-weight: bold;
            color: #333;
            cursor: pointer;
            box-shadow: 6px 6px 12px rgba(0, 0, 0, 0.1), -6px -6px 12px rgba(255, 255, 255, 0.7);
            transition: all 0.2s ease-in-out;
        }

        .toggle-dark-mode:hover {
            background: linear-gradient(145deg, #f0f0f0, #dcdcdc);
            transform: scale(1.05);
        }
    </style>
</head>
<body>

    <button class="toggle-dark-mode" onclick="toggleDarkMode()">Dark Mode</button>

    <div class="container1">
     <h1>Advanced Word Counter & Text Analyzer</h1>
        <div class="button-container">
            <button id="copyButton">Copy</button>
            <button id="pasteButton">Paste</button>
        </div>
        <textarea id="textInput" placeholder="Enter your text here..."></textarea>
        <div class="output-box">
            <div class="output-grid">
                <div class="output-item"><strong>Characters (with spaces):</strong> <span id="charCountInc">0</span></div>
                <div class="output-item"><strong>Characters (no spaces):</strong> <span id="charCountExc">0</span></div>
                <div class="output-item"><strong>Words:</strong> <span id="wordCount">0</span></div>
                <div class="output-item"><strong>Sentences:</strong> <span id="sentenceCount">0</span></div>
                <div class="output-item"><strong>Paragraphs:</strong> <span id="paragraphCount">0</span></div>
                <div class="output-item"><strong>Spaces:</strong> <span id="spaceCount">0</span></div>
                <div class="output-item"><strong>Reading Time:</strong> <span id="readingTime">0 min</span></div>
            </div>
        </div>
    </div>

    <script>
        const textInput = document.getElementById('textInput');
        const copyButton = document.getElementById('copyButton');
        const pasteButton = document.getElementById('pasteButton');

        // Update counts
        textInput.addEventListener('input', function () {
            const text = this.value;

            document.getElementById('charCountInc').textContent = text.length;
            document.getElementById('charCountExc').textContent = text.replace(/\s+/g, '').length;
            document.getElementById('wordCount').textContent = text.trim().split(/\s+/).filter(word => word).length;
            document.getElementById('sentenceCount').textContent = text.split(/[.!?]/).filter(sentence => sentence.trim().length > 0).length;
            document.getElementById('paragraphCount').textContent = text.split(/\n+/).filter(paragraph => paragraph.trim().length > 0).length;
            document.getElementById('spaceCount').textContent = text.split(' ').length - 1;
            document.getElementById('readingTime').textContent = Math.ceil(text.trim().split(/\s+/).length / 200) + ' min';
        });

        function toggleDarkMode() {
            document.body.classList.toggle('dark-mode');
        }
    </script>
 <p>Keeping track of the number of words in a document is essential for writers, students, and professionals. A <strong>Word Count</strong> tool helps users quickly determine the number of words, characters, and even sentences in their text.</p>

        <h2 style="color: #d6336c;">What is a Word Count Tool?</h2>
        <p>A <strong>Word Count</strong> tool is an online utility that counts words and characters in a given text. It is useful for:</p>
        <ul>
            <li>Writers who need to meet a specific word limit.</li>
            <li>Students working on assignments with word count requirements.</li>
            <li>SEO professionals optimizing content length.</li>
        </ul>

        <h2 style="color: #d6336c;">How to Use a Word Count Tool?</h2>
        <p>Using a Word Count tool is simple:</p>
        <ul>
            <li>Copy and paste your text into the tool.</li>
            <li>Click the count button or see live results.</li>
            <li>View the total number of words, characters, and sentences.</li>
        </ul>

        <h2 style="color: #d6336c;">Why Use a Word Count Tool?</h2>
        <p>Manually counting words is time-consuming and inaccurate. A <strong>Word Count</strong> tool helps by:</p>
        <ul>
            <li>Providing instant and accurate word count results.</li>
            <li>Helping content creators maintain SEO-friendly article lengths.</li>
            <li>Ensuring word limit compliance for academic and professional writing.</li>
        </ul>

        <h2 style="color: #d6336c;">Conclusion</h2>
        <p>The <strong>Word Count</strong> tool is essential for writers, bloggers, students, and professionals. It saves time and ensures accuracy in writing tasks, making it an invaluable resource for content creation.</p>

        <p style="text-align: center; font-size: 14px; color: #555; margin-top: 20px;">
            &copy; 2025 <a href="https://www.calculator360.info" style="color: #d6336c; text-decoration: none;" target="_blank" rel="dofollow">Calculator360.info</a>. All Rights Reserved.
        </p>
</body>
</html>
