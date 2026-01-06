<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Flashcard Luyện Nghe (351-500)</title>
    <style>
        :root {
            --primary: #4f46e5;
            --primary-light: #818cf8;
            --success: #10b981;
            --danger: #ef4444;
            --warning: #f59e0b;
            --bg: #f3f4f6;
            --card-bg: #ffffff;
            --text-main: #1f2937;
            --text-sub: #6b7280;
        }

        * { box-sizing: border-box; }

        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background-color: var(--bg);
            color: var(--text-main);
            margin: 0;
            padding: 0;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        /* --- Header & Progress --- */
        header {
            width: 100%;
            background: white;
            padding: 15px 20px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .header-content {
            max-width: 600px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        h1 { margin: 0; font-size: 1.2rem; color: var(--primary); }

        .btn-settings {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            padding: 5px;
            transition: transform 0.2s;
        }
        .btn-settings:hover { transform: rotate(45deg); }

        .progress-container {
            max-width: 600px;
            margin: 0 auto;
            background: #e5e7eb;
            height: 8px;
            border-radius: 4px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background: var(--success);
            width: 0%;
            transition: width 0.3s ease;
        }

        .stats-text {
            font-size: 0.85rem;
            color: var(--text-sub);
            text-align: center;
            margin-top: 5px;
        }

        /* --- Main Stage --- */
        .stage {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            width: 100%;
            padding: 20px;
            max-width: 500px;
        }

        .card-scene {
            width: 100%;
            height: 320px;
            perspective: 1000px;
            margin-bottom: 25px;
            cursor: pointer;
        }

        .card {
            width: 100%;
            height: 100%;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s cubic-bezier(0.4, 0.2, 0.2, 1);
            border-radius: 24px;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
        }

        .card.is-flipped { transform: rotateY(180deg); }

        /* Animation classes for swiping */
        .swipe-left { animation: flyLeft 0.5s forwards; }
        .swipe-right { animation: flyRight 0.5s forwards; }

        @keyframes flyLeft {
            to { transform: translateX(-120%) rotate(-10deg); opacity: 0; }
        }
        @keyframes flyRight {
            to { transform: translateX(120%) rotate(10deg); opacity: 0; }
        }

        .card-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            background: white;
            border-radius: 24px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 30px;
            text-align: center;
            border: 1px solid rgba(0,0,0,0.05);
        }

        .card-back { transform: rotateY(180deg); background: #fdfeff; }

        /* Card Content Front (Listening Mode) */
        .listening-icon {
            font-size: 4rem;
            color: white;
            background: var(--primary);
            width: 100px;
            height: 100px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 20px;
            box-shadow: 0 4px 15px rgba(79, 70, 229, 0.3);
            transition: transform 0.2s;
            cursor: pointer;
        }
        .listening-icon:active { transform: scale(0.95); }
        
        .front-type {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--text-sub);
            background: #f3f4f6;
            padding: 5px 15px;
            border-radius: 8px;
            margin-bottom: 20px;
        }

        /* Card Content Back */
        .word { font-size: 2.2rem; font-weight: 700; color: var(--primary); margin-bottom: 10px; line-height: 1.2; word-break: break-word;}
        .hint { color: var(--text-sub); font-size: 0.9rem; margin-top: auto; }
        
        .ipa { font-size: 1.1rem; color: var(--text-sub); font-family: 'Lucida Sans Unicode', sans-serif; margin-bottom: 5px; }
        .type { display: inline-block; background: #e0e7ff; color: var(--primary); padding: 4px 8px; border-radius: 6px; font-size: 0.8rem; font-weight: 600; margin-bottom: 15px; }
        .meaning { font-size: 1.6rem; font-weight: 600; color: var(--text-main); line-height: 1.3; }

        .audio-btn-small {
            position: absolute;
            top: 20px;
            right: 20px;
            background: var(--bg);
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--primary);
            transition: all 0.2s;
        }
        .audio-btn-small:hover { background: #e0e7ff; transform: scale(1.1); }

        /* --- Controls --- */
        .controls {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            width: 100%;
        }

        .btn {
            padding: 16px;
            border: none;
            border-radius: 16px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.1s, box-shadow 0.2s;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }
        .btn:active { transform: scale(0.96); }

        .btn-unknown { background-color: #fee2e2; color: var(--danger); border: 2px solid transparent; }
        .btn-unknown:hover { border-color: var(--danger); }
        
        .btn-known { background-color: #d1fae5; color: var(--success); border: 2px solid transparent; }
        .btn-known:hover { border-color: var(--success); }

        .shortcut-hint { font-size: 0.7rem; opacity: 0.7; font-weight: normal; }

        /* --- Footer / Lists --- */
        .toolbar {
            margin-top: 30px;
            display: flex;
            gap: 10px;
        }
        .btn-tool {
            padding: 8px 16px;
            background: white;
            border: 1px solid #d1d5db;
            color: var(--text-sub);
            border-radius: 8px;
            font-size: 0.9rem;
            cursor: pointer;
        }
        .btn-tool:hover { background: #f9fafb; color: var(--text-main); }

        /* List Modal Style */
        .list-section {
            width: 100%;
            max-width: 600px;
            margin-top: 20px;
            background: white;
            border-radius: 16px;
            padding: 20px;
            display: none;
            margin-bottom: 40px;
        }
        .list-section.active { display: block; }
        
        .list-grid {
            display: grid;
            grid-template-columns: 1fr;
            gap: 10px;
            max-height: 400px;
            overflow-y: auto;
        }
        .list-item {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            border-bottom: 1px dashed #eee;
            font-size: 0.95rem;
        }
        .list-item span:first-child { font-weight: 600; color: var(--primary); }

        /* --- Start Overlay --- */
        .start-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: var(--bg);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 500;
        }
        .start-btn {
            padding: 20px 40px;
            font-size: 1.5rem;
            background: var(--primary);
            color: white;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(79, 70, 229, 0.4);
            transition: transform 0.2s;
        }
        .start-btn:active { transform: scale(0.95); }

        /* --- Settings Modal --- */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.5);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 200;
        }
        .modal-overlay.active { display: flex; }
        
        .modal-content {
            background: white;
            padding: 25px;
            border-radius: 16px;
            width: 90%;
            max-width: 400px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.2);
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        .modal-header h3 { margin: 0; color: var(--primary); }
        .close-btn {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: #999;
        }

        .setting-group label { display: block; margin-bottom: 8px; font-weight: 600; }
        .voice-select {
            width: 100%;
            padding: 10px;
            border: 1px solid #d1d5db;
            border-radius: 8px;
            font-size: 1rem;
            margin-bottom: 20px;
        }

        /* --- Empty State --- */
        .empty-state {
            text-align: center;
            display: none;
        }
        .empty-state h2 { color: var(--success); }

    </style>
</head>
<body>

    <!-- Start Overlay for Audio Permissions -->
    <div class="start-overlay" id="start-screen">
        <h1 style="margin-bottom: 30px; text-align: center;">Flashcard Luyện Nghe<br>(351 - 500)</h1>
        <button class="start-btn" onclick="startApp()">Bắt đầu học ▶</button>
        <p style="margin-top: 20px; color: #666;">(Nhấn để bật âm thanh)</p>
    </div>

    <header>
        <div class="header-content">
            <h1>Từ Vựng 351-500</h1>
            <div style="display: flex; align-items: center; gap: 10px;">
                <div style="font-size: 0.9rem; font-weight: bold; color: var(--primary);">
                    <span id="queue-count">0</span> cần học
                </div>
                <button class="btn-settings" onclick="openSettings()">⚙️</button>
            </div>
        </div>
        <div class="progress-container">
            <div class="progress-bar" id="progress-bar"></div>
        </div>
        <div class="stats-text">
            Đã thuộc: <span id="known-count">0</span> / <span id="total-count">0</span>
        </div>
    </header>

    <div class="stage">
        <!-- Card -->
        <div class="card-scene" onclick="flipCard()">
            <div class="card" id="flashcard">
                <!-- Front (Listening Only) -->
                <div class="card-face card-front">
                    <!-- Icon Loa Lớn -->
                    <div class="listening-icon" onclick="event.stopPropagation(); speakWord()">
                        🔊
                    </div>
                    <!-- Loại từ -->
                    <div class="front-type" id="front-type">(n)</div>
                    <div class="hint">Nghe và đoán từ<br><small>(Chạm để lật xem đáp án)</small></div>
                </div>

                <!-- Back (Answer) -->
                <div class="card-face card-back">
                    <button class="audio-btn-small" onclick="event.stopPropagation(); speakWord()">
                         <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon><path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"></path></svg>
                    </button>
                    <div class="word" id="back-word">...</div>
                    <div class="ipa" id="back-ipa">...</div>
                    <div class="type" id="back-type">...</div>
                    <div class="meaning" id="back-meaning">...</div>
                </div>
            </div>
        </div>

        <!-- Buttons -->
        <div class="controls">
            <button class="btn btn-unknown" onclick="handleResult(false)">
                ❌ Chưa thuộc
                <span class="shortcut-hint">(Mũi tên trái)</span>
            </button>
            <button class="btn btn-known" onclick="handleResult(true)">
                ✅ Đã thuộc
                <span class="shortcut-hint">(Mũi tên phải)</span>
            </button>
        </div>

        <!-- Toolbar -->
        <div class="toolbar">
            <button class="btn-tool" onclick="shuffleCards()">🔀 Đảo từ</button>
            <button class="btn-tool" onclick="toggleList()">📋 Xem danh sách</button>
            <button class="btn-tool" onclick="resetData()">🔄 Học lại từ đầu</button>
        </div>

        <!-- Empty State Msg -->
        <div class="empty-state" id="empty-state">
            <h2>🎉 Chúc mừng!</h2>
            <p>Bạn đã thuộc hết toàn bộ danh sách từ vựng này.</p>
            <button class="btn btn-known" onclick="resetData()">Bắt đầu lại</button>
        </div>
    </div>

    <!-- Hidden List Section -->
    <div class="list-section" id="list-section">
        <h3 style="color: var(--success); margin-top:0;">Danh sách đã thuộc</h3>
        <div class="list-grid" id="known-list-render"></div>
        <h3 style="color: var(--danger); margin-top:20px;">Danh sách cần học</h3>
        <div class="list-grid" id="queue-list-render"></div>
    </div>

    <!-- Settings Modal -->
    <div class="modal-overlay" id="settings-modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3>Cài đặt âm thanh</h3>
                <button class="close-btn" onclick="closeSettings()">✕</button>
            </div>
            <div class="setting-group">
                <label for="voice-select">Chọn giọng đọc (Anh-Anh):</label>
                <select id="voice-select" class="voice-select" onchange="saveVoiceSetting()">
                    <option value="">Đang tải giọng...</option>
                </select>
                <small style="color: #666; display: block; margin-top: -10px; margin-bottom: 20px;">
                    Chỉ hiển thị các giọng British English (UK) có trên thiết bị của bạn.
                </small>
            </div>
            <button class="btn btn-known" onclick="closeSettings()" style="width:100%; padding: 10px;">Đóng</button>
        </div>
    </div>

    <script>
        // Data Source 351-500
        const rawData = [
            { id: 351, word: "cook", type: "(v)", ipa: "/kʊk/", meaning: "nấu ăn" },
            { id: 352, word: "eat", type: "(v)", ipa: "/iːt/", meaning: "ăn" },
            { id: 353, word: "drink", type: "(v)", ipa: "/drɪŋk/", meaning: "uống" },
            { id: 354, word: "hungry", type: "(adj)", ipa: "/ˈhʌŋɡri/", meaning: "đói" },
            { id: 355, word: "thirsty", type: "(adj)", ipa: "/ˈθɜːrsti/", meaning: "khát" },
            { id: 356, word: "delicious", type: "(adj)", ipa: "/dɪˈlɪʃəs/", meaning: "ngon" },
            { id: 357, word: "sweet", type: "(adj)", ipa: "/swiːt/", meaning: "ngọt" },
            { id: 358, word: "sour", type: "(adj)", ipa: "/ˈsaʊər/", meaning: "chua" },
            { id: 359, word: "spicy", type: "(adj)", ipa: "/ˈspaɪsi/", meaning: "cay" },
            { id: 360, word: "order", type: "(v)", ipa: "/ˈɔːrdər/", meaning: "gọi món" },
            { id: 361, word: "buy", type: "(v)", ipa: "/baɪ/", meaning: "mua" },
            { id: 362, word: "family", type: "(n)", ipa: "/ˈfæməli/", meaning: "gia đình" },
            { id: 363, word: "mother", type: "(n)", ipa: "/ˈmʌðər/", meaning: "mẹ" },
            { id: 364, word: "father", type: "(n)", ipa: "/ˈfɑːðər/", meaning: "bố" },
            { id: 365, word: "parent", type: "(n)", ipa: "/ˈpɛərənt/", meaning: "phụ huynh" },
            { id: 366, word: "son", type: "(n)", ipa: "/sʌn/", meaning: "con trai" },
            { id: 367, word: "daughter", type: "(n)", ipa: "/ˈdɔːtər/", meaning: "con gái" },
            { id: 368, word: "children", type: "(n)", ipa: "/tʃaɪld/ /ˈtʃɪldrən/", meaning: "đứa trẻ / những đứa trẻ" },
            { id: 369, word: "brother", type: "(n)", ipa: "/ˈbrʌðər/", meaning: "anh/em trai" },
            { id: 370, word: "sister", type: "(n)", ipa: "/ˈsɪstər/", meaning: "chị/em gái" },
            { id: 371, word: "baby", type: "(n)", ipa: "/ˈbeɪbi/", meaning: "em bé" },
            { id: 372, word: "husband", type: "(n)", ipa: "/ˈhʌzbənd/", meaning: "chồng" },
            { id: 373, word: "wife", type: "(n)", ipa: "/waɪf/", meaning: "vợ" },
            { id: 374, word: "grandfather", type: "(n)", ipa: "/ˈɡrænfɑːðər/", meaning: "ông" },
            { id: 375, word: "grandmother", type: "(n)", ipa: "/ˈɡrænmʌðər/", meaning: "bà" },
            { id: 376, word: "grandparents", type: "(n)", ipa: "/ˈɡrænˌpɛərənts/", meaning: "ông bà" },
            { id: 377, word: "uncle", type: "(n)", ipa: "/ˈʌŋkəl/", meaning: "chú, bác, cậu" },
            { id: 378, word: "aunt", type: "(n)", ipa: "/ænt/", meaning: "cô, dì, bác gái" },
            { id: 379, word: "cousin", type: "(n)", ipa: "/ˈkʌzən/", meaning: "anh/chị/em họ" },
            { id: 380, word: "nephew", type: "(n)", ipa: "/ˈnɛfjuː/", meaning: "cháu trai (của cô/dì/chú/bác)" },
            { id: 381, word: "niece", type: "(n)", ipa: "/niːs/", meaning: "cháu gái (của cô/dì/chú/bác)" },
            { id: 382, word: "relative", type: "(n)", ipa: "/ˈrɛlətɪv/", meaning: "họ hàng" },
            { id: 383, word: "home", type: "(n)", ipa: "/hoʊm/", meaning: "nhà, mái ấm" },
            { id: 384, word: "house", type: "(n)", ipa: "/haʊs/", meaning: "ngôi nhà" },
            { id: 385, word: "apartment", type: "(n)", ipa: "/əˈpɑːrtmənt/", meaning: "căn hộ" },
            { id: 386, word: "room", type: "(n)", ipa: "/ruːm/", meaning: "căn phòng" },
            { id: 387, word: "pet", type: "(n)", ipa: "/pɛt/", meaning: "thú cưng" },
            { id: 388, word: "birthday", type: "(n)", ipa: "/ˈbɜːrθdeɪ/", meaning: "sinh nhật" },
            { id: 389, word: "gift", type: "(n)", ipa: "/ɡɪft/ /ˈprɛzənt/", meaning: "món quà" },
            { id: 390, word: "party", type: "(n)", ipa: "/ˈpɑːrti/", meaning: "bữa tiệc" },
            { id: 391, word: "wedding", type: "(n)", ipa: "/ˈwɛdɪŋ/", meaning: "đám cưới" },
            { id: 392, word: "marriage", type: "(n)", ipa: "/ˈmærɪdʒ/", meaning: "hôn nhân" },
            { id: 393, word: "single", type: "(adj)", ipa: "/ˈsɪŋɡəl/", meaning: "độc thân" },
            { id: 394, word: "married", type: "(adj)", ipa: "/ˈmærid/", meaning: "đã kết hôn" },
            { id: 395, word: "divorced", type: "(adj)", ipa: "/dɪˈvɔːrst/", meaning: "đã ly hôn" },
            { id: 396, word: "grow up", type: "(phr. v)", ipa: "/ɡroʊ ʌp/", meaning: "lớn lên" },
            { id: 397, word: "get married", type: "(phr. v)", ipa: "/ɡɛt ˈmærid/", meaning: "kết hôn" },
            { id: 398, word: "have children", type: "(phr)", ipa: "/hæv ˈtʃɪldrən/", meaning: "có con" },
            { id: 399, word: "young", type: "(adj)", ipa: "/jʌŋ/", meaning: "trẻ" },
            { id: 400, word: "together", type: "(adv)", ipa: "/təˈɡɛðər/", meaning: "cùng nhau" },
            { id: 401, word: "sometimes", type: "(adv)", ipa: "/ˈsʌmtaɪmz/", meaning: "thỉnh thoảng" },
            { id: 402, word: "friend", type: "(n)", ipa: "/frɛnd/", meaning: "bạn bè" },
            { id: 403, word: "boyfriend", type: "(n)", ipa: "/ˈbɔɪfrɛnd/", meaning: "bạn trai" },
            { id: 404, word: "girlfriend", type: "(n)", ipa: "/ˈɡɜːrlfrɛnd/", meaning: "bạn gái" },
            { id: 405, word: "partner", type: "(n)", ipa: "/ˈpɑːrtnər/", meaning: "bạn đời, đối tác" },
            { id: 406, word: "couple", type: "(n)", ipa: "/ˈkʌpəl/", meaning: "cặp đôi" },
            { id: 407, word: "neighbor", type: "(n)", ipa: "/ˈneɪbər/", meaning: "hàng xóm" },
            { id: 408, word: "colleague", type: "(n)", ipa: "/ˈkɑːliːɡ/", meaning: "đồng nghiệp" },
            { id: 409, word: "boss", type: "(n)", ipa: "/bɔːs/", meaning: "ông chủ, sếp" },
            { id: 410, word: "team", type: "(n)", ipa: "/tiːm/", meaning: "đội, nhóm" },
            { id: 411, word: "group", type: "(n)", ipa: "/ɡruːp/", meaning: "nhóm" },
            { id: 412, word: "guest", type: "(n)", ipa: "/ɡɛst/", meaning: "khách" },
            { id: 413, word: "member", type: "(n)", ipa: "/ˈmɛmbər/", meaning: "thành viên" },
            { id: 414, word: "relationship", type: "(n)", ipa: "/rɪˈleɪʃənʃɪp/", meaning: "mối quan hệ" },
            { id: 415, word: "friendship", type: "(n)", ipa: "/ˈfrɛndʃɪp/", meaning: "tình bạn" },
            { id: 416, word: "date", type: "(n, v)", ipa: "/deɪt/", meaning: "cuộc hẹn hò, hẹn hò" },
            { id: 417, word: "meeting", type: "(n)", ipa: "/ˈmiːtɪŋ/", meaning: "cuộc gặp gỡ" },
            { id: 418, word: "conversation", type: "(n)", ipa: "/ˌkɑːnvərˈseɪʃən/", meaning: "cuộc trò chuyện" },
            { id: 419, word: "email", type: "(n)", ipa: "/ˈiːmeɪl/", meaning: "thư điện tử" },
            { id: 420, word: "phone call", type: "(n)", ipa: "/foʊn kɔːl/", meaning: "cuộc gọi điện thoại" },
            { id: 421, word: "message", type: "(n)", ipa: "/ˈmɛsɪdʒ/", meaning: "tin nhắn" },
            { id: 422, word: "present", type: "(n)", ipa: "/ˈprɛzənt/", meaning: "món quà" },
            { id: 423, word: "problem", type: "(n)", ipa: "/ˈprɑːbləm/", meaning: "vấn đề" },
            { id: 424, word: "feeling", type: "(n)", ipa: "/ˈfiːlɪŋ/", meaning: "cảm giác" },
            { id: 425, word: "trust", type: "(n, v)", ipa: "/trʌst/", meaning: "sự tin tưởng, tin cậy" },
            { id: 426, word: "respect", type: "(n, v)", ipa: "/rɪˈspɛkt/", meaning: "sự tôn trọng, tôn trọng" },
            { id: 427, word: "argument", type: "(n)", ipa: "/ˈɑːrɡjumənt/", meaning: "cuộc tranh cãi" },
            { id: 428, word: "meet", type: "(v)", ipa: "/miːt/", meaning: "gặp gỡ" },
            { id: 429, word: "talk", type: "(v)", ipa: "/tɔːk/", meaning: "nói chuyện" },
            { id: 430, word: "know", type: "(v)", ipa: "/noʊ/", meaning: "biết" },
            { id: 431, word: "disagree", type: "(v)", ipa: "/ˌdɪsəˈɡriː/", meaning: "không đồng ý" },
            { id: 432, word: "share", type: "(v)", ipa: "/ʃɛər/", meaning: "chia sẻ" },
            { id: 433, word: "phone", type: "(v)", ipa: "/foʊn/", meaning: "gọi điện" },
            { id: 434, word: "text", type: "(v)", ipa: "/tɛkst/", meaning: "nhắn tin" },
            { id: 435, word: "friendly", type: "(adj)", ipa: "/ˈfrɛndli/", meaning: "thân thiện" },
            { id: 436, word: "kind", type: "(adj)", ipa: "/kaɪnd/", meaning: "tốt bụng" },
            { id: 437, word: "happy", type: "(adj)", ipa: "/ˈhæpi/", meaning: "vui vẻ, hạnh phúc" },
            { id: 438, word: "sad", type: "(adj)", ipa: "/sæd/", meaning: "buồn" },
            { id: 439, word: "angry", type: "(adj)", ipa: "/ˈæŋɡri/", meaning: "tức giận" },
            { id: 440, word: "close", type: "(adj)", ipa: "/kloʊs/", meaning: "thân thiết" },
            { id: 441, word: "computer", type: "(n)", ipa: "/kəmˈpjuːtər/", meaning: "máy tính" },
            { id: 442, word: "laptop", type: "(n)", ipa: "/ˈlæptɑːp/", meaning: "máy tính xách tay" },
            { id: 443, word: "screen", type: "(n)", ipa: "/skriːn/", meaning: "màn hình" },
            { id: 444, word: "keyboard", type: "(n)", ipa: "/ˈkiːbɔːrd/", meaning: "bàn phím" },
            { id: 445, word: "mouse", type: "(n)", ipa: "/maʊs/", meaning: "chuột máy tính" },
            { id: 446, word: "internet", type: "(n)", ipa: "/ˈɪntərnɛt/", meaning: "mạng internet" },
            { id: 447, word: "website", type: "(n)", ipa: "/ˈwɛbsaɪt/", meaning: "trang web" },
            { id: 448, word: "password", type: "(n)", ipa: "/ˈpæswɜːrd/", meaning: "mật khẩu" },
            { id: 449, word: "photo", type: "(n)", ipa: "/ˈfoʊtoʊ/", meaning: "ảnh" },
            { id: 450, word: "video", type: "(n)", ipa: "/ˈvɪdioʊ/", meaning: "vi-đê-ô" },
            { id: 451, word: "app", type: "(n)", ipa: "/æp/", meaning: "ứng dụng" },
            { id: 452, word: "online", type: "(adj, adv)", ipa: "/ˌɑːnˈlaɪn/", meaning: "trực tuyến" },
            { id: 453, word: "offline", type: "(adj, adv)", ipa: "/ˌɔːfˈlaɪn/", meaning: "ngoại tuyến" },
            { id: 454, word: "download", type: "(v)", ipa: "/ˌdaʊnˈloʊd/", meaning: "tải xuống" },
            { id: 455, word: "upload", type: "(v)", ipa: "/ˌʌpˈloʊd/", meaning: "tải lên" },
            { id: 456, word: "click", type: "(v)", ipa: "/klɪk/", meaning: "nhấp chuột" },
            { id: 457, word: "type", type: "(v)", ipa: "/taɪp/", meaning: "gõ, đánh máy" },
            { id: 458, word: "send", type: "(v)", ipa: "/sɛnd/", meaning: "gửi" },
            { id: 459, word: "receive", type: "(v)", ipa: "/rɪˈsiːv/", meaning: "nhận" },
            { id: 460, word: "search", type: "(v)", ipa: "/sɜːrtʃ/", meaning: "tìm kiếm" },
            { id: 461, word: "watch", type: "(v)", ipa: "/wɑːtʃ/", meaning: "xem" },
            { id: 462, word: "play", type: "(v)", ipa: "/pleɪ/", meaning: "chơi" },
            { id: 463, word: "turn on", type: "(phr. v)", ipa: "/tɜːrn ɑːn/", meaning: "bật, mở" },
            { id: 464, word: "connect", type: "(v)", ipa: "/kəˈnɛkt/", meaning: "kết nối" },
            { id: 465, word: "printer", type: "(n)", ipa: "/ˈprɪntər/", meaning: "máy in" },
            { id: 466, word: "Wi-Fi", type: "(n)", ipa: "/ˈwaɪfaɪ/", meaning: "Wi-Fi" },
            { id: 467, word: "file", type: "(n)", ipa: "/faɪl/", meaning: "tệp, tài liệu" },
            { id: 468, word: "call", type: "(n, v)", ipa: "/kɔːl/", meaning: "cuộc gọi, gọi điện" },
            { id: 469, word: "modern", type: "(adj)", ipa: "/ˈmɑːdərn/", meaning: "hiện đại" },
            { id: 470, word: "digital", type: "(adj)", ipa: "/ˈdɪdʒɪtəl/", meaning: "kỹ thuật số" },
            { id: 471, word: "living room", type: "(n)", ipa: "/ˈlɪvɪŋ ruːm/", meaning: "phòng khách" },
            { id: 472, word: "bedroom", type: "(n)", ipa: "/ˈbɛdruːm/", meaning: "phòng ngủ" },
            { id: 473, word: "bathroom", type: "(n)", ipa: "/ˈbæθruːm/", meaning: "phòng tắm" },
            { id: 474, word: "garden", type: "(n)", ipa: "/ˈɡɑːrdn/", meaning: "vườn" },
            { id: 475, word: "door", type: "(n)", ipa: "/dɔːr/", meaning: "cửa ra vào" },
            { id: 476, word: "window", type: "(n)", ipa: "/ˈwɪndoʊ/", meaning: "cửa sổ" },
            { id: 477, word: "floor", type: "(n)", ipa: "/flɔːr/", meaning: "sàn nhà" },
            { id: 478, word: "wall", type: "(n)", ipa: "/wɔːl/", meaning: "bức tường" },
            { id: 479, word: "roof", type: "(n)", ipa: "/ruːf/", meaning: "mái nhà" },
            { id: 480, word: "table", type: "(n)", ipa: "/ˈteɪbəl/", meaning: "cái bàn" },
            { id: 481, word: "sofa / couch", type: "(n)", ipa: "/ˈsoʊfə/ /kaʊtʃ/", meaning: "ghế sô-pha" },
            { id: 482, word: "bed", type: "(n)", ipa: "/bɛd/", meaning: "cái giường" },
            { id: 483, word: "lamp", type: "(n)", ipa: "/læmp/", meaning: "cái đèn" },
            { id: 484, word: "television", type: "(n)", ipa: "/ˈtɛlɪvɪʒən/", meaning: "ti-vi" },
            { id: 485, word: "clock", type: "(n)", ipa: "/klɑːk/", meaning: "đồng hồ treo tường" },
            { id: 486, word: "picture", type: "(n)", ipa: "/ˈpɪktʃər/", meaning: "bức tranh" },
            { id: 487, word: "mirror", type: "(n)", ipa: "/ˈmɪrər/", meaning: "cái gương" },
            { id: 488, word: "fridge", type: "(n)", ipa: "/frɪdʒ/", meaning: "tủ lạnh" },
            { id: 489, word: "cooker / stove", type: "(n)", ipa: "/ˈkʊkər/ /stoʊv/", meaning: "bếp nấu" },
            { id: 490, word: "sink", type: "(n)", ipa: "/sɪŋk/", meaning: "bồn rửa" },
            { id: 491, word: "toilet", type: "(n)", ipa: "/ˈtɔɪlət/", meaning: "nhà vệ sinh, bồn cầu" },
            { id: 492, word: "key", type: "(n)", ipa: "/kiː/", meaning: "chìa khóa" },
            { id: 493, word: "address", type: "(n)", ipa: "/ˈædrɛs/", meaning: "địa chỉ" },
            { id: 494, word: "light", type: "(n)", ipa: "/laɪt/", meaning: "ánh sáng, đèn" },
            { id: 495, word: "furniture", type: "(n)", ipa: "/ˈfɜːrnɪtʃər/", meaning: "đồ nội thất" },
            { id: 496, word: "shelf", type: "(n)", ipa: "/ʃɛlf/", meaning: "cái kệ, giá sách" },
            { id: 497, word: "carpet", type: "(n)", ipa: "/ˈkɑːrpɪt/", meaning: "tấm thảm" },
            { id: 498, word: "sleep", type: "(v)", ipa: "/sliːp/", meaning: "ngủ" },
            { id: 499, word: "sit", type: "(v)", ipa: "/sɪt/", meaning: "ngồi" },
            { id: 500, word: "countryside", type: "(n)", ipa: "/ˈkʌntrisaɪd/", meaning: "vùng nông thôn" }
        ];

        // State variables
        let studyQueue = [];
        let knownList = [];
        let currentCard = null;
        let isAnimating = false;
        let voicesList = [];
        let britishVoices = [];
        let selectedVoiceURI = ''; // Store user's choice
        const STORAGE_KEY = 'vocab_app_listening_v3_351_500';

        // --- Initialization ---
        function init() {
            loadProgress();
            
            if (studyQueue.length === 0 && knownList.length === 0) {
                studyQueue = [...rawData];
            } else if (studyQueue.length === 0 && knownList.length < rawData.length) {
                const knownIds = knownList.map(k => k.id);
                studyQueue = rawData.filter(d => !knownIds.includes(d.id));
            }

            // Listen for voices
            if (window.speechSynthesis.onvoiceschanged !== undefined) {
                window.speechSynthesis.onvoiceschanged = () => {
                    loadVoices();
                };
            }
            // Try initial load
            loadVoices();

            renderStats();
            // We do NOT call loadNextCard here. The "Start" button calls it.
        }

        function startApp() {
            document.getElementById('start-screen').style.display = 'none';
            // Also resume audio context if needed, but for speech synthesis, this click is enough
            loadNextCard();
        }

        // --- Voice Logic ---
        function loadVoices() {
            voicesList = window.speechSynthesis.getVoices();
            if(voicesList.length === 0) return;

            // Filter ONLY British English
            // Criteria: lang is en-GB, or name contains UK/GB/United Kingdom
            britishVoices = voicesList.filter(voice => {
                const lang = voice.lang.replace('_', '-');
                const name = voice.name;
                return lang === 'en-GB' || 
                       lang.startsWith('en-GB') ||
                       name.includes('UK English') ||
                       name.includes('Great Britain') ||
                       name.includes('United Kingdom');
            });

            // Populate Dropdown
            const selectEl = document.getElementById('voice-select');
            selectEl.innerHTML = ''; // clear

            if (britishVoices.length === 0) {
                const opt = document.createElement('option');
                opt.text = "Không tìm thấy giọng Anh-Anh trên thiết bị này.";
                selectEl.add(opt);
                return;
            }

            britishVoices.forEach(voice => {
                const option = document.createElement('option');
                option.value = voice.voiceURI;
                let displayName = voice.name;
                if(voice.voiceURI === selectedVoiceURI) {
                    option.selected = true;
                }
                option.text = displayName;
                selectEl.appendChild(option);
            });

            // If no voice selected yet, pick the first one as default
            if (!selectedVoiceURI && britishVoices.length > 0) {
                selectedVoiceURI = britishVoices[0].voiceURI;
                saveVoiceSetting();
            }
        }

        function openSettings() {
            document.getElementById('settings-modal').classList.add('active');
            loadVoices(); // Refresh list to be sure
        }

        function closeSettings() {
            document.getElementById('settings-modal').classList.remove('active');
        }

        function saveVoiceSetting() {
            const selectEl = document.getElementById('voice-select');
            selectedVoiceURI = selectEl.value;
            saveProgress(); 
        }

        // --- Core Logic ---

        function loadNextCard() {
            const cardEl = document.getElementById('flashcard');
            const scene = document.querySelector('.card-scene');
            const controls = document.querySelector('.controls');
            const emptyState = document.getElementById('empty-state');
            
            cardEl.classList.remove('swipe-left', 'swipe-right', 'is-flipped');
            
            if (studyQueue.length === 0) {
                scene.style.display = 'none';
                controls.style.display = 'none';
                emptyState.style.display = 'block';
                return;
            }

            scene.style.display = 'block';
            controls.style.display = 'grid';
            emptyState.style.display = 'none';

            currentCard = studyQueue[0];
            
            document.getElementById('front-type').textContent = currentCard.type;
            document.getElementById('back-word').textContent = currentCard.word;
            document.getElementById('back-ipa').textContent = currentCard.ipa;
            document.getElementById('back-type').textContent = currentCard.type;
            document.getElementById('back-meaning').textContent = currentCard.meaning;
            
            isAnimating = false;

            // Auto play
            setTimeout(() => {
                speakWord();
            }, 600);
        }

        function flipCard() {
            if(isAnimating) return;
            document.getElementById('flashcard').classList.toggle('is-flipped');
        }

        function handleResult(isKnown) {
            if (!currentCard || isAnimating) return;
            isAnimating = true;

            const cardEl = document.getElementById('flashcard');
            
            if (isKnown) {
                cardEl.classList.add('swipe-right');
                studyQueue.shift(); 
                knownList.push(currentCard);
            } else {
                cardEl.classList.add('swipe-left');
                studyQueue.push(studyQueue.shift());
            }

            saveProgress();
            renderStats();

            setTimeout(() => {
                loadNextCard();
            }, 500);
        }

        function shuffleCards() {
            if (studyQueue.length < 2) return;
            for (let i = studyQueue.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [studyQueue[i], studyQueue[j]] = [studyQueue[j], studyQueue[i]];
            }
            const cardEl = document.getElementById('flashcard');
            cardEl.classList.remove('is-flipped');
            loadNextCard();
            saveProgress();
        }

        function resetData() {
            if (confirm('Bạn có chắc chắn muốn xóa lịch sử và học lại từ đầu?')) {
                localStorage.removeItem(STORAGE_KEY);
                studyQueue = [];
                knownList = [];
                location.reload();
            }
        }

        function speakWord() {
            if (!currentCard) return;
            const synth = window.speechSynthesis;
            synth.cancel();

            // Nếu từ có dấu / (ví dụ: cooker / stove), chỉ đọc từ đầu tiên để tránh dài dòng
            // hoặc đọc cả 2 tuỳ ý. Ở đây mình sẽ đọc nguyên văn text hiển thị.
            // Tuy nhiên, để nghe tự nhiên hơn, có thể thay thế dấu "/" bằng "or"
            let textToSpeak = currentCard.word.replace(/\//g, " or ");

            const utterThis = new SpeechSynthesisUtterance(textToSpeak);
            
            // Logic select voice with Fallback
            let voiceToUse = null;

            // 1. Try to find the exact saved voice
            if (selectedVoiceURI) {
                voiceToUse = britishVoices.find(v => v.voiceURI === selectedVoiceURI);
            }

            // 2. Fallback: If saved voice is invalid, try ANY British voice
            if (!voiceToUse && britishVoices.length > 0) {
                voiceToUse = britishVoices[0];
            }

            if (voiceToUse) {
                utterThis.voice = voiceToUse;
            } else {
                // 3. Last resort: Just set Lang code
                utterThis.lang = 'en-GB';
            }

            utterThis.rate = 0.8;
            synth.speak(utterThis);
        }

        // --- Data Persistence ---
        function saveProgress() {
            const data = { 
                studyQueue, 
                knownList,
                selectedVoiceURI 
            };
            localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
        }

        function loadProgress() {
            const data = localStorage.getItem(STORAGE_KEY);
            if (data) {
                const parsed = JSON.parse(data);
                studyQueue = parsed.studyQueue || [];
                knownList = parsed.knownList || [];
                selectedVoiceURI = parsed.selectedVoiceURI || '';
            }
        }

        // --- UI Updates ---
        function renderStats() {
            const total = rawData.length;
            const known = knownList.length;
            const percent = (known / total) * 100;

            document.getElementById('queue-count').textContent = studyQueue.length;
            document.getElementById('known-count').textContent = known;
            document.getElementById('total-count').textContent = total;
            document.getElementById('progress-bar').style.width = `${percent}%`;

            renderListContent();
        }

        function toggleList() {
            const listSection = document.getElementById('list-section');
            listSection.classList.toggle('active');
            if(listSection.classList.contains('active')){
                listSection.scrollIntoView({behavior: "smooth"});
            }
        }

        function renderListContent() {
            const createItem = (item) => `
                <div class="list-item">
                    <span>${item.word} <small style="color:#666; font-weight:normal">${item.ipa}</small></span>
                    <span>${item.meaning}</span>
                </div>
            `;

            document.getElementById('known-list-render').innerHTML = 
                knownList.length ? knownList.map(createItem).join('') : '<p style="text-align:center; color:#999">Chưa có từ nào.</p>';
            
            document.getElementById('queue-list-render').innerHTML = 
                studyQueue.length ? studyQueue.map(createItem).join('') : '<p style="text-align:center; color:#999">Đã học hết.</p>';
        }

        // --- Keyboard Shortcuts ---
        document.addEventListener('keydown', (e) => {
            if (document.getElementById('settings-modal').classList.contains('active')) {
                if(e.key === 'Escape') closeSettings();
                return;
            }
            // If start screen is visible, space starts app
            if (document.getElementById('start-screen').style.display !== 'none') {
                if (e.code === 'Space' || e.key === 'Enter') startApp();
                return;
            }

            if (studyQueue.length === 0) return;

            if (e.code === 'Space') {
                e.preventDefault(); 
                flipCard();
            } else if (e.code === 'ArrowLeft') {
                handleResult(false);
            } else if (e.code === 'ArrowRight') {
                handleResult(true);
            }
        });

        // Start App
        init();

    </script>
</body>
</html>
