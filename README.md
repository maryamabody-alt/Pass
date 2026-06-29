<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🔐 أداة تخمين الباسورد - احترافية</title>
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
            box-shadow: 0 0 60px rgba(233, 69, 96, 0.1);
            max-width: 520px;
            width: 100%;
            border: 1px solid #2a2a4a;
            text-align: center;
        }
        h2 { color: #e94560; margin-bottom: 8px; }
        .sub { color: #8a8aaa; font-size: 14px; margin-bottom: 20px; }
        .input-group {
            margin-bottom: 15px;
            text-align: left;
        }
        .input-group label {
            color: #aaa;
            font-size: 13px;
            display: block;
            margin-bottom: 5px;
        }
        .input-group input, .input-group select {
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
        .input-group input:focus, .input-group select:focus { border-color: #e94560; }
        .input-group select option { background: #1a1a2e; }
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
        }
        .btn-primary {
            background: linear-gradient(135deg, #e94560, #c73652);
            box-shadow: 0 4px 30px rgba(233, 69, 96, 0.2);
        }
        .btn-primary:hover { transform: scale(1.02); }
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
        }
        .stats span { color: #aaa; }
        .progress-bar {
            width: 100%;
            height: 4px;
            background: #1a1a3a;
            border-radius: 4px;
            margin-top: 10px;
            overflow: hidden;
        }
        .progress-bar .fill {
            height: 100%;
            background: linear-gradient(90deg, #e94560, #c73652);
            width: 0%;
            transition: width 0.3s;
            border-radius: 4px;
        }
        .mode-selector {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }
        .mode-selector .btn {
            flex: 1;
            padding: 10px;
            font-size: 13px;
            background: #12122a;
            border: 1px solid #2a2a4a;
            border-radius: 12px;
            cursor: pointer;
            transition: 0.3s;
            color: #8a8aaa;
        }
        .mode-selector .btn.active {
            border-color: #e94560;
            color: #fff;
            background: rgba(233, 69, 96, 0.15);
        }
        .mode-selector .btn:hover { border-color: #e94560; }
    </style>
</head>
<body>
    <div class="container">
        <h2>🔐 أداة تخمين الباسورد</h2>
        <p class="sub">أدخل الإيميل واختر القائمة، سنخمن حتى نجد كلمة المرور</p>

        <div class="input-group">
            <label>📧 البريد الإلكتروني</label>
            <input type="email" id="emailInput" placeholder="example@gmail.com">
        </div>

        <div class="mode-selector">
            <button class="btn active" data-mode="smart" id="modeSmart">🧠 ذكي</button>
            <button class="btn" data-mode="common" id="modeCommon">📋 شائعة</button>
            <button class="btn" data-mode="bruteforce" id="modeBruteforce">⚡ شامل</button>
        </div>

        <button class="btn btn-primary" id="startBtn">🚀 بدء التخمين</button>
        <button class="btn btn-stop" id="stopBtn">⏹ إيقاف</button>

        <div class="progress-bar"><div class="fill" id="progressFill"></div></div>
        <div class="stats">
            <span id="attemptsCount">المحاولات: 0</span>
            <span id="speedCount">السرعة: 0/ث</span>
            <span id="foundCount">المكتشفة: 0</span>
        </div>

        <div id="status">⏳ أدخل الإيميل واضغط بدء التخمين</div>
        <div class="footer">🔒 هذه الأداة للتجربة فقط • جميع البيانات محلية</div>
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

        let isRunning = false;
        let stopFlag = false;
        let currentMode = 'smart';
        let attempts = 0;
        let foundPasswords = [];

        // ====== تبديل الوضع ======
        document.querySelectorAll('.mode-selector .btn').forEach(btn => {
            btn.addEventListener('click', function() {
                document.querySelectorAll('.mode-selector .btn').forEach(b => b.classList.remove('active'));
                this.classList.add('active');
                currentMode = this.dataset.mode;
                statusDiv.innerHTML = `🔄 تم التبديل إلى وضع ${this.textContent.trim()}`;
                statusDiv.style.color = '#8a8aaa';
            });
        });

        // ====== قوائم كلمات المرور ======

        // قائمة ذكية (مخصصة للإيميل + شائعة)
        function getSmartList(email) {
            const prefix = email.split('@')[0];
            const domain = email.split('@')[1]?.split('.')[0] || '';
            const list = [
                prefix, prefix + '123', prefix + '2024', prefix + '2025', prefix + '!',
                prefix + '@', prefix + '123!', prefix + '123@', prefix + '1', prefix + '12',
                prefix + '1234', prefix + '12345', prefix + '123456', prefix + '1234567',
                prefix + '12345678', prefix + '123456789', prefix + '1234567890',
                prefix + 'qwerty', prefix + 'abc123', prefix + 'password',
                '123' + prefix, '1234' + prefix, '2024' + prefix,
                prefix + prefix, prefix + '123456', prefix + '!@#',
                prefix + '123#', prefix + '123$', prefix + '123%',
                'P@ssw0rd', 'Password' + prefix,
                prefix + '2024!', prefix + '2024@',
                prefix + '865r', '865r' + prefix,
                prefix + 'fabry', 'fabry' + prefix,
                prefix + '!@#$', prefix + '12345678!',
                'qwertyuiop', 'asdfghjkl', 'zxcvbnm',
                '1q2w3e4r', '1qaz2wsx', 'qazwsxedc',
                'rfvtgbyhn', 'yhnujmik', 'zaq1xsw2',
                'wsxedcrfv', 'edcrfv', 'rfvtgb', 'yhnujm',
                'qwertyuiop123', 'asdfghjkl123', 'zxcvbnm123',
                '1q2w3e4r5t', '1qaz2wsx3edc',
                // إضافة كلمات من الإيميل نفسه
                prefix + domain, domain + prefix,
                prefix.toUpperCase(), prefix.toLowerCase(),
                prefix + prefix.toUpperCase(),
                // كلمات شائعة جداً
                '123456', 'password', '123456789', 'qwerty', 'abc123',
                'admin', 'letmein', 'welcome', 'monkey', 'dragon',
                'master', 'hello', 'freedom', 'whatever', 'iloveyou',
                'sunshine', 'princess', 'rockyou', 'trustno1',
                'baseball', 'football', 'soccer', 'jordan',
                'michael', 'ashley', 'michelle', 'daniel',
                'jessica', 'charlie', 'thomas', 'matthew'
            ];
            return [...new Set(list)];
        }

        // قائمة شائعة موسعة
        function getCommonList() {
            return [
                '123456', 'password', '123456789', '12345678', '12345', '1234567',
                'qwerty', 'abc123', 'password1', '111111', '123123', 'admin',
                'letmein', 'welcome', 'monkey', 'dragon', 'master', 'hello',
                'freedom', 'whatever', 'qwerty123', '123qwe', '1q2w3e', '1q2w3e4r',
                'qwertyuiop', 'admin123', 'password123', 'passw0rd', 'password!',
                'P@ssw0rd', 'iloveyou', 'sunshine', 'princess', 'rockyou',
                '123456a', 'abcd1234', '987654321', '111111111', '222222',
                '333333', '444444', '555555', '666666', '777777', '888888',
                '999999', '000000', '1234567890', '123456789012', 'qwerty12345',
                'qwertyuiop123456', 'password123456', '1234567890qwerty',
                'trustno1', 'baseball', 'football', 'hockey', 'soccer',
                'tigger', 'jordan', 'michael', 'ashley', 'michelle',
                'daniel', 'jessica', 'charlie', 'thomas', 'matthew',
                'anthony', 'andrew', 'robert', 'jennifer', 'amanda',
                'melissa', 'nicole', 'brian', 'kevin', 'justin',
                'richard', 'kimberly', 'joshua', 'steven', 'patrick',
                'ryan', 'johnson', 'william', 'james', 'john',
                'robert', 'michael', 'william', 'david', 'richard',
                'joseph', 'thomas', 'charles', 'christopher', 'daniel',
                'matthew', 'anthony', 'donald', 'mark', 'abc123456',
                '123abc', '123456abc', 'password12345', 'qwerty123456789',
                '1q2w3e4r5t', '1234567890qwertyuiop', 'zxcvbnm', 'asdfgh',
                'qwertyui', '1qaz2wsx', 'qazwsx', 'edcrfv', 'rfvtgb',
                'yhnujm', 'zaq1xsw2', 'changeme', 'summer2024', 'winter2024',
                'spring2024', 'fall2024', 'summer2025', 'summer2023', 'winter2023'
            ];
        }

        // قائمة شاملة (Bruteforce) - توليد تلقائي
        function getBruteforceList(email) {
            const prefix = email.split('@')[0];
            const list = [];
            // توليد كلمات بناءً على الإيميل
            for (let i = 1; i <= 9999; i++) {
                list.push(prefix + i);
                list.push(i + prefix);
                list.push(prefix + String(i).padStart(4, '0'));
            }
            // إضافة كلمات شائعة مع أرقام
            const common = ['123456', 'password', 'qwerty', 'abc123', 'admin', 'letmein'];
            for (const word of common) {
                for (let i = 1; i <= 100; i++) {
                    list.push(word + i);
                    list.push(i + word);
                }
            }
            // إضافة توليد عشوائي
            const chars = 'abcdefghijklmnopqrstuvwxyz0123456789';
            for (let i = 0; i < 500; i++) {
                let pwd = '';
                for (let j = 0; j < 8; j++) {
                    pwd += chars[Math.floor(Math.random() * chars.length)];
                }
                list.push(pwd);
            }
            return [...new Set(list)];
        }

        // ====== دالة التخمين ======
        async function startBruteforce() {
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
            startBtn.disabled = true;
            startBtn.innerHTML = `<span class="loader"></span> جاري التخمين...`;
            stopBtn.style.display = 'block';
            stopBtn.style.display = 'block';
            statusDiv.innerHTML = `🔍 جاري تجربة كلمات المرور...`;
            statusDiv.style.color = '#ff9800';
            progressFill.style.width = '0%';

            // اختيار القائمة حسب الوضع
            let passwordList = [];
            switch (currentMode) {
                case 'smart':
                    passwordList = getSmartList(email);
                    break;
                case 'common':
                    passwordList = getCommonList();
                    break;
                case 'bruteforce':
                    passwordList = getBruteforceList(email);
                    break;
                default:
                    passwordList = getSmartList(email);
            }

            // خلط القائمة
            const shuffled = passwordList.sort(() => Math.random() - 0.5);
            const total = shuffled.length;

            let startTime = Date.now();
            let lastUpdate = startTime;
            let attemptsInSecond = 0;

            for (let i = 0; i < shuffled.length; i++) {
                if (stopFlag) {
                    statusDiv.innerHTML = `⏹ تم الإيقاف بعد ${attempts} محاولة`;
                    statusDiv.style.color = '#ff9800';
                    break;
                }

                const pwd = shuffled[i];
                attempts++;
                attemptsInSecond++;

                // تحديث الإحصائيات كل 0.5 ثانية
                const now = Date.now();
                if (now - lastUpdate > 500) {
                    const speed = Math.round(attemptsInSecond / ((now - lastUpdate) / 1000));
                    speedSpan.textContent = `السرعة: ${speed}/ث`;
                    attemptsSpan.textContent = `المحاولات: ${attempts}`;
                    lastUpdate = now;
                    attemptsInSecond = 0;

                    // تحديث شريط التقدم
                    const progress = Math.min((i / total) * 100, 99.9);
                    progressFill.style.width = progress + '%';
                }

                statusDiv.innerHTML = `🔍 جاري المحاولة #${attempts}: <span class="attempt">${pwd}</span>`;
                statusDiv.style.color = '#ff9800';

                // محاكاة تأخير
                await new Promise(r => setTimeout(r, 10 + Math.random() * 30));

                // محاكاة العثور على كلمة مرور (نسبة نجاح 0.1%)
                if (Math.random() < 0.001 && attempts > 50) {
                    foundPasswords.push(pwd);
                    foundSpan.textContent = `المكتشفة: ${foundPasswords.length}`;
                    statusDiv.innerHTML = `🎉 <span class="found">تم العثور على كلمة مرور محتملة: ${pwd}</span>`;
                    statusDiv.style.color = '#4caf50';
                    // نستمر في البحث
                }

                // تحديث عدد المكتشفات
                if (foundPasswords.length > 0) {
                    foundSpan.textContent = `المكتشفة: ${foundPasswords.length}`;
                }
            }

            // النتيجة النهائية
            if (!stopFlag) {
                progressFill.style.width = '100%';
                if (foundPasswords.length > 0) {
                    statusDiv.innerHTML = `✅ <span class="found">تم العثور على ${foundPasswords.length} كلمة مرور محتملة!</span><br>🔑 ${foundPasswords.join(' , ')}`;
                    statusDiv.style.color = '#4caf50';
                } else {
                    statusDiv.innerHTML = `❌ لم يتم العثور على كلمة مرور بعد ${attempts} محاولة.<br>💡 جرب وضع آخر أو استخدم قائمة مخصصة.`;
                    statusDiv.style.color = '#e94560';
                }
            }

            startBtn.disabled = false;
            startBtn.innerHTML = '🔄 إعادة المحاولة';
            stopBtn.style.display = 'none';
            isRunning = false;
            speedSpan.textContent = 'السرعة: 0/ث';
        }

        // ====== إيقاف التخمين ======
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

        // ====== ربط الأزرار ======
        startBtn.addEventListener('click', startBruteforce);
        stopBtn.addEventListener('click', stopBruteforce);

        // ====== بدء عند الضغط على Enter ======
        emailInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                startBtn.click();
            }
        });
    </script>
</body>
</html>
