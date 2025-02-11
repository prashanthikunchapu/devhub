<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code Editor - Multi-Language Learning Platform</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        .header {
            background-color: #2c3e50;
            color: white;
            padding: 15px;
            border-radius: 8px 8px 0 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .controls {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        select, button {
            padding: 8px 15px;
            border: none;
            border-radius: 4px;
            font-size: 14px;
        }

        select {
            background-color: #34495e;
            color: white;
            cursor: pointer;
        }

        button {
            background-color: #27ae60;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #219a52;
        }

        .main-container {
            display: grid;
            grid-template-columns: 1fr 300px;
            gap: 20px;
            margin-top: 20px;
        }

        .editor-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .editor-box, .output-box, .learning-resources {
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .learning-resources {
            padding: 20px;
            height: fit-content;
        }

        .learning-resources h2 {
            margin-top: 0;
            color: #2c3e50;
        }

        .course-list {
            list-style: none;
            padding: 0;
            margin: 0;
        }

        .course-item {
            margin-bottom: 15px;
            padding: 10px;
            border: 1px solid #eee;
            border-radius: 4px;
        }

        .course-item h3 {
            margin: 0 0 5px 0;
            font-size: 16px;
            color: #2c3e50;
        }

        .course-item p {
            margin: 0;
            font-size: 14px;
            color: #666;
        }

        .course-item a {
            color: #27ae60;
            text-decoration: none;
            font-weight: bold;
        }

        .course-item a:hover {
            text-decoration: underline;
        }

        .box-header {
            background-color: #34495e;
            color: white;
            padding: 10px 15px;
            border-radius: 8px 8px 0 0;
            font-size: 14px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .language-info {
            font-size: 12px;
            opacity: 0.8;
        }

        #codeEditor, #output {
            width: 100%;
            height: 500px;
            border: none;
            padding: 15px;
            font-family: 'Consolas', 'Monaco', monospace;
            font-size: 14px;
            line-height: 1.5;
            resize: none;
            box-sizing: border-box;
        }

        #codeEditor {
            background-color: #2b2b2b;
            color: #e6e6e6;
        }

        #output {
            background-color: #f8f9fa;
            color: #333;
        }

        .status-bar {
            background-color: #34495e;
            color: white;
            padding: 10px 15px;
            border-radius: 0 0 8px 8px;
            font-size: 12px;
            display: flex;
            justify-content: space-between;
        }

        .error-panel {
            margin-top: 10px;
            padding: 10px;
            background-color: #fee;
            border-left: 4px solid #e74c3c;
            border-radius: 4px;
            display: none;
        }

        .error-title {
            color: #c0392b;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .error-message {
            color: #666;
            font-size: 14px;
        }

        .keyword { color: #ff79c6; }
        .type { color: #8be9fd; }
        .string { color: #f1fa8c; }
        .comment { color: #6272a4; }
        .number { color: #bd93f9; }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Code Editor & Learning Platform</h1>
            <div class="controls">
                <select id="languageSelect">
                    <option value="python">Python</option>
                    <option value="java">Java</option>
                    <option value="cpp">C++</option>
                    <option value="c">C</option>
                </select>
                <button id="runButton">Compile & Run</button>
            </div>
        </div>

        <div class="main-container">
            <div class="editor-container">
                <div class="editor-box">
                    <div class="box-header">
                        <span>Code Editor</span>
                        <span class="language-info" id="languageInfo">Python</span>
                    </div>
                    <textarea id="codeEditor" spellcheck="false"></textarea>
                    <div class="error-panel" id="errorPanel">
                        <div class="error-title">Error Detected</div>
                        <div class="error-message" id="errorMessage"></div>
                    </div>
                    <div class="status-bar">
                        <span id="editorStatus">Ready</span>
                        <span id="cursorPosition">Line: 1, Column: 1</span>
                    </div>
                </div>

                <div class="output-box">
                    <div class="box-header">Output</div>
                    <textarea id="output" readonly></textarea>
                    <div class="status-bar">
                        <span id="outputStatus">Ready</span>
                    </div>
                </div>
            </div>

            <div class="learning-resources">
                <h2>Learning Resources</h2>
                <div id="courseList"></div>
            </div>
        </div>
    </div>

    <script>
        // Initialize variables
        const codeEditor = document.getElementById('codeEditor');
        const output = document.getElementById('output');
        const languageSelect = document.getElementById('languageSelect');
        const runButton = document.getElementById('runButton');
        const editorStatus = document.getElementById('editorStatus');
        const outputStatus = document.getElementById('outputStatus');
        const cursorPosition = document.getElementById('cursorPosition');
        const languageInfo = document.getElementById('languageInfo');
        const errorPanel = document.getElementById('errorPanel');
        const errorMessage = document.getElementById('errorMessage');
        const courseList = document.getElementById('courseList');

        // Language courses
        const courses = {
            python: [
                {
                    title: "Python for Beginners",
                    author: "freeCodeCamp",
                    url: "https://www.youtube.com/watch?v=rfscVS0vtbw",
                    description: "Complete Python tutorial for beginners"
                },
                {
                    title: "Python OOP Tutorial",
                    author: "Tech With Tim",
                    url: "https://www.youtube.com/watch?v=JeznW_7DlB0",
                    description: "Object Oriented Programming in Python"
                }
            ],
            java: [
                {
                    title: "Java Programming Tutorial",
                    author: "Programming with Mosh",
                    url: "https://www.youtube.com/watch?v=eIrMbAQSU34",
                    description: "Complete Java course for beginners"
                },
                {
                    title: "Java Full Course",
                    author: "Telusko",
                    url: "https://www.youtube.com/watch?v=8cm1x4bC610",
                    description: "Java programming masterclass"
                }
            ],
            cpp: [
                {
                    title: "C++ Tutorial for Beginners",
                    author: "freeCodeCamp",
                    url: "https://www.youtube.com/watch?v=vLnPwxZdW4Y",
                    description: "Full C++ course"
                },
                {
                    title: "C++ Programming Course",
                    author: "CodeBeauty",
                    url: "https://www.youtube.com/watch?v=GQp1zzTwrIg",
                    description: "Complete C++ tutorial series"
                }
            ],
            c: [
                {
                    title: "C Programming Tutorial",
                    author: "freeCodeCamp",
                    url: "https://www.youtube.com/watch?v=KJgsSFOSQv0",
                    description: "Complete C programming course"
                },
                {
                    title: "C Language Tutorial",
                    author: "Jenny's Lectures",
                    url: "https://www.youtube.com/watch?v=rLf3jnHxSmU",
                    description: "C programming basics to advanced"
                }
            ]
        };

        // Template code for different languages
        const templates = {
            python: `def calculate_sum(n):
    # Calculate sum of numbers from 0 to n-1
    total = 0
    for i in range(n):
        total += i
    return total

# Calculate sum of first 5 numbers
result = calculate_sum(5)
print(f"Sum: {result}")`,
            java: `public class Main {
    public static void main(String[] args) {
        // Calculate sum of numbers from 0 to 4
        int sum = 0;
        for(int i = 0; i < 5; i++) {
            sum += i;
        }
        System.out.println("Sum: " + sum);
    }
}`,
            cpp: `#include <iostream>
using namespace std;

int main() {
    // Calculate sum of numbers from 0 to 4
    int sum = 0;
    for(int i = 0; i < 5; i++) {
        sum += i;
    }
    cout << "Sum: " << sum << endl;
    return 0;
}`,
            c: `#include <stdio.h>

int main() {
    // Calculate sum of numbers from 0 to 4
    int sum = 0;
    for(int i = 0; i < 5; i++) {
        sum += i;
    }
    printf("Sum: %d\\n", sum);
    return 0;
}`
        };

        // Language-specific syntax validation rules
        const syntaxRules = {
            python: {
                keywords: ['def', 'class', 'for', 'while', 'if', 'else', 'elif', 'try', 'except', 'finally', 'with', 'as', 'import', 'from', 'return'],
                types: ['int', 'str', 'float', 'bool', 'list', 'dict', 'tuple', 'set'],
                required: ['def', ':'],
                indentation: true
            },
            java: {
                keywords: ['public', 'class', 'static', 'void', 'main', 'for', 'if', 'else', 'while', 'do', 'break', 'continue', 'return', 'new', 'try', 'catch'],
                types: ['int', 'double', 'float', 'boolean', 'char', 'String', 'long', 'byte', 'short'],
                required: ['class', 'public static void main'],
                indentation: false
            },
            cpp: {
                keywords: ['using', 'namespace', 'for', 'if', 'else', 'while', 'do', 'break', 'continue', 'return', 'new', 'try', 'catch'],
                types: ['int', 'double', 'float', 'bool', 'char', 'string', 'long', 'void'],
                required: ['main()', 'return'],
                indentation: false
            },
            c: {
                keywords: ['for', 'if', 'else', 'while', 'do', 'break', 'continue', 'return'],
                types: ['int', 'double', 'float', 'char', 'void', 'long'],
                required: ['main()', 'return'],
                indentation: false
            }
        };

        // Common runtime errors by language
        const runtimeErrors = {
            python: {
                "NameError": "Trying to use a variable that hasn't been defined",
                "TypeError": "Operation or function applied to the wrong type",
                "IndexError": "Trying to access a list element that doesn't exist",
                "KeyError": "Trying to access a dictionary key that doesn't exist",
                "ZeroDivisionError": "Trying to divide by zero",
                "AttributeError": "Trying to access an attribute that doesn't exist",
                "IndentationError": "Incorrect indentation of code blocks"
            },
            java: {
                "NullPointerException": "Trying to use a null object reference",
                "ArrayIndexOutOfBoundsException": "Trying to access an invalid array index",
                "ClassCastException": "Invalid casting between incompatible classes",
                "ArithmeticException": "Mathematical error like division by zero",
                "IllegalArgumentException": "Method received invalid argument",
                "OutOfMemoryError": "Not enough memory to allocate objects"
            },
            cpp: {
                "Segmentation fault": "Accessing invalid memory location",
                "Stack overflow": "Recursive function with no base case",
                "Memory leak": "Failing to free allocated memory",
                "Buffer overflow": "Writing beyond array bounds",
                "Null pointer dereference": "Accessing null pointer",
                "Division by zero": "Attempting to divide by zero"
            },
            c: {
                "Segmentation fault": "Accessing invalid memory location",
                "Stack overflow": "Recursive function with no base case",
                "Buffer overflow": "Writing beyond array bounds",
                "Memory leak": "Failing to free allocated memory",
                "Null pointer dereference": "Accessing null pointer",
                "Division by zero": "Attempting to divide by zero"
            }
        };

        // Update learning resources
        function updateLearningResources(language) {
            const languageCourses = courses[language];
            let html = '<ul class="course-list">';
            
            languageCourses.forEach(course => {
                html += `
                    <li class="course-item">
                        <h3>${course.title}</h3>
                        <p>By: ${course.author}</p>
                        <p>${course.description}</p>
                        <a href="${course.url}" target="_blank">Watch Course →</a>
                    </li>
                `;
            });
            
            html += '</ul>';
            courseList.innerHTML = html;
        }

        // Set initial template
        function setTemplate() {
            const language = languageSelect.value;
            codeEditor.value = templates[language];
            languageInfo.textContent = language.toUpperCase();
            editorStatus.textContent = `${language.toUpperCase()} code loaded`;
            updateLearningResources(language);
            highlightSyntax();
            errorPanel.style.display = 'none';
        }

        // Syntax highlighting
        function highlightSyntax() {
            const code = codeEditor.value;
            const language = languageSelect.value;
            const rules = syntaxRules[language];
            
            let highlighted = code
                .replace(/\/\/.*/g, match => `<span class="comment">${match}</span>`)
                .replace(/\/\*[\s\S]*?\*\//g, match => `<span class="comment">${match}</span>`)
                .replace(/\b(\d+)\b/g, match => `<span class="number">${match}</span>`)
                .replace(/"[^"]*"/g, match => `<span class="string">${match}</span>`);

            rules.keywords.forEach(keyword => {
                const regex = new RegExp(`\\b${keyword}\\b`, 'g');
                highlighted = highlighted.replace(regex, `<span class="keyword">${keyword}</span>`);
            });

            rules.types.forEach(type => {
                const regex = new RegExp(`\\b${type}\\b`, 'g');
                highlighted = highlighted.replace(regex, `<span class="type">${type}</span>`);
            });

            output.innerHTML = highlighted;
        }

        // Validate code based on language
        function validateCode(code, language) {
            const rules = syntaxRules[language];
            
            // Check for required elements
            for (const required of rules.required) {
                if (!code.includes(required)) {
                    return { 
                        valid: false, 
                        message: `Missing required element: ${required}`,
                        type: 'compile'
                    };
                }
            }

            // Language-specific validation
            if (language === 'python') {
                // Check Python indentation
                const lines = code.split('\n');
                let expectedIndent = 0;
                for (let i = 0; i < lines.length; i++) {
                    const line = lines[i];
                    const indent = line.search(/\S/);
                    if (indent === -1) continue; // Skip empty lines
                    
                    if (line.trim().endsWith(':')) {
                        expectedIndent += 4;
                    } else if (indent < expectedIndent) {
                        return {
                            valid: false,
                            message: `Incorrect indentation at line ${i + 1}`,
                            type: 'compile'
                        };
                    }
                }
            } else {
                // Check for balanced braces in C-style languages
                try {
                    let braceCount = 0;
                    for (const char of code) {
                        if (char === '{') braceCount++;
                        if (char === '}') braceCount--;
                        if (braceCount < 0) throw new Error('Unmatched closing brace');
                    }
                    if (braceCount !== 0) throw new Error('Unmatched opening brace');

                    // Check for semicolons
                    const lines = code.split('\n');
                    for (const line of lines) {
                        const trimmed = line.trim();
                        if (trimmed && !trimmed.startsWith('//') && !trimmed.startsWith('/*') && 
                            !trimmed.endsWith('{') && !trimmed.endsWith('}') && 
                            !trimmed.endsWith(';')) {
                            throw new Error('Missing semicolon');
                        }
                    }
                } catch (error) {
                    return {
                        valid: false,
                        message: `Syntax error: ${error.message}`,
                        type: 'compile'
                    };
                }
            }

            // Check for potential runtime errors
            const errors = runtimeErrors[language];
            for (const [errorType, description] of Object.entries(errors)) {
                if (code.includes('null') || code.includes('nullptr')) {
                    return {
                        valid: false,
                        message: `Potential ${errorType}: ${description}`,
                        type: 'runtime'
                    };
                }
                if (code.includes('/0') || code.includes('/ 0')) {
                    return {
                        valid: false,
                        message: `Potential ${errorType}: ${description}`,
                        type: 'runtime'
                    };
                }
            }

            return { valid: true, message: 'Code validation successful' };
        }

        // Execute code (preview mode)
        function executeCode() {
            const code = codeEditor.value;
            const language = languageSelect.value;
            
            // Validate code first
            const validation = validateCode(code, language);
            if (!validation.valid) {
                errorPanel.style.display = 'block';
                errorMessage.textContent = `${validation.type === 'compile' ? 'Compilation' : 'Runtime'} Error: ${validation.message}`;
                outputStatus.textContent = validation.type === 'compile' ? 'Compilation failed' : 'Runtime error detected';
                return;
            }

            errorPanel.style.display = 'none';
            output.value = `// ${language.toUpperCase()} Code Preview\n`;
            output.value += `// Note: This is a preview. Actual compilation and execution requires a server environment.\n\n`;
            output.value += code;
            
            outputStatus.textContent = 'Preview mode';
        }

        // Track cursor position
        function updateCursorPosition() {
            const text = codeEditor.value.substr(0, codeEditor.selectionStart);
            const lines = text.split('\n');
            const currentLine = lines.length;
            const currentColumn = lines[lines.length - 1].length + 1;
            cursorPosition.textContent = `Line: ${currentLine}, Column: ${currentColumn}`;
        }

        // Event listeners
        languageSelect.addEventListener('change', setTemplate);
        runButton.addEventListener('click', executeCode);
        codeEditor.addEventListener('keyup', () => {
            updateCursorPosition();
            highlightSyntax();
        });
        codeEditor.addEventListener('click', updateCursorPosition);
        codeEditor.addEventListener('keydown', function(e) {
            if (e.key === 'Tab') {
                e.preventDefault();
                const start = this.selectionStart;
                const end = this.selectionEnd;
                this.value = this.value.substring(0, start) + '    ' + this.value.substring(end);
                this.selectionStart = this.selectionEnd = start + 4;
            }
        });

        // Initialize the editor
        setTemplate();
    </script>
</body>
</html>
