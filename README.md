# gsat1
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GSATMoNSTer</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .gradient-header { background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); }
        body { font-family: 'Microsoft JhengHei', sans-serif; }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">

    <nav class="bg-white border-b border-gray-200 sticky top-0 z-50">
        <div class="container mx-auto px-4 h-16 flex justify-between items-center">
            <h1 class="text-2xl font-black text-blue-600 tracking-tight">🎓 學測應援站</h1>
            <div id="loginSection" class="flex items-center space-x-3">
                <span class="text-gray-400 text-sm italic">載入驗證系統...</span>
            </div>
        </div>
    </nav>

    <header class="gradient-header py-12 text-white text-center px-4">
        <h2 class="text-4xl font-extrabold mb-4">天道酬勤</h2>
        <p class="text-blue-50 text-lg opacity-90">串聯全台戰友，資源共享，精準偵錯</p>
    </header>

    <main class="container mx-auto px-4 py-8 max-w-6xl">
        <div class="grid lg:grid-cols-3 gap-8">
            
            <div class="lg:col-span-1 space-y-6">
                <section class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100 text-center">
                    <h3 class="text-lg font-bold text-gray-700 mb-6">📅 讀書馬拉松</h3>
                    <div class="inline-block bg-orange-50 px-8 py-6 rounded-2xl mb-6">
                        <p class="text-gray-500 text-xs font-medium uppercase tracking-widest">目前連續簽到</p>
                        <p id="streakCount" class="text-5xl font-black text-orange-500 my-1">0</p>
                        <p class="text-gray-500 text-xs font-bold">DAYS</p>
                    </div>
                    <button onclick="doCheckIn()" class="w-full bg-orange-500 hover:bg-orange-600 text-white font-bold py-4 rounded-2xl shadow-lg mb-4">立即簽到</button>
                    <button onclick="shareToIG()" class="w-full text-pink-500 font-bold py-2 text-sm">分享進度到 Instagram</button>
                </section>
            </div>

            <div class="lg:col-span-2 space-y-8">
                
                <section class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100">
                    <h3 class="text-xl font-bold text-gray-800 mb-6">✍️ 戰友筆記分享區</h3>
                    <div class="bg-blue-50 p-6 rounded-2xl mb-6">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                            <input type="text" id="noteTitle" placeholder="標題：例如 113 學測數學重點" class="px-4 py-2 rounded-xl border-none text-sm">
                            <input type="text" id="noteLink" placeholder="Google Drive 連結" class="px-4 py-2 rounded-xl border-none text-sm">
                        </div>
                        <button onclick="uploadNote()" class="bg-blue-600 text-white px-6 py-2 rounded-xl font-bold text-sm w-full">發佈我的筆記</button>
                    </div>
                    <div id="noteList" class="max-h-64 overflow-y-auto space-y-2 text-sm">
                        </div>
                </section>

                <section class="bg-white p-8 rounded-3xl shadow-sm border border-gray-100">
                    <h3 class="text-xl font-bold text-gray-800 mb-4 font-bold">📚 歷屆試題與 AI 偵錯</h3>
                    <div class="grid md:grid-cols-2 gap-4 mb-6 text-sm">
                        <div class="p-4 bg-gray-50 rounded-xl font-bold">學測考古題：<a href="#" class="text-blue-500">113年</a> / <a href="#" class="text-blue-500">112年</a></div>
                        <div class="p-4 bg-gray-50 rounded-xl font-bold">模考專區：<a href="#" class="text-green-500">114北模</a> / <a href="#" class="text-green-500">114全模</a></div>
                    </div>
                    <div class="bg-slate-900 p-6 rounded-2xl text-white">
                        <h4 class="text-sm font-bold mb-2">🤖 AI 弱點診斷</h4>
                        <div class="flex gap-2">
                            <input type="text" id="aiInput" placeholder="輸入題號..." class="bg-slate-800 border-none rounded-lg px-4 py-2 flex-1 text-sm">
                            <button onclick="aiDiagnose()" class="bg-blue-500 px-4 py-2 rounded-lg text-sm">分析</button>
                        </div>
                        <div id="aiResult" class="mt-3 text-xs text-slate-400"></div>
                    </div>
                </section>

            </div>
        </div>
    </main>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
        import { getAuth, signInWithPopup, GoogleAuthProvider, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

        /* 📍 在這裡插入您的 Firebase 密鑰！
           請從 Firebase Console > 專案設定 > 您的應用程式 中複製內容替換下方
        */
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

        // 登入登出邏輯
        window.login = () => signInWithPopup(auth, provider).catch(err => alert("登入失敗"));
        window.logout = () => signOut(auth).then(() => location.reload());

        onAuthStateChanged(auth, (user) => {
            const loginSection = document.getElementById('loginSection');
            if (user) {
                loginSection.innerHTML = `<button onclick="logout()" class="text-sm bg-gray-100 px-4 py-2 rounded-full font-bold">登出 (${user.displayName})</button>`;
            } else {
                loginSection.innerHTML = `<button onclick="login()" class="bg-blue-600 text-white px-6 py-2 rounded-full font-bold text-sm shadow-md">Google 登入</button>`;
            }
        });

        // 筆記發布邏輯
        window.uploadNote = async () => {
            const user = auth.currentUser;
            if (!user) return alert("請先登入");
            const title = document.getElementById('noteTitle').value;
            const link = document.getElementById('noteLink').value;
            if (!title || !link) return alert("請填寫標題與連結");
            
            await addDoc(collection(db, "notes"), {
                title, link, userName: user.displayName, createdAt: serverTimestamp()
            });
            document.getElementById('noteTitle').value = "";
            document.getElementById('noteLink').value = "";
            alert("發佈成功！");
        };

        // 即時監聽筆記
        const q = query(collection(db, "notes"), orderBy("createdAt", "desc"));
        onSnapshot(q, (snapshot) => {
            const list = document.getElementById('noteList');
            list.innerHTML = "";
            snapshot.forEach(doc => {
                const d = doc.data();
                list.innerHTML += `<div class="p-3 border-b flex justify-between"><span><b>${d.title}</b> <small>(${d.userName})</small></span><a href="${d.link}" target="_blank" class="text-blue-500">查看</a></div>`;
            });
        });
    </script>

    <script>
        function doCheckIn() {
            let s = parseInt(localStorage.getItem('study_streak') || 0) + 1;
            localStorage.setItem('study_streak', s);
            document.getElementById('streakCount').innerText = s;
            alert("簽到成功！");
        }
        function shareToIG() { alert("文案已複製！"); }
        function aiDiagnose() {
            document.getElementById('aiResult').innerText = "系統已接收題號，正在開發觀念對應功能...";
        }
        document.getElementById('streakCount').innerText = localStorage.getItem('study_streak') || 0;
    </script>
</body>
</html>
