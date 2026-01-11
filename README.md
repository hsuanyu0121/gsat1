<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>P!LOT | 你的 AI 學習夥伴</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Noto Sans TC', sans-serif; }
        /* 模仿 NotebookLM 的深色背景和卡片風格 */
        .bg-notebook { background-color: #f1f3f4; } /* 淺灰色背景 */
        .sidebar { background-color: #ffffff; border-right: 1px solid #e0e0e0; }
        .content-card { background-color: #ffffff; border-radius: 12px; box-shadow: 0 1px 3px rgba(0,0,0,0.08); }
        .notebook-btn { background-color: #1a73e8; color: white; padding: 10px 15px; border-radius: 8px; font-weight: 500; transition: background-color 0.2s; }
        .notebook-btn:hover { background-color: #1565c0; }
    </style>
</head>
<body class="bg-notebook text-gray-800 flex min-h-screen">

    <aside class="sidebar w-64 p-5 flex flex-col justify-between">
        <div>
            <div class="flex items-center mb-8">
                <span class="text-2xl font-black text-blue-600">🎓</span>
                <span class="ml-2 text-xl font-bold text-gray-800">P!LOT</span>
            </div>

            <nav class="space-y-3 text-gray-700 font-medium">
                <a href="#dashboard" class="flex items-center p-3 rounded-lg hover:bg-gray-100 transition-colors">
                    <span class="mr-3">🏠</span> 儀表板
                </a>
                <a href="#ai-partner" class="flex items-center p-3 rounded-lg hover:bg-gray-100 transition-colors">
                    <span class="mr-3">🤖</span> AI 學習夥伴
                </a>
                <a href="#my-notes" class="flex items-center p-3 rounded-lg hover:bg-gray-100 transition-colors">
                    <span class="mr-3">✍️</span> 我的筆記庫
                </a>
                <a href="#past-papers" class="flex items-center p-3 rounded-lg hover:bg-gray-100 transition-colors">
                    <span class="mr-3">📚</span> 歷屆試題
                </a>
                <a href="#community" class="flex items-center p-3 rounded-lg hover:bg-gray-100 transition-colors">
                    <span class="mr-3">💬</span> 戰友交流區
                </a>
            </nav>
        </div>

        <div id="loginSection" class="mt-8 pt-5 border-t border-gray-200">
            <span class="text-gray-500 text-sm">載入驗證系統...</span>
        </div>
    </aside>

    <main class="flex-1 p-8 overflow-y-auto">
        <div class="max-w-4xl mx-auto space-y-8">

            <section id="dashboard" class="content-card p-6">
                <h2 class="text-2xl font-bold mb-5 text-gray-800">儀表板</h2>
                <div class="grid md:grid-cols-2 gap-6">
                    <div class="bg-blue-50 p-5 rounded-lg flex items-center justify-between">
                        <div>
                            <p class="text-blue-700 text-sm font-medium">每日簽到進度</p>
                            <p id="streakCountDisplay" class="text-3xl font-black text-blue-800 mt-1">0 <span class="text-xl">DAYS</span></p>
                        </div>
                        <button onclick="doCheckIn()" class="notebook-btn bg-blue-600 hover:bg-blue-700">今日簽到</button>
                    </div>
                    <div class="bg-green-50 p-5 rounded-lg">
                        <p class="text-green-700 text-sm font-medium">2026 學測倒數</p>
                        <p id="countdownDisplay" class="text-3xl font-black text-green-800 mt-1">-- <span class="text-xl">天</span></p>
                    </div>
                </div>
                <p id="dailyQuote" class="text-gray-600 text-center mt-6 p-4 bg-gray-50 rounded-lg italic text-sm">
                    「讀書百遍，其義自見。」
                </p>
            </section>

            <section id="ai-partner" class="content-card p-6">
                <h2 class="text-2xl font-bold mb-5 text-gray-800 flex items-center">
                    <span class="mr-2">🤖</span> AI 學習夥伴
                </h2>
                <p class="text-gray-600 mb-4 text-sm">輸入題號或關鍵字，AI 將從你的筆記庫與公眾觀念庫中找出解答。</p>
                <div class="flex flex-col md:flex-row gap-3">
                    <input type="text" id="aiQueryInput" placeholder="例如：113數A-選填B 或 三角函數公式" class="flex-1 px-4 py-2.5 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <button onclick="queryAI()" class="notebook-btn">查詢觀念</button>
                </div>
                <div id="aiResultDisplay" class="mt-5 p-4 bg-blue-50 border border-blue-100 rounded-lg text-blue-800 text-sm hidden">
                    </div>
            </section>

            <section id="my-notes" class="content-card p-6">
                <h2 class="text-2xl font-bold mb-5 text-gray-800 flex items-center">
                    <span class="mr-2">✍️</span> 我的筆記庫
                </h2>
                <p class="text-gray-600 mb-4 text-sm">管理你的筆記，並讓 AI 協助你索引。</p>
                
                <div class="bg-gray-50 p-5 rounded-lg mb-6">
                    <h3 class="text-lg font-bold mb-3 text-gray-700">上傳新筆記</h3>
                    <div class="grid md:grid-cols-2 gap-4 mb-3">
                        <input type="text" id="uploadNoteTitle" placeholder="筆記標題 (例如：數A單元三重點)" class="px-4 py-2.5 rounded-lg border border-gray-300">
                        <input type="text" id="uploadNoteLink" placeholder="Google Drive/PDF 連結" class="px-4 py-2.5 rounded-lg border border-gray-300">
                    </div>
                    <button onclick="uploadUserNote()" class="notebook-btn w-full">上傳我的筆記</button>
                </div>

                <h3 class="text-lg font-bold mb-3 text-gray-700">已上傳筆記列表</h3>
                <div id="userNoteList" class="space-y-3 max-h-64 overflow-y-auto">
                    <p class="text-gray-500 text-sm">登入後即可查看或上傳你的筆記。</p>
                </div>
            </section>

            <section id="past-papers" class="content-card p-6">
                <h2 class="text-2xl font-bold mb-5 text-gray-800 flex items-center">
                    <span class="mr-2">📚</span> 歷屆試題
                </h2>
                <div class="grid md:grid-cols-2 gap-5">
                    <div class="bg-blue-50 p-5 rounded-lg">
                        <h3 class="font-bold text-blue-700 mb-3">學測考古題</h3>
                        <ul class="space-y-2 text-sm text-blue-600">
                            <li><a href="#" class="hover:underline">113 學測全科試題</a></li>
                            <li><a href="#" class="hover:underline">112 學測全科試題</a></li>
                        </ul>
                    </div>
                    <div class="bg-green-50 p-5 rounded-lg">
                        <h3 class="font-bold text-green-700 mb-3">模擬考試題</h3>
                        <ul class="space-y-2 text-sm text-green-600">
                            <li><a href="#" class="hover:underline">114 北模第一次</a></li>
                            <li><a href="#" class="hover:underline">114 全模第一次</a></li>
                        </ul>
                    </div>
                </div>
            </section>
            
            <section id="community" class="content-card p-6">
                <h2 class="text-2xl font-bold mb-5 text-gray-800 flex items-center">
                    <span class="mr-2">💬</span> 戰友交流區
                </h2>
                <p class="text-gray-600 mb-4 text-sm">查看最新戰友分享的筆記或提問！</p>
                <div id="communityNoteList" class="space-y-4 max-h-64 overflow-y-auto">
                    <p class="text-gray-500 text-sm">正在載入戰友分享...</p>
                </div>
            </section>

        </div>
    </main>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp, where } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        // ⚠️ 請在此處插入您的 Firebase 密鑰！
        // 替換 YOUR_API_KEY, YOUR_PROJECT_ID 等為你從 Firebase Console 取得的值
        const firebaseConfig = {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_PROJECT_ID.appspot.com",
            messagingSenderId: "YOUR_SENDER_ID",
            appId: "YOUR_APP_ID"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        const provider = new GoogleAuthProvider();

        let currentUser = null; // 用於儲存當前登入用戶

        // --- 登入/登出邏輯 ---
        window.login = () => signInWithPopup(auth, provider).catch(err => console.error("Login failed:", err));
        window.logout = () => signOut(auth).then(() => location.reload());

        // --- 監聽登入狀態並更新 UI ---
        onAuthStateChanged(auth, (user) => {
            currentUser = user; // 更新當前用戶
            const loginSection = document.getElementById('loginSection');
            if (user) {
                loginSection.innerHTML = `
                    <div class="flex items-center space-x-2">
                        <img src="${user.photoURL}" class="w-8 h-8 rounded-full border border-gray-200">
                        <span class="text-gray-700 text-sm font-medium">${user.displayName}</span>
                        <button onclick="logout()" class="text-sm text-red-500 hover:underline">登出</button>
                    </div>
                `;
                // 登入後載入用戶自己的筆記
                loadUserNotes(user.uid);
            } else {
                loginSection.innerHTML = `<button onclick="login()" class="notebook-btn w-full">Google 登入</button>`;
                document.getElementById('userNoteList').innerHTML = `<p class="text-gray-500 text-sm">登入後即可查看或上傳你的筆記。</p>`;
            }
        });

        // --- 上傳用戶自己的筆記 ---
        window.uploadUserNote = async () => {
            if (!currentUser) return alert("請先登入才能上傳筆記喔！");

            const title = document.getElementById('uploadNoteTitle').value;
            const link = document.getElementById('uploadNoteLink').value;

            if (!title || !link) return alert("請填寫筆記標題與連結");

            try {
                await addDoc(collection(db, "userNotes"), { // 使用 'userNotes' 集合儲存用戶自己的筆記
                    title,
                    link,
                    userName: currentUser.displayName,
                    userId: currentUser.uid, // 記錄是哪個用戶上傳的
                    createdAt: serverTimestamp()
                });
                document.getElementById('uploadNoteTitle').value = "";
                document.getElementById('uploadNoteLink').value = "";
                alert("你的筆記已成功上傳！");
            } catch (e) {
                console.error("上傳筆記失敗: ", e);
            }
        };

        // --- 載入用戶自己的筆記列表 ---
        function loadUserNotes(userId) {
            const q = query(collection(db, "userNotes"), where("userId", "==", userId), orderBy("createdAt", "desc"));
            onSnapshot(q, (snapshot) => {
                const userNoteList = document.getElementById('userNoteList');
                userNoteList.innerHTML = "";
                if (snapshot.empty) {
                    userNoteList.innerHTML = `<p class="text-gray-500 text-sm">你還沒有上傳任何筆記。</p>`;
                    return;
                }
                snapshot.forEach((doc) => {
                    const data = doc.data();
                    userNoteList.innerHTML += `
                        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                            <span class="text-sm text-gray-700 font-medium">${data.title}</span>
                            <a href="${data.link}" target="_blank" class="text-blue-600 text-xs hover:underline">查看</a>
                        </div>
                    `;
                });
            });
        }
        
        // --- 載入戰友交流區筆記 --- (所有用戶的公開筆記)
        const communityQ = query(collection(db, "userNotes"), orderBy("createdAt", "desc")); // 這裡假設用戶筆記也是公開的
        onSnapshot(communityQ, (snapshot) => {
            const communityNoteList = document.getElementById('communityNoteList');
            communityNoteList.innerHTML = "";
            if (snapshot.empty) {
                communityNoteList.innerHTML = `<p class="text-gray-500 text-sm">目前還沒有戰友分享筆記。</p>`;
                return;
            }
            snapshot.forEach((doc) => {
                const data = doc.data();
                communityNoteList.innerHTML += `
                    <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                        <div>
                            <p class="text-sm font-medium text-gray-700">${data.title}</p>
                            <p class="text-xs text-gray-500">by ${data.userName}</p>
                        </div>
                        <a href="${data.link}" target="_blank" class="text-blue-600 text-xs hover:underline">查看</a>
                    </div>
                `;
            });
        });
        
    </script>

    <script>
        // --- 簽到功能 ---
        function doCheckIn() {
            // TODO: 將簽到狀態存入 Firestore，綁定用戶 ID
            let streak = parseInt(localStorage.getItem('study_streak') || 0) + 1;
            localStorage.setItem('study_streak', streak);
            document.getElementById('streakCountDisplay').innerText = streak;
            alert("✅ 簽到成功！你的毅力值得肯定！");
            updateDailyQuote();
        }

        // --- 學測倒數計時 ---
        function updateCountdown() {
            const examDate = new Date('2026-01-24T09:00:00'); // 假設 2026 學測日期
            const now = new Date();
            const diffTime = examDate.getTime() - now.getTime();
            const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
            document.getElementById('countdownDisplay').innerText = diffDays > 0 ? diffDays : '0';
            
            // 簡易進度條 (你可以設定一個總天數來計算百分比)
            const totalDays = 365; // 從今天開始到學測的總天數，需要手動設定或用程式計算
            // const passedDays = totalDays - diffDays;
            // const progress = (passedDays / totalDays) * 100;
            // if (progress >= 0 && progress <= 100) {
            //    document.getElementById('countdownProgress').style.width = `${progress}%`;
            // }
        }

        // --- 每日一句 ---
        const quotes = [
            "讀書百遍，其義自見。",
            "沒有奇蹟，只有累積。",
            "成功者找方法，失敗者找藉口。",
            "現在的努力，是為了將來的不費力。",
            "堅持下去，世界將為你讓路。"
        ];
        function updateDailyQuote() {
            const today = new Date().toDateString();
            let lastQuoteDay = localStorage.getItem('lastQuoteDay');
            let quoteIndex = parseInt(localStorage.getItem('quoteIndex') || 0);

            if (today !== lastQuoteDay) {
                quoteIndex = (quoteIndex + 1) % quotes.length;
                localStorage.setItem('lastQuoteDay', today);
                localStorage.setItem('quoteIndex', quoteIndex);
            }
            document.getElementById('dailyQuote').innerText = `「${quotes[quoteIndex]}」`;
        }


        // --- AI 學習夥伴查詢 ---
        function queryAI() {
            const queryInput = document.getElementById('aiQueryInput').value.trim();
            const aiResultDisplay = document.getElementById('aiResultDisplay');

            if (!queryInput) {
                aiResultDisplay.classList.remove('hidden');
                aiResultDisplay.innerHTML = `<p>請輸入您想查詢的題號或關鍵字。</p>`;
                return;
            }

            aiResultDisplay.classList.remove('hidden');
            aiResultDisplay.innerHTML = `<p>正在從觀念庫中為您搜尋「<strong>${queryInput}</strong>」...</p><p class="mt-2 text-xs text-blue-600">（此為模擬功能，未來將串接您的觀念資料庫）</p>`;
            
            // TODO: 未來這裡會根據 queryInput 到你的觀念資料庫 (Firestore 或 JSON) 進行檢索
            // 這裡可以加入模擬延遲，讓使用者感覺像真的在搜尋
            setTimeout(() => {
                const simulatedResult = `
                    <p class="font-bold">找到相關觀念：三角函數的疊合</p>
                    <p class="mt-2">應用範圍：求 ${queryInput} 函數的最大最小值。</p>
                    <p class="mt-2">核心公式：$a \\sin \\theta + b \\cos \\theta = \\sqrt{a^2+b^2} \\sin(\\theta+\\alpha)$</p>
                    <p class="mt-2"><a href="#" class="text-blue-600 hover:underline">查看更多筆記與例題</a></p>
                `;
                aiResultDisplay.innerHTML = simulatedResult;
            }, 1500); // 模擬 1.5 秒的查詢時間
        }


        // 初始化所有非 Firebase 相關功能
        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('streakCountDisplay').innerText = localStorage.getItem('study_streak') || 0;
            updateCountdown();
            setInterval(updateCountdown, 1000 * 60 * 60); // 每小時更新一次倒數
            updateDailyQuote();
        });
    </script>
</body>
</html>
