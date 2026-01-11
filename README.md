<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>學測腦 GSAT Brain</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Noto Sans TC', sans-serif; background-color: #f8f9fa; }
        .sidebar { width: 300px; height: 100vh; position: fixed; left: 0; top: 0; background: #ffffff; border-right: 1px solid #e9ecef; }
        .main-content { margin-left: 300px; padding: 40px; }
        .glass-card { background: rgba(255, 255, 255, 0.8); backdrop-filter: blur(10px); border: 1px solid #e9ecef; border-radius: 16px; }
        .notebook-input { border: 1px solid #dee2e6; transition: all 0.2s; }
        .notebook-input:focus { border-color: #1a73e8; ring: 2px; ring-color: #1a73e8; }
    </style>
</head>
<body class="flex">

    <aside class="sidebar flex flex-col p-6">
        <div class="mb-10">
            <h1 class="text-2xl font-black text-blue-600 tracking-tighter italic">GSAT Brain</h1>
            <p class="text-xs text-gray-400 mt-1">你的 AI 學習助手</p>
        </div>

        <nav class="flex-1 space-y-2">
            <a href="#" class="block p-3 text-gray-700 hover:bg-gray-50 rounded-xl font-medium">🏠 儀表板</a>
            <a href="#" class="block p-3 text-gray-700 hover:bg-gray-50 rounded-xl font-medium">🤖 AI 觀念檢索</a>
            <a href="#" class="block p-3 text-gray-700 hover:bg-gray-50 rounded-xl font-medium">📚 考古題庫</a>
            <a href="#" class="block p-3 text-gray-700 hover:bg-gray-50 rounded-xl font-medium">✍️ 共享筆記</a>
        </nav>

        <div id="loginSection" class="pt-6 border-t">
            </div>
    </aside>

    <main class="main-content w-full max-w-5xl">
        
        <div class="grid grid-cols-3 gap-6 mb-10">
            <div class="glass-card p-6">
                <p class="text-gray-400 text-xs font-bold uppercase">學測倒數</p>
                <h3 id="countdownDisplay" class="text-3xl font-black text-gray-800 mt-2">--</h3>
            </div>
            <div class="glass-card p-6">
                <p class="text-gray-400 text-xs font-bold uppercase">連續簽到</p>
                <h3 id="streakCountDisplay" class="text-3xl font-black text-orange-500 mt-2">0</h3>
            </div>
            <div class="glass-card p-6 flex items-center justify-center">
                <button onclick="doCheckIn()" class="bg-blue-600 text-white px-8 py-3 rounded-xl font-bold hover:bg-blue
