
<script>
        // بيانات الفيديو (20 فيديو) - روابط متوافقة مع الهواتف
        let videos = [
            { id: 1, name: "فيديو قصير", title: "افلام كرتون", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/BigBuckBunny.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4", duration: 180 },
            { id: 2, name: "فيديو 2", title: "كرتون", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ElephantsDream.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4", duration: 210 },
            { id: 3, name: "فيديو 3", title: "حفلات مباشرة", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ForBiggerBlazes.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerBlazes.mp4", duration: 195 },
            { id: 4, name: "فيديو 4", title: "أغاني مصورة", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ForBiggerEscapes.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerEscapes.mp4", duration: 240 },
            { id: 5, name: "فيديو 5", title: "عروض موسيقية", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ForBiggerFun.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerFun.mp4", duration: 185 },
            { id: 6, name: "فيديو 6", title: "مقاطع ترفيهية", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ForBiggerJoyrides.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerJoyrides.mp4", duration: 220 },
            { id: 7, name: "فيديو 7", title: "برامج موسيقية", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/ForBiggerMeltdowns.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerMeltdowns.mp4", duration: 190 },
            { id: 8, name: "فيديو 8", title: "عروض راقصة", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/Sintel.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/Sintel.mp4", duration: 205 },
            { id: 9, name: "فيديو 9", title: "حفلات غنائية", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/SubaruOutbackOnStreetAndDirt.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/SubaruOutbackOnStreetAndDirt.mp4", duration: 215 },
            { id: 10, name: "فيديو 10", title: "مقاطع تعليمية", icon: "fas fa-video", image: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/images/TearsOfSteel.jpg", url: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/TearsOfSteel.mp4", duration: 195 },
            { id: 11, name: "فيديو 11", title: "حفلات حية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4", duration: 200 },
            { id: 12, name: "فيديو 12", title: "عروض فنية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4", duration: 185 },
            { id: 13, name: "فيديو 13", title: "مقاطع تاريخية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerBlazes.mp4", duration: 220 },
            { id: 14, name: "فيديو 14", title: "عروض مسرحية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerEscapes.mp4", duration: 195 },
            { id: 15, name: "فيديو 15", title: "حفلات عالمية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerFun.mp4", duration: 210 },
            { id: 16, name: "فيديو 16", title: "مقاطع كوميدية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerJoyrides.mp4", duration: 180 },
            { id: 17, name: "فيديو 17", title: "عروض سحرية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerMeltdowns.mp4", duration: 195 },
            { id: 18, name: "فيديو 18", title: "حفلات أوركسترا", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/Sintel.mp4", duration: 220 },
            { id: 19, name: "فيديو 19", title: "مقاطع وثائقية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/SubaruOutbackOnStreetAndDirt.mp4", duration: 240 },
            { id: 20, name: "فيديو 20", title: "عروض ضوئية", icon: "fas fa-video", image: "https://images.unsplash.com/photo-1493225457124-a3eb161ffa5f?ixlib=rb-4.0.3&auto=format&fit=crop&w=200&q=80", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/TearsOfSteel.mp4", duration: 190 }
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
        let isSmartPlay = true;
        let currentVideoUrl = "";
        
        // عناصر DOM
        const settingsBtn = document.getElementById('settingsBtn');
        const backBtn = document.getElementById('backBtn');
        const settingsOverlay = document.getElementById('settingsOverlay');
        const closeSettings = document.getElementById('closeSettings');
        const playPauseBtn = document.getElementById('playPauseBtn');
        const playIcon = document.getElementById('playIcon');
        const prevBtn = document.getElementById('prevBtn');
        const nextBtn = document.getElementById('nextBtn');
        const progressBar = document.getElementById('progressBar');
        const progress = document.getElementById('progress');
        const currentTimeElement = document.getElementById('currentTime');
        const durationElement = document.getElementById('duration');
        const artistName = document.getElementById('artistName');
        const songTitle = document.getElementById('songTitle');
        const contentContainer = document.getElementById('contentContainer');
        const contentTitle = document.getElementById('contentTitle');
        const addMoreBtn = document.getElementById('addMoreBtn');
        const visualizer = document.getElementById('visualizer');
        const visualizerContainer = document.getElementById('visualizerContainer');
        const videoContainer = document.getElementById('videoContainer');
        const videoPlayer = document.getElementById('videoPlayer');
        const audioPlayer = document.getElementById('audioPlayer');
        const mediaArea = document.getElementById('mediaArea');
        const exitFullscreenBtn = document.getElementById('exitFullscreenBtn');
        const fullscreenIndicator = document.getElementById('fullscreenIndicator');
        const videoLoading = document.getElementById('videoLoading');
        const videoLoadingText = document.getElementById('videoLoadingText');
        
        // أزرار اختيار النوع
        const radioTypeBtn = document.getElementById('radioTypeBtn');
        const musicTypeBtn = document.getElementById('musicTypeBtn');
        const videoTypeBtn = document.getElementById('videoTypeBtn');
        
        // عناصر التحكم في الوضع (من الإعدادات)
        const musicModeBtn = document.getElementById('musicModeBtn');
        const radioModeBtn = document.getElementById('radioModeBtn');
        const videoModeBtn = document.getElementById('videoModeBtn');
        
        // إنشاء أشرطة المؤثرات البصرية
        function createVisualizerBars() {
            visualizer.innerHTML = '';
            for (let i = 0; i < 25; i++) {
                const bar = document.createElement('div');
                bar.className = 'bar';
                bar.style.animationDelay = `${i * 0.03}s`;
                visualizer.appendChild(bar);
            }
        }
        
        // إنشاء مؤثرات إضافية (دوائر ونقاط)
        function createAdditionalEffects() {
            // إضافة دوائر
            const circle1 = document.createElement('div');
            circle1.className = 'circle circle-1';
            visualizerContainer.appendChild(circle1);
            
            const circle2 = document.createElement('div');
            circle2.className = 'circle circle-2';
            visualizerContainer.appendChild(circle2);
            
            const circle3 = document.createElement('div');
            circle3.className = 'circle circle-3';
            visualizerContainer.appendChild(circle3);
            
            // إضافة نقاط متحركة
            for (let i = 0; i < 15; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = `${Math.random() * 100}%`;
                particle.style.top = `${Math.random() * 100}%`;
                particle.style.animationDelay = `${Math.random() * 4}s`;
                particle.style.animationDuration = `${3 + Math.random() * 3}s`;
                visualizerContainer.appendChild(particle);
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
                    
                    // إعداد مشغل الراديو
                    setupRadioPlayer(item.url);
                    break;
                    
                case 'music':
                    item = songs.find(s => s.id === contentId);
                    artistName.textContent = item.name;
                    songTitle.textContent = item.title;
                    
                    // إعداد مشغل الأغاني
                    setupMusicPlayer(item.url, item.duration);
                    break;
                    
                case 'video':
                    item = videos.find(v => v.id === contentId);
                    artistName.textContent = item.name;
                    songTitle.textContent = item.title;
                    currentVideoUrl = item.url;
                    
                    // إعداد مشغل الفيديو على Android
                    setupAndroidVideoPlayer(item.url, item.duration);
                    break;
            }
            
            // إخفاء مؤشر التحميل بعد فترة قصيرة
            setTimeout(() => {
                showLoadingIndicator(contentId, contentType, false);
            }, 1000);
        }
        
        // إعداد مشغل الفيديو على Android (محسّن)
        function setupAndroidVideoPlayer(url, videoDuration) {
            // إظهار مؤشر التحميل
            videoLoading.style.display = 'block';
            videoLoadingText.textContent = 'جاري تحميل الفيديو...';
            
            // إخفاء المؤثرات البصرية
            visualizerContainer.style.display = 'none';
            
            // إضافة حالة الفيديو النشط
            mediaArea.classList.add('video-active');
            
            // إظهار زر الرجوع
            backBtn.style.display = 'flex';
            
            // إظهار مشغل الفيديو العادي
            videoContainer.style.display = 'block';
            
            // إعداد الفيديو ليعمل على Android
            prepareVideoForAndroid(url, videoDuration);
        }
        
        // تحضير الفيديو للعمل على Android
        function prepareVideoForAndroid(url, videoDuration) {
            if (!url) {
                videoLoadingText.textContent = 'لا يوجد رابط فيديو متاح';
                setTimeout(() => {
                    videoLoading.style.display = 'none';
                }, 2000);
                return;
            }
            
            // إعداد مشغل HTML5 مع خصائص متوافقة مع Android
            videoPlayer.src = url;
            videoPlayer.preload = "auto";
            videoPlayer.muted = false; // إلغاء كتم الصوت
            videoPlayer.controls = true; // إظهار عناصر التحكم المدمجة
            videoPlayer.playsInline = true; // مهم للعمل على iOS/Android
            videoPlayer.webkitPlaysInline = true; // دعم خاص لمتصفحات WebKit
            videoPlayer.setAttribute('playsinline', ''); // خاصية HTML
            videoPlayer.setAttribute('webkit-playsinline', ''); // خاصية WebKit
            
            // إضافة حدث عند تحميل البيانات
            videoPlayer.addEventListener('loadeddata', function() {
                videoLoadingText.textContent = 'تم تحميل الفيديو، جاهز للتشغيل';
                setTimeout(() => {
                    videoLoading.style.display = 'none';
                }, 500);
                
                // تشغيل الفيديو مباشرة
                this.play().then(() => {
                    isPlaying = true;
                    playIcon.className = 'fas fa-pause';
                    startVisualizer();
                    startProgressForVideo();
                    
                    // إخفاء عناصر التحكم المدمجة مؤقتاً بعد 3 ثوانٍ
                    setTimeout(() => {
                        if (isPlaying) {
                            videoPlayer.controls = false;
                        }
                    }, 3000);
                    
                }).catch(error => {
                    console.log("خطأ في التشغيل التلقائي، انتظر تفاعل المستخدم:", error);
                    videoLoadingText.textContent = 'اضغط زر التشغيل للبدء';
                    videoPlayer.controls = true; // عرض عناصر التحكم للسماح بالمستخدم بالتشغيل
                });
            });
            
            // حدث عند بدء التشغيل
            videoPlayer.addEventListener('play', function() {
                isPlaying = true;
                playIcon.className = 'fas fa-pause';
                startVisualizer();
                startProgressForVideo();
                videoPlayer.controls = false; // إخفاء عناصر التحكم بعد بدء التشغيل
            });
            
            // حدث عند الإيقاف
            videoPlayer.addEventListener('pause', function() {
                isPlaying = false;
                playIcon.className = 'fas fa-play';
                stopVisualizer();
                clearInterval(progressInterval);
                videoPlayer.controls = true; // إظهار عناصر التحكم عند الإيقاف
            });
            
            // حدث عند انتهاء الفيديو
            videoPlayer.addEventListener('ended', function() {
                isPlaying = false;
                playIcon.className = 'fas fa-play';
                stopVisualizer();
                clearInterval(progressInterval);
                
                // العودة إلى الصفحة الرئيسية بعد ثانيتين
                setTimeout(() => {
                    goBackToHome();
                }, 2000);
            });
            
            // حدث عند وجود خطأ
            videoPlayer.addEventListener('error', function(e) {
                console.log("خطأ في الفيديو:", e);
                videoLoadingText.textContent = 'حدث خطأ في تحميل الفيديو';
                setTimeout(() => {
                    videoLoading.style.display = 'none';
                    videoPlayer.controls = true;
                    
                    // عرض زر للضغط لتجربة طريقة بديلة
                    const alternativeBtn = document.createElement('button');
                    alternativeBtn.textContent = 'جرب طريقة بديلة';
                    alternativeBtn.style.cssText = `
                        background: #4169e1;
                        color: white;
                        border: none;
                        padding: 10px 20px;
                        border-radius: 10px;
                        margin-top: 10px;
                        cursor: pointer;
                    `;
                    alternativeBtn.addEventListener('click', function() {
                        openInNativePlayer(url);
                    });
                    
                    videoLoading.appendChild(alternativeBtn);
                }, 1000);
            });
            
            // تحميل الفيديو
            videoPlayer.load();
        }
        
        // فتح الفيديو في مشغل الجهاز الأصلي (كحل بديل)
        function openInNativePlayer(url) {
            // إنشاء رابط تشغيل مباشر
            const link = document.createElement('a');
            link.href = url;
            link.target = '_blank';
            link.rel = 'noopener noreferrer';
            
            // محاولة فتح في نافذة جديدة/مشغل
            const userAgent = navigator.userAgent || navigator.vendor || window.opera;
            if (/android/i.test(userAgent)) {
                // لـ Android، حاول فتح في مشغل الفيديو الافتراضي
                window.open(url, '_system');
            } else {
                // للمتصفحات العادية
                window.open(url, '_blank');
            }
        }
        
        // تبديل ملء الشاشة
        function toggleFullscreen() {
            if (!isFullscreen) {
                // تفعيل ملء الشاشة
                if (mediaArea.requestFullscreen) {
                    mediaArea.requestFullscreen();
                } else if (mediaArea.webkitRequestFullscreen) {
                    mediaArea.webkitRequestFullscreen();
                } else if (mediaArea.mozRequestFullScreen) {
                    mediaArea.mozRequestFullScreen();
                } else if (mediaArea.msRequestFullscreen) {
                    mediaArea.msRequestFullscreen();
                }
                
                isFullscreen = true;
                mediaArea.classList.add('fullscreen-mode');
                exitFullscreenBtn.style.display = 'flex';
                backBtn.style.display = 'none';
                
                // إخفاء رأس التطبيق في وضع ملء الشاشة
                document.querySelector('.app-header').style.display = 'none';
                
                // إعادة تشغيل الفيديو إذا كان متوقفاً
                if (currentContentType === 'video' && !isPlaying) {
                    videoPlayer.play();
                }
            } else {
                exitFullscreenMode();
            }
        }
        
        // الخروج من وضع ملء الشاشة
        function exitFullscreenMode() {
            if (!isFullscreen) return;
            
            if (document.exitFullscreen) {
                document.exitFullscreen();
            } else if (document.webkitExitFullscreen) {
                document.webkitExitFullscreen();
            } else if (document.mozCancelFullScreen) {
                document.mozCancelFullScreen();
            } else if (document.msExitFullscreen) {
                document.msExitFullscreen();
            }
            
            isFullscreen = false;
            mediaArea.classList.remove('fullscreen-mode');
            exitFullscreenBtn.style.display = 'none';
            
            // إظهار زر الرجوع بعد الخروج من ملء الشاشة
            if (currentContentType === 'video') {
                backBtn.style.display = 'flex';
            }
            
            // إظهار رأس التطبيق
            document.querySelector('.app-header').style.display = 'flex';
        }
        
        // العودة إلى الصفحة الرئيسية
        function goBackToHome() {
            if (currentContentType === 'video') {
                // إيقاف الفيديو
                videoPlayer.pause();
                videoPlayer.currentTime = 0;
                
                // إزالة حالة الفيديو النشط
                mediaArea.classList.remove('video-active');
                mediaArea.classList.remove('fullscreen-mode');
                exitFullscreenBtn.style.display = 'none';
                
                // إعادة إظهار المؤثرات البصرية
                visualizerContainer.style.display = 'flex';
                videoContainer.style.display = 'none';
                videoLoading.style.display = 'none';
                
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
                stopVisualizer();
                clearInterval(progressInterval);
                
                // إزالة أي عناصر إضافية
                const extraElements = videoLoading.querySelectorAll('button');
                extraElements.forEach(el => el.remove());
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
            visualizerContainer.style.display = 'flex';
            
            // إخفاء زر الرجوع
            backBtn.style.display = 'none';
            
            // إخفاء مؤشر تحميل الفيديو
            videoLoading.style.display = 'none';
            
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
            visualizerContainer.style.display = 'flex';
            
            // إخفاء زر الرجوع
            backBtn.style.display = 'none';
            
            // إخفاء مؤشر تحميل الفيديو
            videoLoading.style.display = 'none';
            
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
        
        // بدء المؤثرات البصرية
        function startVisualizer() {
            const bars = document.querySelectorAll('.bar');
            bars.forEach(bar => {
                bar.style.animationPlayState = 'running';
            });
            
            const circles = document.querySelectorAll('.circle');
            circles.forEach(circle => {
                circle.style.animationPlayState = 'running';
            });
            
            const particles = document.querySelectorAll('.particle');
            particles.forEach(particle => {
                particle.style.animationPlayState = 'running';
            });
        }
        
        // إيقاف المؤثرات البصرية
        function stopVisualizer() {
            const bars = document.querySelectorAll('.bar');
            bars.forEach(bar => {
                bar.style.animationPlayState = 'paused';
            });
            
            const circles = document.querySelectorAll('.circle');
            circles.forEach(circle => {
                circle.style.animationPlayState = 'paused';
            });
            
            const particles = document.querySelectorAll('.particle');
            particles.forEach(particle => {
                particle.style.animationPlayState = 'paused';
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
        
        // تغيير الوضع من الإعدادات
        function changeMode(mode) {
            // تحديث أزرار الوضع
            musicModeBtn.classList.toggle('active', mode === 'music');
            radioModeBtn.classList.toggle('active', mode === 'radio');
            videoModeBtn.classList.toggle('active', mode === 'video');
            
            // تغيير نوع المحتوى حسب الوضع
            if (mode !== currentContentType) {
                changeContentType(mode);
            }
        }
        
        // تمكين/تعطيل التشغيل الذكي
        function toggleSmartPlay(enable) {
            isSmartPlay = enable;
        }
        
        // التعامل مع تغيير شريط التقدم
        function handleProgressBarClick(e) {
            if (currentContentType !== 'video') return;
            
            const rect = progressBar.getBoundingClientRect();
            const clickPosition = e.clientX - rect.left;
            const width = rect.width;
            const percent = clickPosition / width;
            
            // تحديث وقت الفيديو
            if (videoPlayer.duration) {
                videoPlayer.currentTime = percent * videoPlayer.duration;
                currentTime = videoPlayer.currentTime;
            }
            
            // تحديث العرض
            const progressPercent = percent * 100;
            progress.style.width = `${progressPercent}%`;
            updateTimeDisplay();
        }
        
        // التعامل مع الضغط على الفيديو للتكبير/التصغير
        function setupVideoControls() {
            // عند الضغط على الفيديو
            videoPlayer.addEventListener('click', function(e) {
                // منع النقر العادي إذا كانت هناك عناصر تحكم مدمجة
                if (this.controls) return;
                
                // التبديل بين التشغيل والإيقاف
                if (isPlaying) {
                    this.pause();
                } else {
                    this.play();
                }
            });
            
            // عند الضغط المزدوج على الفيديو (ملء الشاشة)
            videoPlayer.addEventListener('dblclick', toggleFullscreen);
            
            // التحكم في ملء الشاشة عند تغيير حجم الفيديو
            document.addEventListener('fullscreenchange', handleFullscreenChange);
            document.addEventListener('webkitfullscreenchange', handleFullscreenChange);
            document.addEventListener('mozfullscreenchange', handleFullscreenChange);
            document.addEventListener('MSFullscreenChange', handleFullscreenChange);
        }
        
        function handleFullscreenChange() {
            const isFullscreenNow = !!(document.fullscreenElement || 
                document.webkitFullscreenElement || 
                document.mozFullScreenElement || 
                document.msFullscreenElement);
            
            if (!isFullscreenNow && isFullscreen) {
                exitFullscreenMode();
            }
        }
        
        // إضافة عناصر تحكم لمسية للفيديو
        function addTouchControlsForVideo() {
            // إضافة أزرار تحكم خاصة للشاشات اللمسية
            const videoControls = document.createElement('div');
            videoControls.id = 'touchVideoControls';
            videoControls.style.cssText = `
                position: absolute;
                bottom: 10px;
                left: 10px;
                right: 10px;
                display: flex;
                justify-content: center;
                gap: 20px;
                z-index: 100;
                opacity: 0;
                transition: opacity 0.3s;
            `;
            
            // زر التشغيل/الإيقاف
            const playBtn = document.createElement('button');
            playBtn.innerHTML = '<i class="fas fa-play"></i>';
            playBtn.style.cssText = `
                background: rgba(0,0,0,0.7);
                color: white;
                border: none;
                width: 50px;
                height: 50px;
                border-radius: 50%;
                font-size: 1.2rem;
                display: flex;
                align-items: center;
                justify-content: center;
                cursor: pointer;
            `;
            playBtn.addEventListener('click', () => {
                if (videoPlayer.paused) {
                    videoPlayer.play();
                } else {
                    videoPlayer.pause();
                }
            });
            
            // زر ملء الشاشة
            const fullscreenBtn = document.createElement('button');
            fullscreenBtn.innerHTML = '<i class="fas fa-expand"></i>';
            fullscreenBtn.style.cssText = playBtn.style.cssText;
            fullscreenBtn.addEventListener('click', toggleFullscreen);
            
            videoControls.appendChild(playBtn);
            videoControls.appendChild(fullscreenBtn);
            videoContainer.appendChild(videoControls);
            
            // إظهار/إخفاء عناصر التحكم عند اللمس
            let controlsTimeout;
            videoContainer.addEventListener('touchstart', function() {
                videoControls.style.opacity = '1';
                clearTimeout(controlsTimeout);
                controlsTimeout = setTimeout(() => {
                    videoControls.style.opacity = '0';
                }, 3000);
            });
            
            // تحديث زر التشغيل عند تغيير حالة الفيديو
            videoPlayer.addEventListener('play', () => {
                playBtn.innerHTML = '<i class="fas fa-pause"></i>';
            });
            
            videoPlayer.addEventListener('pause', () => {
                playBtn.innerHTML = '<i class="fas fa-play"></i>';
            });
        }
        
        // تهيئة التطبيق
        function initApp() {
            createVisualizerBars();
            createAdditionalEffects();
            createContentItems('radio');
            selectContent(1, 'radio');
            
            // تعيين الوقت الافتراضي
            updateTimeDisplay();
            
            // إعداد عناصر التحكم بالفيديو
            setupVideoControls();
            addTouchControlsForVideo();
            
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
            
            // التحكم في التشغيل الذكي
            const smartPlayToggle = document.getElementById('smartPlayToggle');
            smartPlayToggle.addEventListener('change', function() {
                toggleSmartPlay(this.checked);
            });
            
            // إغلاق الإعدادات بالنقر خارجها
            settingsOverlay.addEventListener('click', (e) => {
                if (e.target === settingsOverlay) {
                    settingsOverlay.style.display = 'none';
                }
            });
            
            // كشف نظام التشغيل تلقائياً
            const userAgent = navigator.userAgent || navigator.vendor || window.opera;
            if (/android/i.test(userAgent)) {
                // تفعيل التشغيل الذكي تلقائياً للأندرويد
                isSmartPlay = true;
                if (smartPlayToggle) smartPlayToggle.checked = true;
                
                // تحسين تجربة الفيديو على Android
                videoPlayer.setAttribute('playsinline', '');
                videoPlayer.setAttribute('webkit-playsinline', '');
            }
        }
        
        // تشغيل التطبيق عند تحميل الصفحة
        window.addEventListener('DOMContentLoaded', initApp);
</script>
