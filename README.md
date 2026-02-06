
<F.M>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <!-- منع التكبير في المتصفح -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>مشغل الوسائط - Media Player</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
            touch-action: manipulation;
        }
        
        /* منع التكبير بالنقر المزدوج */
        * {
            -webkit-touch-callout: none;
            -webkit-text-size-adjust: none;
            -webkit-tap-highlight-color: rgba(0,0,0,0);
        }
        
        html {
            overflow: hidden;
            height: 100%;
            position: fixed;
            width: 100%;
        }
        
        body {
            background-color: #000;
            color: #fff;
            overflow-x: hidden;
            height: 100vh;
            width: 100%;
            position: fixed;
            padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
            -webkit-overflow-scrolling: touch;
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
            overflow: hidden;
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
            -webkit-overflow-scrolling: touch;
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
        
        /* لوحة المؤثرات الصوتية الجذابة */
        .visualizer-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 5;
            background: radial-gradient(circle, rgba(65, 105, 225, 0.1) 0%, rgba(10, 10, 10, 0.8) 70%);
        }
        
        /* حالة الفيديو النشط - إخفاء المؤثرات */
        .media-area.video-active .visualizer-container {
            display: none;
        }
        
        .visualizer-title {
            font-size: 1.2rem;
            color: #fff;
            margin-bottom: 20px;
            text-align: center;
            font-weight: 600;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.5);
        }
        
        .visualizer-bars {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 250px;
            width: 100%;
            gap: 3px;
            padding: 0 20px;
        }
        
        .bar {
            width: 12px;
            min-height: 20px;
            background: linear-gradient(to top, 
                rgba(138, 43, 226, 0.9), 
                rgba(65, 105, 225, 0.8),
                rgba(100, 149, 237, 0.7));
            border-radius: 6px 6px 3px 3px;
            transition: height 0.2s ease;
            box-shadow: 
                0 0 10px rgba(138, 43, 226, 0.5),
                inset 0 0 5px rgba(255, 255, 255, 0.3);
            position: relative;
            overflow: hidden;
        }
        
        .bar::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 30%;
            background: linear-gradient(to bottom, 
                rgba(255, 255, 255, 0.8), 
                rgba(255, 255, 255, 0.3));
            border-radius: 6px 6px 3px 3px;
        }
        
        .bar-pulse {
            position: absolute;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 8px;
            height: 8px;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 50%;
            animation: pulse 1.5s infinite;
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }
        
        @keyframes pulse {
            0% { 
                transform: translateX(-50%) scale(0.8);
                opacity: 0.7;
            }
            50% { 
                transform: translateX(-50%) scale(1.3);
                opacity: 1;
            }
            100% { 
                transform: translateX(-50%) scale(0.8);
                opacity: 0.7;
            }
        }
        
        /* مؤشرات الصوت التفاعلية */
        .sound-waves {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-top: 20px;
            gap: 10px;
        }
        
        .wave {
            width: 4px;
            height: 20px;
            background: linear-gradient(to top, 
                rgba(65, 105, 225, 0.7), 
                rgba(138, 43, 226, 0.7));
            border-radius: 2px;
            animation: waveMove 2s infinite ease-in-out;
        }
        
        @keyframes waveMove {
            0%, 100% { 
                height: 20px;
                opacity: 0.6;
            }
            25% { 
                height: 40px;
                opacity: 0.8;
            }
            50% { 
                height: 60px;
                opacity: 1;
            }
            75% { 
                height: 40px;
                opacity: 0.8;
            }
        }
        
        .video-container {
            width: 100%;
            height: 100%;
            display: none;
            border-radius: 15px;
            overflow: hidden;
            position: relative;
            transition: all 0.5s ease;
            background-color: #000;
        }
        
        /* حالة الفيديو النشط - توسيع الفيديو */
        .media-area.video-active .video-container {
            display: block;
            width: 100%;
            height: 65%;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.8);
        }
        
        /* زر الرجوع للقائمة في الفيديو */
        .video-back-btn {
            position: absolute;
            top: 15px;
            right: 15px;
            z-index: 1001;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            border: none;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: none;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 1.2rem;
            transition: all 0.3s ease;
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
        
        .video-back-btn:hover {
            background-color: rgba(65, 105, 225, 0.8);
            transform: scale(1.1);
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
            
            .visualizer-bars {
                height: 200px;
            }
            
            .bar {
                width: 8px;
            }
            
            .visualizer-title {
                font-size: 1rem;
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
            
            .visualizer-bars {
                height: 180px;
            }
        }
        
        /* مساحة فارغة للجزء السفلي الآمن */
        .safe-area-bottom {
            height: env(safe-area-inset-bottom);
            width: 100%;
        }
        
        /* صفحة التحميل للفيديو */
        .video-loading {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            z-index: 10;
            display: none;
        }
        
        .video-loading-text {
            margin-top: 15px;
            color: #fff;
            font-size: 1rem;
        }
        
        /* زر مشاهدة الفيديو */
        .watch-video-btn {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(45deg, #8a2be2, #4169e1);
            border: none;
            color: white;
            padding: 15px 30px;
            border-radius: 30px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            display: none;
            align-items: center;
            gap: 10px;
            z-index: 20;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5);
            transition: all 0.3s ease;
        }
        
        .watch-video-btn:hover {
            transform: translate(-50%, -50%) scale(1.05);
            box-shadow: 0 15px 30px rgba(138, 43, 226, 0.4);
        }
        
        /* نوافذ الإعدادات والعناصر الإضافية */
        .settings-overlay,
        .player-selector-btn,
        .video-unsupported-warning {
            display: none;
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
                
                <!-- لوحة المؤثرات الصوتية الجذابة -->
                <div class="visualizer-container" id="visualizerContainer">
                    <div class="visualizer-title">
                        <i class="fas fa-music"></i> المؤثرات الصوتية
                    </div>
                    <div class="visualizer-bars" id="visualizerBars">
                        <!-- سيتم إنشاء الأشرطة بواسطة JavaScript -->
                    </div>
                    <div class="sound-waves" id="soundWaves">
                        <!-- سيتم إنشاء موجات الصوت بواسطة JavaScript -->
                    </div>
                </div>
            </div>
            
            <!-- زر مشاهدة الفيديو -->
            <button class="watch-video-btn" id="watchVideoBtn">
                <i class="fas fa-play-circle"></i>
                مشاهدة الفيديو
            </button>
            
            <!-- صفحة تحميل الفيديو -->
            <div class="video-loading" id="videoLoading">
                <div class="audio-loading" style="width: 40px; height: 40px; border-width: 4px;"></div>
                <div class="video-loading-text">جاري تحميل الفيديو...</div>
            </div>
            
            <!-- مشغل الفيديو -->
            <div class="video-container" id="videoContainer">
                <!-- سيتم إضافة iframe ديناميكياً -->
                
                <!-- زر الرجوع للقائمة -->
                <button class="video-back-btn" id="videoBackBtn">
                    <i class="fas fa-arrow-right"></i>
                </button>
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
    
    <!-- عنصر صوتي مخفي -->
    <audio id="audioPlayer" style="display: none;"></audio>
    
    <script>
        // البيانات الأولية مع روابط صوت حقيقية للراديو
        const radioStations = [
            { id: 1, name: "LB طرب", type: "راديو", icon: "fas fa-microphone", image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQX12GI9uvjX-fN6StHaAe2WyNLVAhrOhmWkw&s", url: "https://stream.zeno.fm/qngq01c4mfhvv.mp3", duration: "مباشر" },
            { id: 2, name: "FM طرب", type: "راديو", icon: "fas fa-microphone", image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTAHb8V-_pReH14jreKRE6cJwt6CiQGeGImdw&s", url: "https://stream.zeno.fm/4nnehdnvpg8uv.mp3", duration: "مباشر" },
            { id: 3, name: "الرجال", type: "راديو", icon: "fas fa-microphone", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/wrfc0nkbl2wuv.mp3", duration: "مباشر" },
            { id: 4, name: "FM ميلودي", type: "راديو", icon: "fas fa-microphone", image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTkmoG8iI3dP_TQV-zvvjHTMRVG6iH4UcNOiw&s", url: "https://stream.zeno.fm/3gkedytbapbuv.mp3", duration: "مباشر" },
            { id: 5, name: "FM ستار", type: "راديو", icon: "fas fa-microphone", image: "https://images.unsplash.com/photo-1585779034823-7e9ac8faec70?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://stream.zeno.fm/qv4bv7t3afhvv.mp3", duration: "مباشر" }
        ];
        
        // بيانات الأغاني مع روابط صوت حقيقية
        let songs = [
            { id: 1, name: "أحلام", title: "أغنية 1", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3", duration: 180 },
            { id: 2, name: "كاظم الساهر", title: "أغنية 2", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3", duration: 210 },
            { id: 3, name: "نجوى كرم", title: "أغنية 3", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3", duration: 195 },
            { id: 4, name: "ماجد المهندس", title: "أغنية 4", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-4.mp3", duration: 240 },
            { id: 5, name: "نوال الكويتية", title: "أغنية 5", icon: "fas fa-music", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-5.mp3", duration: 185 }
        ];
        
        // بيانات الفيديو مع روابط مباشرة
        let videos = [
            { id: 1, name: "فيديو قصير", title: "افلام كرتون", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/BigBuckBunny.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4", duration: 180, external: true },
            { id: 2, name: "فيديو تعليمي", title: "كرتون تعليمي", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ElephantsDream.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4", duration: 210, external: true },
            { id: 3, name: "فيديو ترفيهي", title: "مقاطع مضحكة", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ForBiggerBlazes.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerBlazes.mp4", duration: 195, external: true },
            { id: 4, name: "فيديو موسيقي", title: "أغاني مصورة", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.youtube.com/embed/dQw4w9WgXcQ", duration: 240, external: true },
            { id: 5, name: "فيديو مباشر", title: "بث حي", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://www.youtube.com/embed/jNQXAC9IVRw", duration: 185, external: true }
        ];
        
        // حالة التطبيق
        let isPlaying = false;
        let currentContentId = 1;
        let currentContentType = "radio";
        let currentTime = 0;
        let duration = 0;
        let progressInterval = null;
        let fullscreenTimeout = null;
        let isFullscreen = false;
        let visualizerInterval = null;
        let audioContext = null;
        let analyser = null;
        let dataArray = null;
        let source = null;
        
        // عناصر DOM
        const settingsBtn = document.getElementById('settingsBtn');
        const backBtn = document.getElementById('backBtn');
        const playPauseBtn = document.getElementById('playPauseBtn');
        const playIcon = document.getElementById('playIcon');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
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
        const visualizerBars = document.getElementById('visualizerBars');
        const soundWaves = document.getElementById('soundWaves');
        const videoContainer = document.getElementById('videoContainer');
        const audioPlayer = document.getElementById('audioPlayer');
        const mediaArea = document.getElementById('mediaArea');
        const exitFullscreenBtn = document.getElementById('exitFullscreenBtn');
        const fullscreenIndicator = document.getElementById('fullscreenIndicator');
        const watchVideoBtn = document.getElementById('watchVideoBtn');
        const videoLoading = document.getElementById('videoLoading');
        const videoBackBtn = document.getElementById('videoBackBtn');
        
        // أزرار اختيار النوع
        const radioTypeBtn = document.getElementById('radioTypeBtn');
        const musicTypeBtn = document.getElementById('musicTypeBtn');
        const videoTypeBtn = document.getElementById('videoTypeBtn');
        
        // إنشاء لوحة المؤثرات الصوتية الجذابة
        function createVisualizer() {
            visualizerBars.innerHTML = '';
            soundWaves.innerHTML = '';
            
            // إنشاء 25 شريط بأطوال وألوان مختلفة
            for (let i = 0; i < 25; i++) {
                const bar = document.createElement('div');
                bar.className = 'bar';
                
                // إضافة نبض في الأسفل
                const pulse = document.createElement('div');
                pulse.className = 'bar-pulse';
                bar.appendChild(pulse);
                
                // تعيين أطوال وألوان مختلفة
                const randomHeight = 20 + Math.random() * 80;
                bar.style.height = `${randomHeight}px`;
                bar.style.animationDelay = `${i * 0.05}s`;
                
                visualizerBars.appendChild(bar);
            }
            
            // إنشاء 8 موجات صوتية
            for (let i = 0; i < 8; i++) {
                const wave = document.createElement('div');
                wave.className = 'wave';
                wave.style.animationDelay = `${i * 0.1}s`;
                soundWaves.appendChild(wave);
            }
        }
        
        // تهيئة AudioContext لتحليل الصوت
        function initAudioVisualizer() {
            if (!audioContext) {
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                analyser = audioContext.createAnalyser();
                analyser.fftSize = 256;
                source = audioContext.createMediaElementSource(audioPlayer);
                source.connect(analyser);
                analyser.connect(audioContext.destination);
                
                const bufferLength = analyser.frequencyBinCount;
                dataArray = new Uint8Array(bufferLength);
            }
        }
        
        // تحديث المؤثرات البصرية بناءً على الصوت
        function updateVisualizer() {
            if (!analyser || !dataArray) return;
            
            analyser.getByteFrequencyData(dataArray);
            const bars = document.querySelectorAll('.bar');
            
            bars.forEach((bar, i) => {
                const barIndex = Math.floor((i / bars.length) * dataArray.length);
                const value = dataArray[barIndex];
                const height = Math.max(20, (value / 255) * 150);
                
                bar.style.height = `${height}px`;
                
                // تغيير اللون بناءً على شدة الصوت
                if (value > 200) {
                    bar.style.background = 'linear-gradient(to top, #ff416c, #ff4b2b)';
                } else if (value > 150) {
                    bar.style.background = 'linear-gradient(to top, #ff9966, #ff5e62)';
                } else if (value > 100) {
                    bar.style.background = 'linear-gradient(to top, #36d1dc, #5b86e5)';
                } else {
                    bar.style.background = 'linear-gradient(to top, #8a2be2, #4169e1)';
                }
            });
        }
        
        // تشغيل المؤثرات البصرية
        function startVisualizer() {
            if (visualizerInterval) clearInterval(visualizerInterval);
            
            // إذا كان هناك صوت، قم بتهيئة وتحليل الصوت
            if (currentContentType !== 'video' && audioPlayer.src) {
                initAudioVisualizer();
                visualizerInterval = setInterval(updateVisualizer, 100);
            } else {
                // تشغيل تأثيرات افتراضية
                const bars = document.querySelectorAll('.bar');
                visualizerInterval = setInterval(() => {
                    bars.forEach(bar => {
                        const randomHeight = 20 + Math.random() * 100;
                        bar.style.height = `${randomHeight}px`;
                    });
                }, 300);
            }
            
            // تشغيل موجات الصوت
            const waves = document.querySelectorAll('.wave');
            waves.forEach(wave => {
                wave.style.animationPlayState = 'running';
            });
        }
        
        // إيقاف المؤثرات البصرية
        function stopVisualizer() {
            if (visualizerInterval) {
                clearInterval(visualizerInterval);
                visualizerInterval = null;
            }
            
            // إيقاف موجات الصوت
            const waves = document.querySelectorAll('.wave');
            waves.forEach(wave => {
                wave.style.animationPlayState = 'paused';
            });
            
            // إعادة الأشرطة إلى الطول الافتراضي
            const bars = document.querySelectorAll('.bar');
            bars.forEach(bar => {
                bar.style.height = '20px';
                bar.style.background = 'linear-gradient(to top, #8a2be2, #4169e1)';
            });
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
                    items = itemsToShow || songs.slice(0, 20);
                    contentTitle.textContent = `قائمة الأغاني (${songs.length})`;
                    addMoreBtn.style.display = 'flex';
                    break;
                case 'video':
                    items = itemsToShow || videos.slice(0, 20);
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
                    
                    // إعداد مشغل الفيديو (التشغيل التلقائي بمشغل خارجي)
                    setupVideoPlayer(item);
                    break;
            }
            
            // إخفاء مؤشر التحميل بعد فترة قصيرة
            setTimeout(() => {
                showLoadingIndicator(contentId, contentType, false);
            }, 1000);
        }
        
        // إعداد مشغل الفيديو (التشغيل التلقائي بمشغل خارجي)
        function setupVideoPlayer(item) {
            // إخفاء صورة الفنان وإظهار زر مشاهدة الفيديو
            artistImage.style.display = 'none';
            videoContainer.style.display = 'block';
            videoContainer.innerHTML = '';
            
            // إضافة زر الرجوع للقائمة
            videoBackBtn.style.display = 'flex';
            
            // إضافة حالة الفيديو النشط
            mediaArea.classList.add('video-active');
            
            // إظهار زر مشاهدة الفيديو
            watchVideoBtn.style.display = 'flex';
            watchVideoBtn.onclick = function() {
                openExternalVideoPlayer(item.url);
            };
            
            // إيقاف أي تشغيل سابق
            stopAllPlayers();
            
            // إزالة أي وقت سابق
            if (fullscreenTimeout) {
                clearTimeout(fullscreenTimeout);
                fullscreenTimeout = null;
            }
            
            // إخفاء زر الرجوع الرئيسي وإظهار زر الرجوع في الفيديو
            backBtn.style.display = 'none';
            
            // تشغيل الفيديو تلقائياً بعد 2 ثانية
            setTimeout(() => {
                if (!item.external) {
                    // إذا كان الفيديو داخلياً
                    showVideoLoading(true);
                    setTimeout(() => {
                        showVideoLoading(false);
                        playInternalVideo(item.url);
                    }, 2000);
                } else {
                    // إذا كان الفيديو خارجياً، افتح المشغل الخارجي تلقائياً
                    openExternalVideoPlayer(item.url);
                }
            }, 2000);
        }
        
        // فتح مشغل فيديو خارجي
        function openExternalVideoPlayer(videoUrl) {
            showVideoLoading(true);
            
            // محاولة فتح في نافذة جديدة أولاً
            setTimeout(() => {
                const newWindow = window.open(videoUrl, '_blank', 'noopener,noreferrer');
                
                if (!newWindow || newWindow.closed || typeof newWindow.closed == 'undefined') {
                    // إذا فشل فتح نافذة جديدة، حاول استخدام iframe
                    playVideoInIframe(videoUrl);
                } else {
                    // إخفاء تحميل الفيديو بعد فتح النافذة
                    setTimeout(() => {
                        showVideoLoading(false);
                        // عرض رسالة للمستخدم
                        alert('تم فتح الفيديو في مشغل خارجي. يمكنك العودة للتطبيق بعد المشاهدة.');
                    }, 1500);
                }
            }, 1000);
        }
        
        // تشغيل الفيديو في iframe
        function playVideoInIframe(videoUrl) {
            showVideoLoading(true);
            
            // تنظيف الـ container
            videoContainer.innerHTML = '';
            
            // إضافة زر الرجوع
            videoContainer.appendChild(videoBackBtn);
            
            // إنشاء iframe
            const iframe = document.createElement('iframe');
            iframe.src = videoUrl;
            iframe.style.width = '100%';
            iframe.style.height = '100%';
            iframe.style.border = 'none';
            iframe.allow = 'autoplay; fullscreen; encrypted-media; picture-in-picture';
            iframe.allowFullscreen = true;
            
            // إضافة iframe إلى container
            videoContainer.appendChild(iframe);
            
            // إخفاء زر مشاهدة الفيديو
            watchVideoBtn.style.display = 'none';
            
            // إخفاء تحميل الفيديو بعد تحميل iframe
            iframe.onload = function() {
                showVideoLoading(false);
                // محاولة تشغيل الفيديو تلقائياً
                try {
                    iframe.contentWindow.postMessage('{"event":"command","func":"playVideo","args":""}', '*');
                } catch (e) {
                    console.log('لا يمكن تشغيل الفيديو تلقائياً:', e);
                }
            };
        }
        
        // تشغيل الفيديو داخلياً (إذا كان مدعوماً)
        function playInternalVideo(videoUrl) {
            // تنظيف الـ container
            videoContainer.innerHTML = '';
            
            // إضافة زر الرجوع
            videoContainer.appendChild(videoBackBtn);
            
            // إنشاء عنصر فيديو
            const video = document.createElement('video');
            video.src = videoUrl;
            video.style.width = '100%';
            video.style.height = '100%';
            video.controls = true;
            video.autoplay = true;
            video.playsInline = true;
            
            // إضافة الفيديو إلى container
            videoContainer.appendChild(video);
            
            // إخفاء زر مشاهدة الفيديو
            watchVideoBtn.style.display = 'none';
            
            // إضافة حدث للرجوع عند انتهاء الفيديو
            video.onended = function() {
                setTimeout(() => {
                    goBackToHome();
                }, 2000);
            };
        }
        
        // إظهار/إخفاء تحميل الفيديو
        function showVideoLoading(show) {
            if (show) {
                videoLoading.style.display = 'block';
            } else {
                videoLoading.style.display = 'none';
            }
        }
        
        // العودة إلى الصفحة الرئيسية
        function goBackToHome() {
            // إيقاف أي تشغيل سابق
            stopAllPlayers();
            
            // إعادة إظهار صورة الفنان
            artistImage.style.display = 'block';
            videoContainer.style.display = 'none';
            videoContainer.innerHTML = '';
            watchVideoBtn.style.display = 'none';
            videoBackBtn.style.display = 'none';
            showVideoLoading(false);
            
            // إزالة حالة الفيديو النشط
            mediaArea.classList.remove('video-active');
            mediaArea.classList.remove('fullscreen-mode');
            exitFullscreenBtn.style.display = 'none';
            
            // إعادة إظهار العناصر المخفية
            document.querySelectorAll('.progress-area, .controls, .content-type-selector, .content-bar').forEach(el => {
                el.style.opacity = '1';
                el.style.height = 'auto';
                el.style.padding = '';
                el.style.margin = '';
            });
            
            // إعادة تعيين المحتوى
            artistName.textContent = "أحلام";
            songTitle.textContent = "أغنية مختارة";
            artistImage.src = "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=1000&q=80";
            
            // إعادة تعيين التشغيل
            isPlaying = false;
            playIcon.className = 'fas fa-play';
            
            // إلغاء وقت ملء الشاشة
            if (fullscreenTimeout) {
                clearTimeout(fullscreenTimeout);
                fullscreenTimeout = null;
            }
            
            // إخفاء زر الرجوع
            backBtn.style.display = 'none';
        }
        
        // إعداد مشغل الراديو
        function setupRadioPlayer(url) {
            stopAllPlayers();
            
            // إزالة حالة الفيديو النشط
            mediaArea.classList.remove('video-active');
            exitFullscreenBtn.style.display = 'none';
            artistImage.style.display = 'block';
            videoContainer.style.display = 'none';
            watchVideoBtn.style.display = 'none';
            videoBackBtn.style.display = 'none';
            
            // إخفاء زر الرجوع
            backBtn.style.display = 'none';
            
            if (fullscreenTimeout) {
                clearTimeout(fullscreenTimeout);
                fullscreenTimeout = null;
            }
            
            if (url) {
                audioPlayer.src = url;
                audioPlayer.load();
                
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
            stopAllPlayers();
            
            mediaArea.classList.remove('video-active');
            exitFullscreenBtn.style.display = 'none';
            artistImage.style.display = 'block';
            videoContainer.style.display = 'none';
            watchVideoBtn.style.display = 'none';
            videoBackBtn.style.display = 'none';
            
            backBtn.style.display = 'none';
            
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
                
                isPlaying = true;
                playIcon.className = 'fas fa-pause';
                startVisualizer();
                startProgress();
            }
        }
        
        // إيقاف جميع المشغلات
        function stopAllPlayers() {
            clearInterval(progressInterval);
            audioPlayer.pause();
            stopVisualizer();
            
            // إغلاق AudioContext إذا كان مفتوحاً
            if (audioContext && audioContext.state !== 'closed') {
                audioContext.close().then(() => {
                    audioContext = null;
                    analyser = null;
                    dataArray = null;
                    source = null;
                });
            }
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
                
                switch(currentContentType) {
                    case 'radio':
                        audioPlayer.play().catch(e => console.log("خطأ في تشغيل الراديو:", e));
                        startProgressForRadio();
                        break;
                    case 'music':
                        audioPlayer.play().catch(e => console.log("خطأ في تشغيل الأغنية:", e));
                        startProgress();
                        break;
                }
            } else {
                playIcon.className = 'fas fa-play';
                stopVisualizer();
                
                switch(currentContentType) {
                    case 'radio':
                        audioPlayer.pause();
                        break;
                    case 'music':
                        audioPlayer.pause();
                        break;
                }
                
                clearInterval(progressInterval);
            }
        }
        
        // تغيير نوع المحتوى
        function changeContentType(type) {
            if (currentContentType === 'video' && type !== 'video') {
                goBackToHome();
            }
            
            currentContentType = type;
            
            radioTypeBtn.classList.toggle('active', type === 'radio');
            musicTypeBtn.classList.toggle('active', type === 'music');
            videoTypeBtn.classList.toggle('active', type === 'video');
            
            currentContentId = 1;
            
            createContentItems(type);
            selectContent(1, type);
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
        
        // تهيئة التطبيق
        function initApp() {
            createVisualizer();
            createContentItems('radio');
            selectContent(1, 'radio');
            
            updateTimeDisplay();
            
            // إضافة مستمعي الأحداث
            backBtn.addEventListener('click', goBackToHome);
            
            videoBackBtn.addEventListener('click', goBackToHome);
            
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
            
            // منع التكبير بالنقر المزدوج
            document.addEventListener('dblclick', function(e) {
                e.preventDefault();
            }, { passive: false });
        }
        
        // تشغيل التطبيق عند تحميل الصفحة
        window.addEventListener('DOMContentLoaded', initApp);
    </script>
</body>
</html>
