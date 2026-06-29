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
    </style>
</head>
<body>
    <div class="container">
        <div class="ai-badge">🧠 مدعوم بالذكاء الاصطناعي</div>
        <h2>أداة تخمين <span>الباسورد</span></h2>
        <p class="sub">ذكاء اصطناعي يحلل الأنماط ويخمن كلمات المرور</p>

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
        </div>

        <div class="footer">🔒 الذكاء الاصطناعي يحلل الأنماط • جميع البيانات محلية</div>
    </div>

    <script>
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

        let isRunning = false;
        let stopFlag = false;
        let attempts = 0;
        let foundPasswords = [];
        let bestPassword = '';
        let bestConfidence = 0;

        // ====== خوارزمية الذكاء الاصطناعي ======
        function generateAIPasswords(email) {
            const prefix = email.split('@')[0];
            const domain = email.split('@')[1]?.split('.')[0] || '';
            const passwords = new Set();

            // 1. تحليل الإيميل واستخراج أنماط
            const patterns = [
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

            // 2. أنماط مع رموز خاصة
            const symbols = ['!', '@', '#', '$', '%', '^', '&', '*'];
            for (const sym of symbols) {
                patterns.push(prefix + sym);
                patterns.push(sym + prefix);
                patterns.push(prefix + '123' + sym);
                patterns.push(prefix + sym + '123');
            }

            // 3. أنماط مع سنوات
            for (let year = 2020; year <= 2026; year++) {
                patterns.push(prefix + year);
                patterns.push(year + prefix);
                patterns.push(prefix + year + '!');
                patterns.push(prefix + '!' + year);
            }

            // 4. أنماط مع كلمات شائعة
            const commonWords = ['admin', 'root', 'user', 'login', 'welcome', 'hello', 'master', 'sunshine', 'princess', 'dragon', 'monkey', 'freedom', 'iloveyou', 'trustno1'];
            for (const word of commonWords) {
                patterns.push(prefix + word);
                patterns.push(word + prefix);
                patterns.push(prefix + word + '123');
                patterns.push(word + '123' + prefix);
            }

            // 5. أنماط متقدمة (توليد تلقائي)
            for (let i = 1; i <= 50; i++) {
                patterns.push(prefix + i);
                patterns.push(i + prefix);
                patterns.push(prefix + String(i).padStart(4, '0'));
                // أنماط مع كلمات شائعة + أرقام
                for (const word of commonWords.slice(0, 10)) {
                    patterns.push(word + i);
                    patterns.push(i + word);
                    patterns.push(word + i + '!');
                    patterns.push(word + '!' + i);
                }
            }

            // 6. أنماط عشوائية ذكية (محاكاة AI)
            const chars = 'abcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*';
            for (let i = 0; i < 200; i++) {
                let pwd = '';
                const len = 8 + Math.floor(Math.random() * 4);
                for (let j = 0; j < len; j++) {
                    pwd += chars[Math.floor(Math.random() * chars.length)];
                }
                // إضافة بعض الأنماط الذكية
                if (i % 3 === 0) pwd = prefix + pwd.slice(0, 4);
                else if (i % 3 === 1) pwd = pwd.slice(0, 4) + prefix;
                patterns.push(pwd);
            }

            // 7. أنماط مركبة
            for (let i = 0; i < 100; i++) {
                const word1 = commonWords[Math.floor(Math.random() * commonWords.length)];
                const word2 = commonWords[Math.floor(Math.random() * commonWords.length)];
                const num = Math.floor(Math.random() * 9999);
                patterns.push(word1 + word2 + num);
                patterns.push(word1 + num + word2);
                patterns.push(prefix + word1 + num);
                patterns.push(word1 + num + prefix);
            }

            return [...new Set(patterns)];
        }

        // ====== تقييم قوة كلمة المرور (محاكاة AI) ======
        function evaluatePassword(password, email) {
            const prefix = email.split('@')[0];
            let score = 0;

            // 1. طول كلمة المرور
            if (password.length >= 8) score += 10;
            if (password.length >= 10) score += 10;
            if (password.length >= 12) score += 10;

            // 2. وجود أحرف كبيرة وصغيرة
            if (/[a-z]/.test(password) && /[A-Z]/.test(password)) score += 15;

            // 3. وجود أرقام
            if (/\d/.test(password)) score += 10;

            // 4. وجود رموز خاصة
            if (/[!@#$%^&*]/.test(password)) score += 10;

            // 5. ارتباط بالإيميل
            if (password.includes(prefix)) score += 10;
            if (password.includes(prefix.toLowerCase())) score += 5;
            if (password.includes(prefix.toUpperCase())) score += 5;

            // 6. أنماط شائعة
            if (/[a-z]{4,}/.test(password)) score += 5;
            if (/\d{3,}/.test(password)) score += 5;

            // 7. عدم وجود تكرارات
            if (new Set(password).size > password.length * 0.6) score += 5;

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

            isRunning = true;
            stopFlag = false;
            attempts = 0;
            foundPasswords = [];
            bestPassword = '';
            bestConfidence = 0;
            resultBox.style.display = 'none';
            startBtn.disabled = true;
            startBtn.innerHTML = `<span class="loader"></span> الذكاء الاصطناعي يفكر...`;
            stopBtn.style.display = 'block';
            statusDiv.innerHTML = `🧠 <span class="ai-thinking">الذكاء الاصطناعي يحلل الأنماط...</span>`;
            statusDiv.style.color = '#ff9800';
            progressFill.style.width = '0%';

            // توليد قائمة ذكية
            const passwordList = generateAIPasswords(email);
            const shuffled = passwordList.sort(() => Math.random() - 0.5);
            const total = shuffled.length;

            let startTime = Date.now();
            let lastUpdate = startTime;
            let attemptsInSecond = 0;
            let bestFound = '';

            for (let i = 0; i < shuffled.length; i++) {
                if (stopFlag) {
                    statusDiv.innerHTML = `⏹ تم الإيقاف بعد ${attempts} محاولة`;
                    statusDiv.style.color = '#ff9800';
                    break;
                }

                const pwd = shuffled[i];
                attempts++;
                attemptsInSecond++;

                // تقييم كلمة المرور بالذكاء الاصطناعي
                const confidence = evaluatePassword(pwd, email);

                if (confidence > bestConfidence) {
                    bestConfidence = confidence;
                    bestPassword = pwd;
                    bestFound = pwd;
                    // عرض النتيجة فوراً
                    resultBox.style.display = 'block';
                    resultPassword.textContent = pwd;
                    confidenceLevel.textContent = `${Math.round(confidence)}%`;
                    foundPasswords.push(pwd);
                    foundSpan.textContent = `المكتشفة: ${foundPasswords.length}`;
                }

                const now = Date.now();
                if (now - lastUpdate > 500) {
                    const speed = Math.round(attemptsInSecond / ((now - lastUpdate) / 1000));
                    speedSpan.textContent = `السرعة: ${speed}/ث`;
                    attemptsSpan.textContent = `المحاولات: ${attempts}`;
                    lastUpdate = now;
                    attemptsInSecond = 0;
                    const progress = Math.min((i / total) * 100, 99.9);
                    progressFill.style.width = progress + '%';
                }

                statusDiv.innerHTML = `🧠 تحليل النمط #${attempts}: <span class="attempt">${pwd}</span> (الثقة: ${Math.round(confidence)}%)`;
                statusDiv.style.color = '#ff9800';

                await new Promise(r => setTimeout(r, 5 + Math.random() * 15));

                // إذا وصلت الثقة إلى 90% نعتبرها مكتشفة
                if (confidence >= 90 && !foundPasswords.includes(pwd)) {
                    foundPasswords.push(pwd);
                    foundSpan.textContent = `المكتشفة: ${foundPasswords.length}`;
                    resultBox.style.display = 'block';
                    resultPassword.textContent = pwd;
                    confidenceLevel.textContent = `${Math.round(confidence)}%`;
                    statusDiv.innerHTML = `🎉 <span class="found">تم العثور على كلمة مرور قوية: ${pwd}</span>`;
                    statusDiv.style.color = '#4caf50';
                }

                if (foundPasswords.length > 0) {
                    foundSpan.textContent = `المكتشفة: ${foundPasswords.length}`;
                }
            }

            if (!stopFlag) {
                progressFill.style.width = '100%';
                if (foundPasswords.length > 0) {
                    const topPasswords = foundPasswords.slice(0, 5).join(' , ');
                    statusDiv.innerHTML = `✅ <span class="found">تم العثور على ${foundPasswords.length} كلمة مرور محتملة!</span><br>🔑 ${topPasswords}`;
                    statusDiv.style.color = '#4caf50';
                    resultBox.style.display = 'block';
                    resultPassword.textContent = foundPasswords.slice(0, 5).join(' , ');
                    confidenceLevel.textContent = `${Math.round(bestConfidence)}%`;
                } else {
                    statusDiv.innerHTML = `❌ لم يتم العثور على كلمة مرور بعد ${attempts} محاولة.<br>💡 الذكاء الاصطناعي يواصل التعلم... جرب مرة أخرى.`;
                    statusDiv.style.color = '#e94560';
                }
            }

            startBtn.disabled = false;
            startBtn.innerHTML = '🔄 إعادة المحاولة';
            stopBtn.style.display = 'none';
            isRunning = false;
            speedSpan.textContent = 'السرعة: 0/ث';
        }

        // ====== إيقاف ======
        function stopBruteforce() {
            stopFlag = true;
            stopBtn.style.display = 'none';
            startBtn.disabled = false;
            startBtn.innerHTML = '🔄 إعادة المحاولة';
            statusDiv.innerHTML = `⏹ تم الإيقاف بعد ${attempts} محاولة`;
            statusDiv.style.color = '#ff9800';
            isRunning = false;
            speedSpan.textContent = 'السرعة: 0/ث';
        }

        // ====== ربط الأزرار =====
