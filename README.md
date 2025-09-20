# studymate-pdf-bot
source code

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StudyAI - Your Personal Learning Assistant</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        
        .card-hover {
            transition: all 0.3s ease;
        }
        
        .card-hover:hover {
            transform: translateY(-4px);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        
        .flip-card {
            perspective: 1000px;
            height: 300px;
        }
        
        .flip-card-inner {
            position: relative;
            width: 100%;
            height: 100%;
            text-align: center;
            transition: transform 0.6s;
            transform-style: preserve-3d;
        }
        
        .flip-card.flipped .flip-card-inner {
            transform: rotateY(180deg);
        }
        
        .flip-card-front, .flip-card-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        
        .flip-card-back {
            transform: rotateY(180deg);
        }
        
        .progress-bar {
            transition: width 0.5s ease;
        }
        
        .typing-animation {
            border-right: 2px solid #667eea;
            animation: blink 1s infinite;
        }
        
        @keyframes blink {
            0%, 50% { border-color: transparent; }
            51%, 100% { border-color: #667eea; }
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">
    <!-- Header -->
    <header class="gradient-bg text-white shadow-lg">
        <div class="container mx-auto px-6 py-4">
            <div class="flex items-center justify-between">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-white rounded-lg flex items-center justify-center">
                        <span class="text-2xl">🧠</span>
                    </div>
                    <h1 class="text-2xl font-bold">StudyAI</h1>
                </div>
                <nav class="hidden md:flex space-x-6">
                    <button onclick="showSection('dashboard')" class="nav-btn hover:text-blue-200 transition-colors">Dashboard</button>
                    <button onclick="showSection('pdf-analyzer')" class="nav-btn hover:text-blue-200 transition-colors">PDF Study</button>
                    <button onclick="showSection('flashcards')" class="nav-btn hover:text-blue-200 transition-colors">Flashcards</button>
                    <button onclick="showSection('quiz')" class="nav-btn hover:text-blue-200 transition-colors">Quiz</button>
                    <button onclick="showSection('chat')" class="nav-btn hover:text-blue-200 transition-colors">AI Tutor</button>
                </nav>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-6 py-8">
        <!-- Dashboard Section -->
        <section id="dashboard" class="section active">
            <div class="mb-8">
                <h2 class="text-3xl font-bold text-gray-800 mb-2">Welcome back! 👋</h2>
                <p class="text-gray-600">Ready to continue your learning journey?</p>
            </div>

            <!-- Stats Cards -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                <div class="bg-white rounded-xl p-6 shadow-md card-hover">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-gray-500 text-sm">Study Streak</p>
                            <p class="text-2xl font-bold text-blue-600" id="streak-count">7</p>
                        </div>
                        <div class="w-12 h-12 bg-blue-100 rounded-lg flex items-center justify-center">
                            <span class="text-2xl">🔥</span>
                        </div>
                    </div>
                </div>
                
                <div class="bg-white rounded-xl p-6 shadow-md card-hover">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-gray-500 text-sm">Cards Mastered</p>
                            <p class="text-2xl font-bold text-green-600" id="cards-mastered">24</p>
                        </div>
                        <div class="w-12 h-12 bg-green-100 rounded-lg flex items-center justify-center">
                            <span class="text-2xl">✅</span>
                        </div>
                    </div>
                </div>
                
                <div class="bg-white rounded-xl p-6 shadow-md card-hover">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-gray-500 text-sm">Quiz Score</p>
                            <p class="text-2xl font-bold text-purple-600" id="quiz-score">85%</p>
                        </div>
                        <div class="w-12 h-12 bg-purple-100 rounded-lg flex items-center justify-center">
                            <span class="text-2xl">🎯</span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Quick Actions -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-white rounded-xl p-6 shadow-md card-hover">
                    <h3 class="text-xl font-semibold mb-4">Quick Study</h3>
                    <div class="space-y-3">
                        <button onclick="startQuickReview()" class="w-full bg-blue-600 text-white py-3 rounded-lg hover:bg-blue-700 transition-colors">
                            📚 Review Flashcards
                        </button>
                        <button onclick="startQuickQuiz()" class="w-full bg-green-600 text-white py-3 rounded-lg hover:bg-green-700 transition-colors">
                            🧩 Take Quick Quiz
                        </button>
                    </div>
                </div>
                
                <div class="bg-white rounded-xl p-6 shadow-md card-hover">
                    <h3 class="text-xl font-semibold mb-4">Study Progress</h3>
                    <div class="space-y-4">
                        <div>
                            <div class="flex justify-between text-sm mb-1">
                                <span>Mathematics</span>
                                <span>75%</span>
                            </div>
                            <div class="w-full bg-gray-200 rounded-full h-2">
                                <div class="bg-blue-600 h-2 rounded-full progress-bar" style="width: 75%"></div>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between text-sm mb-1">
                                <span>Science</span>
                                <span>60%</span>
                            </div>
                            <div class="w-full bg-gray-200 rounded-full h-2">
                                <div class="bg-green-600 h-2 rounded-full progress-bar" style="width: 60%"></div>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between text-sm mb-1">
                                <span>History</span>
                                <span>90%</span>
                            </div>
                            <div class="w-full bg-gray-200 rounded-full h-2">
                                <div class="bg-purple-600 h-2 rounded-full progress-bar" style="width: 90%"></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- PDF Analyzer Section -->
        <section id="pdf-analyzer" class="section">
            <div class="mb-8">
                <h2 class="text-3xl font-bold text-gray-800 mb-2">PDF Study Analyzer 📄</h2>
                <p class="text-gray-600">Upload PDFs to generate questions and study materials</p>
            </div>

            <!-- Upload Area -->
            <div class="bg-white rounded-xl p-6 shadow-md mb-6">
                <div id="upload-area" class="border-2 border-dashed border-gray-300 rounded-lg p-8 text-center hover:border-blue-400 transition-colors cursor-pointer">
                    <div class="mb-4">
                        <div class="w-16 h-16 bg-blue-100 rounded-full flex items-center justify-center mx-auto mb-4">
                            <span class="text-2xl">📄</span>
                        </div>
                        <h3 class="text-lg font-semibold mb-2">Upload Your PDF</h3>
                        <p class="text-gray-600 mb-4">Drag and drop your PDF file here, or click to browse</p>
                        <input type="file" id="pdf-input" accept=".pdf" class="hidden" onchange="handlePDFUpload(event)">
                        <button onclick="document.getElementById('pdf-input').click()" class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                            Choose PDF File
                        </button>
                    </div>
                </div>

                <!-- Processing Status -->
                <div id="processing-status" class="hidden mt-6">
                    <div class="bg-blue-50 border border-blue-200 rounded-lg p-4">
                        <div class="flex items-center space-x-3">
                            <div class="animate-spin rounded-full h-6 w-6 border-b-2 border-blue-600"></div>
                            <div>
                                <h4 class="font-semibold text-blue-800">Analyzing PDF...</h4>
                                <p class="text-sm text-blue-600" id="processing-text">Extracting text and generating questions</p>
                            </div>
                        </div>
                        <div class="mt-3">
                            <div class="w-full bg-blue-200 rounded-full h-2">
                                <div id="processing-progress" class="bg-blue-600 h-2 rounded-full transition-all duration-500" style="width: 0%"></div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- PDF Analysis Results -->
                <div id="pdf-results" class="hidden mt-6">
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                        <!-- Document Summary -->
                        <div class="bg-gray-50 rounded-lg p-6">
                            <h4 class="text-lg font-semibold mb-4 flex items-center">
                                <span class="mr-2">📊</span> Document Summary
                            </h4>
                            <div class="space-y-3">
                                <div class="flex justify-between">
                                    <span class="text-gray-600">Pages:</span>
                                    <span id="pdf-pages" class="font-semibold">-</span>
                                </div>
                                <div class="flex justify-between">
                                    <span class="text-gray-600">Words:</span>
                                    <span id="pdf-words" class="font-semibold">-</span>
                                </div>
                                <div class="flex justify-between">
                                    <span class="text-gray-600">Reading Time:</span>
                                    <span id="pdf-reading-time" class="font-semibold">-</span>
                                </div>
                                <div class="flex justify-between">
                                    <span class="text-gray-600">Subject:</span>
                                    <span id="pdf-subject" class="font-semibold">-</span>
                                </div>
                            </div>
                        </div>

                        <!-- Key Topics -->
                        <div class="bg-gray-50 rounded-lg p-6">
                            <h4 class="text-lg font-semibold mb-4 flex items-center">
                                <span class="mr-2">🎯</span> Key Topics
                            </h4>
                            <div id="key-topics" class="space-y-2">
                                <!-- Topics will be populated here -->
                            </div>
                        </div>
                    </div>

                    <!-- Generated Questions -->
                    <div class="mt-6">
                        <div class="flex items-center justify-between mb-4">
                            <h4 class="text-lg font-semibold flex items-center">
                                <span class="mr-2">❓</span> Generated Questions
                            </h4>
                            <div class="flex space-x-2">
                                <button onclick="generateMoreQuestions()" class="bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700 transition-colors text-sm">
                                    ➕ Generate More
                                </button>
                                <button onclick="exportToFlashcards()" class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors text-sm">
                                    📚 Export to Flashcards
                                </button>
                            </div>
                        </div>
                        
                        <div id="generated-questions" class="space-y-4">
                            <!-- Questions will be populated here -->
                        </div>
                    </div>

                    <!-- Study Actions -->
                    <div class="mt-6 flex flex-wrap gap-3">
                        <button onclick="startPDFQuiz()" class="bg-purple-600 text-white px-6 py-3 rounded-lg hover:bg-purple-700 transition-colors">
                            🧩 Take Quiz on This PDF
                        </button>
                        <button onclick="createStudyPlan()" class="bg-indigo-600 text-white px-6 py-3 rounded-lg hover:bg-indigo-700 transition-colors">
                            📅 Create Study Plan
                        </button>
                        <button onclick="summarizePDF()" class="bg-teal-600 text-white px-6 py-3 rounded-lg hover:bg-teal-700 transition-colors">
                            📝 Generate Summary
                        </button>
                    </div>
                </div>
            </div>

            <!-- Study Plan -->
            <div id="study-plan" class="hidden bg-white rounded-xl p-6 shadow-md">
                <h4 class="text-lg font-semibold mb-4 flex items-center">
                    <span class="mr-2">📅</span> Personalized Study Plan
                </h4>
                <div id="study-plan-content" class="space-y-4">
                    <!-- Study plan will be populated here -->
                </div>
            </div>
        </section>

        <!-- Flashcards Section -->
        <section id="flashcards" class="section">
            <div class="mb-8">
                <h2 class="text-3xl font-bold text-gray-800 mb-2">Flashcards 📚</h2>
                <p class="text-gray-600">Study with interactive flashcards</p>
            </div>

            <div class="bg-white rounded-xl p-6 shadow-md mb-6">
                <div class="flex flex-col md:flex-row gap-4 mb-6">
                    <select id="subject-select" class="flex-1 p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent">
                        <option value="math">Mathematics</option>
                        <option value="science">Science</option>
                        <option value="history">History</option>
                    </select>
                    <button onclick="createNewCard()" class="bg-green-600 text-white px-6 py-3 rounded-lg hover:bg-green-700 transition-colors">
                        ➕ Add New Card
                    </button>
                </div>

                <div id="flashcard-container" class="max-w-md mx-auto">
                    <div class="flip-card" id="current-card">
                        <div class="flip-card-inner">
                            <div class="flip-card-front bg-gradient-to-br from-blue-500 to-purple-600 text-white">
                                <div class="text-center">
                                    <h3 class="text-xl font-semibold mb-4">What is the derivative of x²?</h3>
                                    <p class="text-sm opacity-75">Click to reveal answer</p>
                                </div>
                            </div>
                            <div class="flip-card-back bg-gradient-to-br from-green-500 to-blue-600 text-white">
                                <div class="text-center">
                                    <h3 class="text-xl font-semibold mb-4">2x</h3>
                                    <p class="text-sm opacity-75">The power rule: d/dx(xⁿ) = nxⁿ⁻¹</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="flex justify-center space-x-4 mt-6">
                    <button onclick="previousCard()" class="bg-gray-600 text-white px-6 py-2 rounded-lg hover:bg-gray-700 transition-colors">
                        ← Previous
                    </button>
                    <button onclick="flipCard()" class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                        Flip Card
                    </button>
                    <button onclick="nextCard()" class="bg-gray-600 text-white px-6 py-2 rounded-lg hover:bg-gray-700 transition-colors">
                        Next →
                    </button>
                </div>

                <div class="flex justify-center space-x-4 mt-4">
                    <button onclick="markDifficult()" class="bg-red-500 text-white px-4 py-2 rounded-lg hover:bg-red-600 transition-colors text-sm">
                        😰 Difficult
                    </button>
                    <button onclick="markEasy()" class="bg-green-500 text-white px-4 py-2 rounded-lg hover:bg-green-600 transition-colors text-sm">
                        😊 Easy
                    </button>
                </div>
            </div>
        </section>

        <!-- Quiz Section -->
        <section id="quiz" class="section">
            <div class="mb-8">
                <h2 class="text-3xl font-bold text-gray-800 mb-2">Interactive Quiz 🧩</h2>
                <p class="text-gray-600">Test your knowledge with adaptive questions</p>
            </div>

            <div class="bg-white rounded-xl p-6 shadow-md">
                <div id="quiz-setup" class="text-center">
                    <h3 class="text-xl font-semibold mb-6">Choose Your Quiz Settings</h3>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
                        <select id="quiz-subject" class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                            <option value="math">Mathematics</option>
                            <option value="science">Science</option>
                            <option value="history">History</option>
                        </select>
                        <select id="quiz-difficulty" class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                            <option value="easy">Easy</option>
                            <option value="medium">Medium</option>
                            <option value="hard">Hard</option>
                        </select>
                        <select id="quiz-questions" class="p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                            <option value="5">5 Questions</option>
                            <option value="10">10 Questions</option>
                            <option value="15">15 Questions</option>
                        </select>
                    </div>
                    <button onclick="startQuiz()" class="bg-blue-600 text-white px-8 py-3 rounded-lg hover:bg-blue-700 transition-colors text-lg">
                        🚀 Start Quiz
                    </button>
                </div>

                <div id="quiz-active" class="hidden">
                    <div class="mb-6">
                        <div class="flex justify-between items-center mb-2">
                            <span class="text-sm text-gray-600">Question <span id="current-question">1</span> of <span id="total-questions">10</span></span>
                            <span class="text-sm text-gray-600">Score: <span id="current-score">0</span></span>
                        </div>
                        <div class="w-full bg-gray-200 rounded-full h-2">
                            <div id="quiz-progress" class="bg-blue-600 h-2 rounded-full progress-bar" style="width: 10%"></div>
                        </div>
                    </div>

                    <div class="mb-6">
                        <h3 id="question-text" class="text-xl font-semibold mb-4">What is 2 + 2?</h3>
                        <div id="answer-options" class="space-y-3">
                            <button class="answer-option w-full p-4 text-left border border-gray-300 rounded-lg hover:bg-blue-50 hover:border-blue-300 transition-colors">
                                A) 3
                            </button>
                            <button class="answer-option w-full p-4 text-left border border-gray-300 rounded-lg hover:bg-blue-50 hover:border-blue-300 transition-colors">
                                B) 4
                            </button>
                            <button class="answer-option w-full p-4 text-left border border-gray-300 rounded-lg hover:bg-blue-50 hover:border-blue-300 transition-colors">
                                C) 5
                            </button>
                            <button class="answer-option w-full p-4 text-left border border-gray-300 rounded-lg hover:bg-blue-50 hover:border-blue-300 transition-colors">
                                D) 6
                            </button>
                        </div>
                    </div>

                    <div class="flex justify-between">
                        <button onclick="previousQuestion()" id="prev-btn" class="bg-gray-600 text-white px-6 py-2 rounded-lg hover:bg-gray-700 transition-colors" disabled>
                            ← Previous
                        </button>
                        <button onclick="nextQuestion()" id="next-btn" class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                            Next →
                        </button>
                    </div>
                </div>

                <div id="quiz-results" class="hidden text-center">
                    <div class="mb-6">
                        <div class="w-24 h-24 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-4">
                            <span class="text-4xl">🎉</span>
                        </div>
                        <h3 class="text-2xl font-bold mb-2">Quiz Complete!</h3>
                        <p class="text-gray-600">Great job on finishing the quiz</p>
                    </div>
                    
                    <div class="bg-gray-50 rounded-lg p-6 mb-6">
                        <div class="grid grid-cols-3 gap-4 text-center">
                            <div>
                                <p class="text-2xl font-bold text-blue-600" id="final-score">8</p>
                                <p class="text-sm text-gray-600">Correct</p>
                            </div>
                            <div>
                                <p class="text-2xl font-bold text-red-600" id="final-wrong">2</p>
                                <p class="text-sm text-gray-600">Wrong</p>
                            </div>
                            <div>
                                <p class="text-2xl font-bold text-green-600" id="final-percentage">80%</p>
                                <p class="text-sm text-gray-600">Score</p>
                            </div>
                        </div>
                    </div>
                    
                    <button onclick="restartQuiz()" class="bg-blue-600 text-white px-8 py-3 rounded-lg hover:bg-blue-700 transition-colors">
                        🔄 Take Another Quiz
                    </button>
                </div>
            </div>
        </section>

        <!-- AI Chat Section -->
        <section id="chat" class="section">
            <div class="mb-8">
                <h2 class="text-3xl font-bold text-gray-800 mb-2">AI Tutor 🤖</h2>
                <p class="text-gray-600">Get personalized help and explanations</p>
            </div>

            <div class="bg-white rounded-xl shadow-md h-96 flex flex-col">
                <div id="chat-messages" class="flex-1 p-6 overflow-y-auto space-y-4">
                    <div class="flex items-start space-x-3">
                        <div class="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center text-white text-sm">
                            AI
                        </div>
                        <div class="bg-blue-50 rounded-lg p-3 max-w-xs">
                            <p class="text-sm">Hello! I'm your AI tutor. Ask me anything about your studies, and I'll help explain concepts, solve problems, or create practice questions for you! 📚</p>
                        </div>
                    </div>
                </div>
                
                <div class="border-t p-4">
                    <form onsubmit="sendMessage(event)" class="flex space-x-3">
                        <input 
                            type="text" 
                            id="chat-input" 
                            placeholder="Ask me anything about your studies..."
                            class="flex-1 p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                        >
                        <button type="submit" class="bg-blue-600 text-white px-6 py-3 rounded-lg hover:bg-blue-700 transition-colors">
                            Send
                        </button>
                    </form>
                </div>
            </div>
        </section>
    </main>

    <!-- Add New Card Modal -->
    <div id="card-modal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50">
        <div class="bg-white rounded-xl p-6 w-full max-w-md mx-4">
            <h3 class="text-xl font-semibold mb-4">Create New Flashcard</h3>
            <form onsubmit="saveNewCard(event)">
                <div class="mb-4">
                    <label class="block text-sm font-medium text-gray-700 mb-2">Question</label>
                    <textarea id="card-question" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500" rows="3" placeholder="Enter your question..."></textarea>
                </div>
                <div class="mb-6">
                    <label class="block text-sm font-medium text-gray-700 mb-2">Answer</label>
                    <textarea id="card-answer" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500" rows="3" placeholder="Enter the answer..."></textarea>
                </div>
                <div class="flex space-x-3">
                    <button type="button" onclick="closeCardModal()" class="flex-1 bg-gray-600 text-white py-2 rounded-lg hover:bg-gray-700 transition-colors">
                        Cancel
                    </button>
                    <button type="submit" class="flex-1 bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 transition-colors">
                        Save Card
                    </button>
                </div>
            </form>
        </div>
    </div>

    <script>
        // Global state
        let currentSection = 'dashboard';
        let currentCardIndex = 0;
        let currentQuizQuestion = 0;
        let quizScore = 0;
        let quizAnswers = [];
        let isCardFlipped = false;
        let currentPDFContent = null;
        let generatedQuestions = [];

        // Sample data
        const flashcards = {
            math: [
                { question: "What is the derivative of x²?", answer: "2x\n\nThe power rule: d/dx(xⁿ) = nxⁿ⁻¹" },
                { question: "What is the integral of 2x?", answer: "x² + C\n\nWhere C is the constant of integration" },
                { question: "What is the slope-intercept form?", answer: "y = mx + b\n\nWhere m is slope and b is y-intercept" }
            ],
            science: [
                { question: "What is Newton's First Law?", answer: "An object at rest stays at rest, and an object in motion stays in motion, unless acted upon by an external force." },
                { question: "What is the chemical formula for water?", answer: "H₂O\n\nTwo hydrogen atoms bonded to one oxygen atom" }
            ],
            history: [
                { question: "When did World War II end?", answer: "September 2, 1945\n\nWith Japan's formal surrender" },
                { question: "Who was the first President of the United States?", answer: "George Washington\n\nServed from 1789 to 1797" }
            ]
        };

        const quizQuestions = {
            math: {
                easy: [
                    { question: "What is 2 + 2?", options: ["3", "4", "5", "6"], correct: 1 },
                    { question: "What is 5 × 3?", options: ["12", "15", "18", "20"], correct: 1 },
                    { question: "What is 10 ÷ 2?", options: ["3", "4", "5", "6"], correct: 2 }
                ],
                medium: [
                    { question: "What is the square root of 64?", options: ["6", "7", "8", "9"], correct: 2 },
                    { question: "What is 15% of 200?", options: ["25", "30", "35", "40"], correct: 1 }
                ],
                hard: [
                    { question: "What is the derivative of sin(x)?", options: ["cos(x)", "-cos(x)", "sin(x)", "-sin(x)"], correct: 0 }
                ]
            }
        };

        // Navigation
        function showSection(sectionName) {
            document.querySelectorAll('.section').forEach(section => {
                section.classList.remove('active');
                section.style.display = 'none';
            });
            
            document.getElementById(sectionName).style.display = 'block';
            document.getElementById(sectionName).classList.add('active');
            currentSection = sectionName;

            // Update nav buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('text-blue-200');
            });
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            showSection('dashboard');
            updateFlashcard();
        });

        // Dashboard functions
        function startQuickReview() {
            showSection('flashcards');
        }

        function startQuickQuiz() {
            showSection('quiz');
        }

        // Flashcard functions
        function updateFlashcard() {
            const subject = document.getElementById('subject-select').value;
            const cards = flashcards[subject];
            
            if (cards && cards.length > 0) {
                const card = cards[currentCardIndex];
                const cardElement = document.getElementById('current-card');
                
                cardElement.querySelector('.flip-card-front h3').textContent = card.question;
                cardElement.querySelector('.flip-card-back h3').textContent = card.answer.split('\n')[0];
                cardElement.querySelector('.flip-card-back p').textContent = card.answer.split('\n').slice(1).join('\n');
                
                // Reset flip state
                cardElement.classList.remove('flipped');
                isCardFlipped = false;
            }
        }

        function flipCard() {
            const cardElement = document.getElementById('current-card');
            cardElement.classList.toggle('flipped');
            isCardFlipped = !isCardFlipped;
        }

        function nextCard() {
            const subject = document.getElementById('subject-select').value;
            const cards = flashcards[subject];
            
            if (currentCardIndex < cards.length - 1) {
                currentCardIndex++;
                updateFlashcard();
            }
        }

        function previousCard() {
            if (currentCardIndex > 0) {
                currentCardIndex--;
                updateFlashcard();
            }
        }

        function markDifficult() {
            // In a real app, this would save the difficulty rating
            showNotification("Card marked as difficult! 📚", "info");
        }

        function markEasy() {
            // In a real app, this would save the difficulty rating
            showNotification("Great job! Card marked as easy! ✅", "success");
            nextCard();
        }

        function createNewCard() {
            document.getElementById('card-modal').classList.remove('hidden');
            document.getElementById('card-modal').classList.add('flex');
        }

        function closeCardModal() {
            document.getElementById('card-modal').classList.add('hidden');
            document.getElementById('card-modal').classList.remove('flex');
            document.getElementById('card-question').value = '';
            document.getElementById('card-answer').value = '';
        }

        function saveNewCard(event) {
            event.preventDefault();
            
            const question = document.getElementById('card-question').value.trim();
            const answer = document.getElementById('card-answer').value.trim();
            const subject = document.getElementById('subject-select').value;
            
            if (question && answer) {
                flashcards[subject].push({ question, answer });
                closeCardModal();
                showNotification("New flashcard created! 🎉", "success");
                updateFlashcard();
            }
        }

        // Quiz functions
        function startQuiz() {
            const subject = document.getElementById('quiz-subject').value;
            const difficulty = document.getElementById('quiz-difficulty').value;
            const numQuestions = parseInt(document.getElementById('quiz-questions').value);
            
            // Reset quiz state
            currentQuizQuestion = 0;
            quizScore = 0;
            quizAnswers = [];
            
            // Get questions (in a real app, this would be more sophisticated)
            const availableQuestions = quizQuestions[subject]?.[difficulty] || quizQuestions.math.easy;
            
            // Show quiz interface
            document.getElementById('quiz-setup').classList.add('hidden');
            document.getElementById('quiz-active').classList.remove('hidden');
            
            // Update UI
            document.getElementById('total-questions').textContent = Math.min(numQuestions, availableQuestions.length);
            document.getElementById('current-score').textContent = '0';
            
            loadQuizQuestion();
        }

        function loadQuizQuestion() {
            const subject = document.getElementById('quiz-subject').value;
            const difficulty = document.getElementById('quiz-difficulty').value;
            const questions = quizQuestions[subject]?.[difficulty] || quizQuestions.math.easy;
            
            if (currentQuizQuestion < questions.length) {
                const question = questions[currentQuizQuestion];
                
                document.getElementById('current-question').textContent = currentQuizQuestion + 1;
                document.getElementById('question-text').textContent = question.question;
                
                const optionsContainer = document.getElementById('answer-options');
                optionsContainer.innerHTML = '';
                
                question.options.forEach((option, index) => {
                    const button = document.createElement('button');
                    button.className = 'answer-option w-full p-4 text-left border border-gray-300 rounded-lg hover:bg-blue-50 hover:border-blue-300 transition-colors';
                    button.textContent = `${String.fromCharCode(65 + index)}) ${option}`;
                    button.onclick = () => selectAnswer(index);
                    optionsContainer.appendChild(button);
                });
                
                // Update progress
                const progress = ((currentQuizQuestion + 1) / parseInt(document.getElementById('total-questions').textContent)) * 100;
                document.getElementById('quiz-progress').style.width = progress + '%';
                
                // Update buttons
                document.getElementById('prev-btn').disabled = currentQuizQuestion === 0;
            } else {
                finishQuiz();
            }
        }

        function selectAnswer(answerIndex) {
            // Remove previous selections
            document.querySelectorAll('.answer-option').forEach(btn => {
                btn.classList.remove('bg-blue-100', 'border-blue-500');
            });
            
            // Highlight selected answer
            const selectedButton = document.querySelectorAll('.answer-option')[answerIndex];
            selectedButton.classList.add('bg-blue-100', 'border-blue-500');
            
            // Store answer
            quizAnswers[currentQuizQuestion] = answerIndex;
            
            // Check if correct
            const subject = document.getElementById('quiz-subject').value;
            const difficulty = document.getElementById('quiz-difficulty').value;
            const questions = quizQuestions[subject]?.[difficulty] || quizQuestions.math.easy;
            
            if (answerIndex === questions[currentQuizQuestion].correct) {
                quizScore++;
                document.getElementById('current-score').textContent = quizScore;
            }
        }

        function nextQuestion() {
            if (quizAnswers[currentQuizQuestion] !== undefined) {
                currentQuizQuestion++;
                loadQuizQuestion();
            } else {
                showNotification("Please select an answer first! 🤔", "warning");
            }
        }

        function previousQuestion() {
            if (currentQuizQuestion > 0) {
                currentQuizQuestion--;
                loadQuizQuestion();
            }
        }

        function finishQuiz() {
            const totalQuestions = parseInt(document.getElementById('total-questions').textContent);
            const percentage = Math.round((quizScore / totalQuestions) * 100);
            
            document.getElementById('quiz-active').classList.add('hidden');
            document.getElementById('quiz-results').classList.remove('hidden');
            
            document.getElementById('final-score').textContent = quizScore;
            document.getElementById('final-wrong').textContent = totalQuestions - quizScore;
            document.getElementById('final-percentage').textContent = percentage + '%';
            
            // Update dashboard stats
            document.getElementById('quiz-score').textContent = percentage + '%';
        }

        function restartQuiz() {
            document.getElementById('quiz-results').classList.add('hidden');
            document.getElementById('quiz-setup').classList.remove('hidden');
        }

        // Chat functions
        function sendMessage(event) {
            event.preventDefault();
            
            const input = document.getElementById('chat-input');
            const message = input.value.trim();
            
            if (message) {
                addChatMessage(message, 'user');
                input.value = '';
                
                // Simulate AI response
                setTimeout(() => {
                    const response = generateAIResponse(message);
                    addChatMessage(response, 'ai');
                }, 1000);
            }
        }

        function addChatMessage(message, sender) {
            const messagesContainer = document.getElementById('chat-messages');
            const messageDiv = document.createElement('div');
            messageDiv.className = 'flex items-start space-x-3 fade-in';
            
            if (sender === 'user') {
                messageDiv.innerHTML = `
                    <div class="w-8 h-8 bg-gray-600 rounded-full flex items-center justify-center text-white text-sm">
                        You
                    </div>
                    <div class="bg-gray-100 rounded-lg p-3 max-w-xs">
                        <p class="text-sm">${message}</p>
                    </div>
                `;
            } else {
                messageDiv.innerHTML = `
                    <div class="w-8 h-8 bg-blue-600 rounded-full flex items-center justify-center text-white text-sm">
                        AI
                    </div>
                    <div class="bg-blue-50 rounded-lg p-3 max-w-xs">
                        <p class="text-sm typing-animation" id="typing-message"></p>
                    </div>
                `;
            }
            
            messagesContainer.appendChild(messageDiv);
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
            
            if (sender === 'ai') {
                typeMessage(message, messageDiv.querySelector('#typing-message'));
            }
        }

        function typeMessage(message, element) {
            let i = 0;
            element.textContent = '';
            
            function type() {
                if (i < message.length) {
                    element.textContent += message.charAt(i);
                    i++;
                    setTimeout(type, 30);
                } else {
                    element.classList.remove('typing-animation');
                }
            }
            
            type();
        }

        function generateAIResponse(message) {
            const responses = {
                math: [
                    "Great question! Let me help you with that math concept. 📊",
                    "I'd be happy to explain that mathematical principle! 🔢",
                    "That's an interesting math problem. Here's how to approach it... 📐"
                ],
                science: [
                    "Excellent science question! Let me break that down for you. 🔬",
                    "That's a fascinating scientific concept! Here's what you need to know... ⚗️",
                    "Science is amazing! Let me explain that phenomenon... 🧪"
                ],
                general: [
                    "That's a great question! I'm here to help you learn. 📚",
                    "I love helping students understand new concepts! Let me explain... 🎓",
                    "Learning is a journey, and I'm here to guide you! 🌟"
                ]
            };
            
            const lowerMessage = message.toLowerCase();
            let category = 'general';
            
            if (lowerMessage.includes('math') || lowerMessage.includes('equation') || lowerMessage.includes('calculate')) {
                category = 'math';
            } else if (lowerMessage.includes('science') || lowerMessage.includes('chemistry') || lowerMessage.includes('physics')) {
                category = 'science';
            }
            
            const categoryResponses = responses[category];
            return categoryResponses[Math.floor(Math.random() * categoryResponses.length)];
        }

        // Utility functions
        function showNotification(message, type = 'info') {
            const notification = document.createElement('div');
            notification.className = `fixed top-4 right-4 p-4 rounded-lg text-white z-50 fade-in ${
                type === 'success' ? 'bg-green-500' : 
                type === 'warning' ? 'bg-yellow-500' : 
                type === 'error' ? 'bg-red-500' : 'bg-blue-500'
            }`;
            notification.textContent = message;
            
            document.body.appendChild(notification);
            
            setTimeout(() => {
                notification.remove();
            }, 3000);
        }

        // Event listeners
        document.getElementById('subject-select').addEventListener('change', function() {
            currentCardIndex = 0;
            updateFlashcard();
        });

        // Click outside modal to close
        document.getElementById('card-modal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeCardModal();
            }
        });

        // PDF Analysis Functions
        async function handlePDFUpload(event) {
            const file = event.target.files[0];
            if (!file || file.type !== 'application/pdf') {
                showNotification("Please select a valid PDF file! 📄", "error");
                return;
            }

            showProcessingStatus();
            
            try {
                const arrayBuffer = await file.arrayBuffer();
                const pdf = await pdfjsLib.getDocument(arrayBuffer).promise;
                
                updateProcessingProgress(25, "Extracting text from PDF...");
                
                let fullText = '';
                for (let i = 1; i <= pdf.numPages; i++) {
                    const page = await pdf.getPage(i);
                    const textContent = await page.getTextContent();
                    const pageText = textContent.items.map(item => item.str).join(' ');
                    fullText += pageText + ' ';
                }
                
                updateProcessingProgress(50, "Analyzing content...");
                
                const analysis = analyzePDFContent(fullText, pdf.numPages);
                currentPDFContent = { text: fullText, analysis: analysis };
                
                updateProcessingProgress(75, "Generating questions...");
                
                generatedQuestions = generateQuestionsFromText(fullText);
                
                updateProcessingProgress(100, "Analysis complete!");
                
                setTimeout(() => {
                    hideProcessingStatus();
                    displayPDFResults(analysis, generatedQuestions);
                }, 1000);
                
            } catch (error) {
                console.error('Error processing PDF:', error);
                hideProcessingStatus();
                showNotification("Error processing PDF. Please try again! ❌", "error");
            }
        }

        function showProcessingStatus() {
            document.getElementById('processing-status').classList.remove('hidden');
            document.getElementById('pdf-results').classList.add('hidden');
        }

        function hideProcessingStatus() {
            document.getElementById('processing-status').classList.add('hidden');
        }

        function updateProcessingProgress(percentage, text) {
            document.getElementById('processing-progress').style.width = percentage + '%';
            document.getElementById('processing-text').textContent = text;
        }

        function analyzePDFContent(text, numPages) {
            const words = text.split(/\s+/).filter(word => word.length > 0);
            const wordCount = words.length;
            const readingTime = Math.ceil(wordCount / 200); // Average reading speed
            
            // Simple subject detection based on keywords
            const subjects = {
                'Mathematics': ['equation', 'formula', 'theorem', 'proof', 'calculate', 'derivative', 'integral', 'algebra', 'geometry'],
                'Science': ['experiment', 'hypothesis', 'theory', 'molecule', 'atom', 'cell', 'DNA', 'physics', 'chemistry', 'biology'],
                'History': ['century', 'war', 'empire', 'revolution', 'ancient', 'medieval', 'dynasty', 'civilization'],
                'Literature': ['author', 'novel', 'poem', 'character', 'plot', 'theme', 'metaphor', 'symbolism'],
                'Computer Science': ['algorithm', 'programming', 'software', 'database', 'network', 'code', 'function', 'variable']
            };
            
            let detectedSubject = 'General';
            let maxMatches = 0;
            
            for (const [subject, keywords] of Object.entries(subjects)) {
                const matches = keywords.filter(keyword => 
                    text.toLowerCase().includes(keyword.toLowerCase())
                ).length;
                
                if (matches > maxMatches) {
                    maxMatches = matches;
                    detectedSubject = subject;
                }
            }
            
            // Extract key topics (simplified approach)
            const sentences = text.split(/[.!?]+/).filter(s => s.trim().length > 20);
            const keyTopics = extractKeyTopics(text, sentences);
            
            return {
                pages: numPages,
                words: wordCount,
                readingTime: readingTime,
                subject: detectedSubject,
                keyTopics: keyTopics
            };
        }

        function extractKeyTopics(text, sentences) {
            // Simple keyword extraction based on frequency and importance
            const commonWords = new Set(['the', 'a', 'an', 'and', 'or', 'but', 'in', 'on', 'at', 'to', 'for', 'of', 'with', 'by', 'is', 'are', 'was', 'were', 'be', 'been', 'have', 'has', 'had', 'do', 'does', 'did', 'will', 'would', 'could', 'should', 'may', 'might', 'can', 'this', 'that', 'these', 'those']);
            
            const words = text.toLowerCase().match(/\b[a-z]{3,}\b/g) || [];
            const wordFreq = {};
            
            words.forEach(word => {
                if (!commonWords.has(word)) {
                    wordFreq[word] = (wordFreq[word] || 0) + 1;
                }
            });
            
            const sortedWords = Object.entries(wordFreq)
                .sort(([,a], [,b]) => b - a)
                .slice(0, 8)
                .map(([word]) => word.charAt(0).toUpperCase() + word.slice(1));
            
            return sortedWords;
        }

        function generateQuestionsFromText(text) {
            const sentences = text.split(/[.!?]+/).filter(s => s.trim().length > 20);
            const questions = [];
            
            // Generate different types of questions
            const questionTypes = [
                { type: 'definition', template: 'What is {}?', pattern: /(\w+) is (.*?)(?:\.|$)/gi },
                { type: 'explanation', template: 'Explain {}', pattern: /(\w+) (?:means|refers to|indicates) (.*?)(?:\.|$)/gi },
                { type: 'process', template: 'How does {} work?', pattern: /(process|method|procedure) (?:of|for) (\w+)/gi },
                { type: 'comparison', template: 'Compare {} and {}', pattern: /(\w+) (?:versus|vs|compared to) (\w+)/gi }
            ];
            
            // Sample questions based on content analysis
            const sampleQuestions = [
                {
                    question: "What are the main concepts discussed in this document?",
                    answer: "Based on the content analysis, the key concepts include the topics identified in the document.",
                    type: "comprehension",
                    difficulty: "medium"
                },
                {
                    question: "Summarize the key points from the first section",
                    answer: "The first section covers the foundational concepts that are built upon throughout the document.",
                    type: "summary",
                    difficulty: "easy"
                },
                {
                    question: "What are the practical applications mentioned?",
                    answer: "The document discusses various real-world applications and their implementations.",
                    type: "application",
                    difficulty: "hard"
                },
                {
                    question: "How do the concepts relate to each other?",
                    answer: "The concepts are interconnected through various relationships and dependencies discussed in the text.",
                    type: "analysis",
                    difficulty: "hard"
                }
            ];
            
            // Add more sophisticated question generation based on text content
            const words = text.split(/\s+/);
            if (words.length > 100) {
                // Generate questions based on content
                const importantSentences = sentences
                    .filter(s => s.length > 50 && s.length < 200)
                    .slice(0, 6);
                
                importantSentences.forEach((sentence, index) => {
                    if (sentence.includes('is') || sentence.includes('are')) {
                        const parts = sentence.split(/\s+is\s+|\s+are\s+/);
                        if (parts.length >= 2) {
                            questions.push({
                                question: `What ${parts[0].toLowerCase()}?`,
                                answer: parts[1].trim(),
                                type: "definition",
                                difficulty: index < 2 ? "easy" : "medium"
                            });
                        }
                    }
                });
            }
            
            return [...sampleQuestions, ...questions].slice(0, 8);
        }

        function displayPDFResults(analysis, questions) {
            // Update document summary
            document.getElementById('pdf-pages').textContent = analysis.pages;
            document.getElementById('pdf-words').textContent = analysis.words.toLocaleString();
            document.getElementById('pdf-reading-time').textContent = `${analysis.readingTime} min`;
            document.getElementById('pdf-subject').textContent = analysis.subject;
            
            // Display key topics
            const topicsContainer = document.getElementById('key-topics');
            topicsContainer.innerHTML = '';
            analysis.keyTopics.forEach(topic => {
                const topicElement = document.createElement('div');
                topicElement.className = 'bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm inline-block mr-2 mb-2';
                topicElement.textContent = topic;
                topicsContainer.appendChild(topicElement);
            });
            
            // Display generated questions
            const questionsContainer = document.getElementById('generated-questions');
            questionsContainer.innerHTML = '';
            questions.forEach((q, index) => {
                const questionElement = document.createElement('div');
                questionElement.className = 'bg-gray-50 rounded-lg p-4 border-l-4 border-blue-500';
                questionElement.innerHTML = `
                    <div class="flex items-start justify-between mb-2">
                        <h5 class="font-semibold text-gray-800">Question ${index + 1}</h5>
                        <span class="text-xs px-2 py-1 rounded-full ${
                            q.difficulty === 'easy' ? 'bg-green-100 text-green-800' :
                            q.difficulty === 'medium' ? 'bg-yellow-100 text-yellow-800' :
                            'bg-red-100 text-red-800'
                        }">${q.difficulty}</span>
                    </div>
                    <p class="text-gray-700 mb-2">${q.question}</p>
                    <details class="text-sm">
                        <summary class="cursor-pointer text-blue-600 hover:text-blue-800">Show Answer</summary>
                        <p class="mt-2 text-gray-600 bg-white p-3 rounded border-l-2 border-blue-200">${q.answer}</p>
                    </details>
                `;
                questionsContainer.appendChild(questionElement);
            });
            
            // Show results
            document.getElementById('pdf-results').classList.remove('hidden');
            showNotification("PDF analyzed successfully! 🎉", "success");
        }

        function generateMoreQuestions() {
            if (!currentPDFContent) return;
            
            const additionalQuestions = generateQuestionsFromText(currentPDFContent.text);
            generatedQuestions = [...generatedQuestions, ...additionalQuestions].slice(0, 15);
            
            displayPDFResults(currentPDFContent.analysis, generatedQuestions);
            showNotification("Generated more questions! 📚", "success");
        }

        function exportToFlashcards() {
            if (generatedQuestions.length === 0) return;
            
            const subject = currentPDFContent.analysis.subject.toLowerCase();
            if (!flashcards[subject]) {
                flashcards[subject] = [];
            }
            
            generatedQuestions.forEach(q => {
                flashcards[subject].push({
                    question: q.question,
                    answer: q.answer
                });
            });
            
            showNotification("Questions exported to flashcards! 📚", "success");
        }

        function startPDFQuiz() {
            if (generatedQuestions.length === 0) return;
            
            // Convert questions to quiz format
            const quizData = generatedQuestions.slice(0, 5).map(q => {
                // Generate multiple choice options
                const correctAnswer = q.answer.substring(0, 50) + "...";
                const wrongAnswers = [
                    "This is not the correct answer",
                    "Another incorrect option",
                    "This option is also wrong"
                ];
                
                const options = [correctAnswer, ...wrongAnswers].sort(() => Math.random() - 0.5);
                const correctIndex = options.indexOf(correctAnswer);
                
                return {
                    question: q.question,
                    options: options,
                    correct: correctIndex
                };
            });
            
            // Add to quiz questions temporarily
            quizQuestions.pdf = { easy: quizData };
            
            // Switch to quiz section and start
            showSection('quiz');
            document.getElementById('quiz-subject').value = 'pdf';
            startQuiz();
        }

        function createStudyPlan() {
            if (!currentPDFContent) return;
            
            const analysis = currentPDFContent.analysis;
            const studyPlan = generateStudyPlan(analysis);
            
            const planContainer = document.getElementById('study-plan-content');
            planContainer.innerHTML = '';
            
            studyPlan.forEach((day, index) => {
                const dayElement = document.createElement('div');
                dayElement.className = 'bg-gray-50 rounded-lg p-4 border-l-4 border-indigo-500';
                dayElement.innerHTML = `
                    <h5 class="font-semibold text-gray-800 mb-2">Day ${index + 1}: ${day.title}</h5>
                    <ul class="space-y-1 text-sm text-gray-600">
                        ${day.tasks.map(task => `<li class="flex items-center"><span class="mr-2">•</span>${task}</li>`).join('')}
                    </ul>
                    <div class="mt-2 text-xs text-gray-500">Estimated time: ${day.duration}</div>
                `;
                planContainer.appendChild(dayElement);
            });
            
            document.getElementById('study-plan').classList.remove('hidden');
            showNotification("Study plan created! 📅", "success");
        }

        function generateStudyPlan(analysis) {
            const readingTime = analysis.readingTime;
            const topics = analysis.keyTopics;
            
            const plan = [
                {
                    title: "Initial Reading & Overview",
                    tasks: [
                        "Read through the entire document once",
                        "Identify main themes and concepts",
                        "Create a concept map of key topics"
                    ],
                    duration: `${Math.ceil(readingTime * 1.5)} minutes`
                },
                {
                    title: "Deep Dive into Key Concepts",
                    tasks: [
                        `Focus on understanding: ${topics.slice(0, 3).join(', ')}`,
                        "Take detailed notes on each concept",
                        "Look up unfamiliar terms"
                    ],
                    duration: `${Math.ceil(readingTime)} minutes`
                },
                {
                    title: "Practice & Application",
                    tasks: [
                        "Answer the generated questions",
                        "Create your own questions",
                        "Practice explaining concepts out loud"
                    ],
                    duration: "30 minutes"
                },
                {
                    title: "Review & Reinforcement",
                    tasks: [
                        "Review flashcards created from this PDF",
                        "Take the practice quiz",
                        "Summarize key points in your own words"
                    ],
                    duration: "20 minutes"
                }
            ];
            
            return plan;
        }

        function summarizePDF() {
            if (!currentPDFContent) return;
            
            const text = currentPDFContent.text;
            const sentences = text.split(/[.!?]+/).filter(s => s.trim().length > 20);
            
            // Simple extractive summarization
            const summary = sentences
                .slice(0, Math.min(5, sentences.length))
                .map(s => s.trim())
                .join('. ') + '.';
            
            // Create summary modal or section
            const summaryModal = document.createElement('div');
            summaryModal.className = 'fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50';
            summaryModal.innerHTML = `
                <div class="bg-white rounded-xl p-6 w-full max-w-2xl mx-4 max-h-96 overflow-y-auto">
                    <h3 class="text-xl font-semibold mb-4 flex items-center">
                        <span class="mr-2">📝</span> Document Summary
                    </h3>
                    <div class="prose text-gray-700 mb-6">
                        <p>${summary}</p>
                    </div>
                    <div class="flex justify-end">
                        <button onclick="this.closest('.fixed').remove()" class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition-colors">
                            Close
                        </button>
                    </div>
                </div>
            `;
            
            document.body.appendChild(summaryModal);
        }

        // Drag and drop functionality
        document.addEventListener('DOMContentLoaded', function() {
            const uploadArea = document.getElementById('upload-area');
            
            uploadArea.addEventListener('dragover', function(e) {
                e.preventDefault();
                uploadArea.classList.add('border-blue-400', 'bg-blue-50');
            });
            
            uploadArea.addEventListener('dragleave', function(e) {
                e.preventDefault();
                uploadArea.classList.remove('border-blue-400', 'bg-blue-50');
            });
            
            uploadArea.addEventListener('drop', function(e) {
                e.preventDefault();
                uploadArea.classList.remove('border-blue-400', 'bg-blue-50');
                
                const files = e.dataTransfer.files;
                if (files.length > 0) {
                    document.getElementById('pdf-input').files = files;
                    handlePDFUpload({ target: { files: files } });
                }
            });
            
            uploadArea.addEventListener('click', function() {
                document.getElementById('pdf-input').click();
            });
        });

        // Keyboard shortcuts
        document.addEventListener('keydown', function(e) {
            if (currentSection === 'flashcards') {
                if (e.key === ' ') {
                    e.preventDefault();
                    flipCard();
                } else if (e.key === 'ArrowLeft') {
                    previousCard();
                } else if (e.key === 'ArrowRight') {
                    nextCard();
                }
            }
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'981eec1de0fa936d',t:'MTc1ODM0NjEzNy4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
