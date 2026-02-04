
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
        
        /* إخفاء شريط التمرير لجميع المتصفحات */
        body::-webkit-scrollbar,
        .container::-webkit-scrollbar,
        .settings-container::-webkit-scrollbar {
            display: none;
            width: 0;
            height: 0;
        }
        
        body {
            -ms-overflow-style: none;
            scrollbar-width: none;
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
        
        /* زر الرجوع */
        .back-btn {
            background-color: rgba(60, 60, 60, 0.8);
            border: none;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            color: #fff;
            font-size: 1.2rem;
            cursor: pointer;
            display: none;
            align-items: center;
            justify-content: center;
            transition: all 0.3s ease;
            margin-left: 10px;
        }
        
        .back-btn:hover, .back-btn:active {
            background-color: rgba(65, 105, 225, 0.8);
            transform: translateX(-5px);
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
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط */
        .media-area.video-active {
            justify-content: flex-start;
            padding-top: 10px;
            background-color: transparent;
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
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط - إخفاء صورة الفنان */
        .media-area.video-active .artist-image-container {
            display: none;
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
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط - إخفاء المؤثرات */
        .media-area.video-active .visualizer {
            display: none;
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
            position: relative;
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط - توسيع الفيديو */
        .media-area.video-active .video-container {
            display: block;
            width: 100%;
            height: 65%;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.8);
        }
        
        /* حالة ملء الشاشة */
        .media-area.fullscreen-mode {
            position: fixed;
            top: 0;
            right: 0;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1000;
            background-color: #000;
            margin: 0;
            padding: 0;
            border-radius: 0;
        }
        
        .media-area.fullscreen-mode .video-container {
            width: 100%;
            height: 100%;
            border-radius: 0;
        }
        
        .media-area.fullscreen-mode .artist-info {
            display: none;
        }
        
        #videoPlayer {
            width: 100%;
            height: 100%;
            object-fit: cover;
            background-color: #000;
        }
        
        /* معلومات الفنان */
        .artist-info {
            text-align: center;
            margin-top: 10px;
            padding: 10px;
            width: 100%;
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط - تعديل معلومات الفنان */
        .media-area.video-active .artist-info {
            margin-top: 20px;
            padding: 15px;
            background-color: rgba(20, 20, 20, 0.7);
            border-radius: 15px;
            width: 90%;
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
        
        /* زر الخروج من ملء الشاشة */
        .exit-fullscreen-btn {
            position: absolute;
            top: 20px;
            left: 20px;
            z-index: 1001;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 1.2rem;
            display: none;
            transition: all 0.3s ease;
        }
        
        .exit-fullscreen-btn:hover {
            background-color: rgba(65, 105, 225, 0.8);
            transform: scale(1.1);
        }
        
        /* شريط التقدم */
        .progress-area {
            width: 100%;
            padding: 15px 0;
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط - إخفاء شريط التقدم مؤقتاً */
        .media-area.video-active ~ .progress-area {
            opacity: 0;
            height: 0;
            padding: 0;
            margin: 0;
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
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط - إخفاء عناصر التحكم مؤقتاً */
        .media-area.video-active ~ .controls {
            opacity: 0;
            height: 0;
            padding: 0;
            margin: 0;
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
        
        /* قسم اختيار النوع */
        .content-type-selector {
            width: 100%;
            padding: 10px 0;
            margin: 10px 0;
            display: flex;
            justify-content: center;
            gap: 15px;
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط - إخفاء قسم الاختيار مؤقتاً */
        .media-area.video-active ~ .content-type-selector {
            opacity: 0;
            height: 0;
            padding: 0;
            margin: 0;
        }
        
        .type-btn {
            padding: 10px 20px;
            background-color: rgba(40, 40, 40, 0.9);
            border: none;
            color: #aaa;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1rem;
            font-weight: 600;
            min-width: 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        
        .type-btn:hover, .type-btn:active {
            background-color: rgba(65, 105, 225, 0.5);
            transform: translateY(-2px);
        }
        
        .type-btn.active {
            background-color: rgba(138, 43, 226, 0.8);
            color: #fff;
            box-shadow: 0 5px 15px rgba(138, 43, 226, 0.4);
        }
        
        /* شريط المحتوى */
        .content-bar {
            width: 100%;
            padding: 15px 0;
            margin-top: 10px;
            margin-bottom: 20px;
            transition: all 0.5s ease;
        }
        
        /* حالة الفيديو النشط - إخفاء شريط المحتوى مؤقتاً */
        .media-area.video-active ~ .content-bar {
            opacity: 0;
            height: 0;
            padding: 0;
            margin: 0;
        }
        
        .content-title {
            font-size: 1.1rem;
            margin-bottom: 12px;
            color: #ddd;
            padding-right: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .add-more-btn {
            background: rgba(65, 105, 225, 0.3);
            border: none;
            color: #fff;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 0.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .add-more-btn:hover {
            background: rgba(65, 105, 225, 0.7);
            transform: scale(1.05);
        }
        
        .content-container {
            width: 100%;
            overflow-x: auto;
            overflow-y: hidden;
            white-space: nowrap;
            padding: 10px 5px;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: thin;
            scrollbar-color: #4169e1 #222;
        }
        
        /* تخصيص شريط التمرير */
        .content-container::-webkit-scrollbar {
            height: 6px;
        }
        
        .content-container::-webkit-scrollbar-track {
            background: #222;
            border-radius: 3px;
        }
        
        .content-container::-webkit-scrollbar-thumb {
            background: linear-gradient(to right, #8a2be2, #4169e1);
            border-radius: 3px;
        }
        
        .content-container::-webkit-scrollbar-thumb:hover {
            background: linear-gradient(to right, #4169e1, #8a2be2);
        }
        
        .content-item {
            display: inline-block;
            width: 80px;
            height: 100px; /* زيادة الارتفاع لاستيعاب الصورة */
            margin-left: 15px;
            border-radius: 12px;
            background-color: rgba(40, 40, 40, 0.9);
            display: inline-flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-start;
            text-align: center;
            color: #fff;
            font-size: 0.8rem;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .content-item:first-child {
            margin-right: 0;
        }
        
        .content-item:hover, .content-item:active {
            background-color: rgba(65, 105, 225, 0.7);
            transform: translateY(-5px);
        }
        
        .content-item.active {
            background-color: rgba(138, 43, 226, 0.8);
            box-shadow: 0 5px 15px rgba(138, 43, 226, 0.4);
        }
        
        .content-image {
            width: 100%;
            height: 60px;
            object-fit: cover;
            border-radius: 10px 10px 0 0;
            margin-bottom: 8px;
        }
        
        .content-icon {
            font-size: 1.5rem;
            margin-bottom: 8px;
        }
        
        .content-name {
            white-space: normal;
            max-height: 30px;
            overflow: hidden;
            padding: 0 5px;
            font-size: 0.75rem;
        }
        
        /* مؤشر تحميل الصوت */
        .audio-loading {
            position: absolute;
            top: 5px;
            left: 5px;
            width: 15px;
            height: 15px;
            border: 2px solid #4169e1;
            border-top-color: transparent;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            display: none;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        /* مؤشر ملء الشاشة */
        .fullscreen-indicator {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 1rem;
            z-index: 1001;
            display: none;
            animation: fadeInOut 2s ease;
        }
        
        @keyframes fadeInOut {
            0%, 100% { opacity: 0; }
            10%, 90% { opacity: 1; }
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
            
            .content-item {
                width: 70px;
                height: 90px;
                margin-left: 12px;
            }
            
            .type-btn {
                padding: 8px 15px;
                min-width: 80px;
                font-size: 0.9rem;
            }
            
            .media-area.video-active .video-container {
                height: 55%;
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
            
            .media-area.video-active .video-container {
                height: 60%;
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
            <div style="display: flex; align-items: center;">
                <button class="back-btn" id="backBtn">
                    <i class="fas fa-arrow-right"></i>
                </button>
                <button class="settings-btn" id="settingsBtn">
                    <i class="fas fa-sliders-h"></i>
                </button>
            </div>
        </div>
        
        <!-- منطقة الفنان/الفيديو -->
        <div class="media-area" id="mediaArea">
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
            
            <!-- زر الخروج من ملء الشاشة -->
            <button class="exit-fullscreen-btn" id="exitFullscreenBtn">
                <i class="fas fa-compress"></i>
            </button>
            
            <!-- مؤشر ملء الشاشة -->
            <div class="fullscreen-indicator" id="fullscreenIndicator">
                <i class="fas fa-expand"></i> ملء الشاشة
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
        
        <!-- اختيار نوع المحتوى -->
        <div class="content-type-selector">
            <button class="type-btn active" id="radioTypeBtn">
                <i class="fas fa-broadcast-tower"></i>
                راديو
            </button>
            <button class="type-btn" id="musicTypeBtn">
                <i class="fas fa-music"></i>
                أغاني
            </button>
            <button class="type-btn" id="videoTypeBtn">
                <i class="fas fa-video"></i>
                فيديو
            </button>
        </div>
        
        <!-- شريط المحتوى الديناميكي -->
        <div class="content-bar">
            <div class="content-title">
                <span id="contentTitle">محطات الراديو</span>
                <button class="add-more-btn" id="addMoreBtn" style="display: none;">
                    <i class="fas fa-plus"></i> إضافة المزيد
                </button>
            </div>
            <div class="content-container" id="contentContainer">
                <!-- سيتم إنشاء عناصر المحتوى بواسطة JavaScript -->
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
    
    <!-- عنصر صوتي مخفي -->
    <audio id="audioPlayer" style="display: none;"></audio>
    
    <script>
        // البيانات الأولية مع روابط صوت حقيقية للراديو (20 محطة)
        const radioStations = [
            { id: 1, name: "LB طرب", type: "راديو", icon: "fas fa-microphone", image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQX12GI9uvjX-fN6StHaAe2WyNLVAhrOhmWkw&s", url: "https://stream.zeno.fm/qngq01c4mfhvv.mp3", duration: "مباشر" },
            { id: 2, name: "FM طرب", type: "راديو", icon: "fas fa-microphone", image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTAHb8V-_pReH14jreKRE6cJwt6CiQGeGImdw&s", url: "https://stream.zeno.fm/4nnehdnvpg8uv.mp3", duration: "مباشر" },
            { id: 3, name: "الرجال", type: "راديو", icon: "fas fa-microphone", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/wrfc0nkbl2wuv.mp3", duration: "مباشر" },
            { id: 4, name: "FM ميلودي", type: "راديو", icon: "fas fa-microphone", image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTkmoG8iI3dP_TQV-zvvjHTMRVG6iH4UcNOiw&s", url: "https://stream.zeno.fm/3gkedytbapbuv.mp3", duration: "مباشر" },
            { id: 5, name: "FM ستار", type: "راديو", icon: "fas fa-microphone", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/qv4bv7t3afhvv.mp3", duration: "مباشر" },
            { id: 6, name: "نغم الطرب", type: "راديو", icon: "fas fa-microphone", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/4nnehdnvpg8uv.mp3", duration: "مباشر" },
            { id: 7, name: "FM Arta", type: "راديو", icon: "fas fa-microphone", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "http://edge.mixlr.com/channel/qtgru.mp3", duration: "مباشر" },
            { id: 8, name: "FM فرح", type: "راديو", icon: "fas fa-microphone", image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQQwHVhkYPrZ9RYBTs4NtKC3Jt-DU8UGrwxvg&s", url: "http://radio.farah.fm/.mp3", duration: "مباشر" },
            { id: 9, name: "FM شام", type: "راديو", icon: "fas fa-microphone", image: "https://static2.mytuner.mobi/media/tvos_radios/765/sham-fm-dh-shm-f-m.af4e42e5.jpg", url: "http://radioshamfm.grtvstream.com:8400/.mp3", duration: "مباشر" },
            { id: 10, name: "فيروز", type: "راديو", icon: "fas fa-microphone", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/zv3e5wykprhvv.mp3", duration: "مباشر" },
            { id: 11, name: "ملحم بركات", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/gwhk3u1keuhvv.mp3", duration: "مباشر" },
            { id: 12, name: "طرب زمان", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "http://live.medi1.com/Tarab#.mp3", duration: "مباشر" },
            { id: 13, name: "اليسا", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/kpkvku1ssnhvv.mp3", duration: "مباشر" },
            { id: 14, name: "عرب ميكس", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/efx5psd00qruv.mp3", duration: "مباشر" },
            { id: 15, name: "ميلودي", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/3gkedytbapbuv.mp3", duration: "مباشر" },
            { id: 16, name: "اربيسك", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "http://185.4.87.79:8000/stream.mp3", duration: "مباشر" },
            { id: 17, name: "عرب", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "http://arabnights-prod.live-streams.nl:8020/live.mp3", duration: "مباشر" },
            { id: 18, name: "روتانا Mix", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "http://curiosity.shoutca.st:6035/.mp3", duration: "مباشر" },
            { id: 19, name: "صوت الغد", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://sawtelghad.org:7001/.mp3", duration: "مباشر" },
            { id: 20, name: "جيكوغا", type: "راديو", icon: "fas fa-broadcast-tower", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "http://37.26.69.30:8080/.mp3", duration: "مباشر" }
        ];
        
        // بيانات الأغاني مع روابط صوت حقيقية (20 أغنية)
        let songs = [
            { id: 1, name: "أحلام", title: "أغنية 1", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3", duration: 180 },
            { id: 2, name: "كاظم الساهر", title: "أغنية 2", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3", duration: 210 },
            { id: 3, name: "نجوى كرم", title: "أغنية 3", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3", duration: 195 },
            { id: 4, name: "ماجد المهندس", title: "أغنية 4", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-4.mp3", duration: 240 },
            { id: 5, name: "نوال الكويتية", title: "أغنية 5", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-5.mp3", duration: 185 },
            { id: 6, name: "عمرو دياب", title: "أغنية 6", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-6.mp3", duration: 220 },
            { id: 7, name: "ناصيف زيتون", title: "أغنية 7", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-7.mp3", duration: 190 },
            { id: 8, name: "عاصي الحلاني", title: "أغنية 8", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-8.mp3", duration: 205 },
            { id: 9, name: "وائل جسار", title: "أغنية 9", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-9.mp3", duration: 215 },
            { id: 10, name: "ناريمان", title: "أغنية 10", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-10.mp3", duration: 195 },
            { id: 11, name: "محمد عبده", title: "أغنية 11", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-11.mp3", duration: 200 },
            { id: 12, name: "راشد الماجد", title: "أغنية 12", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-12.mp3", duration: 185 },
            { id: 13, name: "عبدالمجيد عبدالله", title: "أغنية 13", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3", duration: 220 },
            { id: 14, name: "مشاعل", title: "أغنية 14", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3", duration: 195 },
            { id: 15, name: "علي عبدالكريم", title: "أغنية 15", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3", duration: 210 },
            { id: 16, name: "لطيفة", title: "أغنية 16", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-4.mp3", duration: 180 },
            { id: 17, name: "صابر الرباعي", title: "أغنية 17", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-5.mp3", duration: 195 },
            { id: 18, name: "شذى حسون", title: "أغنية 18", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-6.mp3", duration: 220 },
            { id: 19, name: "محمد العريفي", title: "أغنية 19", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-7.mp3", duration: 240 },
            { id: 20, name: "نوال الزغبي", title: "أغنية 20", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-8.mp3", duration: 190 }
        ];
        
        // بيانات الفيديو (20 فيديو)
        let videos = [
            { id: 1, name: "فيديو قصير", title: "افلام كرتون", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/BigBuckBunny.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4", duration: 180 },
            { id: 2, name: "فيديو 2", title: "كرتون", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ElephantsDream.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4", duration: 210 },
            { id: 3, name: "فيديو 3", title: "حفلات مباشرة", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ForBiggerBlazes.jpg", url: "http://gifted-satoshi.192-185-7-210.plesk.page.192-185-7-210.hgws27.hgwin.temp.domains/api/ProveAttachmentApi/open-prove-attachment/765b0ad0-20c5-4a99-f798-08dcef7f61b1", duration: 195 },
            { id: 4, name: "فيديو 4", title: "أغاني مصورة", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 240 },
            { id: 5, name: "فيديو 5", title: "عروض موسيقية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 185 },
            { id: 6, name: "فيديو 6", title: "مقاطع ترفيهية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 220 },
            { id: 7, name: "فيديو 7", title: "برامج موسيقية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 190 },
            { id: 8, name: "فيديو 8", title: "عروض راقصة", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 205 },
            { id: 9, name: "فيديو 9", title: "حفلات غنائية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 215 },
            { id: 10, name: "فيديو 10", title: "مقاطع تعليمية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 195 },
            { id: 11, name: "فيديو 11", title: "حفلات حية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 200 },
            { id: 12, name: "فيديو 12", title: "عروض فنية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 185 },
            { id: 13, name: "فيديو 13", title: "مقاطع تاريخية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 220 },
            { id: 14, name: "فيديو 14", title: "عروض مسرحية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 195 },
            { id: 15, name: "فيديو 15", title: "حفلات عالمية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 210 },
            { id: 16, name: "فيديو 16", title: "مقاطع كوميدية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 180 },
            { id: 17, name: "فيديو 17", title: "عروض سحرية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 195 },
            { id: 18, name: "فيديو 18", title: "حفلات أوركسترا", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 220 },
            { id: 19, name: "فيديو 19", title: "مقاطع وثائقية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 240 },
            { id: 20, name: "فيديو 20", title: "عروض ضوئية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "", duration: 190 }
        ];
        
        // حالة التطبيق
        let isPlaying = false;
        let currentContentId = 1;
        let currentContentType = "radio"; // radio, music, video
        let currentTime = 0;
        let duration = 0;
        let progressInterval = null;
        let fullscreenTimeout = null;
        let isFullscreen = false;
        
        // عناصر DOM
        const settingsBtn = document.getElementById('settingsBtn');
        const backBtn = document.getElementById('backBtn');
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
        const contentContainer = document.getElementById('contentContainer');
        const contentTitle = document.getElementById('contentTitle');
        const addMoreBtn = document.getElementById('addMoreBtn');
        const visualizer = document.getElementById('visualizer');
        const videoContainer = document.getElementById('videoContainer');
        const videoPlayer = document.getElementById('videoPlayer');
        const audioPlayer = document.getElementById('audioPlayer');
        const mediaArea = document.getElementById('mediaArea');
        const exitFullscreenBtn = document.getElementById('exitFullscreenBtn');
        const fullscreenIndicator = document.getElementById('fullscreenIndicator');
        
        // أزرار اختيار النوع
        const radioTypeBtn = document.getElementById('radioTypeBtn');
        const musicTypeBtn = document.getElementById('musicTypeBtn');
        const videoTypeBtn = document.getElementById('videoTypeBtn');
        
        // عناصر التحكم في الوضع (من الإعدادات)
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
        
        // إنشاء عناصر المحتوى بناءً على النوع
        function createContentItems(contentType, itemsToShow = null) {
            contentContainer.innerHTML = '';
            let items = [];
            
            switch(contentType) {
                case 'radio':
                    items = itemsToShow || radioStations;
                    contentTitle.textContent = 'محطات الراديو';
                    addMoreBtn.style.display = 'none';
                    break;
                case 'music':
                    items = itemsToShow || songs.slice(0, 20); // عرض 20 أغنية
                    contentTitle.textContent = `قائمة الأغاني (${songs.length})`;
                    addMoreBtn.style.display = 'flex';
                    break;
                case 'video':
                    items = itemsToShow || videos.slice(0, 20); // عرض 20 فيديو
                    contentTitle.textContent = `مقاطع الفيديو (${videos.length})`;
                    addMoreBtn.style.display = 'flex';
                    break;
            }
            
            items.forEach(item => {
                const contentItem = document.createElement('div');
                contentItem.className = `content-item ${item.id === currentContentId && currentContentType === contentType ? 'active' : ''}`;
                contentItem.dataset.id = item.id;
                contentItem.dataset.type = contentType;
                
                // إضافة مؤشر تحميل
                const loadingIndicator = document.createElement('div');
                loadingIndicator.className = 'audio-loading';
                contentItem.appendChild(loadingIndicator);
                
                // إنشاء عنصر الصورة
                let imageElement = '';
                if (item.image) {
                    imageElement = `<img class="content-image" src="${item.image}" alt="${item.name}" onerror="this.src='https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80'">`;
                } else {
                    imageElement = `<div class="content-icon"><i class="${item.icon}"></i></div>`;
                }
                
                contentItem.innerHTML += `
                    ${imageElement}
                    <div class="content-name">${item.name}</div>
                `;
                
                contentItem.addEventListener('click', () => {
                    selectContent(item.id, contentType);
                });
                
                contentContainer.appendChild(contentItem);
            });
        }
        
        // تشغيل المحتوى مباشرة عند الاختيار
        async function selectContent(contentId, contentType) {
            // إظهار مؤشر التحميل
            showLoadingIndicator(contentId, contentType, true);
            
            // تحديث العناصر النشطة
            document.querySelectorAll('.content-item').forEach(item => {
                item.classList.remove('active');
                if (parseInt(item.dataset.id) === contentId && item.dataset.type === contentType) {
                    item.classList.add('active');
                }
            });
            
            currentContentId = contentId;
            currentContentType = contentType;
            
            let item = null;
            switch(contentType) {
                case 'radio':
                    item = radioStations.find(s => s.id === contentId);
                    artistName.textContent = item.name;
                    songTitle.textContent = 'بث مباشر';
                    artistImage.src = item.image || 'https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=1000&q=80';
                    
                    // إعداد مشغل الراديو
                    setupRadioPlayer(item.url);
                    break;
                    
                case 'music':
                    item = songs.find(s => s.id === contentId);
                    artistName.textContent = item.name;
                    songTitle.textContent = item.title;
                    artistImage.src = item.image || 'https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=1000&q=80';
                    
                    // إعداد مشغل الأغاني
                    setupMusicPlayer(item.url, item.duration);
                    break;
                    
                case 'video':
                    item = videos.find(v => v.id === contentId);
                    artistName.textContent = item.name;
                    songTitle.textContent = item.title;
                    
                    // إعداد واجهة الفيديو
                    setupVideoInterface();
                    
                    // إعداد مشغل الفيديو
                    setupVideoPlayer(item.url, item.duration);
                    break;
            }
            
            // إخفاء مؤشر التحميل بعد فترة قصيرة
            setTimeout(() => {
                showLoadingIndicator(contentId, contentType, false);
            }, 1000);
        }
        
        // إعداد واجهة الفيديو
        function setupVideoInterface() {
            // إخفاء صورة الفنان وإظهار الفيديو
            artistImage.style.display = 'none';
            videoContainer.style.display = 'block';
            
            // إضافة حالة الفيديو النشط
            mediaArea.classList.add('video-active');
            
            // إظهار زر الرجوع
            backBtn.style.display = 'flex';
            
            // إذا كان هناك وقت تشغيل سابق، أزله
            if (fullscreenTimeout) {
                clearTimeout(fullscreenTimeout);
            }
            
            // بعد 5 ثواني، قم بتشغيل ملء الشاشة
            fullscreenTimeout = setTimeout(() => {
                enterFullscreenMode();
            }, 5000);
        }
        
        // تشغيل وضع ملء الشاشة
        function enterFullscreenMode() {
            if (isFullscreen) return;
            
            isFullscreen = true;
            mediaArea.classList.add('fullscreen-mode');
            exitFullscreenBtn.style.display = 'flex';
            
            // إخفاء زر الرجوع في وضع ملء الشاشة
            backBtn.style.display = 'none';
            
            // إظهار مؤشر ملء الشاشة
            fullscreenIndicator.style.display = 'block';
            setTimeout(() => {
                fullscreenIndicator.style.display = 'none';
            }, 2000);
            
            // إخفاء رأس التطبيق وعناصر التحكم
            document.querySelector('.app-header').style.display = 'none';
            
            // إضافة حدث للخروج من ملء الشاشة عند النقر على الفيديو
            videoPlayer.addEventListener('click', toggleFullscreen);
        }
        
        // الخروج من وضع ملء الشاشة
        function exitFullscreenMode() {
            if (!isFullscreen) return;
            
            isFullscreen = false;
            mediaArea.classList.remove('fullscreen-mode');
            exitFullscreenBtn.style.display = 'none';
            
            // إظهار زر الرجوع بعد الخروج من ملء الشاشة
            if (currentContentType === 'video') {
                backBtn.style.display = 'flex';
            }
            
            // إظهار رأس التطبيق
            document.querySelector('.app-header').style.display = 'flex';
            
            // إزالة حدث النقر
            videoPlayer.removeEventListener('click', toggleFullscreen);
        }
        
        // تبديل وضع ملء الشاشة
        function toggleFullscreen() {
            if (isFullscreen) {
                exitFullscreenMode();
            } else {
                enterFullscreenMode();
            }
        }
        
        // العودة إلى الصفحة الرئيسية
        function goBackToHome() {
            if (currentContentType === 'video') {
                // إيقاف الفيديو
                stopAllPlayers();
                
                // إزالة حالة الفيديو النشط
                mediaArea.classList.remove('video-active');
                mediaArea.classList.remove('fullscreen-mode');
                exitFullscreenBtn.style.display = 'none';
                
                // إعادة إظهار صورة الفنان
                artistImage.style.display = 'block';
                videoContainer.style.display = 'none';
                
                // إخفاء زر الرجوع
                backBtn.style.display = 'none';
                
                // إلغاء وقت ملء الشاشة
                if (fullscreenTimeout) {
                    clearTimeout(fullscreenTimeout);
                    fullscreenTimeout = null;
                }
                
                // إعادة تعيين المحتوى
                artistName.textContent = "أحلام";
                songTitle.textContent = "أغنية مختارة";
                artistImage.src = "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=1000&q=80";
                
                // إعادة تشغيل العناصر المخفية
                document.querySelectorAll('.progress-area, .controls, .content-type-selector, .content-bar').forEach(el => {
                    el.style.opacity = '1';
                    el.style.height = 'auto';
                    el.style.padding = '';
                    el.style.margin = '';
                });
                
                // إعادة تعيين التشغيل
                isPlaying = false;
                playIcon.className = 'fas fa-play';
            }
        }
        
        // إضافة المزيد من العناصر
        function addMoreItems() {
            let items = [];
            let startIndex = 0;
            
            switch(currentContentType) {
                case 'music':
                    const currentMusicCount = document.querySelectorAll('.content-item').length;
                    if (currentMusicCount < songs.length) {
                        startIndex = currentMusicCount;
                        items = songs.slice(startIndex, startIndex + 10);
                        createContentItems('music', [...songs.slice(0, startIndex), ...items]);
                    } else {
                        alert('تم عرض جميع الأغاني المتاحة');
                    }
                    break;
                    
                case 'video':
                    const currentVideoCount = document.querySelectorAll('.content-item').length;
                    if (currentVideoCount < videos.length) {
                        startIndex = currentVideoCount;
                        items = videos.slice(startIndex, startIndex + 10);
                        createContentItems('video', [...videos.slice(0, startIndex), ...items]);
                    } else {
                        alert('تم عرض جميع الفيديوهات المتاحة');
                    }
                    break;
            }
        }
        
        // إضافة أغنية جديدة
        function addNewSong(name, title, image, url, duration = 180) {
            const newId = songs.length > 0 ? Math.max(...songs.map(s => s.id)) + 1 : 1;
            const newSong = {
                id: newId,
                name: name,
                title: title,
                icon: "fas fa-music",
                image: image,
                url: url,
                duration: duration
            };
            songs.push(newSong);
            createContentItems('music');
            return newId;
        }
        
        // إضافة فيديو جديد
        function addNewVideo(name, title, image, url, duration = 180) {
            const newId = videos.length > 0 ? Math.max(...videos.map(v => v.id)) + 1 : 1;
            const newVideo = {
                id: newId,
                name: name,
                title: title,
                icon: "fas fa-video",
                image: image,
                url: url,
                duration: duration
            };
            videos.push(newVideo);
            createContentItems('video');
            return newId;
        }
        
        // إظهار/إخفاء مؤشر التحميل
        function showLoadingIndicator(contentId, contentType, show) {
            const items = document.querySelectorAll('.content-item');
            items.forEach(item => {
                if (parseInt(item.dataset.id) === contentId && item.dataset.type === contentType) {
                    const loading = item.querySelector('.audio-loading');
                    if (loading) {
                        loading.style.display = show ? 'block' : 'none';
                    }
                }
            });
        }
        
        // إعداد مشغل الراديو
        function setupRadioPlayer(url) {
            // إيقاف أي تشغيل سابق
            stopAllPlayers();
            
            // إزالة حالة الفيديو النشط
            mediaArea.classList.remove('video-active');
            mediaArea.classList.remove('fullscreen-mode');
            exitFullscreenBtn.style.display = 'none';
            artistImage.style.display = 'block';
            
            // إخفاء زر الرجوع
            backBtn.style.display = 'none';
            
            // إلغاء وقت ملء الشاشة
            if (fullscreenTimeout) {
                clearTimeout(fullscreenTimeout);
                fullscreenTimeout = null;
            }
            
            if (url) {
                audioPlayer.src = url;
                audioPlayer.load();
                
                // محاولة تشغيل الصوت
                audioPlayer.play().then(() => {
                    isPlaying = true;
                    playIcon.className = 'fas fa-pause';
                    startVisualizer();
                    startProgressForRadio();
                }).catch(error => {
                    console.log("خطأ في تشغيل الراديو:", error);
                    playPauseBtn.addEventListener('click', tryPlayRadio, { once: true });
                });
            }
            
            // إعداد الوقت للراديو (مباشر)
            currentTime = 0;
            duration = 0;
            updateTimeDisplay();
        }
        
        // محاولة تشغيل الراديو بعد تفاعل المستخدم
        function tryPlayRadio() {
            audioPlayer.play().then(() => {
                isPlaying = true;
                playIcon.className = 'fas fa-pause';
                startVisualizer();
                startProgressForRadio();
            });
        }
        
        // إعداد مشغل الأغاني
        function setupMusicPlayer(url, songDuration) {
            // إيقاف أي تشغيل سابق
            stopAllPlayers();
            
            // إزالة حالة الفيديو النشط
            mediaArea.classList.remove('video-active');
            mediaArea.classList.remove('fullscreen-mode');
            exitFullscreenBtn.style.display = 'none';
            artistImage.style.display = 'block';
            
            // إخفاء زر الرجوع
            backBtn.style.display = 'none';
            
            // إلغاء وقت ملء الشاشة
            if (fullscreenTimeout) {
                clearTimeout(fullscreenTimeout);
                fullscreenTimeout = null;
            }
            
            if (url) {
                audioPlayer.src = url;
                audioPlayer.load();
                
                audioPlayer.addEventListener('loadedmetadata', () => {
                    duration = audioPlayer.duration;
                    updateTimeDisplay();
                });
                
                // تشغيل الصوت مباشرة
                audioPlayer.play().then(() => {
                    isPlaying = true;
                    playIcon.className = 'fas fa-pause';
                    startVisualizer();
                    startProgress();
                }).catch(error => {
                    console.log("خطأ في تشغيل الأغنية:", error);
                });
            } else {
                duration = songDuration || 180;
                currentTime = 0;
                updateTimeDisplay();
                
                // محاكاة التشغيل
                isPlaying = true;
                playIcon.className = 'fas fa-pause';
                startVisualizer();
                startProgress();
            }
        }
        
        // إعداد مشغل الفيديو
        function setupVideoPlayer(url, videoDuration) {
            // إيقاف أي تشغيل سابق
            stopAllPlayers();
            
            videoPlayer.src = url || '';
            videoPlayer.load();
            
            // تشغيل الفيديو مباشرة
            videoPlayer.play().then(() => {
                isPlaying = true;
                playIcon.className = 'fas fa-pause';
                startVisualizer();
                startProgressForVideo();
            }).catch(error => {
                console.log("خطأ في تشغيل الفيديو:", error);
                
                // إذا فشل التشغيل، استخدم المدة الافتراضية
                duration = videoDuration || 180;
                currentTime = 0;
                updateTimeDisplay();
                
                // محاكاة التشغيل للفيديو
                isPlaying = true;
                playIcon.className = 'fas fa-pause';
                startVisualizer();
                startProgress();
            });
        }
        
        // إيقاف جميع المشغلات
        function stopAllPlayers() {
            clearInterval(progressInterval);
            audioPlayer.pause();
            videoPlayer.pause();
            stopVisualizer();
        }
        
        // بدء التقدم للراديو
        function startProgressForRadio() {
            clearInterval(progressInterval);
            currentTime = 0;
            progress.style.width = '0%';
            
            progressInterval = setInterval(() => {
                currentTime++;
                updateTimeDisplayForRadio();
            }, 1000);
        }
        
        // بدء التقدم العادي
        function startProgress() {
            clearInterval(progressInterval);
            currentTime = 0;
            progress.style.width = '0%';
            
            progressInterval = setInterval(() => {
                if (currentTime >= duration) {
                    clearInterval(progressInterval);
                    isPlaying = false;
                    playIcon.className = 'fas fa-play';
                    stopVisualizer();
                    return;
                }
                
                currentTime++;
                updateTimeDisplay();
                
                const progressPercent = (currentTime / duration) * 100;
                progress.style.width = `${progressPercent}%`;
            }, 1000);
        }
        
        // بدء التقدم للفيديو
        function startProgressForVideo() {
            clearInterval(progressInterval);
            
            progressInterval = setInterval(() => {
                if (videoPlayer.ended) {
                    clearInterval(progressInterval);
                    isPlaying = false;
                    playIcon.className = 'fas fa-play';
                    stopVisualizer();
                    
                    // العودة إلى الصفحة الرئيسية عند انتهاء الفيديو
                    goBackToHome();
                    return;
                }
                
                currentTime = videoPlayer.currentTime;
                duration = videoPlayer.duration;
                updateTimeDisplay();
                
                const progressPercent = (currentTime / duration) * 100;
                progress.style.width = `${progressPercent}%`;
            }, 100);
        }
        
        // تحديث عرض الوقت للراديو
        function updateTimeDisplayForRadio() {
            currentTimeElement.textContent = formatTime(currentTime);
            durationElement.textContent = "مباشر";
        }
        
        // تحديث عرض الوقت
        function updateTimeDisplay() {
            currentTimeElement.textContent = formatTime(currentTime);
            
            if (duration > 0) {
                durationElement.textContent = formatTime(duration);
            } else {
                durationElement.textContent = "مباشر";
            }
        }
        
        // تنسيق الوقت
        function formatTime(seconds) {
            if (seconds === 0) return "0:00";
            
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
        }
        
        // تبديل التشغيل/الإيقاف
        function togglePlayPause() {
            isPlaying = !isPlaying;
            
            if (isPlaying) {
                playIcon.className = 'fas fa-pause';
                startVisualizer();
                
                // تشغيل حسب نوع المحتوى
                switch(currentContentType) {
                    case 'radio':
                        audioPlayer.play().catch(e => console.log("خطأ في تشغيل الراديو:", e));
                        startProgressForRadio();
                        break;
                    case 'music':
                        audioPlayer.play().catch(e => console.log("خطأ في تشغيل الأغنية:", e));
                        startProgress();
                        break;
                    case 'video':
                        videoPlayer.play().catch(e => console.log("خطأ في تشغيل الفيديو:", e));
                        startProgressForVideo();
                        break;
                }
            } else {
                playIcon.className = 'fas fa-play';
                stopVisualizer();
                
                // إيقاف حسب نوع المحتوى
                switch(currentContentType) {
                    case 'radio':
                        audioPlayer.pause();
                        break;
                    case 'music':
                        audioPlayer.pause();
                        break;
                    case 'video':
                        videoPlayer.pause();
                        break;
                }
                
                clearInterval(progressInterval);
            }
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
        
        // تغيير نوع المحتوى
        function changeContentType(type) {
            // إذا كنا في وضع الفيديو، أخرج منه
            if (currentContentType === 'video' && type !== 'video') {
                goBackToHome();
            }
            
            currentContentType = type;
            
            // تحديث أزرار النوع
            radioTypeBtn.classList.toggle('active', type === 'radio');
            musicTypeBtn.classList.toggle('active', type === 'music');
            videoTypeBtn.classList.toggle('active', type === 'video');
            
            // إعادة تعيين المحتوى النشط
            currentContentId = 1;
            
            // إنشاء عناصر جديدة
            createContentItems(type);
            
            // تحديد العنصر الأول وتشغيله
            selectContent(1, type);
        }
        
        // التعامل مع تغيير شريط التقدم
        function handleProgressBarClick(e) {
            const rect = progressBar.getBoundingClientRect();
            const clickPosition = e.clientX - rect.left;
            const width = rect.width;
            const percent = clickPosition / width;
            
            // تحديث حسب نوع المحتوى
            if (currentContentType === 'video') {
                if (videoPlayer.duration) {
                    videoPlayer.currentTime = percent * videoPlayer.duration;
                    currentTime = videoPlayer.currentTime;
                }
            } else if (duration > 0) {
                currentTime = percent * duration;
                if (currentContentType === 'music') {
                    audioPlayer.currentTime = currentTime;
                }
            }
            
            // تحديث العرض
            const progressPercent = percent * 100;
            progress.style.width = `${progressPercent}%`;
            updateTimeDisplay();
        }
        
        // تهيئة التطبيق
        function initApp() {
            createVisualizerBars();
            createContentItems('radio');
            selectContent(1, 'radio');
            
            // تعيين الوقت الافتراضي
            updateTimeDisplay();
            
            // إضافة مستمعي الأحداث
            settingsBtn.addEventListener('click', () => {
                settingsOverlay.style.display = 'flex';
            });
            
            backBtn.addEventListener('click', goBackToHome);
            
            closeSettings.addEventListener('click', () => {
                settingsOverlay.style.display = 'none';
            });
            
            playPauseBtn.addEventListener('click', togglePlayPause);
            
            prevBtn.addEventListener('click', () => {
                let items = [];
                
                switch(currentContentType) {
                    case 'radio': items = radioStations; break;
                    case 'music': items = songs; break;
                    case 'video': items = videos; break;
                }
                
                let prevId = currentContentId > 1 ? currentContentId - 1 : items.length;
                selectContent(prevId, currentContentType);
            });
            
            nextBtn.addEventListener('click', () => {
                let items = [];
                
                switch(currentContentType) {
                    case 'radio': items = radioStations; break;
                    case 'music': items = songs; break;
                    case 'video': items = videos; break;
                }
                
                let nextId = currentContentId < items.length ? currentContentId + 1 : 1;
                selectContent(nextId, currentContentType);
            });
            
            // أزرار تغيير نوع المحتوى
            radioTypeBtn.addEventListener('click', () => changeContentType('radio'));
            musicTypeBtn.addEventListener('click', () => changeContentType('music'));
            videoTypeBtn.addEventListener('click', () => changeContentType('video'));
            
            // زر إضافة المزيد
            addMoreBtn.addEventListener('click', addMoreItems);
            
            // زر الخروج من ملء الشاشة
            exitFullscreenBtn.addEventListener('click', exitFullscreenMode);
            
            // أزرار تغيير الوضع (من الإعدادات)
            musicModeBtn.addEventListener('click', () => changeMode('music'));
            radioModeBtn.addEventListener('click', () => changeMode('radio'));
            videoModeBtn.addEventListener('click', () => changeMode('video'));
            
            // التحكم في شريط التقدم
            progressBar.addEventListener('click', handleProgressBarClick);
            
            // التحكم في مستوى الصوت
            const volumeSlider = document.getElementById('volumeSlider');
            volumeSlider.addEventListener('input', function() {
                const volume = this.value / 100;
                audioPlayer.volume = volume;
                videoPlayer.volume = volume;
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
    </script>
</body>
</html>
