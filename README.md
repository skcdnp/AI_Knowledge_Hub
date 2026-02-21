<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Knowledge Hub</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc;
        }
        .card {
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }
        .card:hover {
            transform: translateY(-4px);
            box-shadow: 0 12px 20px -5px rgba(0, 0, 0, 0.1), 0 8px 8px -5px rgba(0, 0, 0, 0.04);
        }
        .resource-link {
            transition: all 0.2s ease;
        }
        .resource-link:hover {
            background-color: #e2e8f0;
            color: #1e40af;
        }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-7xl mx-auto">
        <!-- Header -->
        <header class="mb-12 text-center">
            <h1 class="text-4xl font-extrabold text-slate-900 mb-4 tracking-tight">AI Knowledge Hub</h1>
            <p class="text-lg text-slate-600 max-w-2xl mx-auto">A curated roadmap of core AI concepts, implementation patterns, and learning resources.</p>
        </header>

        <!-- Search and Filters -->
        <div class="flex flex-col md:flex-row gap-4 mb-8">
            <div class="relative flex-grow">
                <input type="text" id="searchInput" placeholder="Search concepts or patterns..." 
                    class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 bg-white">
            </div>
            <select id="timeFilter" class="px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-blue-500 bg-white min-w-[200px]">
                <option value="">All Durations</option>
                <option value="1 day">1 day</option>
                <option value="1–2 days">1–2 days</option>
                <option value="2–3 days">2–3 days</option>
            </select>
        </div>

        <!-- Grid -->
        <div id="hubGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Cards will be injected here -->
        </div>
    </div>

    <script>
        // Parsed Data from your CSV
        const aiData = [
            {
                concept: "Prompt Engineering",
                pattern: "System prompts + few-shot examples",
                outcome: "Build controlled AI assistant",
                time: "1 day",
                resources: [
                    { name: "OpenAI Prompting Guide", url: "https://platform.openai.com/docs/guides/prompt-engineering" },
                    { name: "YouTube: Prompt Engineering Full Course", url: "https://www.youtube.com/watch?v=dOxUroR57xs" },
                    { name: "Anthropic Prompt Guide", url: "https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering" }
                ]
            },
            {
                concept: "Structured Outputs / Function Calling",
                pattern: "JSON schema outputs + tool calling",
                outcome: "Extract structured data reliably",
                time: "1–2 days",
                resources: [
                    { name: "OpenAI Function Calling Guide", url: "https://platform.openai.com/docs/guides/function-calling" },
                    { name: "YouTube: Function Calling Explained", url: "https://www.youtube.com/watch?v=Q0y3N7d7Z0E" },
                    { name: "Blog: Structured Outputs for LLMs", url: "https://www.pinecone.io/learn/series/langchain/structured-output/" }
                ]
            },
            {
                concept: "Embeddings",
                pattern: "Convert text → vectors for similarity",
                outcome: "Semantic search engine",
                time: "1 day",
                resources: [
                    { name: "OpenAI Embeddings Guide", url: "https://platform.openai.com/docs/guides/embeddings" },
                    { name: "YouTube: Embeddings Simply Explained", url: "https://www.youtube.com/watch?v=yPfU_GzO00U" },
                    { name: "Blog: What are Embeddings?", url: "https://vickiboykis.com/2023/10/23/what-are-embeddings/" }
                ]
            },
            {
                concept: "Vector Databases & RAG",
                pattern: "Retrieve context + inject into prompt",
                outcome: "Chat with your custom data",
                time: "2–3 days",
                resources: [
                    { name: "Pinecone: What is RAG?", url: "https://www.pinecone.io/learn/retrieval-augmented-generation/" },
                    { name: "YouTube: RAG from Scratch", url: "https://www.youtube.com/watch?v=wd7UZ4F2nrk" },
                    { name: "LangChain RAG Documentation", url: "https://python.langchain.com/docs/use_cases/question_answering/" }
                ]
            },
            {
                concept: "Agents & Orchestration",
                pattern: "Loops + Reasoning (ReAct) + Memory",
                outcome: "Autonomous task execution",
                time: "2–3 days",
                resources: [
                    { name: "Blog: LLM Powered Assistants", url: "https://lilianweng.github.io/posts/2023-06-23-agent/" },
                    { name: "CrewAI Documentation", url: "https://docs.crewai.com/" },
                    { name: "YouTube: Intro to AI Agents", url: "https://www.youtube.com/watch?v=sqYpA-7lS6s" }
                ]
            },
            {
                concept: "Evaluation & Guardrails",
                pattern: "RAGAS + LLM-as-a-judge + PII filters",
                outcome: "Safe, accurate production apps",
                time: "2 days",
                resources: [
                    { name: "Guardrails AI Docs", url: "https://docs.guardrailsai.com/" },
                    { name: "YouTube: LLM Guardrails Explained", url: "https://www.youtube.com/watch?v=1aM1KYvl4Dw" },
                    { name: "Blog: Reducing Hallucinations", url: "https://www.pinecone.io/learn/hallucinations/" }
                ]
            },
            {
                concept: "Latency & Cost Optimization",
                pattern: "Model selection + caching",
                outcome: "Scalable apps",
                time: "1 day",
                resources: [
                    { name: "OpenAI Pricing Page", url: "https://platform.openai.com/pricing" },
                    { name: "Blog: LLM Cost Optimization", url: "https://www.pinecone.io/learn/llm-cost/" },
                    { name: "YouTube: Optimize LLM Apps", url: "https://www.youtube.com/watch?v=KZ9b6oFZl9g" }
                ]
            },
            {
                concept: "Vibe Coding",
                pattern: "AI-assisted app building",
                outcome: "Rapid prototypes",
                time: "1–2 days",
                resources: [
                    { name: "YouTube: Build Apps with Cursor AI", url: "https://www.youtube.com/watch?v=Zq3d8l8K1mY" },
                    { name: "Replit AI Guide", url: "https://docs.replit.com/ai" },
                    { name: "GitHub Copilot Docs", url: "https://docs.github.com/en/copilot" }
                ]
            },
            {
                concept: "AI UX Design",
                pattern: "Conversational flows + fallback",
                outcome: "Usable AI apps",
                time: "1 day",
                resources: [
                    { name: "Nielsen Norman AI UX Guidelines", url: "https://www.nngroup.com/articles/ai-ux/" },
                    { name: "YouTube: Designing AI Products", url: "https://www.youtube.com/watch?v=V9n4z1lXvZQ" },
                    { name: "Streamlit Chat App Tutorial", url: "https://docs.streamlit.io/develop/tutorials/chat-and-llms" }
                ]
            }
        ];

        const grid = document.getElementById('hubGrid');
        const searchInput = document.getElementById('searchInput');
        const timeFilter = document.getElementById('timeFilter');

        function renderCards(filterText = '', filterTime = '') {
            grid.innerHTML = '';
            
            const filtered = aiData.filter(item => {
                const matchesSearch = item.concept.toLowerCase().includes(filterText.toLowerCase()) || 
                                     item.pattern.toLowerCase().includes(filterText.toLowerCase());
                const matchesTime = filterTime === '' || item.time === filterTime;
                return matchesSearch && matchesTime;
            });

            filtered.forEach(item => {
                const card = document.createElement('div');
                card.className = 'card bg-white p-6 rounded-2xl border border-slate-200 shadow-sm flex flex-col h-full';
                
                card.innerHTML = `
                    <div class="flex justify-between items-start mb-4">
                        <span class="text-xs font-bold uppercase tracking-wider text-blue-600 bg-blue-50 px-2 py-1 rounded">
                            ${item.time} to master
                        </span>
                    </div>
                    <h3 class="text-xl font-bold text-slate-800 mb-2">${item.concept}</h3>
                    <p class="text-sm text-slate-500 font-medium mb-1">Pattern: <span class="text-slate-700 font-normal">${item.pattern}</span></p>
                    <p class="text-sm text-slate-500 font-medium mb-4">Outcome: <span class="text-slate-700 font-normal">${item.outcome}</span></p>
                    
                    <div class="mt-auto border-t border-slate-100 pt-4">
                        <p class="text-xs font-semibold text-slate-400 uppercase mb-3">Resources</p>
                        <div class="flex flex-wrap gap-2">
                            ${item.resources.map(res => `
                                <a href="${res.url}" target="_blank" class="resource-link text-xs px-3 py-2 bg-slate-100 text-slate-700 rounded-lg flex items-center gap-1">
                                    <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"></path></svg>
                                    ${res.name}
                                </a>
                            `).join('')}
                        </div>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        searchInput.addEventListener('input', () => renderCards(searchInput.value, timeFilter.value));
        timeFilter.addEventListener('change', () => renderCards(searchInput.value, timeFilter.value));

        // Initial render
        renderCards();
    </script>
</body>
</html>