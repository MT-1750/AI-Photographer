
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>AI Photography Assistant</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <meta name="description" content="Upload a photo, choose event type, and get professional Lightroom-style suggestions from AI." />
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap');
        
        :root { 
            --glass: rgba(255,255,255,0.1); 
            --glass-dark: rgba(17,24,39,0.8); 
            --neon-blue: #00d4ff;
            --neon-purple: #a855f7;
            --neon-pink: #ec4899;
            --neon-green: #10b981;
        }
        
        body { 
            font-family: 'Inter', ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; 
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
            min-height: 100vh;
            overflow-x: hidden;
            position: relative;
        }

        /* Enhanced animated background */
        .particles {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        .particle {
            position: absolute;
            width: 3px;
            height: 3px;
            background: var(--neon-blue);
            border-radius: 50%;
            opacity: 0.7;
            animation: float 8s ease-in-out infinite;
        }

        .particle.large {
            width: 4px;
            height: 4px;
            background: var(--neon-purple);
            animation: floatLarge 10s ease-in-out infinite;
        }

        .particle.small {
            width: 1px;
            height: 1px;
            background: var(--neon-pink);
            animation: floatSmall 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0deg); opacity: 0.4; }
            50% { transform: translateY(-30px) rotate(180deg); opacity: 1; }
        }

        @keyframes floatLarge {
            0%, 100% { transform: translateY(0px) rotate(0deg) scale(1); opacity: 0.3; }
            50% { transform: translateY(-40px) rotate(360deg) scale(1.2); opacity: 0.8; }
        }

        @keyframes floatSmall {
            0%, 100% { transform: translateY(0px) rotate(0deg); opacity: 0.6; }
            50% { transform: translateY(-20px) rotate(-180deg); opacity: 1; }
        }

        /* Animated background waves */
        .bg-waves {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.1;
        }

        .wave {
            position: absolute;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, var(--neon-blue), var(--neon-purple), var(--neon-pink));
            border-radius: 50%;
            animation: waveMove 20s linear infinite;
        }

        .wave:nth-child(1) {
            top: -50%;
            left: -50%;
            animation-delay: 0s;
        }

        .wave:nth-child(2) {
            top: -60%;
            right: -50%;
            animation-delay: -7s;
        }

        .wave:nth-child(3) {
            bottom: -50%;
            left: -50%;
            animation-delay: -14s;
        }

        @keyframes waveMove {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Super animated title */
        .super-title {
            background: linear-gradient(45deg, #00d4ff, #a855f7, #ec4899, #10b981, #00d4ff);
            background-size: 400% 400%;
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: gradientShift 3s ease-in-out infinite, titlePulse 2s ease-in-out infinite alternate;
            text-shadow: 0 0 30px rgba(0, 212, 255, 0.3);
            position: relative;
            display: inline-block;
        }

        .super-title::before {
            content: attr(data-text);
            position: absolute;
            top: 0;
            left: 0;
            z-index: -1;
            background: linear-gradient(45deg, #00d4ff, #a855f7);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            filter: blur(2px);
            opacity: 0.7;
            animation: titleGlow 2s ease-in-out infinite alternate;
        }

        @keyframes gradientShift {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }

        @keyframes titlePulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.02); }
        }

        @keyframes titleGlow {
            0% { filter: blur(2px) brightness(1); }
            100% { filter: blur(4px) brightness(1.3); }
        }

        /* Enhanced glowing borders */
        .glow-border {
            position: relative;
            border: 1px solid rgba(0, 212, 255, 0.4);
            box-shadow: 
                0 0 25px rgba(0, 212, 255, 0.15),
                inset 0 0 25px rgba(0, 212, 255, 0.1);
        }

        .glow-border::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, var(--neon-blue), var(--neon-purple), var(--neon-pink), var(--neon-green));
            border-radius: inherit;
            z-index: -1;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .glow-border:hover::before {
            opacity: 0.8;
            animation: borderGlow 2s ease-in-out infinite alternate;
        }

        @keyframes borderGlow {
            0% { filter: blur(3px); }
            100% { filter: blur(8px); }
        }

        /* Tech-style card with enhanced effects */
        .tech-card {
            background: linear-gradient(135deg, rgba(15, 15, 35, 0.95) 0%, rgba(26, 26, 46, 0.9) 100%);
            border: 1px solid rgba(0, 212, 255, 0.3);
            box-shadow: 
                0 8px 40px rgba(0, 0, 0, 0.4),
                0 0 50px rgba(0, 212, 255, 0.1),
                inset 0 1px 0 rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(20px);
            position: relative;
        }

        .tech-card::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, transparent, var(--neon-blue), transparent);
            animation: scan 4s ease-in-out infinite;
        }

        @keyframes scan {
            0%, 100% { opacity: 0; transform: translateX(-100%); }
            50% { opacity: 1; transform: translateX(100%); }
        }

        /* File drop area with enhanced styling */
        .file-drop { 
            transition: all 0.4s ease;
            position: relative;
            background: linear-gradient(135deg, rgba(0, 212, 255, 0.08) 0%, rgba(168, 85, 247, 0.08) 100%);
            border: 2px dashed rgba(0, 212, 255, 0.4);
            cursor: pointer;
        }

        .file-drop.dragover { 
            background: linear-gradient(135deg, rgba(0, 212, 255, 0.2) 0%, rgba(168, 85, 247, 0.2) 100%);
            border-color: var(--neon-blue);
            box-shadow: 
                0 0 40px rgba(0, 212, 255, 0.4),
                inset 0 0 40px rgba(0, 212, 255, 0.15);
            transform: translateY(-3px) scale(1.02);
        }

        /* Enhanced button animations */
        .tech-button {
            position: relative;
            background: linear-gradient(135deg, var(--neon-blue) 0%, var(--neon-purple) 100%);
            border: none;
            overflow: hidden;
            transition: all 0.4s ease;
        }

        .tech-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
            transition: left 0.6s ease;
        }

        .tech-button:hover::before {
            left: 100%;
        }

        .tech-button:hover {
            box-shadow: 0 0 40px rgba(0, 212, 255, 0.6);
            transform: translateY(-3px) scale(1.02);
        }

        .tech-button:active {
            transform: translateY(-1px) scale(1.01);
        }

        /* Input styling */
        .tech-input {
            background: rgba(15, 15, 35, 0.9);
            border: 1px solid rgba(0, 212, 255, 0.4);
            color: #e2e8f0;
            transition: all 0.3s ease;
        }

        .tech-input:focus {
            border-color: var(--neon-blue);
            box-shadow: 0 0 25px rgba(0, 212, 255, 0.4);
            outline: none;
        }

        /* Glowing text */
        .glow-text {
            text-shadow: 0 0 15px rgba(0, 212, 255, 0.6);
        }

        .sr-only { position:absolute; width:1px; height:1px; padding:0; margin:-1px; overflow:hidden; clip:rect(0,0,0,0); border:0; }
        
        /* Enhanced spinner */
        .spinner { 
            border: 3px solid rgba(0, 212, 255, 0.3); 
            border-left-color: var(--neon-blue); 
            border-radius: 50%; 
            width: 1em; 
            height: 1em; 
            display: inline-block; 
            animation: spin 1s linear infinite, spinnerPulse 2s ease-in-out infinite alternate;
        }

        @keyframes spin { to { transform: rotate(360deg); } }
        @keyframes spinnerPulse { 
            0% { filter: brightness(1) drop-shadow(0 0 5px var(--neon-blue)); }
            100% { filter: brightness(1.5) drop-shadow(0 0 20px var(--neon-blue)); }
        }

        pre.prose { white-space:pre-wrap; word-break:break-word; }
        
        .prose-custom { color: #e2e8f0; }
        .prose-custom h4 { color: #00d4ff; margin-bottom: 12px; font-size: 16px; font-weight: 600; }
        .prose-custom p { color: #cbd5e1; margin-bottom: 8px; line-height: 1.6; }
        .prose-custom strong { color: #f1f5f9; font-weight: 600; }
        .prose-custom ul { list-style-type: none; padding-left: 0; }
        .prose-custom ul li { 
            margin-bottom: 10px; 
            padding: 10px 15px;
            background: rgba(0, 212, 255, 0.08);
            border-left: 4px solid var(--neon-blue);
            border-radius: 0 10px 10px 0;
            transition: all 0.3s ease;
            color: #e2e8f0;
        }
        .prose-custom ul li:hover {
            background: rgba(0, 212, 255, 0.15);
            transform: translateX(8px);
            box-shadow: 0 0 20px rgba(0, 212, 255, 0.2);
        }

        /* Status indicator */
        .status-success {
            background: linear-gradient(90deg, rgba(16, 185, 129, 0.15), rgba(16, 185, 129, 0.25));
            border: 1px solid rgba(16, 185, 129, 0.4);
            color: #10b981;
        }

        .status-error {
            background: linear-gradient(90deg, rgba(239, 68, 68, 0.15), rgba(239, 68, 68, 0.25));
            border: 1px solid rgba(239, 68, 68, 0.4);
            color: #ef4444;
        }

        /* Preview card enhancement */
        .preview-card {
            background: linear-gradient(135deg, rgba(15, 15, 35, 0.95) 0%, rgba(26, 26, 46, 0.9) 100%);
            border: 1px solid rgba(0, 212, 255, 0.3);
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.4);
            position: relative;
            overflow: hidden;
        }

        .preview-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(45deg, transparent 30%, rgba(0, 212, 255, 0.15) 50%, transparent 70%);
            transform: translateX(-100%);
            animation: shimmer 4s ease-in-out infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        /* Fade in animation */
        .fade-in {
            animation: fadeIn 0.6s ease-out forwards;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Hover effects for sections */
        .section-hover:hover {
            transform: translateY(-3px);
            box-shadow: 
                0 15px 50px rgba(0, 0, 0, 0.5),
                0 0 60px rgba(0, 212, 255, 0.2);
        }

        /* Typing animation for placeholders */
        @keyframes typing {
            from { width: 0; }
            to { width: 100%; }
        }

        .typing-animation {
            overflow: hidden;
            white-space: nowrap;
            animation: typing 3s steps(50, end), blink-caret 1s step-end infinite;
        }

        @keyframes blink-caret {
            from, to { border-color: transparent; }
            50% { border-color: var(--neon-blue); }
        }

        /* Grid pattern overlay */
        .grid-overlay {
            background-image: 
                linear-gradient(rgba(0, 212, 255, 0.05) 1px, transparent 1px),
                linear-gradient(90deg, rgba(0, 212, 255, 0.05) 1px, transparent 1px);
            background-size: 60px 60px;
        }

        /* Loading wave effect */
        .wave-loading {
            background: linear-gradient(90deg, rgba(0, 212, 255, 0.15) 25%, rgba(0, 212, 255, 0.4) 50%, rgba(0, 212, 255, 0.15) 75%);
            background-size: 200% 100%;
            animation: wave 2s ease-in-out infinite;
        }

        @keyframes wave {
            0% { background-position: -200% 0; }
            100% { background-position: 200% 0; }
        }

        /* Animated Header Styles */
        .animated-header {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 50;
            background: linear-gradient(135deg, rgba(15, 15, 35, 0.95) 0%, rgba(26, 26, 46, 0.9) 100%);
            backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(0, 212, 255, 0.3);
            transform: translateY(-100%);
            animation: slideDown 1s ease-out 0.5s forwards;
        }

        .animated-header::before {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, transparent, var(--neon-blue), var(--neon-purple), transparent);
            animation: headerGlow 3s ease-in-out infinite;
        }

        @keyframes slideDown {
            to { transform: translateY(0); }
        }

        @keyframes headerGlow {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        .header-title {
            background: linear-gradient(45deg, #00d4ff, #a855f7, #ec4899);
            background-size: 200% 200%;
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: headerTextFlow 4s ease-in-out infinite;
        }

        @keyframes headerTextFlow {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }

        .header-icon {
            animation: headerIconFloat 3s ease-in-out infinite;
        }

        @keyframes headerIconFloat {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-5px) rotate(10deg); }
        }

        /* Adjust body padding for fixed header */
        .main-content {
            margin-top: 80px;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center p-6 grid-overlay">
    <!-- Enhanced Animated Background -->
    <div class="bg-waves">
        <div class="wave"></div>
        <div class="wave"></div>
        <div class="wave"></div>
    </div>
    <div class="particles" id="particles"></div>

    <!-- Animated Header -->
    <header class="animated-header">
        <div class="flex items-center justify-between px-6 py-4">
            <div class="flex items-center gap-3">
                <div class="header-icon">
                    <svg class="w-8 h-8 text-cyan-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z"/>
                    </svg>
                </div>
                <h1 class="header-title text-xl font-bold">AI Photography Assistant</h1>
            </div>
            <div class="flex items-center gap-2 text-xs text-slate-400">
                <div class="w-2 h-2 bg-green-400 rounded-full animate-pulse"></div>
                <span>AI Ready</span>
            </div>
        </div>
    </header>

    <main class="main-content w-full max-w-6xl flex-grow flex items-center justify-center relative z-10">
        <div class="tech-card section-hover rounded-3xl overflow-hidden grid grid-cols-1 md:grid-cols-2 gap-6 p-6 fade-in">
            <!-- Left Section -->
            <section class="p-6 flex flex-col gap-6">
                <div class="text-center">
                    <h1 class="super-title text-4xl font-black mb-2" data-text="AI Photography Assistant">
                        AI Photography Assistant
                    </h1>
                    <p class="text-sm text-slate-300">Upload a photo, pick a style, and receive professional editing suggestions with cutting-edge AI analysis.</p>
                </div>

                <!-- File Drop Zone -->
                <div id="drop" tabindex="0"
                     class="file-drop glow-border mt-3 p-6 rounded-xl flex flex-col items-center gap-4 focus:outline-none transition-all duration-300">
                    <div class="relative">
                        <svg class="w-14 h-14 text-cyan-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden>
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M3 15a4 4 0 004 4h10a4 4 0 100-8 5 5 0 10-9.9.9" />
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M7 10l5-5 5 5" />
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 5v12" />
                        </svg>
                        <div class="absolute -top-1 -right-1 w-4 h-4 bg-gradient-to-r from-green-400 to-blue-400 rounded-full animate-pulse"></div>
                    </div>
                    <div class="text-center">
                        <p id="drop-text" class="text-sm font-medium text-slate-200">Drag & drop your image here or <button id="browse-btn" class="text-cyan-400 underline hover:text-cyan-300 transition-colors">browse</button></p>
                        <p class="text-xs text-slate-400 mt-1">Max: 6 MB (auto-optimized). JPG/PNG/WEBP recommended.</p>
                    </div>
                    <input id="file-input" type="file" accept="image/*" class="sr-only" />
                </div>

                <!-- Preview Card -->
                <div id="preview-card" class="hidden preview-card rounded-lg overflow-hidden border border-cyan-400/20 fade-in">
                    <img id="preview-img" alt="preview" class="w-full object-contain max-h-72 bg-black/20" />
                    <div class="flex items-center justify-between p-4 bg-gradient-to-r from-slate-900/50 to-slate-800/50">
                        <div>
                            <div id="img-name" class="text-sm font-medium text-slate-200"></div>
                            <div id="img-meta" class="text-xs text-slate-400"></div>
                        </div>
                        <div class="flex gap-2">
                            <button id="remove-btn" class="px-3 py-1 rounded-lg bg-red-500/20 text-red-400 text-sm hover:bg-red-500/30 transition-all border border-red-500/30">Remove</button>
                            <button id="recompress-btn" class="px-3 py-1 rounded-lg bg-yellow-500/20 text-yellow-400 text-sm hover:bg-yellow-500/30 transition-all border border-yellow-500/30">Optimize</button>
                        </div>
                    </div>
                </div>

                <!-- Option 1: Dropdown Selection -->
                <div class="tech-card rounded-xl border border-cyan-400/20 p-4 section-hover">
                    <label class="block text-sm font-semibold text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-purple-400 mb-3">Option 1: Quick Style Selection</label>
                    
                    <label class="block text-sm font-medium text-slate-300 mb-2">Photography Genre</label>
                    <select id="event-type" class="tech-input w-full p-3 rounded-lg mb-4">
                        <option value="" disabled selected>Select photography genre</option>
                        <optgroup label="People & Events">
                            <option>Wedding</option>
                            <option>Portrait (Outdoor)</option>
                            <option>Portrait (Indoor)</option>
                            <option>Street Photography</option>
                            <option>Family</option>
                            <option>Sports</option>
                            <option>Maternity</option>
                            <option>Engagements</option>
                            <option>Boudoir</option>
                            <option>Headshots</option>
                            <option>Studio Portraits</option>
                            <option>Concert</option>
                            <option>Conference</option>
                            <option>Fashion (Studio)</option>
                            <option>Corporate Event</option>
                        </optgroup>
                        <optgroup label="Nature & Places">
                            <option>Landscape</option>
                            <option>Nature</option>
                            <option>Wildlife</option>
                            <option>Urban Exploration</option>
                            <option>Architectural</option>
                            <option>Real Estate</option>
                            <option>Macro</option>
                            <option>Astrophotography</option>
                            <option>Aerial</option>
                            <option>Adventure</option>
                        </optgroup>
                        <optgroup label="Products & Art">
                            <option>Product</option>
                            <option>Food</option>
                            <option>Still Life</option>
                            <option>Fine Art Nudes</option>
                            <option>Art Exhibitions</option>
                            <option>Hospitality</option>
                            <option>Automotive</option>
                            <option>Artwork Reproduction</option>
                        </optgroup>
                    </select>

                    <label class="block text-sm font-medium text-slate-300 mb-2">Editing Style</label>
                    <select id="style-type" class="tech-input w-full p-3 rounded-lg mb-4">
                        <option value="" disabled selected>Select a style</option>
                        <option>Cinematic</option>
                        <option>Moody</option>
                        <option>Professional</option>
                        <option>Vibrant & Colorful</option>
                        <option>Soft & Dreamy</option>
                        <option>Black & White</option>
                        <option>High Key</option>
                        <option>Low Key</option>
                        <option>Vintage</option>
                        <option>Minimalist</option>
                    </select>

                    <button id="analyze-btn" class="tech-button w-full flex items-center justify-center gap-3 px-4 py-3 rounded-xl text-white font-semibold shadow-lg">
                        <span id="analyze-spinner" class="spinner hidden" aria-hidden></span>
                        <span id="analyze-text">Get AI Suggestions</span>
                    </button>
                </div>
                
                <!-- Option 2: AI Chat -->
                <div class="tech-card rounded-xl border border-purple-400/20 p-4 section-hover">
                    <label class="block text-sm font-semibold text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-pink-400 mb-3">Option 2: AI Assistant Chat</label>
                    <textarea id="user-prompt" rows="3" class="tech-input w-full p-3 rounded-lg resize-none" placeholder="e.g., 'This is a moody forest scene. Make it look cinematic with enhanced atmosphere...'"></textarea>
                    <button id="analyze-chat-btn" class="tech-button w-full mt-3 flex items-center justify-center gap-3 px-4 py-3 rounded-xl text-white font-semibold shadow-lg">
                        <span id="analyze-chat-spinner" class="spinner hidden" aria-hidden></span>
                        <span id="analyze-chat-text">Ask AI Assistant</span>
                    </button>
                </div>

                <button id="start-over" class="w-full px-4 py-3 rounded-xl bg-gradient-to-r from-slate-700 to-slate-600 text-slate-200 font-semibold transition-all hover:from-slate-600 hover:to-slate-500 hover:shadow-lg hover:-translate-y-0.5 border border-slate-500/30">
                    ↻ Start Over
                </button>

                <div id="status" class="text-sm mt-2 text-center rounded-lg p-3 hidden"></div>
            </section>

            <!-- Right Section -->
            <section class="p-6 flex flex-col gap-4">
                <div class="flex items-start justify-between gap-4">
                    <div>
                        <h2 class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-purple-400">AI Analysis Results</h2>
                        <p class="text-sm text-slate-400">Professional editing recommendations appear here</p>
                    </div>
                    <div class="w-3 h-3 bg-gradient-to-r from-green-400 to-blue-400 rounded-full animate-pulse"></div>
                </div>

                <div id="suggestions" class="tech-card flex-1 min-h-[400px] rounded-xl border border-cyan-400/20 p-6 bg-gradient-to-br from-slate-900/50 to-slate-800/50 overflow-auto">
                    <div id="initial" class="text-center text-slate-400 mt-20">
                        <div class="text-6xl mb-4">🤖</div>
                        <p class="typing-animation border-r-2 border-cyan-400">Your professional AI suggestions will appear here...</p>
                    </div>
                </div>
            </section>
        </div>
    </main>

    <footer class="mt-8 text-center text-slate-500 text-sm relative z-10">
        <p class="flex items-center justify-center gap-2">
            <span class="w-2 h-2 bg-gradient-to-r from-cyan-400 to-purple-400 rounded-full animate-pulse"></span>
            © 2025 AI Photography Assistant. All rights reserved.
            <span class="w-2 h-2 bg-gradient-to-r from-purple-400 to-pink-400 rounded-full animate-pulse"></span>
        </p>
    </footer>

    <script>
        // Create enhanced animated background particles
        function createParticles() {
            const particlesContainer = document.getElementById('particles');
            const particleCount = 80;
            
            for (let i = 0; i < particleCount; i++) {
                const particle = document.createElement('div');
                
                // Create different particle types
                if (i % 4 === 0) {
                    particle.className = 'particle large';
                } else if (i % 3 === 0) {
                    particle.className = 'particle small';
                } else {
                    particle.className = 'particle';
                }
                
                particle.style.left = Math.random() * 100 + '%';
                particle.style.top = Math.random() * 100 + '%';
                particle.style.animationDelay = Math.random() * 8 + 's';
                particle.style.animationDuration = (Math.random() * 6 + 6) + 's';
                particlesContainer.appendChild(particle);
            }
        }
        
        // Initialize particles
        createParticles();

        // ---------- CONFIG ----------
        const GENERATE_ENDPOINT = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=';
        const API_KEY = "AIzaSyBdjtZ6z720PjiuhqLtXiJXm14SL1afXbU";
        const MAX_UPLOAD_BYTES = 6 * 1024 * 1024;

        // ---------- UI REFS ----------
        const drop = document.getElementById('drop');
        const fileInput = document.getElementById('file-input');
        const browseBtn = document.getElementById('browse-btn');
        const previewCard = document.getElementById('preview-card');
        const previewImg = document.getElementById('preview-img');
        const imgName = document.getElementById('img-name');
        const imgMeta = document.getElementById('img-meta');
        const removeBtn = document.getElementById('remove-btn');
        const recompressBtn = document.getElementById('recompress-btn');
        const eventType = document.getElementById('event-type');
        const styleType = document.getElementById('style-type');
        const userPrompt = document.getElementById('user-prompt');
        const analyzeBtn = document.getElementById('analyze-btn');
        const analyzeChatBtn = document.getElementById('analyze-chat-btn');
        const analyzeText = document.getElementById('analyze-text');
        const analyzeChatText = document.getElementById('analyze-chat-text');
        const analyzeSpinner = document.getElementById('analyze-spinner');
        const analyzeChatSpinner = document.getElementById('analyze-chat-spinner');
        const suggestionsDiv = document.getElementById('suggestions');
        const statusDiv = document.getElementById('status');
        const startOverBtn = document.getElementById('start-over');
        const initialDiv = document.getElementById('initial');

        // ---------- STATE ----------
        let selectedFile = null;
        let uploadFile = null;
        let isProcessing = false;

        // ---------- UTIL: show status ----------
        function showStatus(msg, isError = false, timeout = 5000) {
            statusDiv.textContent = msg;
            statusDiv.classList.remove('hidden', 'status-success', 'status-error');
            statusDiv.classList.add(isError ? 'status-error' : 'status-success');
            statusDiv.classList.add('fade-in');
            if (timeout) setTimeout(() => statusDiv.classList.add('hidden'), timeout);
        }

        // ---------- UTIL: bytes to human ----------
        function humanBytes(bytes) {
            if (bytes < 1024) return bytes + ' B';
            if (bytes < 1024 * 1024) return (bytes / 1024).toFixed(1) + ' KB';
            return (bytes / (1024 * 1024)).toFixed(2) + ' MB';
        }

        // ---------- IMAGE: client-side resize/compress ----------
        async function fileToImage(file) {
            return new Promise((res, rej) => {
                const img = new Image();
                img.onload = () => res(img);
                img.onerror = rej;
                img.src = URL.createObjectURL(file);
            });
        }

        async function resizeImageFile(file, targetBytes = MAX_UPLOAD_BYTES) {
            const img = await fileToImage(file);
            const MAX_DIM = 2400;
            let [w, h] = [img.naturalWidth, img.naturalHeight];
            let scale = Math.min(1, MAX_DIM / Math.max(w, h));
            let canvas = document.createElement('canvas');
            canvas.width = Math.round(w * scale);
            canvas.height = Math.round(h * scale);
            let ctx = canvas.getContext('2d');
            ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
            let quality = 0.92;
            let blob = await new Promise(r => canvas.toBlob(r, 'image/jpeg', quality));
            let attempts = 0;
            while (blob.size > targetBytes && quality > 0.45 && attempts < 8) {
                quality -= 0.12;
                blob = await new Promise(r => canvas.toBlob(r, 'image/jpeg', quality));
                attempts++;
            }
            while (blob.size > targetBytes && (canvas.width > 800 || canvas.height > 800)) {
                canvas.width = Math.round(canvas.width * 0.85);
                canvas.height = Math.round(canvas.height * 0.85);
                ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                blob = await new Promise(r => canvas.toBlob(r, 'image/jpeg', quality));
            }
            const newFile = new File([blob], (file.name.replace(/\.[^/.]+$/, "") + ".jpg"), { type: 'image/jpeg', lastModified: Date.now() });
            return newFile;
        }

        // ---------- UTIL: convert blob to base64 ----------
        async function blobToBase64(blob) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onloadend = () => {
                    const base64data = reader.result.split(',')[1];
                    resolve(base64data);
                };
                reader.onerror = reject;
                reader.readAsDataURL(blob);
            });
        }

        // ---------- UI: set preview ----------
        function setPreview(file) {
            selectedFile = file;
            uploadFile = file;
            previewImg.src = URL.createObjectURL(file);
            previewImg.onload = () => URL.revokeObjectURL(previewImg.src);
            previewCard.classList.remove('hidden');
            imgName.textContent = file.name;
            imgMeta.textContent = `${file.type} • ${humanBytes(file.size)} • ${new Date(file.lastModified).toLocaleString()}`;
            suggestionsDiv.innerHTML = '<div class="text-slate-400">Your professional suggestions will appear here.</div>';
        }

        // ---------- HANDLERS ----------
        browseBtn.addEventListener('click', (e) => {
            e.preventDefault();
            e.stopPropagation();
            fileInput.click();
        });
        
        fileInput.addEventListener('change', (e) => {
            if (e.target.files?.[0]) handleFile(e.target.files[0]);
        });

        // Make drop area clickable
        drop.addEventListener('click', (e) => {
            if (e.target === browseBtn) return; // Don't interfere with browse button
            fileInput.click();
        });

        ['dragenter', 'dragover'].forEach(ev => {
            drop.addEventListener(ev, (e) => { e.preventDefault(); e.stopPropagation(); drop.classList.add('dragover'); });
        });
        ['dragleave', 'drop'].forEach(ev => {
            drop.addEventListener(ev, (e) => { e.preventDefault(); e.stopPropagation(); drop.classList.remove('dragover'); });
        });
        drop.addEventListener('drop', (e) => {
            const f = e.dataTransfer?.files?.[0];
            if (f) handleFile(f);
        });

        async function handleFile(file) {
            statusDiv.classList.add('hidden');
            if (!file.type.startsWith('image/')) { showStatus('Please upload an image file.', true); return; }
            if (file.size > MAX_UPLOAD_BYTES) {
                showStatus('Large image detected — compressing locally before upload...', false, 4000);
                try {
                    const resized = await resizeImageFile(file, MAX_UPLOAD_BYTES);
                    setPreview(resized);
                } catch (err) {
                    console.error(err);
                    showStatus('Image processing failed. Try a different file.', true);
                }
            } else {
                setPreview(file);
            }
        }

        removeBtn.addEventListener('click', () => {
            selectedFile = null; uploadFile = null;
            previewCard.classList.add('hidden');
            fileInput.value = '';
            suggestionsDiv.innerHTML = '<div class="text-slate-400">Your professional suggestions will appear here.</div>';
        });

        recompressBtn.addEventListener('click', async () => {
            if (!selectedFile) return;
            showStatus('Recompressing image...', false, 3000);
            try {
                const recompressed = await resizeImageFile(selectedFile, MAX_UPLOAD_BYTES);
                setPreview(recompressed);
                showStatus('Image recompressed.', false, 2000);
            } catch (e) { showStatus('Recompress failed.', true); }
        });

        // ---------- ANALYZE: Function to check if prompt is photography-related ----------
        async function isPromptRelevant(prompt) {
            const relevancePrompt = `Classify the following user prompt as either "Photography-related" or "Not Photography-related". Be strict with your classification.\n\nPrompt: "${prompt}"`;
            const payload = {
                contents: [{ parts: [{ text: relevancePrompt }] }]
            };
            try {
                const response = await fetch(GENERATE_ENDPOINT + API_KEY, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                const text = result?.candidates?.[0]?.content?.parts?.[0]?.text;
                return text && text.trim().toLowerCase() === 'photography-related';
            } catch (error) {
                console.error("Relevance check failed:", error);
                return false;
            }
        }

        // ---------- ANALYZE: Function to process AI requests ----------
        async function processAIRequest(finalPrompt) {
            if (!uploadFile) {
                showStatus('Please upload an image first.', true);
                return;
            }

            suggestionsDiv.innerHTML = '<div class="text-center text-slate-400"><span class="spinner mr-2"></span>Analyzing image — this may take a few seconds.</div>';
            statusDiv.classList.add('hidden');

            try {
                const base64Image = await blobToBase64(uploadFile);

                const descriptionPayload = {
                    contents: [{
                        parts: [{ text: "Please describe this photo in one short, descriptive paragraph." }, { inlineData: { mimeType: uploadFile.type, data: base64Image } }]
                    }],
                };

                const descResp = await fetch(GENERATE_ENDPOINT + API_KEY, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(descriptionPayload) });
                const descResult = await descResp.json();
                const description = descResult?.candidates?.[0]?.content?.parts?.[0]?.text || 'No description available.';

                const suggestionsPayload = {
                    contents: [{
                        parts: [{ text: finalPrompt }, { inlineData: { mimeType: uploadFile.type, data: base64Image } }]
                    }],
                };
                const sugResp = await fetch(GENERATE_ENDPOINT + API_KEY, { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(suggestionsPayload) });
                const sugResult = await sugResp.json();
                const suggestions = sugResult?.candidates?.[0]?.content?.parts?.[0]?.text;

                if (!suggestions) {
                    throw new Error('No suggestions returned from API.');
                }

                const fullResponse = `### Photo Analysis\n\n${description}\n\n### Editing Suggestions\n\n${suggestions}`;
                suggestionsDiv.innerHTML = renderMarkdownSimple(fullResponse);
                showStatus('Suggestions generated.', false, 3000);

            } catch (err) {
                console.error(err);
                suggestionsDiv.innerHTML = '<div class="text-center text-rose-600">Failed to generate suggestions. See console for details.</div>';
                showStatus('Failed to generate suggestions. Check the console for details.', true, 7000);
            } finally {
                isProcessing = false;
                analyzeSpinner.classList.add('hidden');
                analyzeChatSpinner.classList.add('hidden');
                analyzeText.textContent = 'Get AI Suggestions';
                analyzeChatText.textContent = 'Ask AI Assistant';
            }
        }

        // ---------- ANALYZE: Separate handler for Dropdown options ----------
        async function handleDropdownAnalysis() {
            if (isProcessing) return;
            if (!eventType.value || !styleType.value) {
                showStatus('Please select a photography genre and editing style.', true);
                return;
            }

            isProcessing = true;
            analyzeSpinner.classList.remove('hidden');
            analyzeText.textContent = 'Analyzing...';
            
            const finalPrompt = `You are a professional photo editor and AI assistant. Based on the provided "${eventType.value}" photo, provide a comprehensive, highly detailed list of Lightroom and Photoshop editing suggestions with specific numerical values to make the image look "${styleType.value}".

Please organize your suggestions into these sections:

**BASIC PANEL:**
- Exposure, Contrast, Highlights, Shadows, Whites, Blacks, Texture, Clarity, Dehaze, Vibrance, Saturation

**HSL/COLOR GRADING:**
- Hue adjustments for specific colors (Red, Orange, Yellow, Green, Cyan, Blue, Purple, Magenta)
- Saturation adjustments for individual colors
- Luminance adjustments for individual colors
- Color grading in shadows, midtones, and highlights

**TONE CURVE:**
- RGB curve adjustments
- Individual color channel curves (Red, Green, Blue)

**DETAIL PANEL:**
- Sharpening (Amount, Radius, Detail, Masking)
- Noise Reduction (Luminance, Detail, Contrast)

**MASKING TECHNIQUES (if applicable):**
- Subject masking for selective adjustments
- Sky masking for dramatic effects
- Color range masking for specific color enhancement
- Luminosity masking for tone-specific edits
- Radial filters for vignetting or spotlight effects

**EFFECTS:**
- Post-crop vignetting
- Grain settings
- Split toning (if vintage style)

For each adjustment, provide the numerical value and a brief explanation. Format as bulleted lists under each section. For example:
- Exposure: +0.7 (Increases overall brightness to showcase details)
- Red Hue: +15 (Shifts reds toward orange for warmer skin tones)
- Subject Mask Exposure: +0.5 (Brightens the main subject while preserving background)`;
            
            await processAIRequest(finalPrompt);
        }

        // ---------- ANALYZE: Separate handler for Chat Assistant ----------
        async function handleChatAnalysis() {
            if (isProcessing) return;
            const prompt = userPrompt.value.trim();
            if (!prompt) {
                showStatus('Please enter your request in the text box.', true);
                return;
            }
            if (!uploadFile) {
                showStatus('Please upload an image first.', true);
                return;
            }

            isProcessing = true;
            analyzeChatSpinner.classList.remove('hidden');
            analyzeChatText.textContent = 'Checking prompt...';

            const relevant = await isPromptRelevant(prompt);
            
            if (!relevant) {
                showStatus('This assistant is for photography and photo editing only. Please enter a request related to your photo.', true);
                isProcessing = false;
                analyzeChatSpinner.classList.add('hidden');
                analyzeChatText.textContent = 'Ask AI Assistant';
                return;
            }
            
            analyzeChatText.textContent = 'Analyzing...';
            
            const finalPrompt = `You are a professional photo editor and AI assistant. The user wants to apply the following editing style: "${prompt}". Based on the image, provide a comprehensive, highly detailed list of Lightroom and Photoshop editing suggestions with specific numerical values.

Please organize your suggestions into these sections:

**BASIC PANEL:**
- Exposure, Contrast, Highlights, Shadows, Whites, Blacks, Texture, Clarity, Dehaze, Vibrance, Saturation, Temperature, Tint

**HSL/COLOR GRADING:**
- Hue adjustments for specific colors (Red, Orange, Yellow, Green, Cyan, Blue, Purple, Magenta)
- Saturation adjustments for individual colors
- Luminance adjustments for individual colors
- Color grading in shadows, midtones, and highlights

**TONE CURVE:**
- RGB curve adjustments
- Individual color channel curves (Red, Green, Blue)

**DETAIL PANEL:**
- Sharpening (Amount, Radius, Detail, Masking)
- Noise Reduction (Luminance, Detail, Contrast)

**MASKING TECHNIQUES (if applicable):**
- Subject masking for selective adjustments
- Sky masking for dramatic effects
- Color range masking for specific color enhancement
- Luminosity masking for tone-specific edits
- Radial filters for vignetting or spotlight effects

**EFFECTS:**
- Post-crop vignetting
- Grain settings
- Split toning (if vintage style)

For each adjustment, provide the numerical value and a brief explanation. Format as bulleted lists under each section. For example:
- Exposure: +0.7 (Increases overall brightness to showcase details)
- Orange Saturation: -20 (Reduces orange tones in skin for more natural look)
- Sky Mask Clarity: +40 (Enhances cloud detail and texture in sky area)`;

            await processAIRequest(finalPrompt);
        }

        // Event listeners for both buttons
        analyzeBtn.addEventListener('click', handleDropdownAnalysis);
        analyzeChatBtn.addEventListener('click', handleChatAnalysis);

        // ---------- ENHANCED MARKDOWN RENDER for comprehensive suggestions ----------
        function renderMarkdownSimple(md) {
            let s = md.replace(/^### (.+)/gm, '<h3 class="text-lg font-bold text-cyan-400 mb-3 mt-4">$1</h3>');
            s = s.replace(/^\*\*(.+?)\*\*:/gm, '<h4 class="text-base font-semibold text-purple-400 mb-2 mt-3">$1:</h4>');
            s = s.replace(/\*\*(.+?)\*\*/g, '<strong class="text-white">$1</strong>');
            s = s.replace(/^- (.+)/gm, '<li>$1</li>');
            
            // Group consecutive list items
            s = s.replace(/<li>(.+?)<\/li>/gs, '<ul><li>$1</li></ul>');
            s = s.replace(/<\/ul>\s*<ul>/g, '');
            
            // Handle paragraphs
            s = s.replace(/\n{2,}/g, '</p><p class="text-slate-300 mb-3 leading-relaxed">');
            s = '<div class="prose-custom p-1"><p class="text-slate-300 mb-3 leading-relaxed">' + s.trim() + '</p></div>';
            
            // Clean up paragraph formatting around headers
            s = s.replace(/<p[^>]*>([\s\S]+?)<\/p>/g, (match, p1) => {
                if (p1.includes('<h3') || p1.includes('<h4') || p1.includes('<ul')) {
                    return p1;
                }
                return `<p class="text-slate-300 mb-3 leading-relaxed">${p1.replace(/\n/g, '<br/>')}</p>`;
            });
            
            // Final cleanup
            s = s.replace(/<br\/><h[34]/g, '</h4><h4');
            s = s.replace(/<p[^>]*><h([34])/g, '<h$1');
            s = s.replace(/<\/h[34]><\/p>/g, '</h4>');
            
            return s;
        }

        // ---------- START OVER ----------
        startOverBtn.addEventListener('click', () => {
            selectedFile = null;
            uploadFile = null;
            fileInput.value = '';
            previewCard.classList.add('hidden');
            suggestionsDiv.innerHTML = '<div class="text-slate-400">Your professional suggestions will appear here.</div>';
            eventType.selectedIndex = 0;
            styleType.selectedIndex = 0;
            userPrompt.value = '';
            showStatus('Reset.', false, 1500);
        });

        // ---------- INIT ----------
        drop.addEventListener('keydown', (e) => { if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); fileInput.click(); } });
    </script>
</body>
</html>
