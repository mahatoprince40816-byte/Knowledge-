
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EduVision | Ultimate Learning Hub</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600&family=Poppins:wght@700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root { --p: #6366f1; --s: #a855f7; --a: #f97316; --bg: #0f172a; --txt: #f8fafc; }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--txt); scroll-behavior: smooth; }
        nav { position: fixed; top: 0; width: 100%; z-index: 1000; display: flex; justify-content: space-between; padding: 1rem 5%; background: rgba(15, 23, 42, 0.9); backdrop-filter: blur(10px); border-bottom: 1px solid rgba(255,255,255,0.1); }
        .hero { height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; background: radial-gradient(circle at center, #1e1b4b, #0f172a); padding: 20px; }
        .hero h1 { font-family: 'Poppins'; font-size: clamp(2rem, 7vw, 4rem); margin-bottom: 1rem; background: linear-gradient(to right, #fff, var(--s)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .btn { padding: 12px 25px; border-radius: 50px; font-weight: 600; cursor: pointer; border: none; transition: 0.3s; text-decoration: none; display: inline-block; }
        .btn-p { background: var(--a); color: white; box-shadow: 0 5px 15px rgba(249,115,22,0.3); }
        .section { padding: 80px 5%; }
        .search-container { max-width: 600px; margin: 0 auto 40px; display: flex; gap: 10px; }
        input { flex: 1; padding: 15px; border-radius: 30px; border: 1px solid rgba(255,255,255,0.2); background: rgba(255,255,255,0.05); color: white; outline: none; }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 25px; }
        .card { background: rgba(255,255,255,0.05); padding: 20px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.1); transition: 0.3s; }
        .card:hover { transform: translateY(-5px); background: rgba(255,255,255,0.08); }
        .card img { width: 100%; height: 280px; object-fit: cover; border-radius: 15px; margin-bottom: 15px; }
        .bubble-container { display: flex; flex-wrap: wrap; justify-content: center; gap: 15px; margin-top: 20px; }
        .bubble { padding: 10px 20px; border-radius: 30px; background: var(--p); cursor: pointer; transition: 0.3s; font-weight: 600; font-size: 0.9rem; }
        .bubble:hover { background: var(--s); transform: scale(1.1); }
        .ai-bot { position: fixed; bottom: 30px; right: 30px; z-index: 2000; }
        .ai-icon { width: 60px; height: 60px; background: var(--p); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 1.5rem; cursor: pointer; box-shadow: 0 10px 20px rgba(0,0,0,0.3); }
        .chat { position: absolute; bottom: 80px; right: 0; width: 320px; height: 450px; background: #1e293b; border-radius: 20px; display: none; flex-direction: column; border: 1px solid var(--p); box-shadow: 0 15px 30px rgba(0,0,0,0.5); overflow: hidden; }
        .chat-h { background: var(--p); padding: 15px; font-weight: bold; display: flex; justify-content: space-between; }
        .chat-m { flex: 1; padding: 15px; overflow-y: auto; display: flex; flex-direction: column; gap: 12px; font-size: 0.9rem; }
        .chat-i { display: flex; padding: 10px; border-top: 1px solid rgba(255,255,255,0.1); gap: 8px; }
        .msg { padding: 10px; border-radius: 15px; max-width: 85%; }
        .msg-ai { background: rgba(255,255,255,0.1); align-self: flex-start; }
        .msg-user { background: var(--p); align-self: flex-end; }
    </style>
</head>
<body>
    <nav>
        <div style="font-weight:800; font-size:1.5rem; color:var(--s)"><i class="fas fa-brain"></i> EDUVISION</div>
    </nav>

    <header class="hero">
        <h1>Anything. Learn Everything.</h1>
        <p style="margin-bottom:2rem; opacity:0.8; max-width: 600px;">Explore the world's books and topics with your personal AI learning assistant.</p>
        <a href="#books" class="btn btn-p">Start Exploring</a>
    </header>

    <section class="section" id="topics">
        <h2 style="text-align:center; margin-bottom:20px;">Trending Topics</h2>
        <div class="bubble-container">
            <div class="bubble" onclick="searchTopic('Space')">Space</div>
            <div class="bubble" onclick="searchTopic('AI')">Artificial Intelligence</div>
            <div class="bubble" onclick="searchTopic('History')">World History</div>
            <div class="bubble" onclick="searchTopic('Art')">Fine Art</div>
            <div class="bubble" onclick="searchTopic('Science')">Modern Science</div>
        </div>
    </section>

    <section class="section" id="books">
        <div class="search-container">
            <input type="text" id="bkIn" placeholder="Search for any book or topic...">
            <button onclick="findBooks()" class="btn btn-p"><i class="fas fa-search"></i></button>
        </div>
        <div class="grid" id="bkGrid"></div>
    </section>

    <div class="ai-bot">
        <div class="chat" id="chatWin">
            <div class="chat-h"><span>EduVision AI Agent</span><i class="fas fa-times" onclick="toggleChat()" style="cursor:pointer"></i></div>
            <div class="chat-m" id="chatMsgs">
                <div class="msg msg-ai">Hi! I'm your AI assistant. Ask me anything about books, science, or your studies!</div>
            </div>
            <div class="chat-i">
                <input type="text" id="aiIn" placeholder="Type your question...">
                <button onclick="askAI()" class="btn btn-p" style="padding: 10px;"><i class="fas fa-paper-plane"></i></button>
            </div>
        </div>
        <div class="ai-icon" onclick="toggleChat()"><i class="fas fa-robot"></i></div>
    </div>

    <script>
        // 1. BOOK SEARCH ENGINE
        async function findBooks() {
            const q = document.getElementById('bkIn').value || 'New Books';
            const g = document.getElementById('bkGrid');
            g.innerHTML = "<p style='grid-column:1/-1; text-align:center;'>Searching the internet...</p>";
            try {
                const res = await fetch(`https://www.googleapis.com/books/v1/volumes?q=${encodeURIComponent(q)}&maxResults=6`);
                const data = await res.json();
                g.innerHTML = "";
                if(!data.items) { g.innerHTML = "<p>No books found. Try a different topic!</p>"; return; }
                data.items.forEach(b => {
                    const info = b.volumeInfo;
                    const img = info.imageLinks ? info.imageLinks.thumbnail.replace('http:', 'https:') : 'https://via.placeholder.com/300x450?text=No+Cover';
                    g.innerHTML += `
                        <div class="card">
                            <img src="${img}">
                            <h3>${info.title.substr(0,40)}...</h3>
                            <p style="color:var(--s); margin:10px 0;">${info.authors?.[0] || 'Expert Author'}</p>
                            <button class="btn" style="border:1px solid var(--p); width:100%; font-size:0.8rem;">READ SUMMARY</button>
                        </div>`;
                });
            } catch(e) { g.innerHTML = "<p>Internet error. Please refresh.</p>"; }
        }

        // 2. TOPIC EXPLORER BUBBLES
        function searchTopic(t) {
            document.getElementById('bkIn').value = t;
            findBooks();
            window.location.hash = "books";
        }

        // 3. AI AGENT BRAIN
        function toggleChat() { const c = document.getElementById('chatWin'); c.style.display = c.style.display === 'flex' ? 'none' : 'flex'; }
        
        function askAI() {
            const i = document.getElementById('aiIn'); 
            const m = document.getElementById('chatMsgs');
            if(!i.value) return;

            // Add User Message
            m.innerHTML += `<div class="msg msg-user">${i.value}</div>`;
            const query = i.value.toLowerCase();
            i.value = "";
            m.scrollTop = m.scrollHeight;

            // AI Processing Response
            setTimeout(() => {
                let ans = "That's an interesting topic! Should I find some books about that for you?";
                if(query.includes("myself")) ans = "You are a learner using EduVision! I can help you find books to improve your skills.";
                if(query.includes("science")) ans = "Science is vast! We have books on Physics, Biology, and Space in the search bar above.";
                if(query.includes("hello") || query.includes("hi")) ans = "Hello! I am your EduVision assistant. What do you want to learn today?";
                
                m.innerHTML += `<div class="msg msg-ai">${ans}</div>`;
                m.scrollTop = m.scrollHeight;
            }, 1000);
        }

        window.onload = findBooks;
    </script>
</body>
</html>
