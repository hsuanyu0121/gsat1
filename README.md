
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>P!LOT 考點導航員 | 專業學習系統</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root { --navy: #1e3a8a; --slate-dark: #334155; --bg-gray: #f8fafc; }
        body { background-color: var(--bg-gray); height: 100vh; overflow: hidden; display: flex; font-family: 'Inter', sans-serif; }
        .column { height: 100vh; overflow-y: auto; padding: 24px; border-right: 1px solid #e2e8f0; }
        .navy-btn { background-color: var(--navy); color: white; transition: all 0.3s; }
        .navy-btn:hover { opacity: 0.9; transform: translateY(-1px); }
        .glass-card { background: white; border-radius: 16px; border: 1px solid #e2e8f0; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); }
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        .upload-dashed { border: 2px dashed #cbd5e1; border-radius: 12px; transition: all 0.3s; cursor: pointer; }
        .upload-dashed:hover { border-color: var(--navy); background-color: #f1f5f9; }
    </style>
</head>
<body class="flex">

    <div class="column w-1/4 bg-white">
        <div class="flex items-center justify-between mb-8">
            <h1 class="text-3xl font-black text-blue-900 italic tracking-tighter">P!LOT</h1>
            <button class="px-4 py-1.5 border border-blue-900 text-blue-900 text-xs font-bold rounded-full hover:bg-blue-900 hover:text-white transition">登入</button>
        </div>

        <div class="space-y-8">
            <div class="p-5 bg-slate-900 text-white rounded-2xl shadow-lg">
                <p class="text-[10px] font-bold opacity-60 uppercase tracking-widest mb-1">Exam Countdown</p>
                <div class="flex items-baseline gap-2">
                    <span class="text-4xl font-black">128</span>
                    <span class="text-sm font-bold">DAYS</span>
                </div>
                <p class="text-[10px] mt-2 opacity-80 italic">「現在的努力，是為了未來的從容。」</p>
            </div>

            <div>
                <h3 class="text-xs font-bold text-slate-400 uppercase tracking-widest mb-4 px-1 text-center">考古題練習進度</h3>
                <div class="p-2">
                    <canvas id="radarChart"></canvas>
                </div>
            </div>

            <nav class="space-y-1">
                <p class="text-[10px] font-bold text-slate-400 px-3 mb-2 uppercase">Subject Focus</p>
                <button class="w-full text-left p-3 hover:bg-slate-50 text-blue-900 font-bold text-sm rounded-xl transition flex items-center gap-3">
                    <span class="w-1.5 h-1.5 rounded-full bg-blue-900"></span> 數學 A / B
                </button>
                <button class="w-full text-left p-3 hover:bg-slate-50 text-blue-900 font-bold text-sm rounded-xl transition flex items-center gap-3">
                    <span class="w-1.5 h-1.5 rounded-full bg-slate-300"></span> 英文
                </button>
            </nav>
        </div>
    </div>

    <div class="column w-2/4 bg-slate-50 border-r border-slate-200">
        <div class="max-w-3xl mx-auto">
            <div class="mb-10 text-center">
                <h2 class="text-2xl font-black text-slate-800">🤖 AI 考點診斷中心</h2>
                <p class="text-sm text-slate-400 mt-2">輸入問題，AI 將依據考古題庫與網路資料進行深度解析</p>
            </div>
            
            <div class="bg-white p-8 rounded-3xl shadow-sm border border-slate-200 mb-8">
                <textarea id="aiInput" rows="7" 
                    class="w-full text-lg border-none focus:ring-0 outline-none resize-none placeholder-slate-200 text-slate-700 font-medium"
                    placeholder="請輸入題號（如：113數A-7）或題目具體描述..."></textarea>
                <div class="flex justify-between items-center mt-6 pt-6 border-t border-slate-100">
                    <div class="flex gap-2">
                        <span class="px-3 py-1 bg-slate-100 text-slate-500 text-[10px] rounded-full font-bold">全科資料庫已就緒</span>
                    </div>
                    <button id="aiBtn" class="navy-btn font-bold px-12 py-4 rounded-2xl shadow-lg shadow-blue-900/20 text-lg active:scale-95">
                        開始診斷
                    </button>
                </div>
            </div>

            <div id="aiResponse" class="hidden animate-in fade-in slide-in-from-bottom-4 duration-500">
                <div class="bg-white p-10 rounded-3xl border border-slate-200 shadow-sm mb-10">
                    <div id="aiText" class="text-slate-600 leading-relaxed text-lg whitespace-pre-wrap"></div>
                </div>
            </div>
        </div>
    </div>

    <div class="column w-1/4 bg-white">
        <div class="space-y-10">
            <section>
                <h4 class="text-sm font-black text-blue-900 mb-4 flex items-center gap-2 italic">🌐 筆記交流分享區</h4>
                <div class="space-y-2 mb-4">
                    <div class="p-3 bg-slate-50 rounded-xl text-xs font-bold text-slate-600 flex justify-between items-center hover:bg-blue-50 transition cursor-pointer">
                        <span class="truncate pr-4">113 數A 頂標筆記 (Jessica)</span>
                        <span class="text-blue-900">↓</span>
                    </div>
                </div>
                <div class="upload-dashed p-4 text-center group" onclick="document.getElementById('pubUp').click()">
                    <span class="text-[11px] font-bold text-slate-400 group-hover:text-blue-900">📤 上傳分享我的筆記</span>
                </div>
                <input type="file" id="pubUp" class="hidden">
            </section>

            <section>
                <h4 class="text-sm font-black text-blue-900 mb-4 flex items-center gap-2 italic">🔒 私藏筆記珍藏區</h4>
                <div class="p-5 bg-blue-900 rounded-2xl text-white shadow-inner shadow-black/20">
                    <p class="text-[10px] font-bold opacity-60 uppercase mb-3 tracking-widest">Personal Vault</p>
                    <div class="text-[11px] opacity-80 mb-4 italic">此區檔案僅您個人可見</div>
                    <button class="w-full py-2.5 bg-white/10 hover:bg-white/20 border border-white/20 rounded-xl text-xs font-bold transition">+ 存入私藏</button>
                </div>
            </section>

            <section>
                <h4 class="text-sm font-black text-blue-900 mb-4 flex items-center gap-2 italic">📑 學測歷屆試題</h4>
                <div class="grid grid-cols-1 gap-2">
                    <button class="w-full text-left px-4 py-3 bg-slate-50 hover:bg-blue-900 hover:text-white rounded-xl text-[11px] font-bold transition flex justify-between">
                        113 學測全科試題 <span>PDF</span>
                    </button>
                    <button class="w-full text-left px-4 py-3 bg-slate-50 hover:bg-blue-900 hover:text-white rounded-xl text-[11px] font-bold transition flex justify-between">
                        112 學測全科試題 <span>PDF</span>
                    </button>
                </div>
            </section>

            <section>
                <h4 class="text-sm font-black text-blue-900 mb-4 flex items-center gap-2 italic">📂 模擬考歷屆試題</h4>
                <div class="space-y-2">
                    <div class="glass-card p-4 flex items-center gap-3 cursor-pointer group hover:border-blue-900 transition">
                        <div class="w-8 h-8 bg-blue-50 rounded flex items-center justify-center text-blue-900 font-bold text-xs">全</div>
                        <span class="text-[11px] font-bold text-slate-600">113 全模第一次詳解</span>
                    </div>
                    <div class="glass-card p-4 flex items-center gap-3 cursor-pointer group hover:border-blue-900 transition">
                        <div class="w-8 h-8 bg-slate-100 rounded flex items-center justify-center text-slate-400 font-bold text-xs">北</div>
                        <span class="text-[11px] font-bold text-slate-600">112 北模第二次詳解</span>
                    </div>
                </div>
            </section>
        </div>
    </div>

    <script>
        // 1. 初始化考古題進度雷達圖
        const ctx = document.getElementById('radarChart');
        new Chart(ctx, {
            type: 'radar',
            data: {
                labels: ['數A', '英文', '自然', '國文', '社會'],
                datasets: [{
                    label: '完成進度 (%)',
                    data: [85, 40, 95, 60, 30],
                    fill: true,
                    backgroundColor: 'rgba(30, 58, 138, 0.15)',
                    borderColor: 'rgb(30, 58, 138)',
                    pointBackgroundColor: 'rgb(30, 58, 138)',
                    pointBorderColor: '#fff',
                    borderWidth: 2
                }]
            },
            options: {
                scales: { r: { min: 0, max: 100, ticks: { display: false }, grid: { color: '#e2e8f0' } } },
                plugins: { legend: { display: false } }
            }
        });

        // 2. AI 診斷核心功能
        document.getElementById('aiBtn').addEventListener('click', async function() {
            const inputEl = document.getElementById('aiInput');
            const resArea = document.getElementById('aiResponse');
            const resText = document.getElementById('aiText');
            const btn = this;

            const userInput = inputEl.value.trim();
            if (!userInput) return;

            btn.disabled = true;
            btn.innerHTML = `<span class="animate-pulse">📡 分析中...</span>`;
            resArea.classList.remove('hidden');
            resText.innerText = "🔍 P!LOT 正在依據資料庫與大數據進行多維度分析...";

            try {
                // 安全解碼 (Base64)
                const _p = ["QUl6YVN5RHNSeUk3QTZ", "tWFFNbFc2Xzk4SDBVS0YtazZwaFZWUThr=="];
                const _k = atob(_p.join(''));
                
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${_k}`, {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify({
                        contents: [{
                            parts: [{
                                text: `你現在是 P!LOT 專業 AI 教師學長。請針對學生提出的問題「${userInput}」：
                                1. 以淺白易懂的語氣給出解析。
                                2. 明確指出該題對應的高中複習章節。
                                3. 分析學生可能的「知識盲區」。
                                4. 給予後續複習建議。`
                            }]
                        }]
                    })
                });

                const data = await response.json();
                resText.innerText = data.candidates[0].content.parts[0].text;

            } catch (err) {
                resText.innerText = "❌ 診斷失敗：" + err.message;
            } finally {
                btn.disabled = false;
                btn.innerText = "開始診斷";
            }
        });
    </script>
</body>
</html>

