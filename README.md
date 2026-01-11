<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>P!LOT</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Noto Sans TC', sans-serif; background-color: #f8fafc; overflow-x: hidden; }
        .sidebar { width: 300px; height: 100vh; position: fixed; left: 0; top: 0; background: white; border-right: 1px solid #e2e8f0; z-index: 50; }
        .main-content { margin-left: 300px; padding: 40px; width: calc(100% - 300px); }
        .bento-card { background: white; border-radius: 24px; border: 1px solid #e2e8f0; transition: all 0.3s ease; }
        .bento-card:hover { transform: translateY(-3px); box-shadow: 0 10px 20px rgba(0,0,0,0.05); }
        .ai-dark { background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%); }
    </style>
</head>
<body class="flex">

    <aside class="sidebar p-6 flex flex-col">
        <div class="mb-10 text-center">
            <h1 class="text-4xl font-black text-blue-600 italic tracking-tighter">P!LOT</h1>
            <p class="text-[10px] text-slate-400 font-bold tracking-widest uppercase">GSAT Mission Control</p>
        </div>

        <div class="flex-1 space-y-6">
            <div class="bg-blue-50 p-5 rounded-3xl border border-blue-100">
                <p class="text-[10px] text-blue-600 font-black mb-1 uppercase">距離 2026 學測</p>
                <div class="text-3xl font-black text-blue-900"><span id="days">--</span> <small class="text-xs">DAYS</small></div>
            </div>

            <div id="radarSection" class="relative">
                <p class="text-[10px] text-slate-400 font-black mb-3 uppercase text-center">考科完成率分析</p>
                <div id="loginWarning" class="absolute inset-0 bg-white/80 backdrop-blur-sm flex items-center justify-center z-10 rounded-xl">
                    <p class="text-[10px] text-slate-500 font-bold">請先登入解鎖分析</p>
                </div>
                <canvas id="skillRadar" width="200" height="200"></canvas>
            </div>
        </div>

        <div id="loginArea" class="pt-6 border-t mt-auto">
            </div>
    </aside>

    <main class="main-content">
        
        <section class="ai-dark p-10 rounded-[40px] text-white shadow-2xl mb-8 relative overflow-hidden">
            <div class="absolute -right-20 -top-20 w-80 h-80 bg-blue-600 rounded-full blur-[120px] opacity-20"></div>
            
            <div class="relative z-10 flex flex-col lg:flex-row gap-10">
                <div class="flex-1">
                    <h2 class="text-2xl font-black mb-6 flex items-center">
                        <span class="bg-blue-500 text-[10px] px-2 py-1 rounded-md mr-3 uppercase">AI Teacher</span>
                        考點診斷與複習計畫
                    </h2>
                    
                    <div class="relative flex items-center mb-6">
                        <input type="text" id="aiInput" placeholder="輸入題號 (如 113數A-7) 或不熟的觀念..." 
                               class="w-full bg-white/10 border border-white/20 rounded-2xl px-6 py-4 text-white focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all">
                        <button onclick="runAIDiagnose()" class="absolute right-2 bg-blue-600 hover:bg-blue-500 px-6 py-2.5 rounded-xl font-bold transition-all">分析</button>
                    </div>

                    <div id="aiResponse" class="hidden animate-in fade-in slide-in-from-bottom-4 duration-500">
                        <div class="p-6 bg-white/5 rounded-3xl border border-white/10">
                            <p id="pointText" class="text-blue-400 font-bold mb-2"></p>
                            <p id="analysisText" class="text-sm text-slate-300 leading-relaxed"></p>
                        </div>
                    </div>
                </div>

                <div id="guideCard" class="hidden w-full lg:w-72 bg-white/10 backdrop-blur-md p-6 rounded-[32px] border border-white/10">
                    <h3 class="text-xs font-black text-blue-400 mb-4 uppercase tracking-widest">客製化複習路徑</h3>
                    <ul id="guideSteps" class="space-y-4 text-xs">
                        </ul>
                </div>
            </div>
        </section>

        <div class="grid grid-cols-12 gap-6">
            <div class="col-span-8 grid grid-cols-2 gap-6">
                <div class="bento-card p-8">
                    <h3 class="font-black text-slate-800 mb-4 flex justify-between">
                        <span>📚 學測歷屆試題</span>
                        <span class="text-blue-500 text-[10px]">2026 Ver.</span>
                    </h3>
                    <div class="grid grid-cols-2 gap-2">
                        <button class="text-left p-3 bg-slate-50 rounded-xl text-xs font-bold text-slate-600 hover:bg-blue-50 transition">113 學測全科</button>
                        <button class="text-left p-3 bg-slate-50 rounded-xl text-xs font-bold text-slate-600 hover:bg-blue-50 transition">112 學測全科</button>
                    </div>
                </div>
                <div class="bento-card p-8 text-center flex flex-col justify-center items-center">
                    <h3 class="font-black text-slate-800 mb-2">🔥 連續簽到</h3>
                    <div class="text-5xl font-black text-orange-500" id="streak">0</div>
                    <button onclick="doCheckIn()" class="mt-4 bg-orange-500 text-white px-8 py-2 rounded-full font-bold text-xs shadow-lg shadow-orange-200">今日簽到</button>
                </div>
                <div class="col-span-2 bento-card p-8 bg-slate-900 text-white border-none">
                    <h3 class="font-black mb-4">💬 筆記交流與即時上傳</h3>
                    <div class="flex gap-4 mb-4">
                        <input type="text" id="noteTitle" placeholder="標題" class="flex-1 bg-white/10 border-none rounded-lg p-2 text-xs">
                        <input type="text" id="noteLink" placeholder="連結" class="flex-1 bg-white/10 border-none rounded-lg p-2 text-xs">
                        <button onclick="uploadNote()" class="bg-blue-600 px-4 py-2 rounded-lg text-xs font-bold">發布</button>
                    </div>
                    <div id="noteList" class="space-y-2 max-h-32 overflow-y-auto">
                        </div>
                </div>
            </div>
            
            <div class="col-span-4 bento-card p-8">
                <h3 class="font-black text-slate-800 mb-4">📝 模擬考試題</h3>
                <div class="space-y-3">
                    <div class="p-3 bg-green-50 rounded-xl text-xs font-bold text-green-700">114 全國第一次模考</div>
                    <div class="p-3 bg-green-50 rounded-xl text-xs font-bold text-green-700">114 北區第一次模考</div>
                </div>
            </div>
        </div>
    </main>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // 📍 插入你的 Firebase 配置
        const firebaseConfig = {
           apiKey: "AIzaSyBNmnC1QQVwBMgTgjUVLw4xXZxIrVlAV6c",
          authDomain: "gsat1-29951.firebaseapp.com",
          projectId: "gsat1-29951",
          storageBucket: "gsat1-29951.firebasestorage.app",
          messagingSenderId: "165497162624",
          appId: "1:165497162624:web:9ed2b73dc6db035cadcf65",
          measurementId: "G-89J4P5W7MP"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth();
        const db = getFirestore(app);
        const provider = new GoogleAuthProvider();

        window.login = () => signInWithPopup(auth, provider);
        window.logout = () => signOut(auth).then(() => location.reload());

        onAuthStateChanged(auth, (user) => {
            const area = document.getElementById('loginArea');
            if (user) {
                area.innerHTML = `<div class="bg-slate-50 p-4 rounded-2xl flex items-center justify-between border">
                    <span class="text-xs font-black text-slate-700 truncate mr-2">${user.displayName}</span>
                    <button onclick="logout()" class="text-[10px] text-red-500 font-black">LOGOUT</button>
                </div>`;
                document.getElementById('loginWarning').classList.add('hidden');
                initRadarChart(); // 登入後啟動雷達圖
            } else {
                area.innerHTML = `<button onclick="login()" class="w-full bg-slate-900 text-white py-4 rounded-2xl font-black text-sm shadow-xl">GOOGLE 登入</button>`;
            }
        });

        // 筆記同步
        window.uploadNote = async () => {
            if (!auth.currentUser) return alert("請先登入");
            const title = document.getElementById('noteTitle').value;
            const link = document.getElementById('noteLink').value;
            if (title && link) {
                await addDoc(collection(db, "notes"), { title, link, user: auth.currentUser.displayName, time: serverTimestamp() });
                alert("發布成功！");
            }
        };

        onSnapshot(query(collection(db, "notes"), orderBy("time", "desc")), (snap) => {
            const list = document.getElementById('noteList');
            list.innerHTML = "";
            snap.forEach(doc => {
                const d = doc.data();
                list.innerHTML += `<div class="bg-white/5 p-2 rounded flex justify-between text-[10px]">
                    <span>${d.title} <small class="opacity-50">by ${d.user}</small></span>
                    <a href="${d.link}" target="_blank" class="text-blue-400">VIEW</a>
                </div>`;
            });
        });
    </script>

    <script>
        // 雷達圖初始化
        function initRadarChart() {
            const ctx = document.getElementById('skillRadar').getContext('2d');
            
            new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: ['國文', '英文', '數A', '數B', '社會', '自然'],
                    datasets: [{
                        label: '完成率 %',
                        data: [85, 70, 45, 20, 90, 60],
                        backgroundColor: 'rgba(59, 130, 246, 0.2)',
                        borderColor: '#3b82f6',
                        borderWidth: 2,
                        pointBackgroundColor: '#3b82f6'
                    }]
                },
                options: { 
                    scales: { r: { min: 0, max: 100, ticks: { display: false } } },
                    plugins: { legend: { display: false } }
                }
            });
        }

        // AI 診斷邏輯
        async function aiDiagnose() {
    // 1. 基本檢查：確認輸入框不是空的
    const userInputField = document.getElementById('aiInput');
    const responseArea = document.getElementById('aiResponse');
    const responseText = document.getElementById('aiText');
    
    if (!userInputField.value.trim()) {
        alert("請輸入你想詢問的題號或問題！");
        return;
    }

    const userInput = userInputField.value.trim();
    responseArea.classList.remove('hidden');
    responseText.innerText = "🤖 P!LOT 正在分析考點中...";

    try {
        // 2. 📍 安全密鑰還原 (已修正語法錯誤)
        const _p = [
            "QUl6YVN5RHNSeUk3QTZ", // 這裡加上了引號和結尾的逗號
            "tWFFNbFc2Xzk4SDBVS0YtazZwaFZWUThr＝" // 這裡加上了引號
        ];
        const _k = atob(_p.join(''));
        
        // 3. 設定 API 網址與指令 (使用 1.5-flash 模型，速度最快)
        const URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${_k}`;

        const promptData = {
            contents: [{
                parts: [{
                    text: `你現在是 P!LOT 網站的 AI 教師學長。
                    請針對以下學生問題搜尋網路上的歷屆考題與解析或是由網頁中的資料庫進行學測考點分析與解答建議：
                    問題：${userInput}
                    
                    要求：
                    - 語氣親切有鼓勵性。
                    - 標註出該題所屬的高中章節。
                    - 給出 3 個複習建議。`
                }]
            }]
        };

        // 4. 發送請求
        const response = await fetch(URL, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(promptData)
        });

        if (!response.ok) throw new Error("API 連線失敗，請檢查金鑰限制。");

        const data = await response.json();
        const aiReply = data.candidates[0].content.parts[0].text;

        // 5. 渲染答案
        responseText.innerText = aiReply;

    } catch (error) {
        console.error("AI 錯誤:", error);
        responseText.innerText = "❌ 導航系統連線失敗。原因：" + error.message;
    }
}

          

        // 倒數計時
        const diff = new Date('2026-01-20') - new Date();
        document.getElementById('days').innerText = Math.max(0, Math.ceil(diff / (1000*60*60*24)));

        // 簽到
        function doCheckIn() {
            let s = parseInt(localStorage.getItem('pilot_streak') || 0) + 1;
            localStorage.setItem('pilot_streak', s);
            document.getElementById('streak').innerText = s;
        }
        document.getElementById('streak').innerText = localStorage.getItem('pilot_streak') || 0;
    </script>
</body>
</html>
