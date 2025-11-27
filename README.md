<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🎬 منشئ فيديوهات احترافية بالـ AI</title>
    <style>
        :root {
            --color-white: #FFFFFF;
            --color-black: #000000;
            --color-bg: #F8F9FA;
            --color-surface: #FFFFFF;
            --color-primary: #2563EB;
            --color-secondary: #F59E0B;
            --color-success: #10B981;
            --color-error: #EF4444;
            --color-text: #1F2937;
            --color-text-secondary: #6B7280;
            --radius: 12px;
            --shadow: 0 10px 25px rgba(0,0,0,0.1);
        }
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { font-family: 'Segoe UI', Tahoma, sans-serif; background: linear-gradient(135deg, var(--color-bg), #E2E8F0); min-height: 100vh; padding: 20px; color: var(--color-text); }
        .container { max-width: 900px; margin: 0 auto; }
        header { text-align: center; margin-bottom: 40px; }
        h1 { font-size: 2.5rem; background: linear-gradient(45deg, var(--color-primary), var(--color-secondary)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; margin-bottom: 10px; }
        .subtitle { color: var(--color-text-secondary); font-size: 1.2rem; }
        .form-wrapper { background: var(--color-surface); padding: 40px; border-radius: var(--radius); box-shadow: var(--shadow); margin-bottom: 30px; }
        .form-group { margin-bottom: 25px; }
        label { display: block; font-weight: 600; margin-bottom: 10px; font-size: 1.1rem; color: var(--color-text); }
        textarea, input, select { width: 100%; padding: 15px; border: 2px solid #E5E7EB; border-radius: var(--radius); font-size: 1rem; transition: border 0.3s; }
        textarea { resize: vertical; min-height: 120px; }
        textarea:focus, input:focus, select:focus { outline: none; border-color: var(--color-primary); box-shadow: 0 0 0 3px rgba(37,99,235,0.1); }
        .style-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-top: 15px; }
        .style-option { padding: 20px; border: 2px solid #E5E7EB; border-radius: var(--radius); cursor: pointer; text-align: center; transition: all 0.3s; font-weight: 500; }
        .style-option:hover, .style-option.selected { border-color: var(--color-primary); background: rgba(37,99,235,0.05); transform: translateY(-2px); }
        .button-group { display: flex; gap: 15px; margin-top: 30px; }
        button { flex: 1; padding: 18px; font-size: 1.1rem; font-weight: 600; border: none; border-radius: var(--radius); cursor: pointer; transition: all 0.3s; }
        .btn-generate { background: linear-gradient(45deg, var(--color-primary), #1D4ED8); color: white; }
        .btn-generate:hover { transform: translateY(-2px); box-shadow: var(--shadow); }
        .btn-download { background: white; color: var(--color-primary); border: 2px solid var(--color-primary); }
        .btn-download:hover:not(:disabled) { background: var(--color-primary); color: white; }
        button:disabled { opacity: 0.6; cursor: not-allowed; }
        .progress { display: none; margin-top: 20px; }
        .progress-bar { height: 8px; background: #E5E7EB; border-radius: 4px; overflow: hidden; }
        .progress-fill { height: 100%; background: linear-gradient(90deg, var(--color-success), var(--color-secondary)); width: 0%; transition: width 0.3s; }
        .preview { display: none; margin-top: 30px; text-align: center; }
        #videoPreview { max-width: 100%; border-radius: var(--radius); box-shadow: var(--shadow); }
        .status { padding: 15px; border-radius: var(--radius); margin: 20px 0; text-align: center; font-weight: 500; }
        .status.success { background: rgba(16,185,129,0.1); color: var(--color-success); border: 1px solid var(--color-success); }
        .status.error { background: rgba(239,68,68,0.1); color: var(--color-error); border: 1px solid var(--color-error); }
        @media (max-width: 768px) { .form-wrapper { padding: 20px; } .button-group { flex-direction: column; } h1 { font-size: 2rem; } }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🎬 منشئ فيديوهات AI احترافية</h1>
            <p class="subtitle">أنشئ فيديوهات عالية الجودة من نصوصك وصورك بأنماط متعددة (إعلاني، درامي، أكشن، عادي)</p>
        </header>

        <div id="status"></div>
        <div id="progress" class="progress">
            <div class="progress-bar"><div class="progress-fill" id="progressFill"></div></div>
            <p id="progressText">0%</p>
        </div>

        <div class="form-wrapper">
            <form id="videoForm">
                <div class="form-group">
                    <label>📝 وصف الفيديو (النص)</label>
                    <textarea id="description" placeholder="اكتب النص أو الوصف الكامل للفيديو..." required></textarea>
                </div>
                <div class="form-group">
                    <label>🖼️ الصورة الخلفية</label>
                    <input type="file" id="image" accept="image/*" required>
                </div>
                <div class="form-group">
                    <label>🎭 نمط الفيديو</label>
                    <div class="style-grid">
                        <div class="style-option" data-style="ad" tabindex="0">إعلاني<br><small>فلاشي وجذاب</small></div>
                        <div class="style-option" data-style="dramatic" tabindex="0">درامي<br><small>عميق وبطيء</small></div>
                        <div class="style-option" data-style="action" tabindex="0">أكشن<br><small>سريع ومثير</small></div>
                        <div class="style-option selected" data-style="normal" tabindex="0">عادي<br><small>سلس وطبيعي</small></div>
                    </div>
                    <input type="hidden" id="videoStyle" value="normal">
                </div>
                <div class="form-group">
                    <label>⏱️ المدة (ثواني)</label>
                    <input type="number" id="duration" min="10" max="120" value="30" required>
                </div>
                <div class="form-group">
                    <label>✨ الجودة</label>
                    <select id="quality" required>
                        <option value="">اختر الجودة</option>
                        <option value="720p">720p (سريع)</option>
                        <option value="1080p" selected>1080p (احترافي)</option>
                        <option value="4k">4K (فائق)</option>
                    </select>
                </div>
                <div class="button-group">
                    <button type="submit" class="btn-generate">🚀 إنشاء الفيديو</button>
                    <button type="button" id="downloadBtn" class="btn-download" disabled>⬇️ تحميل الفيديو</button>
                </div>
            </form>
        </div>

        <div id="preview" class="preview">
            <video id="videoPreview" controls></video>
        </div>
    </div>

    <script>
        const form = document.getElementById('videoForm');
        const status = document.getElementById('status');
        const progress = document.getElementById('progress');
        const progressFill = document.getElementById('progressFill');
        const progressText = document.getElementById('progressText');
        const preview = document.getElementById('preview');
        const videoPreview = document.getElementById('videoPreview');
        const downloadBtn = document.getElementById('downloadBtn');

        let mediaRecorder, recordedChunks = [], canvas, ctx, animationId, stream, img, styleSettings = {
            ad: { zoomSpeed: 0.02, textSpeed: 0.015, color: '#FFD700', shake: 0 },
            dramatic: { zoomSpeed: 0.005, textSpeed: 0.008, color: '#8B0000', shake: 0 },
            action: { zoomSpeed: 0.04, textSpeed: 0.03, color: '#FF4500', shake: 5 },
            normal: { zoomSpeed: 0.01, textSpeed: 0.01, color: '#FFFFFF', shake: 0 }
        };

        // Style selection
        document.querySelectorAll('.style-option').forEach(opt => {
            opt.addEventListener('click', () => {
                document.querySelectorAll('.style-option').forEach(o => o.classList.remove('selected'));
                opt.classList.add('selected');
                document.getElementById('videoStyle').value = opt.dataset.style;
            });
        });

        // Status helper
        function showStatus(msg, type = 'info') {
            status.textContent = msg;
            status.className = `status ${type}`;
            status.style.display = 'block';
        }

        // Progress update
        function updateProgress(percent, text) {
            progress.style.display = 'block';
            progressFill.style.width = percent + '%';
            progressText.textContent = text || percent + '%';
        }

        form.addEventListener('submit', async (e) => {
            e.preventDefault();
            const description = document.getElementById('description').value;
            const imageFile = document.getElementById('image').files[0];
            const style = document.getElementById('videoStyle').value;
            const duration = parseInt(document.getElementById('duration').value) * 1000; // ms
            const quality = document.getElementById('quality').value;

            if (!imageFile || !description || !style || !quality) return showStatus('يرجى ملء جميع الحقول', 'error');

            showStatus('جاري تحميل الصورة...');
            updateProgress(10, 'تحميل الصورة');

            const reader = new FileReader();
            reader.onload = (e) => createVideo(e.target.result, description, style, duration, quality);
            reader.readAsDataURL(imageFile);
        });

        function createVideo(imageSrc, text, style, totalDuration, quality) {
            const sizes = { '720p': [1280, 720], '1080p': [1920, 1080], '4k': [3840, 2160] };
            const [width, height] = sizes[quality];

            canvas = document.createElement('canvas');
            canvas.width = width;
            canvas.height = height;
            ctx = canvas.getContext('2d');
            img = new Image();
            img.onload = startRecording;
            img.src = imageSrc;

            function startRecording() {
                updateProgress(30, 'إعداد الكانفاس');
                stream = canvas.captureStream(30); // 30 FPS
                mediaRecorder = new MediaRecorder(stream, { mimeType: 'video/webm; codecs=vp9' });
                mediaRecorder.ondataavailable = (e) => recordedChunks.push(e.data);
                mediaRecorder.onstop = finishVideo;
                mediaRecorder.start();
                animate(0, totalDuration, style, text);
            }
        }

        function animate(startTime, totalDuration, styleKey, text) {
            const settings = styleSettings[styleKey];
            let progress = 0;

            function loop(currentTime) {
                const elapsed = currentTime - startTime;
                progress = Math.min(elapsed / totalDuration, 1);
                updateProgress(40 + (progress * 40), 'إنشاء الإطارات...');

                // Clear and draw bg image with zoom
                ctx.fillStyle = '#000';
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                const zoom = 1 + settings.zoomSpeed * progress * 2;
                const imgW = canvas.width * zoom;
                const imgH = canvas.height * zoom;
                const offsetX = (canvas.width - imgW) / 2 + (Math.sin(progress * Math.PI * 4) * settings.shake);
                const offsetY = (canvas.height - imgH) / 2 + (Math.cos(progress * Math.PI * 4) * settings.shake);
                ctx.drawImage(img, offsetX, offsetY, imgW, imgH);

                // Draw animated text
                ctx.save();
                ctx.fillStyle = settings.color;
                ctx.strokeStyle = '#000';
                ctx.lineWidth = 3;
                ctx.font = `${Math.max(48 * (1 + progress), 48)}px Arial Black`;
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.shadowColor = settings.color;
                ctx.shadowBlur = 20;
                const textProgress = Math.min(elapsed * settings.textSpeed, 1);
                ctx.globalAlpha = textProgress;
                ctx.strokeText(text, canvas.width / 2, canvas.height / 2);
                ctx.fillText(text, canvas.width / 2, canvas.height / 2);
                ctx.restore();

                if (progress < 1) {
                    animationId = requestAnimationFrame((t) => loop(t));
                } else {
                    mediaRecorder.stop();
                }
            }
            animationId = requestAnimationFrame(loop);
        }

        function finishVideo() {
            updateProgress(100, 'تم!');
            setTimeout(() => {
                const blob = new Blob(recordedChunks, { type: 'video/webm' });
                videoPreview.src = URL.createObjectURL(blob);
                preview.style.display = 'block';
                preview.scrollIntoView({ behavior: 'smooth' });
                downloadBtn.disabled = false;
                downloadBtn.onclick = () => {
                    const a = document.createElement('a');
                    a.href = videoPreview.src;
                    a.download = `professional_video_${Date.now()}.webm`;
                    a.click();
                };
                showStatus('✅ تم إنشاء الفيديو بنجاح! شاهد المعاينة و حملها.', 'success');
                progress.style.display = 'none';
            }, 500);
        }

        // Cleanup on page unload
        window.onunload = () => { if (mediaRecorder) mediaRecorder.stop(); if (animationId) cancelAnimationFrame(animationId); };
    </script>
</body>
</html>
