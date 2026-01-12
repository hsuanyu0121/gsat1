<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>P!LOT | AI 視覺導航系統</title>
    <script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>
    // 3. Firebase 邏輯設定 (放在 script 標籤的最上方)
const firebaseConfig = {
    apiKey: "AIzaSyBNmnC1QQVwBMgTgjUVLw4xXZxIrVlAV6c",
  authDomain: "gsat1-29951.firebaseapp.com",
  projectId: "gsat1-29951",
  storageBucket: "gsat1-29951.firebasestorage.app",
  messagingSenderId: "165497162624",
  appId: "1:165497162624:web:9ed2b73dc6db035cadcf65",
  measurementId: "G-89J4P5W7MP"
};

// 初始化並執行
if (!firebase.apps.length) firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const provider = new firebase.auth.GoogleAuthProvider();

function signInWithGoogle() {
    auth.signInWithPopup(provider).catch((error) => alert("登入失敗: " + error.message));
}

function signOut() {
    auth.signOut().then(() => alert("已登出"));
}

    // 監聽登入狀態並自動改版面
    auth.onAuthStateChanged((user) => {
    const loginGroup = document.getElementById('loginGroup');
    const userInfo = document.getElementById('userInfo');
    if (user) {
        loginGroup.classList.add('hidden');
        userInfo.classList.remove('hidden');
        document.getElementById('userName').innerText = user.displayName;
        document.getElementById('userAvatar').src = user.photoURL;
    } else {
        loginGroup.classList.remove('hidden');
        userInfo.classList.add('hidden');
        }
    });
    </script>
    <script>window.MathJax = { tex: { inlineMath: [['$', '$']] } };</script>
    <script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root { --navy: #1e3a8a; --bg-gray: #f8fafc; }
        body { background-color: var(--bg-gray); height: 100vh; overflow: hidden; display: flex; font-family: 'Inter', sans-serif; }
        .column { height: 100vh; overflow-y: auto; padding: 24px; border-right: 1px solid #e2e8f0; }
        .navy-btn { background-color: var(--navy); color: white; transition: 0.3s; }
        .navy-btn:disabled { background-color: #94a3b8; cursor: not-allowed; }
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        .image-preview-container { display: none; position: relative; width: fit-content; margin: 0 auto 15px auto; }
        .image-preview { max-height: 180px; border-radius: 16px; border: 3px solid #e2e8f0; }
        .remove-img { position: absolute; top: -10px; right: -10px; background: #ef4444; color: white; border-radius: 50%; width: 24px; height: 24px; font-size: 14px; font-weight: bold; cursor: pointer; display: flex; align-items: center; justify-content: center; border: 2px solid white; }
    </style>
</head>
<body class="flex">

    <div class="column w-1/4 bg-white shadow-xl z-20">
        <div class="flex flex-col h-full">
            <h1 class="text-3xl font-black text-blue-900 italic mb-8">P!LOT</h1>
            <div class="space-y-6">
                <div class="p-6 bg-slate-900 text-white rounded-2xl shadow-lg">
                    <p class="text-[10px] font-bold opacity-50 mb-1">EXAM COUNTDOWN</p>
                    <div class="text-4xl font-black">128 <span class="text-sm">Days</span></div>
                </div>
                <div class="p-4 bg-slate-50 rounded-2xl border border-slate-100">
                    <h3 class="text-[10px] font-bold text-slate-400 uppercase text-center mb-4 italic tracking-widest">考古題進度分析</h3>
                    <canvas id="radarChart"></canvas>
                </div>
            </div>
        </div>
    </div>

    <div class="column w-2/4 bg-slate-50 border-r border-slate-200">
        <div class="max-w-3xl mx-auto">
            <div class="text-center mb-8 pt-4">
                <h2 class="text-2xl font-black text-slate-800 italic">🤖 AI 視覺診斷員</h2>
                <p class="text-xs text-slate-400 mt-2 font-medium italic">支援照片上傳、相機直拍與考點連動</p>
            </div>
            
            <div class="bg-white p-6 rounded-[2rem] shadow-sm border border-slate-200 mb-6 transition-all">
                <div id="previewContainer" class="image-preview-container">
                    <div class="remove-img" onclick="clearImage()">×</div>
                    <img id="imgPreview" class="image-preview" src="" alt="題目預覽">
                </div>
                
                <textarea id="aiInput" rows="4" 
                    class="w-full text-lg border-none focus:ring-0 outline-none resize-none placeholder-slate-200 text-slate-700 font-medium"
                    placeholder="輸入文字或使用下方按鈕拍攝/上傳題目..."></textarea>
                
                <div class="flex justify-between items-center mt-4 pt-4 border-t border-slate-100">
                    <div class="flex gap-4">
                        <button onclick="document.getElementById('fileInput').click()" class="text-navy hover:scale-110 transition flex items-center gap-2">
                            <span class="text-2xl">📷</span> 
                            <span class="text-xs font-black uppercase tracking-widest">拍攝/上傳題目</span>
                        </button>
                        <input type="file" id="fileInput" class="hidden" accept="image/*" onchange="handleImage(this)">
                    </div>
                    <button id="aiBtn" class="navy-btn font-bold px-12 py-3 rounded-2xl shadow-lg active:scale-95">診斷題目</button>
                </div>
            </div>

            <div id="aiResponse" class="hidden animate-in fade-in slide-in-from-bottom-4 duration-500 mb-10">
                <div class="bg-white p-10 rounded-[2rem] border border-slate-200 shadow-sm relative group">
                    <div id="aiText" class="text-slate-600 leading-relaxed text-lg whitespace-pre-wrap"></div>
                    <div class="mt-8 pt-4 border-t border-slate-100 flex justify-end">
                        <button onclick="saveToHistory()" class="navy-btn text-xs font-bold px-6 py-2 rounded-xl">📌 存入右側錯題本</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="column w-1/4 bg-white border-r-0">
        <div class="space-y-8">
            <section>
                <h4 class="text-sm font-black text-blue-900 mb-4 italic">📖 數位錯題本</h4>
                <div id="historyList" class="space-y-3">
                    <div class="text-[11px] text-slate-300 p-8 text-center border-2 border-dashed border-slate-50 rounded-2xl italic">尚無歷史診斷</div>
                </div>
                <button onclick="analyzeAllHistory()" class="w-full mt-4 py-2 bg-slate-50 text-slate-500 rounded-xl text-[10px] font-bold hover:bg-navy hover:text-white transition">⚡ 診斷近期易錯觀念</button>
            </section>
            
            <section class="p-6 bg-blue-900 rounded-[2rem] text-white shadow-xl">
                <p class="text-[9px] font-bold opacity-60 mb-2 uppercase tracking-widest">Secure Vault</p>
                <button class="w-full py-2.5 bg-white/10 rounded-xl text-[10px] font-bold border border-white/20 transition hover:bg-white/20">🔒 進入私藏空間</button>
            </section>

            <section>
                <h4 class="text-sm font-black text-blue-900 mb-4 italic">📑 資源中心</h4>
                <div class="space-y-2">
                    <div class="p-3 bg-slate-50 rounded-xl text-[10px] font-bold flex justify-between cursor-pointer hover:bg-slate-100">113 學測全科試題 <span class="text-navy">PDF</span></div>
                    <div class="p-3 bg-slate-50 rounded-xl text-[10px] font-bold flex justify-between cursor-pointer hover:bg-slate-100">模考歷屆詳解包 <span class="text-navy">ZIP</span></div>
                </div>
            </section>
        </div>
    </div>

    <script>
        let selectedBase64 = null;

        // 1. 處理圖片：預覽與轉換
        function handleImage(input) {
            const file = input.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    document.getElementById('imgPreview').src = e.target.result;
                    document.getElementById('previewContainer').style.display = 'block';
                    selectedBase64 = e.target.result.split(',')[1];
                }
                reader.readAsDataURL(file);
            }
        }

        function clearImage() {
            document.getElementById('previewContainer').style.display = 'none';
            document.getElementById('fileInput').value = "";
            selectedBase64 = null;
        }

        // 2. AI 診斷引擎 (含模糊檢測邏輯)
        document.getElementById('aiBtn').addEventListener('click', async function() {
            const textInput = document.getElementById('aiInput').value.trim();
            const resArea = document.getElementById('aiResponse');
            const resText = document.getElementById('aiText');
            
            if (!textInput && !selectedBase64) return alert("請輸入文字或拍攝題目！");

            this.disabled = true;
            this.innerHTML = `<span class="animate-pulse">正在解析...</span>`;
            resArea.classList.remove('hidden');
            resText.innerText = "P!LOT 正在讀取內容並診斷考點...";

            try {
                const _p = ["QUl6YVN5RHNSeUk3QTZ", "tWFFNbFc2Xzk4SDBVS0YtazZwaFZWUThr=="];
                const _k = atob((_p[0] + _p[1]).replace(/[\s＝]/g, "").replace(/＝/g, "="));
                
                const parts = [{ text: `你現在是 P!LOT AI 學長。
                1. 如果圖片過於模糊或無法看清文字，請回覆：【ERROR_BLUR】
                2. 若能看清，請針對題目提供：解析、複習章節、盲區分析。
                3. 數學公式務必用 $ 包圍。` }];
                
                if (textInput) parts.push({ text: `文字：${textInput}` });
                if (selectedBase64) parts.push({ inline_data: { mime_type: "image/jpeg", data: selectedBase64 } });

                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${_k}`, {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify({ contents: [{ parts: parts }] })
                });

                const data = await response.json();
                const result = data.candidates[0].content.parts[0].text;

                if (result.includes("【ERROR_BLUR】")) {
                    resText.innerText = "❌ 圖片無法辨識：照片可能過於模糊或光線不足，請重新拍攝清楚的題目。";
                    resText.classList.add("text-red-500", "font-bold");
                } else {
                    resText.classList.remove("text-red-500", "font-bold");
                    resText.innerText = result;
                    if (window.MathJax) MathJax.typesetPromise();
                }
            } catch (err) {
                resText.innerText = "❌ 診斷異常：" + err.message;
            } finally {
                this.disabled = false;
                this.innerText = "診斷題目";
            }
        });

        // 3. 數位錯題本 (LocalStorage)
        function saveToHistory() {
            const text = document.getElementById('aiInput').value || "視覺診斷紀錄";
            const history = JSON.parse(localStorage.getItem('pilot_history') || '[]');
            history.unshift({ date: new Date().toLocaleDateString(), text: text });
            localStorage.setItem('pilot_history', JSON.stringify(history.slice(0, 8)));
            renderHistory();
        }

        function renderHistory() {
            const history = JSON.parse(localStorage.getItem('pilot_history') || '[]');
            const list = document.getElementById('historyList');
            if (history.length === 0) return;
            list.innerHTML = history.map(item => `
                <div class="p-3 bg-slate-50 rounded-xl border border-transparent hover:border-navy transition cursor-pointer">
                    <p class="text-[10px] font-black text-slate-700 truncate">${item.text}</p>
                    <p class="text-[8px] text-slate-400 mt-1">${item.date}</p>
                </div>
            `).join('');
        }
        
        // 4. 雷達圖
        new Chart(document.getElementById('radarChart'), {
            type: 'radar',
            data: { labels: ['數A', '英文', '自然', '國文', '社會'], datasets: [{ data: [85, 45, 90, 65, 30], backgroundColor: 'rgba(30, 58, 138, 0.1)', borderColor: 'rgb(30, 58, 138)', borderWidth: 2 }] },
            options: { scales: { r: { min: 0, max: 100, ticks: { display: false } } }, plugins: { legend: { display: false } } }
        });

        renderHistory();
    </script>
</body>
</html>

