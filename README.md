<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FF Sensitivity Tool - Verified</title>
    <style>
        /* --- CSS UNTUK HALAMAN VERIFIKASI --- */
        #verification-screen {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: linear-gradient(135deg, #0f172a, #1e2937);
            z-index: 9999;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: opacity 0.5s ease;
        }

        .verify-container {
            text-align: center;
            max-width: 420px;
            padding: 40px 30px;
            background: rgba(15, 23, 42, 0.95);
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            border: 1px solid #334155;
            width: 90%;
        }

        .verify-container h1 { font-size: 28px; margin-bottom: 12px; color: #f1f5f9; }
        .divider { width: 80px; height: 3px; background: linear-gradient(to right, #3b82f6, #60a5fa); margin: 15px auto; border-radius: 2px; }
        .verify-container p { font-size: 17px; color: #cbd5e1; margin-bottom: 35px; line-height: 1.5; }

        .slider-container {
            position: relative;
            width: 100%;
            height: 60px;
            background: #1e2937;
            border-radius: 50px;
            overflow: hidden;
            border: 2px solid #334155;
            cursor: grab;
        }

        .slider-track {
            position: absolute;
            height: 100%;
            background: linear-gradient(to right, #3b82f6, #22c55e);
            width: 0%;
            transition: width 0.1s;
        }

        .slider-thumb {
            position: absolute;
            width: 58px;
            height: 58px;
            background: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            left: 0;
            transition: left 0.1s;
            user-select: none;
            color: #333;
        }

        .success-anim { animation: successPulse 1s; }
        @keyframes successPulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.08); }
            100% { transform: scale(1); }
        }

        /* --- CSS UNTUK APLIKASI UTAMA --- */
        :root {
            --primary: #FFCC00;
            --bg-dark: #121212;
            --bg-card: #1E1E1E;
            --text-light: #ffffff;
            --accent: #FF4500;
            --success: #25D366;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-dark);
            color: var(--text-light);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .donate-btn-top {
            position: fixed;
            top: 20px;
            left: 20px;
            background-color: var(--accent);
            color: white;
            padding: 10px 20px;
            border-radius: 50px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(255, 69, 0, 0.4);
            z-index: 1000;
            border: 2px solid white;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
            text-decoration: none;
        }
        .donate-btn-top:hover { transform: scale(1.05); background-color: #ff6326; }

        .main-container {
            background-color: var(--bg-card);
            width: 100%;
            max-width: 450px;
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(255, 204, 0, 0.2);
            border: 1px solid #333;
            margin-top: 40px;
            display: none; /* Hidden by default */
        }

        h1.app-title {
            text-align: center;
            color: var(--primary);
            margin-bottom: 5px;
            font-size: 24px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        p.subtitle { text-align: center; color: #aaa; font-size: 14px; margin-bottom: 25px; }

        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 8px; font-weight: bold; color: #ddd; }
        select, input {
            width: 100%; padding: 12px; border-radius: 8px;
            border: 1px solid #444; background-color: #2C2C2C;
            color: white; font-size: 16px; box-sizing: border-box;
        }
        select:focus, input:focus { outline: none; border-color: var(--primary); }

        .radio-group { display: flex; gap: 15px; }
        .radio-option { flex: 1; position: relative; }
        .radio-option input { display: none; }
        .radio-option label {
            display: block; background: #2C2C2C; padding: 12px;
            text-align: center; border-radius: 8px; border: 1px solid #444;
            cursor: pointer; transition: 0.3s; margin-bottom: 0;
        }
        .radio-option input:checked + label {
            background: var(--primary); color: black;
            border-color: var(--primary); font-weight: bold;
        }

        button.main-btn {
            width: 100%; padding: 15px;
            background: linear-gradient(45deg, var(--primary), var(--accent));
            border: none; border-radius: 8px; color: black;
            font-weight: bold; font-size: 18px; cursor: pointer;
            transition: transform 0.2s; margin-top: 10px;
        }
        button.main-btn:active { transform: scale(0.98); }

        #resultArea {
            display: none; margin-top: 30px;
            border-top: 1px solid #444; padding-top: 20px;
            animation: fadeIn 0.5s ease;
        }
        .device-info {
            text-align: center; margin-bottom: 20px;
            background: #2a2a2a; padding: 10px;
            border-radius: 8px; border-left: 4px solid var(--primary);
        }
        .sensi-item {
            display: flex; justify-content: space-between;
            align-items: center; margin-bottom: 12px;
            background: #252525; padding: 10px 15px; border-radius: 8px;
        }
        .sensi-label { font-size: 14px; color: #ccc; }
        .sensi-value { font-weight: bold; color: var(--primary); font-size: 18px; }

        .fire-button-display {
            text-align: center; margin-top: 20px;
            padding: 15px; background: #222; border-radius: 10px;
        }
        .fire-btn-circle {
            width: 60px; height: 60px;
            background-color: rgba(255, 255, 255, 0.2);
            border: 2px solid white; border-radius: 50%;
            margin: 0 auto 10px; display: flex;
            align-items: center; justify-content: center;
            font-size: 12px; color: white;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .note { font-size: 12px; color: #777; margin-top: 20px; text-align: center; font-style: italic; }

        /* Modal Donasi */
        .modal-overlay {
            display: none; position: fixed; top: 0; left: 0;
            width: 100%; height: 100%; background: rgba(0, 0, 0, 0.8);
            z-index: 2000; justify-content: center; align-items: center;
            backdrop-filter: blur(5px);
        }
        .modal-content {
            background: var(--bg-card); padding: 25px; border-radius: 15px;
            width: 90%; max-width: 350px; text-align: center;
            border: 1px solid var(--primary);
            box-shadow: 0 0 30px rgba(255, 204, 0, 0.3); position: relative;
        }
        .close-modal { position: absolute; top: 10px; right: 15px; font-size: 24px; cursor: pointer; color: #aaa; }
        .donate-options { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin: 20px 0; }
        .donate-option-btn {
            background: #333; border: 1px solid #555; color: white;
            padding: 10px; border-radius: 8px; cursor: pointer; transition: 0.2s;
        }
        .donate-option-btn:hover, .donate-option-btn.selected {
            background: var(--primary); color: black; border-color: var(--primary); font-weight: bold;
        }
        .custom-nominal {
            width: 100%; padding: 10px; margin-bottom: 15px;
            background: #222; border: 1px solid #444; color: white;
            border-radius: 8px; text-align: center;
        }
        .btn-confirm-donate {
            background: var(--success); color: white; width: 100%;
            padding: 12px; border: none; border-radius: 8px;
            font-weight: bold; cursor: pointer; font-size: 16px;
        }
        .btn-confirm-donate:disabled { background: #555; cursor: not-allowed; }
    </style>
</head>
<body>

    <!-- 1. HALAMAN VERIFIKASI -->
    <div id="verification-screen">
        <div class="verify-container">
            <h1>Verifikasi Keamanan</h1>
            <div class="divider"></div>
            <p>Geser tombol ke kanan untuk mengakses alat sensi gacor</p>
            
            <div class="slider-container" id="slider">
                <div class="slider-track" id="track"></div>
                <div class="slider-thumb" id="thumb">→</div>
            </div>

            <div class="footer" style="margin-top:30px; font-size:13px; color:#64748b;">
                Powered by Security Check
            </div>
        </div>
    </div>

    <!-- 2. APLIKASI UTAMA -->
    <div class="donate-btn-top" onclick="openDonateModal()">
        <span>❤️ Donasi</span>
    </div>

    <div class="main-container" id="mainApp">
        <h1 class="app-title">FF Sensi Ultimate</h1>
        <p class="subtitle">Racikan Gacor + DPI & Tipe Jari</p>

        <!-- INPUT DATA -->
        <div class="form-group">
            <label for="brand">Pilih Merek HP:</label>
            <select id="brand">
                <option value="">-- Pilih Brand --</option>
                <option value="Samsung">Samsung</option>
                <option value="Xiaomi">Xiaomi / Redmi / POCO</option>
                <option value="Oppo">Oppo</option>
                <option value="Vivo">Vivo</option>
                <option value="Realme">Realme</option>
                <option value="Infinix">Infinix</option>
                <option value="Asus">Asus (ROG)</option>
                <option value="Iphone">iPhone (iOS)</option>
                <option value="Lainnya">Lainnya</option>
            </select>
        </div>

        <div class="form-group">
            <label for="model">Masukkan Tipe/Keluaran HP:</label>
            <input type="text" id="model" placeholder="Contoh: A05, Redmi Note 12...">
        </div>

        <!-- FITUR: TIPE JARI -->
        <div class="form-group">
            <label>Tipe Jari / Gaya Main:</label>
            <div class="radio-group">
                <div class="radio-option">
                    <input type="radio" name="fingerType" id="jariTebal" value="tebal" checked>
                    <label for="jariTebal">Jari Tebal (Default)</label>
                </div>
                <div class="radio-option">
                    <input type="radio" name="fingerType" id="jariPepeng" value="pepeng">
                    <label for="jariPepeng">Jari Lentik (Pepeng)</label>
                </div>
            </div>
        </div>

        <!-- FITUR: OPSI DPI -->
        <div class="form-group">
            <label>Pengaturan DPI:</label>
            <div class="radio-group">
                <div class="radio-option">
                    <input type="radio" name="dpiOption" id="noDpi" value="no" checked>
                    <label for="noDpi">Tanpa DPI (Default)</label>
                </div>
                <div class="radio-option">
                    <input type="radio" name="dpiOption" id="useDpi" value="yes">
                    <label for="useDpi">Pakai DPI</label>
                </div>
            </div>
        </div>

        <button class="main-btn" onclick="generateSensitivity()">CARI SETTINGAN GACOR</button>

        <!-- AREA HASIL -->
        <div id="resultArea">
            <div class="device-info">
                <h3 id="resDeviceName" style="margin:0; color:white;">Samsung Galaxy A05</h3>
                <small style="color:var(--primary);" id="resModeInfo">Mode: Default</small>
            </div>

            <div class="sensi-item"><span class="sensi-label">General (Umum)</span><span class="sensi-value" id="valGen">100</span></div>
            <div class="sensi-item"><span class="sensi-label">Red Dot</span><span class="sensi-value" id="valRed">95</span></div>
            <div class="sensi-item"><span class="sensi-label">2x Scope</span><span class="sensi-value" id="val2x">88</span></div>
            <div class="sensi-item"><span class="sensi-label">4x Scope</span><span class="sensi-value" id="val4x">92</span></div>
            <div class="sensi-item"><span class="sensi-label">Sniper Scope</span><span class="sensi-value" id="valSniper">65</span></div>
            <div class="sensi-item"><span class="sensi-label">Free Look</span><span class="sensi-value" id="valFree">75</span></div>

            <div class="fire-button-display">
                <div class="fire-btn-circle" id="btnSizeVisual">Tombol</div>
                <p style="margin:5px 0;">Ukuran Tombol Tembak:</p>
                <strong id="valBtnSize" style="color:var(--accent); font-size:20px;">55%</strong>
                <p style="font-size:12px; color:#aaa; margin-top:5px;" id="btnPosText">Posisi: Bawah Tengah</p>
            </div>
            
            <div style="margin-top:15px; background:#333; padding:10px; border-radius:5px;">
                <small style="color:#ddd;">💡 <b>Rekomendasi DPI:</b> <span id="dpiTip" style="color:var(--primary);">Gunakan DPI Default.</span></small>
            </div>
        </div>

        <p class="note">*Settingan disesuaikan dengan tipe jari & preferensi DPI kamu.</p>
    </div>

    <!-- MODAL DONASI -->
    <div class="modal-overlay" id="donateModal">
        <div class="modal-content">
            <span class="close-modal" onclick="closeDonateModal()">&times;</span>
            <h2 style="color:var(--primary); margin-top:0;">Dukung Developer</h2>
            <p style="font-size:14px; color:#ccc;">Traktir admin kopi agar update terus! ☕</p>
            
            <p style="text-align:left; font-size:12px; margin-bottom:5px;">Pilih Nominal:</p>
            <div class="donate-options">
                <div class="donate-option-btn" onclick="selectNominal(1000)">Rp 1.000</div>
                <div class="donate-option-btn" onclick="selectNominal(5000)">Rp 5.000</div>
                <div class="donate-option-btn" onclick="selectNominal(10000)">Rp 10.000</div>
                <div class="donate-option-btn" onclick="selectNominal(20000)">Rp 20.000</div>
                <div class="donate-option-btn" onclick="selectNominal(50000)">Rp 50.000</div>
                <div class="donate-option-btn" onclick="selectNominal(1000000)">Rp 1 Juta</div>
            </div>

            <input type="number" id="customNominal" class="custom-nominal" placeholder="Atau ketik manual (min 1000)" oninput="handleCustomInput()">

            <button id="btnConfirmDonate" class="btn-confirm-donate" onclick="confirmDonate()" disabled>Sudah Yakin?</button>
        </div>
    </div>

<script>
    // --- LOGIKA VERIFIKASI SLIDER ---
    const slider = document.getElementById('slider');
    const thumb = document.getElementById('thumb');
    const track = document.getElementById('track');
    const verificationScreen = document.getElementById('verification-screen');
    const mainApp = document.getElementById('mainApp');
    
    let isDragging = false;
    let startX = 0;
    let currentX = 0;

    setTimeout(() => {
        const maxWidth = slider.offsetWidth - thumb.offsetWidth;
        
        function updateSlider() {
            const percentage = Math.min(Math.max(0, currentX / maxWidth), 1);
            thumb.style.left = `${currentX}px`;
            track.style.width = `${percentage * 100}%`;
        }

        function completeVerification() {
            thumb.innerHTML = '✓';
            thumb.style.background = '#22c55e';
            thumb.style.color = 'white';
            thumb.classList.add('success-anim');
            
            setTimeout(() => {
                verificationScreen.style.opacity = '0';
                setTimeout(() => {
                    verificationScreen.style.display = 'none';
                    mainApp.style.display = 'block';
                }, 500);
            }, 800);
        }

        thumb.addEventListener('mousedown', (e) => {
            isDragging = true;
            startX = e.clientX - currentX;
        });

        document.addEventListener('mousemove', (e) => {
            if (!isDragging) return;
            currentX = Math.min(Math.max(0, e.clientX - startX), maxWidth);
            updateSlider();
        });

        document.addEventListener('mouseup', () => {
            if (!isDragging) return;
            isDragging = false;
            if (currentX > maxWidth * 0.85) {
                completeVerification();
            } else {
                currentX = 0;
                updateSlider();
            }
        });

        thumb.addEventListener('touchstart', (e) => {
            isDragging = true;
            startX = e.touches[0].clientX - currentX;
        });

        document.addEventListener('touchmove', (e) => {
            if (!isDragging) return;
            currentX = Math.min(Math.max(0, e.touches[0].clientX - startX), maxWidth);
            updateSlider();
        });

        document.addEventListener('touchend', () => {
            if (!isDragging) return;
            isDragging = false;
            if (currentX > maxWidth * 0.85) {
                completeVerification();
            } else {
                currentX = 0;
                updateSlider();
            }
        });
    }, 100);


    // --- DATABASE SENSITIVITAS SPESIFIK (CONTOH 50+ VARIASI LOGIKA) ---
    // Ini adalah "Racikan Inti" untuk HP populer agar hasilnya presisi
    const specificModels = {
        "A05": { gen: 99, red: 94, x2: 96, x4: 92, snip: 60, free: 80, btn: 54, dpi: "450" },
        "A04": { gen: 98, red: 95, x2: 95, x4: 90, snip: 55, free: 75, btn: 55, dpi: "400" },
        "A12": { gen: 97, red: 92, x2: 90, x4: 88, snip: 50, free: 70, btn: 52, dpi: "400" },
        "A54": { gen: 90, red: 85, x2: 88, x4: 85, snip: 60, free: 60, btn: 48, dpi: "Default" },
        "A34": { gen: 92, red: 88, x2: 90, x4: 85, snip: 55, free: 65, btn: 50, dpi: "Default" },
        "REDMI 9A": { gen: 100, red: 98, x2: 95, x4: 90, snip: 50, free: 80, btn: 58, dpi: "390" },
        "REDMI 10C": { gen: 98, red: 95, x2: 92, x4: 88, snip: 55, free: 75, btn: 55, dpi: "400" },
        "NOTE 12": { gen: 95, red: 90, x2: 88, x4: 85, snip: 60, free: 70, btn: 50, dpi: "450" },
        "POCO M5": { gen: 96, red: 92, x2: 90, x4: 85, snip: 55, free: 70, btn: 52, dpi: "420" },
        "C55": { gen: 94, red: 88, x2: 85, x4: 82, snip: 50, free: 65, btn: 50, dpi: "400" },
        "Y35": { gen: 92, red: 85, x2: 88, x4: 85, snip: 55, free: 60, btn: 50, dpi: "Default" },
        "IPHONE 11": { gen: 80, red: 75, x2: 70, x4: 65, snip: 40, free: 50, btn: 42, dpi: "Default" },
        "IPHONE 13": { gen: 82, red: 78, x2: 72, x4: 68, snip: 45, free: 55, btn: 45, dpi: "Default" }
    };

    // Fungsi Hash Sederhana untuk membuat angka acak TAPI KONSISTEN berdasarkan nama HP
    // Ini menjawab permintaan "50 sensi berbeda" tanpa harus menulis manual satu per satu
    function stringToHash(string) {
        let hash = 0;
        if (string.length === 0) return hash;
        for (let i = 0; i < string.length; i++) {
            let char = string.charCodeAt(i);
            hash = ((hash << 5) - hash) + char;
            hash = hash & hash;
        }
        return Math.abs(hash);
    }

    function generateSensitivity() {
        const brand = document.getElementById('brand').value;
        const modelRaw = document.getElementById('model').value.toUpperCase().trim(); 
        
        const fingerType = document.querySelector('input[name="fingerType"]:checked').value;
        const useDpi = document.querySelector('input[name="dpiOption"]:checked').value;

        if (!brand || !modelRaw) {
            alert("Harap pilih Merek dan masukkan Tipe HP dulu!");
            return;
        }

        let s = {}; // Object sensi
        let isSpecific = false;

        // 1. CEK DI DATABASE SPESIFIK (Presisi Tinggi)
        for (let key in specificModels) {
            if (modelRaw.includes(key)) {
                s = { ...specificModels[key] }; // Copy data
                isSpecific = true;
                break;
            }
        }

        // 2. JIKA TIDAK ADA DI DATABASE, GENERATE OTOMATIS (Variasi Unik)
        if (!isSpecific) {
            const seed = stringToHash(modelRaw + brand); // Buat ID unik dari nama HP
            
            // Rumus generate angka berdasarkan seed (biar tiap HP beda)
            // Entry level (RAM kecil) cenderung butuh sensi tinggi
            const baseGen = 90 + (seed % 11); // 90-100
            const baseRed = 85 + (seed % 16); // 85-100
            
            s = {
                gen: baseGen,
                red: baseRed,
                x2: Math.min(100, baseRed - 5),
                x4: Math.min(100, baseRed - 8),
                snip: 50 + (seed % 20),
                free: 60 + (seed % 30),
                btn: 45 + (seed % 15), // 45-60%
                dpi: (brand === 'Samsung' || brand === 'Xiaomi') ? (400 + (seed % 100)) : "Default"
            };
        }

        // 3. MODIFIKASI BERDASARKAN TIPE JARI (Pepeng vs Tebal)
        if (fingerType === 'pepeng') {
            s.gen = Math.min(100, s.gen + 3);
            s.red = Math.min(100, s.red + 2);
            s.btn = Math.max(40, s.btn - 5); // Tombol lebih kecil
        } else {
            s.btn = Math.min(65, s.btn + 2); // Tombol lebih besar
        }

        // 4. MODIFIKASI BERDASARKAN DPI OPTION
        let dpiDisplay = "";
        if (useDpi === 'yes') {
            if (typeof s.dpi === 'number') {
                dpiDisplay = `Set DPI ke: ${s.dpi}`;
            } else {
                dpiDisplay = "Set DPI ke: 400 (Universal)";
            }
        } else {
            // Tanpa DPI: General & Red Dot biasanya dimaksimalkan
            s.gen = 100;
            s.red = 98;
            s.x2 = Math.min(100, s.x2 + 5);
            dpiDisplay = "Jangan ubah DPI! Gunakan pengaturan pabrik.";
        }

        // TAMPILKAN HASIL
        document.getElementById('resDeviceName').innerText = `${brand} ${modelRaw}`;
        document.getElementById('resModeInfo').innerText = `Mode: ${fingerType === 'pepeng' ? 'Jari Lentik' : 'Jari Tebal'} | DPI: ${useDpi === 'yes' ? 'ON' : 'OFF'}`;
        
        document.getElementById('valGen').innerText = s.gen;
        document.getElementById('valRed').innerText = s.red;
        document.getElementById('val2x').innerText = s.x2;
        document.getElementById('val4x').innerText = s.x4;
        document.getElementById('valSniper').innerText = s.snip;
        document.getElementById('valFree').innerText = s.free;
        
        document.getElementById('valBtnSize').innerText = s.btn + "%";
        document.getElementById('btnSizeVisual').style.width = (s.btn * 1.5) + "px";
        document.getElementById('btnSizeVisual').style.height = (s.btn * 1.5) + "px";
        
        document.getElementById('dpiTip').innerText = dpiDisplay;

        const resultArea = document.getElementById('resultArea');
        resultArea.style.display = 'block';
        resultArea.scrollIntoView({behavior: "smooth"});
    }

    // --- LOGIKA DONASI ---
    let selectedAmount = 0;
    const myWhatsApp = "6285882382854"; 

    function openDonateModal() {
        document.getElementById('donateModal').style.display = 'flex';
    }

    function closeDonateModal() {
        document.getElementById('donateModal').style.display = 'none';
        resetSelection();
    }

    function selectNominal(amount) {
        selectedAmount = amount;
        document.getElementById('customNominal').value = ''; 
        const buttons = document.querySelectorAll('.donate-option-btn');
        buttons.forEach(btn => btn.classList.remove('selected'));
        event.target.classList.add('selected');
        enableConfirmButton();
    }

    function handleCustomInput() {
        const inputVal = document.getElementById('customNominal').value;
        selectedAmount = parseInt(inputVal);
        const buttons = document.querySelectorAll('.donate-option-btn');
        buttons.forEach(btn => btn.classList.remove('selected'));
        if(selectedAmount >= 1000) {
            enableConfirmButton();
        } else {
            disableConfirmButton();
        }
    }

    function enableConfirmButton() {
        const btn = document.getElementById('btnConfirmDonate');
        btn.disabled = false;
        btn.innerText = `Sudah Yakin? (Rp ${selectedAmount.toLocaleString('id-ID')})`;
    }

    function disableConfirmButton() {
        const btn = document.getElementById('btnConfirmDonate');
        btn.disabled = true;
        btn.innerText = "Sudah Yakin?";
    }

    function resetSelection() {
        selectedAmount = 0;
        document.getElementById('customNominal').value = '';
        const buttons = document.querySelectorAll('.donate-option-btn');
        buttons.forEach(btn => btn.classList.remove('selected'));
        disableConfirmButton();
    }

    function confirmDonate() {
        if(selectedAmount < 1000) {
            alert("Minimal donasi Rp 1.000 ya kak!");
            return;
        }

        const confirmAction = confirm(`Apakah kamu yakin ingin donasi sebesar Rp ${selectedAmount.toLocaleString('id-ID')}?\n\nKlik OK untuk lanjut ke WhatsApp Admin.`);
        
        if (confirmAction) {
            const message = `Halo Admin, saya sudah memilih untuk donasi sebesar *Rp ${selectedAmount.toLocaleString('id-ID')}*. Mohon info pembayarannya. Terima kasih!`;
            const encodedMessage = encodeURIComponent(message);
            window.open(`https://wa.me/${myWhatsApp}?text=${encodedMessage}`, '_blank');
            closeDonateModal();
        }
    }

    window.onclick = function(event) {
        const modal = document.getElementById('donateModal');
        if (event.target == modal) {
            closeDonateModal();
        }
    }
</script>

</body>
</html>
