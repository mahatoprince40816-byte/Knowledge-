<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EduVision | AI Learning Hub</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&family=Poppins:wght@700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root { --p: #6366f1; --s: #a855f7; --a: #f97316; --bg: #0f172a; --txt: #f8fafc; }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--txt); line-height: 1.6; }
        nav { position: fixed; top: 0; width: 100%; z-index: 1000; display: flex; justify-content: space-between; padding: 1rem 5%; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(10px); border-bottom: 1px solid rgba(255,255,255,0.1); }
        .hero { height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; background: radial-gradient(circle at center, #1e1b4b, #0f172a); padding: 20px; }
        .hero h1 { font-family: 'Poppins'; font-size: clamp(2.5rem, 8vw, 4rem); margin-bottom: 1rem; background: linear-gradient(to right, #fff, var(--s)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .btn { padding: 12px 30px; border-radius: 50px; font-weight: 600; cursor: pointer; border: none; transition: 0.3s; text-decoration: none; }
        .btn-p { background: var(--a); color: white; box-shadow: 0 0 20px rgba(249,115,22,0.4); }
        .section { padding: 100px 5%; }
        .search-box { max-width: 600px; margin: 0 auto 40px; display: flex; gap: 10px; }
        input { flex: 1; padding: 15px; border-radius: 30px; border: none; outline: none; background: rgba(255,255,255,0.05); color: white; border: 1px solid rgba(255,255,255,0.2); }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 25px; }
        .card { background: rgba(255,255,255,0.05); padding: 20px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.1); text-align: center; }
        .card img { width: 100%; height: 250px; object-fit: cover; border-radius: 10px; margin-bottom: 15px; }
        .ai-bot { position: fixed; bottom: 30px; right: 30px; z-index: 2000; }
        .ai-icon { width: 60px; height: 60px; background: var(--p); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.5rem; cursor: pointer; box-shadow: 0 10px 20px rgba(0,0,0,0.3); }
        .chat { position: absolute; bottom: 80px; right: 0; width: 320px; height: 450px; background: #1e293b; border-radius: 20px; display: none; flex-direction: column; border: 1px solid var(--p); overflow: hidden; }
        .chat-h { background: var(--p); padding: 15px; font-weight: bold; }
        .chat-m { flex: 1; padding: 15px; overflow-y: auto; display: flex; flex-direction: column; gap: 10px; font-size: 0.9rem; }
        .chat-i { display: flex; padding: 10px; border-top: 1px solid rgba(255,255,255,0.1); gap: 5px; }
    </style>
</head>
<body>
    <nav><div style="font-weight:800; color:var(--s)">EDUVISION</div></nav>
    <header class="hero">
        <h1>Anything. Learn Everything.</h1>
        <p style="margin-bottom:2rem; opacity:0.8">Your AI-powered gateway to books and knowledge.</p>
        <a href="#books" class="btn btn-p">Start Learning Now</a>
    </header>
    <section class="section" id="books">
        <h2 style="text-align:center; margin-bottom:30px;">Find Your Next Book</h2>
        <div class="search-box">
            <input type="text" id="bkIn" placeholder="Search for books (e.g. Science, Art)...">
            <button onclick="findBooks()" class="btn btn-p" style="padding: 10px 20px;">Search</button>
        </div>
        <div class="grid" id="bkGrid"></div>
    </section>
    <div class="ai-bot">
        <div class="chat" id="chatWin">
            <div class="chat-h">EduVision AI</div>
            <div class="chat-m" id="chatMsgs"><div>Hi! I'm your AI assistant. Ask me anything!</div></div>
            <div class="chat-i">
                <input type="text" id="aiIn" placeholder="Ask a question...">
                <button onclick="askAI()" class="btn btn-p" style="padding: 5px 15px;"><i class="fas fa-paper-plane"></i></button>
            </div>
        </div>
        <div class="ai-icon" onclick="toggleChat()"><i class="fas fa-robot"></i></div>
    </div>
    <script>
        async function findBooks() {
            const q = document.getElementById('bkIn').value || 'Knowledge';
            const g = document.getElementById('bkGrid');
            g.innerHTML = "<p>Searching library...</p>";
            try {
                const res = await fetch(`https://www.googleapis.com/books/v1/volumes?q=${encodeURIComponent(q)}&maxResults=6`);
                const data = await res.json();
                g.innerHTML = "";
                if(!data.items) { g.innerHTML = "<p>No books found. Try another word!</p>"; return; }
                data.items.forEach(b => {
                    const info = b.volumeInfo;
                    const img = info.imageLinks ? info.imageLinks.thumbnail.replace('http:', 'https:') : 'https://via.placeholder.com/150';
                    g.innerHTML += `<div class="card"><img src="${img}"><h3>${info.title.substr(0,30)}</h3><p>${info.authors?.[0] || 'Author'}</p></div>`;
                });
            } catch(e) { g.innerHTML = "<p>Connection error. Try again.</p>"; }
        }
        function toggleChat() { const c = document.getElementById('chatWin'); c.style.display = c.style.display === 'flex' ? 'none' : 'flex'; }
        function askAI() {
            const i = document.getElementById('aiIn'); const m = document.getElementById('chatMsgs');
            if(!i.value) return;
            m.innerHTML += `<div style="text-align:right; color:var(--a)">${i.value}</div>`;
            const val = i.value.toLowerCase(); i.value = "";
            setTimeout(() => {
                let ans = "I'm looking into that for you!";
                if(val.includes("hello")) ans = "Hello! How can I help you learn today?";
                if(val.includes("book")) ans = "I recommend checking our search bar for the latest books!";
                m.innerHTML += `<div style="background:rgba(255,255,255,0.1); padding:8px; border-radius:10px;">${ans}</div>`;
                m.scrollTop = m.scrollHeight;
            }, 800);
        }
        window.onload = findBooks;
    </script>
</body>
</html> Knowledge-
