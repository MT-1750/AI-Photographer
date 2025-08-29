
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>AI Photography Assistant</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <meta name="description" content="Upload a photo, choose event type, and get professional Lightroom-style suggestions from AI." />
    <style>
        :root { --glass: rgba(255,255,255,0.7); --glass-dark: rgba(17,24,39,0.5); }
        body { 
            font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; 
            background-color: #ffffff;
            background-image: url('data:image/svg+xml,%3Csvg width="40" height="40" viewBox="0 0 40 40" xmlns="http://www.w3.org/2000/svg"%3E%3Cg fill="%23e0f2fe" fill-opacity="1"%3E%3Cpath d="M0 40L40 0H20L0 20V40ZM40 40V20L20 40H40Z"/%3E%3C/g%3E%3C/svg%3E');
        }
        .file-drop { transition: box-shadow .18s ease, transform .12s ease; }
        .file-drop.dragover { box-shadow: 0 10px 30px rgba(59,130,246,.18); transform: translateY(-4px); }
        .sr-only { position:absolute; width:1px; height:1px; padding:0; margin:-1px; overflow:hidden; clip:rect(0,0,0,0); border:0; }
        .spinner { border:4px solid rgba(0,0,0,0.06); border-left-color: currentColor; border-radius:50%; width:1em; height:1em; display:inline-block; animation:spin 0.9s linear infinite; }
        @keyframes spin { to { transform: rotate(360deg); } }
        pre.prose { white-space:pre-wrap; word-break:break-word; }
        .prose-custom ul { list-style-type: disc; padding-left: 20px; }
        .prose-custom ul li { margin-bottom: 5px; }
    </style>
</head>
<body class="min-h-screen flex flex-col items-center justify-center p-6">
    <main class="w-full max-w-6xl flex-grow flex items-center justify-center">
        <div class="bg-white/90 backdrop-blur-lg rounded-3xl shadow-2xl overflow-hidden grid grid-cols-1 md:grid-cols-2 gap-6 p-6">
            <section class="p-6 flex flex-col gap-4">
                <h1 class="text-3xl font-extrabold text-slate-800">AI Photography Assistant</h1>
                <p class="text-sm text-slate-600">Upload a photo (drag & drop works), pick a style, and receive professional editing suggestions. The app resizes images client-side to reduce upload size.</p>

                <div id="drop" tabindex="0"
                     class="file-drop mt-3 p-5 border-2 border-dashed border-slate-200 rounded-xl flex flex-col items-center gap-3 bg-white focus:outline-none focus:ring-4 focus:ring-blue-200">
                    <svg class="w-12 h-12 text-sky-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden>
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.6" d="M3 15a4 4 0 004 4h10a4 4 0 100-8 5 5 0 10-9.9.9" />
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.6" d="M7 10l5-5 5 5" />
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.6" d="M12 5v12" />
                    </svg>
                    <div class="text-center">
                        <p id="drop-text" class="text-sm font-medium text-slate-700">Drag & drop your image here or <button id="browse-btn" class="text-sky-600 underline">browse</button></p>
                        <p class="text-xs text-slate-400 mt-1">Max: 6 MB (client-resized if larger). JPG/PNG/WEBP recommended.</p>
                    </div>
                    <input id="file-input" type="file" accept="image/*" class="sr-only" />
                </div>

                <div id="preview-card" class="hidden mt-2 rounded-lg overflow-hidden border border-slate-200 bg-slate-50">
                    <img id="preview-img" alt="preview" class="w-full object-contain max-h-72 bg-black/2" />
                    <div class="flex items-center justify-between p-3">
                        <div>
                            <div id="img-name" class="text-sm font-medium text-slate-700"></div>
                            <div id="img-meta" class="text-xs text-slate-500"></div>
                        </div>
                        <div class="flex gap-2">
                            <button id="remove-btn" class="px-3 py-1 rounded-lg bg-rose-100 text-rose-700 text-sm hover:brightness-95">Remove</button>
                            <button id="recompress-btn" class="px-3 py-1 rounded-lg bg-yellow-100 text-yellow-700 text-sm hover:brightness-95">Recompress</button>
                        </div>
                    </div>
                </div>

                <div class="mt-2 p-4 rounded-xl border border-slate-200 bg-slate-50">
                    <label class="block text-sm font-semibold text-slate-700 mb-2">Option 1: Choose from Menus</label>
                    <label class="block text-sm font-medium text-slate-600 mb-1">Photography Genre</label>
                    <select id="event-type" class="w-full p-3 rounded-lg bg-white text-slate-900 mb-3">
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

                    <label class="block text-sm font-medium text-slate-600 mb-1">Editing Style</label>
                    <select id="style-type" class="w-full p-3 rounded-lg bg-white text-slate-900">
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

                    <button id="analyze-btn" class="w-full mt-3 flex items-center justify-center gap-3 px-4 py-3 rounded-xl bg-sky-600 text-white font-semibold shadow hover:scale-[1.01] transition-transform">
                        <span id="analyze-spinner" class="spinner hidden" aria-hidden></span>
                        <span id="analyze-text">Get Suggestions</span>
                    </button>
                </div>
                
                <div class="mt-2 p-4 rounded-xl border border-slate-200 bg-slate-50">
                    <label class="block text-sm font-semibold text-slate-700 mb-2">Option 2: Ask the AI Assistant</label>
                    <textarea id="user-prompt" rows="3" class="w-full p-3 rounded-lg bg-white text-slate-900 border-none focus:outline-none focus:ring-2 focus:ring-blue-400" placeholder="e.g., 'This is a photo of a moody, foggy forest. Suggest editing to make it look cinematic.'"></textarea>
                    <button id="analyze-chat-btn" class="w-full mt-3 flex items-center justify-center gap-3 px-4 py-3 rounded-xl bg-sky-600 text-white font-semibold shadow hover:scale-[1.01] transition-transform">
                        <span id="analyze-chat-spinner" class="spinner hidden" aria-hidden></span>
                        <span id="analyze-chat-text">Ask AI</span>
                    </button>
                </div>

                <button id="start-over" class="w-full px-4 py-3 mt-3 rounded-xl bg-slate-100 text-slate-800 font-semibold transition-transform hover:scale-[1.01]">Start Over</button>

                <p id="note" class="text-xs text-slate-500 mt-2 text-center">Your API key is handled directly by the frontend for a fast, seamless experience.</p>
                <div id="status" class="text-sm mt-2 text-center text-rose-600 hidden"></div>
            </section>

            <section class="p-6 flex flex-col gap-4">
                <div class="flex items-start justify-between gap-4">
                    <div>
                        <h2 class="text-2xl font-bold text-slate-800">AI Suggestions</h2>
                        <p class="text-sm text-slate-500">Professional editing tips appear here.</p>
                    </div>
                </div>

                <div id="suggestions" class="flex-1 min-h-[220px] rounded-xl border border-slate-200 p-4 bg-white overflow-auto">
                    <div id="initial" class="text-center text-slate-400">Your professional suggestions will appear here.</div>
                </div>
            </section>
        </div>
    </main>

    <footer class="mt-8 text-center text-slate-600 text-sm">
        <p>© 2025 AI Photography Assistant. All rights reserved.</p>
    </footer>

    <script>
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

        // ---------- STATE ----------
        let selectedFile = null;
        let uploadFile = null;
        let isProcessing = false;

        // ---------- UTIL: show status ----------
        function showStatus(msg, isError = false, timeout = 5000) {
            statusDiv.textContent = msg;
            statusDiv.classList.remove('hidden');
            statusDiv.classList.toggle('text-rose-600', isError);
            statusDiv.classList.toggle('text-green-600', !isError);
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
        browseBtn.addEventListener('click', () => fileInput.click());
        fileInput.addEventListener('change', (e) => {
            if (e.target.files?.[0]) handleFile(e.target.files[0]);
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
                analyzeText.textContent = 'Get Suggestions';
                analyzeChatText.textContent = 'Ask AI';
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
            
            const finalPrompt = `You are a professional photo editor and AI assistant. Based on the provided "${eventType.value}" photo, provide a highly detailed list of Lightroom-style editing suggestions with specific numerical values to make the image look "${styleType.value}". Focus on all main editing sliders (Exposure, Contrast, Highlights, Shadows, Whites, Blacks, Texture, Clarity, Dehaze, Vibrance, Saturation, Temperature, Tint, Sharpening, Noise Reduction). For each adjustment, provide a brief, one-sentence explanation of its purpose. Provide the suggestions in a clear, bulleted format. For example:
- Exposure: +0.7 (Increases overall image brightness to better showcase details).
- Contrast: -10 (Reduces the difference between light and dark tones for a softer look).
- Highlights: -20 (Recovers detail in bright areas).`;
            
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
                analyzeChatText.textContent = 'Ask AI';
                return;
            }
            
            analyzeChatText.textContent = 'Analyzing...';
            
            const finalPrompt = `You are a professional photo editor and AI assistant. The user wants to apply the following editing style: "${prompt}". Based on the image, provide a highly detailed list of Lightroom-style editing suggestions with specific numerical values for a user to follow. Focus on all main editing sliders (Exposure, Contrast, Highlights, Shadows, Whites, Blacks, Texture, Clarity, Dehaze, Vibrance, Saturation, Temperature, Tint, Sharpening, Noise Reduction). For each adjustment, provide a brief, one-sentence explanation of its purpose. Provide the suggestions in a clear, bulleted format. For example:
- Exposure: +0.7 (Increases overall image brightness to better showcase details).
- Contrast: -10 (Reduces the difference between light and dark tones for a softer look).
- Highlights: -20 (Recovers detail in bright areas).`;

            await processAIRequest(finalPrompt);
        }

        // Event listeners for both buttons
        analyzeBtn.addEventListener('click', handleDropdownAnalysis);
        analyzeChatBtn.addEventListener('click', handleChatAnalysis);

        // ---------- SIMPLE "MARKDOWN" RENDER (limited) ----------
        function renderMarkdownSimple(md) {
            let s = md.replace(/^### (.+)/gm, '<h4>$1</h4>');
            s = s.replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>');
            s = s.replace(/^- (.+)/gm, '<li>$1</li>');
            s = s.replace(/<li>(.+?)<\/li>/gs, '<ul><li>$1</li></ul>');
            s = s.replace(/<\/ul>\n<ul>/g, '');
            s = s.replace(/\n{2,}/g, '</p><p>');
            s = '<div class="prose-custom text-sm text-slate-700 p-1"><p>' + s.trim() + '</p></div>';
            s = s.replace(/<p>([\s\S]+?)<\/p>/g, (match, p1) => `<p>${p1.replace(/\n/g, '<br/>')}</p>`);
            s = s.replace(/<br\/><h4>/g, '</h4><h4>');
            s = s.replace(/<p><h4>/g, '<h4>');
            s = s.replace(/<\/h4><\/p>/g, '</h4>');
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
        drop.addEventListener('keydown', (e) => { if (e.key === 'Enter' || e.key === ' ') fileInput.click(); });
    </script>
</body>
</html>
