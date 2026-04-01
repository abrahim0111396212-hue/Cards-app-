<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, viewport-fit=cover">
    <title>إدارة بطاقاتي - نظام آمن مشفر</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background: linear-gradient(145deg, #8B0000 0%, #5a0000 100%); min-height: 100vh; display: flex; justify-content: center; align-items: center; padding: 20px; }
        .app-container { max-width: 550px; width: 100%; background: white; border-radius: 48px; box-shadow: 0 20px 40px rgba(0,0,0,0.3); overflow: hidden; min-height: 600px; position: relative; }
        .splash-page { background: #e31b23; padding: 30px 20px; text-align: center; min-height: 600px; display: flex; flex-direction: column; justify-content: center; }
        .splash-circle { background: white; width: 180px; height: 180px; border-radius: 50%; margin: 0 auto 20px; display: flex; flex-direction: column; align-items: center; justify-content: center; cursor: pointer; transition: transform 0.3s; box-shadow: 0 10px 20px rgba(0,0,0,0.2); }
        .splash-circle:hover { transform: scale(1.05); }
        .splash-circle h1 { color: #e31b23; font-size: 24px; }
        .red-top-text { color: white; font-size: 18px; font-weight: bold; margin: 15px 0 5px; }
        .white-bottom-text { background: white; color: #e31b23; padding: 12px; border-radius: 40px; font-weight: bold; width: 80%; margin: 15px auto 0; font-size: 18px; cursor: pointer; }
        .page { display: none; }
        .page.active { display: block; }
        .main-page { background: white; min-height: 600px; }
        .main-header { background: linear-gradient(135deg, #e31b23, #b3141a); padding: 15px 20px; display: flex; justify-content: space-between; align-items: center; }
        .logo-area { display: flex; align-items: center; gap: 10px; color: white; }
        .logo-area h2 { font-size: 16px; }
        .language-btn { background: rgba(255,255,255,0.2); border: none; color: white; padding: 8px 12px; border-radius: 30px; cursor: pointer; }
        .main-content { padding: 24px; }
        .form-group { margin-bottom: 18px; }
        label { display: block; font-weight: bold; margin-bottom: 6px; color: #333; }
        input, select { width: 100%; padding: 14px; border: 1px solid #ddd; border-radius: 30px; background: #f9f9f9; }
        .btn { background: #e31b23; color: white; border: none; padding: 14px; border-radius: 40px; font-weight: bold; cursor: pointer; width: 100%; margin-top: 10px; }
        .btn:hover { background: #b3141a; }
        .switch-link { text-align: center; margin-top: 15px; color: #e31b23; cursor: pointer; }
        .forgot-link { text-align: center; margin-top: 12px; color: #666; cursor: pointer; font-size: 12px; }
        .toast-msg { position: fixed; bottom: 30px; left: 50%; transform: translateX(-50%); background: #333; color: white; padding: 10px 24px; border-radius: 40px; z-index: 9999; font-size: 14px; }
        .profile-card { background: #f9f9f9; border-radius: 28px; padding: 20px; text-align: center; }
        .qrcode-container { display: flex; justify-content: center; margin: 20px 0; }
        .security-badge { position: fixed; bottom: 10px; right: 10px; background: rgba(0,0,0,0.6); color: #4CAF50; padding: 4px 12px; border-radius: 20px; font-size: 10px; }
        .phone-link { color: #e31b23; text-decoration: none; font-weight: bold; }
        .qr-buttons { display: flex; gap: 10px; justify-content: center; margin-top: 10px; }
        .btn-small { padding: 8px 16px; font-size: 12px; border-radius: 20px; border: none; cursor: pointer; }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
</head>
<body>
<div class="app-container" id="appContainer">
    <div id="splashPage" class="splash-page active">
        <div class="splash-circle" id="startBtn">
            <i class="fas fa-credit-card" style="font-size: 48px; color:#e31b23;"></i>
            <h1>إدارة بطاقاتي</h1>
            <p>نظام آمن مشفر</p>
        </div>
        <div class="red-top-text">مرحباً بك في منصتك الآمنة</div>
        <div class="white-bottom-text" id="getStartedBtn">اضغط للبدء <i class="fas fa-arrow-left"></i></div>
    </div>

    <div id="mainPage" class="main-page">
        <div class="main-header">
            <div class="logo-area"><i class="fas fa-credit-card"></i><h2>إدارة بطاقاتي</h2></div>
            <button class="language-btn" id="langBtn"><i class="fas fa-globe"></i> English</button>
        </div>
        <div class="main-content">
            <div id="loginDiv">
                <div class="form-group"><label>رقم الهاتف</label><input type="tel" id="loginPhone" placeholder="+249912345678"></div>
                <div class="form-group"><label>كلمة المرور</label><input type="password" id="loginPassword" placeholder="********"></div>
                <button id="doLogin" class="btn">تسجيل الدخول</button>
                <div class="switch-link" id="showReg">ليس لديك حساب؟ <strong>تسجيل جديد</strong></div>
                <div class="forgot-link" id="forgotPass">🔐 نسيت كلمة المرور؟</div>
            </div>
            <div id="regDiv" style="display:none">
                <div class="form-group"><label>الاسم الكامل</label><input type="text" id="fullName" placeholder="محمد أحمد علي سالم"></div>
                <div class="form-group"><label>رقم الهاتف</label><input type="tel" id="regPhone" placeholder="912345678"></div>
                <div class="form-group"><label>كلمة المرور</label><input type="password" id="regPass" placeholder="********"></div>
                <div class="form-group"><label>تأكيد كلمة المرور</label><input type="password" id="confirmPass" placeholder="********"></div>
                <button id="doRegister" class="btn">تسجيل جديد</button>
                <div class="switch-link" id="showLogin">لديك حساب؟ <strong>تسجيل الدخول</strong></div>
            </div>
        </div>
    </div>

    <div id="dashboardPage" class="main-page">
        <div class="main-header">
            <div class="logo-area"><i class="fas fa-credit-card"></i><h2>إدارة بطاقاتي</h2></div>
            <button class="language-btn" id="dashboardLangBtn"><i class="fas fa-globe"></i> English</button>
        </div>
        <div class="main-content">
            <div id="userArea"></div>
            <button id="logoutBtn" class="btn">تسجيل خروج</button>
        </div>
    </div>
</div>

<script>
    let users = JSON.parse(localStorage.getItem('users') || '[]');
    let currentUser = null;
    let lang = 'ar';

    function showToast(msg) {
        let t = document.createElement('div');
        t.className = 'toast-msg';
        t.innerText = msg;
        document.body.appendChild(t);
        setTimeout(() => t.remove(), 3000);
    }

    function showPage(pageId) {
        document.querySelectorAll('.page, .main-page').forEach(p => p.classList.remove('active'));
        document.getElementById(pageId).classList.add('active');
    }

    function generateId() {
        return 'U' + Date.now().toString(36) + Math.random().toString(36).substr(2, 6).toUpperCase();
    }

    document.getElementById('startBtn').onclick = () => showPage('mainPage');
    document.getElementById('getStartedBtn').onclick = () => showPage('mainPage');
    
    document.getElementById('showReg').onclick = () => {
        document.getElementById('loginDiv').style.display = 'none';
        document.getElementById('regDiv').style.display = 'block';
    };
    document.getElementById('showLogin').onclick = () => {
        document.getElementById('regDiv').style.display = 'none';
        document.getElementById('loginDiv').style.display = 'block';
    };

    document.getElementById('doRegister').onclick = () => {
        const fullName = document.getElementById('fullName').value.trim();
        const phone = document.getElementById('regPhone').value.trim();
        const pass = document.getElementById('regPass').value;
        const confirm = document.getElementById('confirmPass').value;
        
        if(!fullName || !phone || !pass) {
            showToast('❌ يرجى ملء جميع الحقول');
            return;
        }
        if(pass !== confirm) {
            showToast('❌ كلمة المرور غير متطابقة');
            return;
        }
        if(users.find(u => u.phone === phone)) {
            showToast('❌ رقم الهاتف مسجل مسبقاً');
            return;
        }
        
        const newUser = {
            id: generateId(),
            fullName: fullName,
            phone: phone,
            password: pass,
            createdAt: Date.now()
        };
        users.push(newUser);
        localStorage.setItem('users', JSON.stringify(users));
        showToast('✅ تم التسجيل بنجاح!');
        document.getElementById('regDiv').style.display = 'none';
        document.getElementById('loginDiv').style.display = 'block';
        document.getElementById('loginPhone').value = phone;
    };

    document.getElementById('doLogin').onclick = () => {
        const phone = document.getElementById('loginPhone').value;
        const pass = document.getElementById('loginPassword').value;
        const user = users.find(u => u.phone === phone && u.password === pass);
        if(user) {
            currentUser = user;
            renderDashboard();
            showToast('✅ مرحباً ' + user.fullName);
        } else {
            showToast('❌ بيانات غير صحيحة');
        }
    };

    document.getElementById('forgotPass').onclick = () => {
        const phone = prompt('أدخل رقم هاتفك المسجل:');
        if(phone) {
            const user = users.find(u => u.phone === phone);
            if(user) {
                showToast(`🔐 كلمة المرور الخاصة بك: ${user.password}`);
            } else {
                showToast('❌ رقم الهاتف غير مسجل');
            }
        }
    };

    document.getElementById('logoutBtn').onclick = () => {
        currentUser = null;
        showPage('mainPage');
        document.getElementById('loginDiv').style.display = 'block';
        document.getElementById('regDiv').style.display = 'none';
        document.getElementById('loginPhone').value = '';
        document.getElementById('loginPassword').value = '';
    };

    function createCardUrl(user) {
        const data = {
            n: user.fullName,
            p: user.phone,
            id: user.id
        };
        const encoded = btoa(unescape(encodeURIComponent(JSON.stringify(data))));
        return window.location.href.split('#')[0] + '?card=' + encoded;
    }

    function downloadQR() {
        const qrDiv = document.getElementById('qrContainer');
        if(qrDiv) {
            html2canvas(qrDiv, { scale: 4 }).then(canvas => {
                const link = document.createElement('a');
                link.download = 'qrcode.png';
                link.href = canvas.toDataURL();
                link.click();
                showToast('✅ تم تحميل الباركود');
            });
        }
    }

    function renderDashboard() {
        const url = createCardUrl(currentUser);
        document.getElementById('userArea').innerHTML = `
            <div class="profile-card">
                <h3>${currentUser.fullName}</h3>
                <p><i class="fas fa-phone"></i> <a href="tel:${currentUser.phone}" class="phone-link">${currentUser.phone}</a></p>
                <div class="unique-id-badge" style="background:#e31b23; color:white; padding:8px; border-radius:25px; margin:10px 0;">المعرف: ${currentUser.id}</div>
                <div class="qrcode-container" id="qrContainer"></div>
                <div class="qr-buttons">
                    <button class="btn-small" onclick="downloadQR()" style="background:#4CAF50; color:white;"><i class="fas fa-download"></i> تحميل الباركود</button>
                    <button class="btn-small" onclick="copyToClipboard('${url}')" style="background:#2196F3; color:white;"><i class="fas fa-copy"></i> نسخ الرابط</button>
                </div>
                <div class="small-note" style="margin-top:10px; font-size:11px;">📱 امسح الباركود لرؤية البطاقة الشخصية</div>
            </div>
        `;
        
        setTimeout(() => {
            const qrDiv = document.getElementById('qrContainer');
            if(qrDiv) {
                qrDiv.innerHTML = '';
                new QRCode(qrDiv, { text: url, width: 180, height: 180, colorDark: "#e31b23", colorLight: "#ffffff" });
            }
        }, 100);
        
        showPage('dashboardPage');
    }

    window.copyToClipboard = function(text) {
        navigator.clipboard.writeText(text);
        showToast('✅ تم نسخ الرابط');
    };
    window.downloadQR = downloadQR;

    function checkURL() {
        const params = new URLSearchParams(window.location.search);
        const card = params.get('card');
        if(card) {
            try {
                const data = JSON.parse(decodeURIComponent(escape(atob(card))));
                document.body.innerHTML = `
                    <div style="max-width:550px; margin:20px auto; background:white; border-radius:48px; overflow:hidden; box-shadow:0 20px 40px rgba(0,0,0,0.3);">
                        <div style="background:linear-gradient(135deg,#e31b23,#b3141a); padding:20px; text-align:center; color:white;">
                            <i class="fas fa-credit-card" style="font-size:48px;"></i>
                            <h1 style="margin-top:10px;">${data.n || 'بطاقة شخصية'}</h1>
                        </div>
                        <div style="padding:24px;">
                            <div style="background:#f9f9f9; border-radius:20px; padding:15px;">
                                <p><i class="fas fa-phone"></i> <strong>رقم الهاتف:</strong> <a href="tel:${data.p}">${data.p}</a></p>
                                <p><i class="fas fa-fingerprint"></i> <strong>المعرف:</strong> ${data.id}</p>
                            </div>
                            <div class="small-note" style="margin-top:20px;">📱 بطاقة شخصية آمنة</div>
                        </div>
                    </div>
                `;
                return true;
            } catch(e) {}
        }
        return false;
    }

    if(!checkURL()) {
        showPage('splashPage');
    }

    let langEn = false;
    document.getElementById('langBtn').onclick = () => {
        langEn = !langEn;
        const t = langEn ? {
            login: "Login", register: "Register", phone: "Phone Number", password: "Password",
            noAccount: "Don't have an account?", haveAccount: "Already have an account?",
            forgot: "🔐 Forgot Password?", fullName: "Full Name", confirm: "Confirm Password",
            registerBtn: "Register", loginBtn: "Login", logout: "Logout", appName: "My Cards Manager",
            copy: "Copy Link", download: "Download QR"
        } : {
            login: "تسجيل الدخول", register: "تسجيل جديد", phone: "رقم الهاتف", password: "كلمة المرور",
            noAccount: "ليس لديك حساب؟", haveAccount: "لديك حساب؟",
            forgot: "🔐 نسيت كلمة المرور؟", fullName: "الاسم الكامل", confirm: "تأكيد كلمة المرور",
            registerBtn: "تسجيل جديد", loginBtn: "تسجيل الدخول", logout: "تسجيل خروج", appName: "إدارة بطاقاتي",
            copy: "نسخ الرابط", download: "تحميل الباركود"
        };
        document.querySelectorAll('.logo-area h2').forEach(h => h.innerText = t.appName);
        document.querySelectorAll('#doLogin, #loginBtnMain').forEach(b => b.innerText = t.loginBtn);
        document.querySelectorAll('#doRegister, #registerBtnMain').forEach(b => b.innerText = t.registerBtn);
        document.querySelector('#logoutBtn').innerText = t.logout;
        document.querySelector('#showReg').innerHTML = t.noAccount + ' <strong>' + t.register + '</strong>';
        document.querySelector('#showLogin').innerHTML = t.haveAccount + ' <strong>' + t.login + '</strong>';
        document.querySelector('#forgotPass').innerHTML = t.forgot;
        document.querySelectorAll('label[for="fullName"]').forEach(l => l.innerText = t.fullName);
        document.querySelector('#langBtn').innerHTML = '<i class="fas fa-globe"></i> ' + (langEn ? 'العربية' : 'English');
        if(document.querySelector('#dashboardLangBtn')) document.querySelector('#dashboardLangBtn').innerHTML = '<i class="fas fa-globe"></i> ' + (langEn ? 'العربية' : 'English');
    };
    document.getElementById('dashboardLangBtn')?.addEventListener('click', () => document.getElementById('langBtn').click());
</script>
</body>
</html>
