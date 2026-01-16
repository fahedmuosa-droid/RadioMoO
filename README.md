
<RadioFM>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>مشغل الوسائط - Media Player</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            background-color: #000;
            color: #fff;
            overflow-x: hidden;
            height: 100vh;
            padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
        }
        
        .container {
            max-width: 100%;
            height: 100vh;
            display: flex;
            flex-direction: column;
            padding: 10px;
            background: linear-gradient(to bottom, #0a0a0a, #000);
            position: relative;
            overflow: hidden;
        }
        
        /* رأس التطبيق */
        .app-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 10px;
            background-color: rgba(20, 20, 20, 0.9);
            border-radius: 15px;
            margin-bottom: 15px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
            z-index: 10;
            position: relative;
        }
        
        .app-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .app-logo {
            width: 50px;
            height: 50px;
            background: linear-gradient(135deg, #8a2be2, #4169e1);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
        }
        
        .app-name {
            font-size: 1.4rem;
            font-weight: 700;
            color: #f0f0f0;
        }
        
        .settings-btn {
            background-color: rgba(60, 60, 60, 0.8);
            border: none;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            color: #fff;
            font-size: 1.2rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }
        
        .settings-btn:hover, .settings-btn:active {
            background-color: rgba(90, 90, 90, 0.9);
            transform: rotate(30deg);
        }
        
        /* منطقة الفنان/الفيديو */
        .media-area {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            margin: 10px 0;
            position: relative;
            border-radius: 20px;
            overflow: hidden;
            background-color: rgba(10, 10, 10, 0.7);
        }
        
        .artist-image-container {
            width: 280px;
            height: 280px;
            border-radius: 50%;
            overflow: hidden;
            position: relative;
            box-shadow: 0 0 40px rgba(100, 100, 255, 0.3);
            margin-bottom: 25px;
            border: 3px solid #333;
        }
        
        #artistImage {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s ease;
        }
        
        .visualizer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-wrap: wrap;
        }
        
        .bar {
            width: 4px;
            height: 10px;
            margin: 0 2px;
            background: linear-gradient(to top, #4169e1, #8a2be2);
            border-radius: 2px;
            animation: soundWave 1.5s infinite ease-in-out;
            opacity: 0.7;
        }
        
        @keyframes soundWave {
            0%, 100% { height: 10px; }
            50% { height: 50px; }
        }
        
        .video-container {
            width: 100%;
            height: 100%;
            display: none;
            border-radius: 15px;
            overflow: hidden;
        }
        
        #videoPlayer {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        /* معلومات الفنان */
        .artist-info {
            text-align: center;
            margin-top: 10px;
            padding: 10px;
            width: 100%;
        }
        
        .artist-name {
            font-size: 1.8rem;
            font-weight: 700;
            color: #fff;
            margin-bottom: 5px;
        }
        
        .song-title {
            font-size: 1rem;
            color: #aaa;
            margin-bottom: 10px;
        }
        
        /* شريط التقدم */
        .progress-area {
            width: 100%;
            padding: 15px 0;
        }
        
        .progress-bar {
            width: 100%;
            height: 6px;
            background-color: #333;
            border-radius: 10px;
            cursor: pointer;
            margin-bottom: 8px;
        }
        
        .progress {
            width: 0%;
            height: 100%;
            background: linear-gradient(to right, #8a2be2, #4169e1);
            border-radius: 10px;
            position: relative;
            transition: width 0.1s linear;
        }
        
        .progress::after {
            content: '';
            position: absolute;
            top: -4px;
            right: -8px;
            width: 16px;
            height: 16px;
            background-color: #fff;
            border-radius: 50%;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.8);
        }
        
        .time-info {
            display: flex;
            justify-content: space-between;
            font-size: 0.9rem;
            color: #aaa;
        }
        
        /* عناصر التحكم */
        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 25px;
            padding: 20px 0;
        }
        
        .control-btn {
            background: none;
            border: none;
            color: #fff;
            font-size: 1.5rem;
            cursor: pointer;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            transition: all 0.3s ease;
        }
        
        .play-pause {
            background-color: rgba(65, 105, 225, 0.8);
            width: 65px;
            height: 65px;
            font-size: 1.8rem;
        }
        
        .control-btn:hover, .control-btn:active {
            background-color: rgba(255, 255, 255, 0.1);
            transform: scale(1.1);
        }
        
        .play-pause:hover, .play-pause:active {
            background-color: rgba(65, 105, 225, 1);
            transform: scale(1.1);
        }
        
        /* شريط المطربين ومحطات الراديو */
        .stations-bar {
            width: 100%;
            padding: 15px 0;
            margin-top: 10px;
            margin-bottom: 20px;
        }
        
        .stations-title {
            font-size: 1.1rem;
            margin-bottom: 12px;
            color: #ddd;
            padding-right: 10px;
        }
        
        .stations-container {
            width: 100%;
            overflow-x: auto;
            overflow-y: hidden;
            white-space: nowrap;
            padding: 10px 5px;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none; /* Firefox */
        }
        
        .stations-container::-webkit-scrollbar {
            display: none; /* Chrome, Safari, Opera */
        }
        
        .station-item {
            display: inline-block;
            width: 80px;
            height: 80px;
            margin-left: 15px;
            border-radius: 12px;
            background-color: rgba(40, 40, 40, 0.9);
            display: inline-flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            color: #fff;
            font-size: 0.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .station-item:first-child {
            margin-right: 0;
        }
        
        .station-item:hover, .station-item:active {
            background-color: rgba(65, 105, 225, 0.7);
            transform: translateY(-5px);
        }
        
        .station-item.active {
            background-color: rgba(138, 43, 226, 0.8);
            box-shadow: 0 5px 15px rgba(138, 43, 226, 0.4);
        }
        
        .station-icon {
            font-size: 1.5rem;
            margin-bottom: 8px;
        }
        
        .station-name {
            white-space: normal;
            max-height: 30px;
            overflow: hidden;
            padding: 0 5px;
        }
        
        /* نافذة الإعدادات */
        .settings-overlay {
            position: fixed;
            top: 0;
            right: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.85);
            z-index: 100;
            display: none;
            justify-content: center;
            align-items: center;
            padding: 20px;
            overflow-y: auto;
        }
        
        .settings-container {
            width: 100%;
            max-width: 500px;
            background-color: #111;
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.7);
            max-height: 90vh;
            overflow-y: auto;
        }
        
        .settings-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 25px;
            padding-bottom: 15px;
            border-bottom: 1px solid #333;
        }
        
        .settings-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: #fff;
        }
        
        .close-settings {
            background: none;
            border: none;
            color: #fff;
            font-size: 1.5rem;
            cursor: pointer;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
        }
        
        .close-settings:hover, .close-settings:active {
            background-color: rgba(255, 255, 255, 0.1);
        }
        
        .settings-section {
            margin-bottom: 25px;
        }
        
        .section-title {
            font-size: 1.2rem;
            margin-bottom: 15px;
            color: #ddd;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .section-title i {
            color: #8a2be2;
        }
        
        .volume-control, .equalizer-control {
            width: 100%;
            margin: 15px 0;
        }
        
        .volume-slider, .equalizer-slider {
            width: 100%;
            height: 8px;
            -webkit-appearance: none;
            appearance: none;
            background: #333;
            border-radius: 10px;
            outline: none;
        }
        
        .volume-slider::-webkit-slider-thumb, .equalizer-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 22px;
            height: 22px;
            border-radius: 50%;
            background: #4169e1;
            cursor: pointer;
        }
        
        .settings-option {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #222;
        }
        
        .option-label {
            font-size: 1rem;
            color: #ccc;
        }
        
        .option-control {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .toggle-switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 26px;
        }
        
        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .toggle-slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            right: 0;
            left: 0;
            bottom: 0;
            background-color: #333;
            transition: .4s;
            border-radius: 34px;
        }
        
        .toggle-slider:before {
            position: absolute;
            content: "";
            height: 18px;
            width: 18px;
            right: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .toggle-slider {
            background-color: #4169e1;
        }
        
        input:checked + .toggle-slider:before {
            transform: translateX(-24px);
        }
        
        .mode-switcher {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-top: 20px;
        }
        
        .mode-btn {
            padding: 10px 20px;
            background-color: #222;
            border: none;
            color: #aaa;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            flex: 1;
        }
        
        .mode-btn.active {
            background-color: #4169e1;
            color: #fff;
        }
        
        .mode-btn:hover, .mode-btn:active {
            background-color: #333;
        }
        
        /* تصميم متجاوب */
        @media (max-width: 480px) {
            .artist-image-container {
                width: 240px;
                height: 240px;
            }
            
            .app-name {
                font-size: 1.2rem;
            }
            
            .controls {
                gap: 15px;
            }
            
            .control-btn {
                width: 45px;
                height: 45px;
                font-size: 1.3rem;
            }
            
            .play-pause {
                width: 60px;
                height: 60px;
                font-size: 1.6rem;
            }
            
            .station-item {
                width: 70px;
                height: 70px;
                margin-left: 12px;
            }
        }
        
        @media (max-height: 700px) {
            .artist-image-container {
                width: 200px;
                height: 200px;
                margin-bottom: 15px;
            }
            
            .artist-name {
                font-size: 1.5rem;
            }
            
            .controls {
                padding: 15px 0;
            }
        }
        
        /* مؤشرات التحميل */
        .loading {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 1.2rem;
            color: #aaa;
        }
        
        /* مساحة فارغة للجزء السفلي الآمن */
        .safe-area-bottom {
            height: env(safe-area-inset-bottom);
            width: 100%;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- رأس التطبيق -->
        <div class="app-header">
            <div class="app-info">
                <div class="app-logo">M</div>
                <div class="app-name">مشغل الوسائط</div>
            </div>
            <button class="settings-btn" id="settingsBtn">
                <i class="fas fa-sliders-h"></i>
            </button>
        </div>
        
        <!-- منطقة الفنان/الفيديو -->
        <div class="media-area">
            <div class="artist-image-container">
                <img id="artistImage" src="https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1000&q=80" alt="صورة الفنان">
                
                <!-- مؤثرات صوتية -->
                <div class="visualizer" id="visualizer">
                    <!-- سيتم إنشاء الأشرطة بواسطة JavaScript -->
                </div>
            </div>
            
            <!-- مشغل الفيديو (مخفي بشكل افتراضي) -->
            <div class="video-container" id="videoContainer">
                <video id="videoPlayer" controls>
                    <source src="" type="video/mp4">
                    متصفحك لا يدعم تشغيل الفيديو.
                </video>
            </div>
            
            <!-- معلومات الفنان -->
            <div class="artist-info">
                <div class="artist-name" id="artistName">أحلام</div>
                <div class="song-title" id="songTitle">أغنية مختارة</div>
            </div>
        </div>
        
        <!-- شريط التقدم -->
        <div class="progress-area">
            <div class="progress-bar" id="progressBar">
                <div class="progress" id="progress"></div>
            </div>
            <div class="time-info">
                <span id="currentTime">0:00</span>
                <span id="duration">0:00</span>
            </div>
        </div>
        
        <!-- عناصر التحكم -->
        <div class="controls">
            <button class="control-btn" id="prevBtn">
                <i class="fas fa-step-backward"></i>
            </button>
            <button class="control-btn" id="repeatBtn">
                <i class="fas fa-redo"></i>
            </button>
            <button class="control-btn play-pause" id="playPauseBtn">
                <i class="fas fa-play" id="playIcon"></i>
            </button>
            <button class="control-btn" id="shuffleBtn">
                <i class="fas fa-random"></i>
            </button>
            <button class="control-btn" id="nextBtn">
                <i class="fas fa-step-forward"></i>
            </button>
        </div>
        
        <!-- شريط المطربين ومحطات الراديو -->
        <div class="stations-bar">
            <div class="stations-title">المطربين ومحطات الراديو</div>
            <div class="stations-container" id="stationsContainer">
                <!-- سيتم إنشاء عناصر المحطات بواسطة JavaScript -->
            </div>
        </div>
        
        <!-- مساحة فارغة للجزء السفلي الآمن -->
        <div class="safe-area-bottom"></div>
    </div>
    
    <!-- نافذة الإعدادات -->
    <div class="settings-overlay" id="settingsOverlay">
        <div class="settings-container">
            <div class="settings-header">
                <div class="settings-title">الإعدادات</div>
                <button class="close-settings" id="closeSettings">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            
            <!-- مبدل الوضع -->
            <div class="mode-switcher">
                <button class="mode-btn active" id="musicModeBtn">موسيقى</button>
                <button class="mode-btn" id="radioModeBtn">راديو</button>
                <button class="mode-btn" id="videoModeBtn">فيديو</button>
            </div>
            
            <!-- إعدادات الصوت -->
            <div class="settings-section">
                <div class="section-title">
                    <i class="fas fa-volume-up"></i>
                    <span>إعدادات الصوت</span>
                </div>
                
                <div class="volume-control">
                    <div class="settings-option">
                        <div class="option-label">مستوى الصوت</div>
                        <div class="option-control">
                            <input type="range" min="0" max="100" value="70" class="volume-slider" id="volumeSlider">
                        </div>
                    </div>
                </div>
                
                <div class="equalizer-control">
                    <div class="settings-option">
                        <div class="option-label">التوازن</div>
                        <div class="option-control">
                            <input type="range" min="-10" max="10" value="0" class="equalizer-slider" id="balanceSlider">
                        </div>
                    </div>
                    
                    <div class="settings-option">
                        <div class="option-label">الجهارة</div>
                        <div class="option-control">
                            <input type="range" min="0" max="20" value="10" class="equalizer-slider" id="bassSlider">
                        </div>
                    </div>
                    
                    <div class="settings-option">
                        <div class="option-label">الجهير</div>
                        <div class="option-control">
                            <input type="range" min="0" max="20" value="10" class="equalizer-slider" id="trebleSlider">
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- إعدادات الصورة -->
            <div class="settings-section">
                <div class="section-title">
                    <i class="fas fa-image"></i>
                    <span>إعدادات الصورة</span>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">السطوع</div>
                    <div class="option-control">
                        <input type="range" min="0" max="100" value="80" class="volume-slider" id="brightnessSlider">
                    </div>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">التكبير</div>
                    <div class="option-control">
                        <input type="range" min="100" max="200" value="100" class="volume-slider" id="zoomSlider">
                    </div>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">مؤثرات الصورة</div>
                    <div class="option-control">
                        <label class="toggle-switch">
                            <input type="checkbox" id="visualEffectsToggle" checked>
                            <span class="toggle-slider"></span>
                        </label>
                    </div>
                </div>
            </div>
            
            <!-- إعدادات المشغل -->
            <div class="settings-section">
                <div class="section-title">
                    <i class="fas fa-play-circle"></i>
                    <span>إعدادات المشغل</span>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">التشغيل التلقائي</div>
                    <div class="option-control">
                        <label class="toggle-switch">
                            <input type="checkbox" id="autoPlayToggle">
                            <span class="toggle-slider"></span>
                        </label>
                    </div>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">حفظ التقدم</div>
                    <div class="option-control">
                        <label class="toggle-switch">
                            <input type="checkbox" id="saveProgressToggle" checked>
                            <span class="toggle-slider"></span>
                        </label>
                    </div>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">إظهار الكلمات</div>
                    <div class="option-control">
                        <label class="toggle-switch">
                            <input type="checkbox" id="lyricsToggle">
                            <span class="toggle-slider"></span>
                        </label>
                    </div>
                </div>
            </div>
            
            <!-- إعدادات أخرى -->
            <div class="settings-section">
                <div class="section-title">
                    <i class="fas fa-cog"></i>
                    <span>إعدادات أخرى</span>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">الوضع الليلي الدائم</div>
                    <div class="option-control">
                        <label class="toggle-switch">
                            <input type="checkbox" id="darkModeToggle" checked>
                            <span class="toggle-slider"></span>
                        </label>
                    </div>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">الإشعارات</div>
                    <div class="option-control">
                        <label class="toggle-switch">
                            <input type="checkbox" id="notificationsToggle" checked>
                            <span class="toggle-slider"></span>
                        </label>
                    </div>
                </div>
                
                <div class="settings-option">
                    <div class="option-label">جودة عالية</div>
                    <div class="option-control">
                        <label class="toggle-switch">
                            <input type="checkbox" id="highQualityToggle">
                            <span class="toggle-slider"></span>
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <script>
        // البيانات الأولية
        const stations = [
            { id: 1, name: "أحلام", type: "artist", icon: "fas fa-microphone" },
            { id: 2, name: "كاظم الساهر", type: "artist", icon: "fas fa-microphone" },
            { id: 3, name: "نوال الكويتية", type: "artist", icon: "fas fa-microphone" },
            { id: 4, name: "ماجد المهندس", type: "artist", icon: "fas fa-microphone" },
            { id: 5, name: "نبيل شعيل", type: "artist", icon: "fas fa-microphone" },
            { id: 6, name: "راشد الماجد", type: "artist", icon: "fas fa-microphone" },
            { id: 7, name: "محمد عبده", type: "artist", icon: "fas fa-microphone" },
            { id: 8, name: "إليسا", type: "artist", icon: "fas fa-microphone" },
            { id: 9, name: "نيكول سابا", type: "artist", icon: "fas fa-microphone" },
            { id: 10, name: "عمرو دياب", type: "artist", icon: "fas fa-microphone" },
            { id: 11, name: "إذاعة القرآن", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 12, name: "إذاعة الأخبار", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 13, name: "إذاعة الأغاني", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 14, name: "إذاعة الشباب", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 15, name: "إذاعة الرياضة", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 16, name: "إذاعة الثقافة", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 17, name: "إذاعة الترفيه", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 18, name: "إذاعة الطريق", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 19, name: "إذاعة السفر", type: "radio", icon: "fas fa-broadcast-tower" },
            { id: 20, name: "إذاعة الهدوء", type: "radio", icon: "fas fa-broadcast-tower" }
        ];
        
        // حالة التطبيق
        let isPlaying = false;
        let currentStation = 1;
        let currentMode = "music"; // music, radio, video
        let currentTime = 0;
        let duration = 180; // 3 دقائق
        
        // عناصر DOM
        const settingsBtn = document.getElementById('settingsBtn');
        const settingsOverlay = document.getElementById('settingsOverlay');
        const closeSettings = document.getElementById('closeSettings');
        const playPauseBtn = document.getElementById('playPauseBtn');
        const playIcon = document.getElementById('playIcon');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
        const repeatBtn = document.getElementById('repeatBtn');
        const shuffleBtn = document.getElementById('shuffleBtn');
        const progressBar = document.getElementById('progressBar');
        const progress = document.getElementById('progress');
        const currentTimeElement = document.getElementById('currentTime');
        const durationElement = document.getElementById('duration');
        const artistImage = document.getElementById('artistImage');
        const artistName = document.getElementById('artistName');
        const songTitle = document.getElementById('songTitle');
        const stationsContainer = document.getElementById('stationsContainer');
        const visualizer = document.getElementById('visualizer');
        const videoContainer = document.getElementById('videoContainer');
        const videoPlayer = document.getElementById('videoPlayer');
        
        // عناصر التحكم في الوضع
        const musicModeBtn = document.getElementById('musicModeBtn');
        const radioModeBtn = document.getElementById('radioModeBtn');
        const videoModeBtn = document.getElementById('videoModeBtn');
        
        // إنشاء أشرطة المؤثرات الصوتية
        function createVisualizerBars() {
            visualizer.innerHTML = '';
            for (let i = 0; i < 20; i++) {
                const bar = document.createElement('div');
                bar.className = 'bar';
                bar.style.animationDelay = `${i * 0.05}s`;
                visualizer.appendChild(bar);
            }
        }
        
        // إنشاء عناصر المحطات
        function createStationItems() {
            stationsContainer.innerHTML = '';
            stations.forEach(station => {
                const stationItem = document.createElement('div');
                stationItem.className = `station-item ${station.id === currentStation ? 'active' : ''}`;
                stationItem.dataset.id = station.id;
                
                stationItem.innerHTML = `
                    <div class="station-icon">
                        <i class="${station.icon}"></i>
                    </div>
                    <div class="station-name">${station.name}</div>
                `;
                
                stationItem.addEventListener('click', () => {
                    selectStation(station.id);
                });
                
                stationsContainer.appendChild(stationItem);
            });
        }
        
        // اختيار محطة
        function selectStation(stationId) {
            currentStation = stationId;
            
            // تحديث العناصر النشطة
            document.querySelectorAll('.station-item').forEach(item => {
                item.classList.remove('active');
                if (parseInt(item.dataset.id) === stationId) {
                    item.classList.add('active');
                }
            });
            
            // تحديث معلومات الفنان
            const station = stations.find(s => s.id === stationId);
            artistName.textContent = station.name;
            
            if (station.type === 'artist') {
                songTitle.textContent = 'أغنية مختارة';
                // هنا يمكنك تحميل صورة الفنان من مصدر خارجي
                // artistImage.src = `url_to_artist_image_${stationId}.jpg`;
            } else {
                songTitle.textContent = 'بث مباشر';
                // صورة افتراضية لمحطات الراديو
                artistImage.src = 'https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1000&q=80';
            }
        }
        
        // تبديل التشغيل/الإيقاف
        function togglePlayPause() {
            isPlaying = !isPlaying;
            
            if (isPlaying) {
                playIcon.className = 'fas fa-pause';
                startProgress();
                startVisualizer();
                
                // إذا كان في وضع الفيديو، قم بتشغيل الفيديو
                if (currentMode === 'video') {
                    videoPlayer.play().catch(e => console.log("فيديو غير متاح:", e));
                }
            } else {
                playIcon.className = 'fas fa-play';
                stopVisualizer();
                
                // إذا كان في وضع الفيديو، قم بإيقاف الفيديو
                if (currentMode === 'video') {
                    videoPlayer.pause();
                }
            }
        }
        
        // بدء شريط التقدم
        function startProgress() {
            if (!isPlaying) return;
            
            currentTime += 1;
            if (currentTime > duration) {
                currentTime = 0;
                // الانتقال إلى المقطع التالي
                selectStation(currentStation < stations.length ? currentStation + 1 : 1);
            }
            
            const progressPercent = (currentTime / duration) * 100;
            progress.style.width = `${progressPercent}%`;
            
            // تحديث الوقت
            currentTimeElement.textContent = formatTime(currentTime);
            durationElement.textContent = formatTime(duration);
            
            setTimeout(startProgress, 1000);
        }
        
        // تنسيق الوقت
        function formatTime(seconds) {
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        }
        
        // بدء المؤثرات الصوتية
        function startVisualizer() {
            const bars = document.querySelectorAll('.bar');
            bars.forEach(bar => {
                bar.style.animationPlayState = 'running';
            });
        }
        
        // إيقاف المؤثرات الصوتية
        function stopVisualizer() {
            const bars = document.querySelectorAll('.bar');
            bars.forEach(bar => {
                bar.style.animationPlayState = 'paused';
            });
        }
        
        // تغيير الوضع
        function changeMode(mode) {
            currentMode = mode;
            
            // تحديث أزرار الوضع
            musicModeBtn.classList.toggle('active', mode === 'music');
            radioModeBtn.classList.toggle('active', mode === 'radio');
            videoModeBtn.classList.toggle('active', mode === 'video');
            
            // التحكم في عرض الفيديو/الصورة
            if (mode === 'video') {
                artistImage.style.display = 'none';
                videoContainer.style.display = 'block';
                
                // هنا يمكنك تعيين مصدر الفيديو
                // videoPlayer.src = 'url_to_video_source.mp4';
                
                // إذا كان التشغيل قيد التشغيل، ابدأ تشغيل الفيديو
                if (isPlaying) {
                    videoPlayer.play().catch(e => console.log("فيديو غير متاح:", e));
                }
            } else {
                artistImage.style.display = 'block';
                videoContainer.style.display = 'none';
                videoPlayer.pause();
            }
        }
        
        // تهيئة التطبيق
        function initApp() {
            createVisualizerBars();
            createStationItems();
            selectStation(1);
            
            // تعيين الوقت الافتراضي
            currentTimeElement.textContent = formatTime(currentTime);
            durationElement.textContent = formatTime(duration);
            
            // إضافة مستمعي الأحداث
            settingsBtn.addEventListener('click', () => {
                settingsOverlay.style.display = 'flex';
            });
            
            closeSettings.addEventListener('click', () => {
                settingsOverlay.style.display = 'none';
            });
            
            playPauseBtn.addEventListener('click', togglePlayPause);
            
            prevBtn.addEventListener('click', () => {
                selectStation(currentStation > 1 ? currentStation - 1 : stations.length);
            });
            
            nextBtn.addEventListener('click', () => {
                selectStation(currentStation < stations.length ? currentStation + 1 : 1);
            });
            
            // التحكم في شريط التقدم
            progressBar.addEventListener('click', (e) => {
                const rect = progressBar.getBoundingClientRect();
                const clickPosition = e.clientX - rect.left;
                const width = rect.width;
                const percent = clickPosition / width;
                
                currentTime = percent * duration;
                progress.style.width = `${percent * 100}%`;
                currentTimeElement.textContent = formatTime(currentTime);
            });
            
            // أزرار تغيير الوضع
            musicModeBtn.addEventListener('click', () => changeMode('music'));
            radioModeBtn.addEventListener('click', () => changeMode('radio'));
            videoModeBtn.addEventListener('click', () => changeMode('video'));
            
            // إعداد المنزلقات
            const volumeSlider = document.getElementById('volumeSlider');
            volumeSlider.addEventListener('input', function() {
                console.log(`مستوى الصوت: ${this.value}%`);
            });
            
            // إغلاق الإعدادات بالنقر خارجها
            settingsOverlay.addEventListener('click', (e) => {
                if (e.target === settingsOverlay) {
                    settingsOverlay.style.display = 'none';
                }
            });
        }
        
        // تشغيل التطبيق عند تحميل الصفحة
        window.addEventListener('DOMContentLoaded', initApp);
        
        // إضافة مستمع للواجهة المحمولة
        window.addEventListener('resize', function() {
            // إعادة حساب المواقع إذا لزم الأمر
        });
    </script>
</body>
</html>
