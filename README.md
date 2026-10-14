<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Ghép ảnh 2 mặt CCCD</title>
    <!-- Thư viện Cropper.js xử lý xoay và cắt ảnh mượt trên điện thoại -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.css"/>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/cropperjs/1.5.13/cropper.min.js"></script>
    
    <style>
        :root { 
            --primary: #007bff; 
            --gray: #6c757d; 
            --success: #28a745; 
            --bg: #f4f6f9; 
        }
        * { box-sizing: border-box; }
        body { 
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; 
            background: var(--bg); 
            display: flex; 
            justify-content: center; 
            padding: 12px; 
            min-height: 100vh; 
            margin: 0; 
        }
        .container { 
            background: #ffffff; 
            padding: 20px 16px; 
            border-radius: 12px; 
            box-shadow: 0 4px 16px rgba(0,0,0,0.08); 
            width: 100%; 
            max-width: 600px; 
            text-align: center; 
        }
        h2 { 
            color: #1a73e8; 
            margin-top: 0; 
            margin-bottom: 16px; 
            font-size: 20px; 
            text-transform: uppercase; 
        }
        .input-group { 
            margin-bottom: 18px; 
            text-align: left; 
        }
        .input-group label { 
            display: block; 
            font-weight: 600; 
            margin-bottom: 6px; 
            color: #333; 
            font-size: 14px; 
        }
        input[type="text"] { 
            width: 100%; 
            padding: 12px; 
            border: 2px solid #ddd; 
            border-radius: 8px; 
            font-size: 15px; 
            outline: none;
            transition: border-color 0.2s;
        }
        input[type="text"]:focus { 
            border-color: var(--primary); 
        }
        .image-section { 
            background: #fff; 
            border: 1px solid #e1e4e8; 
            border-radius: 10px; 
            padding: 12px; 
            margin-bottom: 15px; 
        }
        .toolbar { 
            display: flex; 
            gap: 6px; 
            margin-bottom: 10px; 
            justify-content: flex-end; 
            align-items: center; 
        }
        .btn-tool { 
            background: var(--gray); 
            color: white; 
            border: none; 
            padding: 7px 12px; 
            border-radius: 6px; 
            cursor: pointer; 
            font-size: 13px; 
            font-weight: 500;
        }
        .btn-upload { background: var(--primary); }
        .btn-tool:disabled { opacity: 0.4; cursor: not-allowed; }
        .paste-area { 
            border: 2px dashed #b0b8c1; 
            background: #fafbfc; 
            padding: 10px; 
            border-radius: 8px; 
            cursor: pointer; 
            min-height: 140px; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            justify-content: center; 
            outline: none; 
        }
        .paste-area:focus-within { 
            border-color: var(--primary); 
            background: #eef7ff; 
        }
        .placeholder-text { 
            color: #586069; 
            font-size: 14px; 
            line-height: 1.5; 
            padding: 12px; 
            user-select: none;
        }
        .cropper-wrapper { 
            width: 100%; 
            max-height: 320px; 
            display: none; 
            justify-content: center; 
            overflow: hidden; 
            background: #24292e; 
            border-radius: 6px; 
        }
        .cropper-wrapper img { max-width: 100%; display: block; }
        .btn-action { 
            background: var(--success); 
            color: white; 
            border: none; 
            padding: 14px; 
            font-size: 16px; 
            border-radius: 8px; 
            cursor: pointer; 
            width: 100%; 
            margin: 8px 0; 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            gap: 8px; 
            font-weight: bold; 
        }
        .btn-top { background: var(--primary); margin-bottom: 20px; }
        .status { margin-top: 10px; color: #d73a49; font-weight: 600; min-height: 22px; font-size: 14px; }
        input[type="file"] { display: none; }
    </style>
</head>
<body>

<div class="container">
    <h2>Ghép ảnh 2 mặt CCCD</h2>
    
    <div class="input-group">
        <label for="filename">Hãy ghi chữ C và 4 số cuối SĐT của bạn:</label>
        <input type="text" id="filename" placeholder="Ví dụ: C8910" value="C">
    </div>
    
    <button class="btn-action btn-top" onclick="downloadPNG()">⬇️ Tải xuống ảnh PNG</button>

    <!-- MẶT TRƯỚC -->
    <div class="image-section">
        <div class="toolbar">
            <strong style="margin-right: auto; color: #24292e; font-size: 14px;">Mặt trước CCCD</strong>
            <input type="file" id="file1" accept="image/*" onchange="handleFileSelect(event, 'box1')">
            <button class="btn-tool btn-upload" onclick="document.getElementById('file1').click()">📁 Chọn ảnh</button>
            <button class="btn-tool" onclick="rotateImage('box1', -90)" id="btnRotL1" disabled>↺ Trái</button>
            <button class="btn-tool" onclick="rotateImage('box1', 90)" id="btnRotR1" disabled>↻ Phải</button>
        </div>
        <div id="box1" class="paste-area" tabindex="0">
            <div class="placeholder-text" onclick="document.getElementById('file1').click()">
                Chạm để chọn ảnh mặt trước<br><small>(hoặc Ctrl + V)</small>
            </div>
            <div class="cropper-wrapper" id="wrap1">
                <img id="img1">
            </div>
        </div>
    </div>

    <!-- MẶT SAU -->
    <div class="image-section">
        <div class="toolbar">
            <strong style="margin-right: auto; color: #24292e; font-size: 14px;">Mặt sau CCCD</strong>
            <input type="file" id="file2" accept="image/*" onchange="handleFileSelect(event, 'box2')">
            <button class="btn-tool btn-upload" onclick="document.getElementById('file2').click()">📁 Chọn ảnh</button>
            <button class="btn-tool" onclick="rotateImage('box2', -90)" id="btnRotL2" disabled>↺ Trái</button>
            <button class="btn-tool" onclick="rotateImage('box2', 90)" id="btnRotR2" disabled>↻ Phải</button>
        </div>
        <div id="box2" class="paste-area" tabindex="0">
            <div class="placeholder-text" onclick="document.getElementById('file2').click()">
                Chạm để chọn ảnh mặt sau<br><small>(hoặc Ctrl + V)</small>
            </div>
            <div class="cropper-wrapper" id="wrap2">
                <img id="img2">
            </div>
        </div>
    </div>

    <div id="status" class="status"></div>
    <button class="btn-action" onclick="downloadPNG()">⬇️ Tải xuống ảnh PNG</button>
</div>

<script>
    const appState = {
        box1: { cropper: null, hasImage: false, imgId: 'img1', wrapId: 'wrap1', rotL: 'btnRotL1', rotR: 'btnRotR1' },
        box2: { cropper: null, hasImage: false, imgId: 'img2', wrapId: 'wrap2', rotL: 'btnRotL2', rotR: 'btnRotR2' }
    };

    function setSourceToBox(boxId, dataUrl) {
        const state = appState[boxId];
        const box = document.getElementById(boxId);
        const text = box.querySelector('.placeholder-text');
        const wrap = document.getElementById(state.wrapId);
        const img = document.getElementById(state.imgId);

        if (state.cropper) {
            state.cropper.destroy();
            state.cropper = null;
        }

        img.src = dataUrl;
        wrap.style.display = 'flex';
        text.style.display = 'none';

        state.cropper = new Cropper(img, {
            viewMode: 1,
            autoCropArea: 0.95,
            responsive: true,
            restore: false
        });

        state.hasImage = true;
        document.getElementById(state.rotL).disabled = false;
        document.getElementById(state.rotR).disabled = false;
        document.getElementById('status').innerText = "";
    }

    function handleFileSelect(e, boxId) {
        const file = e.target.files[0];
        if (!file) return;
        const reader = new FileReader();
        reader.onload = (ev) => setSourceToBox(boxId, ev.target.result);
        reader.readAsDataURL(file);
    }

    function handlePaste(e, boxId) {
        e.preventDefault();
        const items = (e.clipboardData || e.originalEvent.clipboardData).items;
        for (let item of items) {
            if (item.type.startsWith("image")) {
                const reader = new FileReader();
                reader.onload = (ev) => setSourceToBox(boxId, ev.target.result);
                reader.readAsDataURL(item.getAsFile());
                return;
            }
        }
    }
    document.getElementById('box1').addEventListener('paste', (e) => handlePaste(e, 'box1'));
    document.getElementById('box2').addEventListener('paste', (e) => handlePaste(e, 'box2'));

    function rotateImage(boxId, angle) {
        const state = appState[boxId];
        if (state.cropper) {
            state.cropper.rotate(angle);
        }
    }

    function loadImage(src) { 
        return new Promise(r => { 
            let i = new Image(); 
            i.onload = () => r(i); 
            i.src = src; 
        }); 
    }

    async function calculateLayout(img1Obj, img2Obj, containerW, containerH, margin, gap) {
        const maxW = containerW - (margin * 2);
        const maxH = containerH - (margin * 2);
        let layout = { img1: null, img2: null };

        if (!img1Obj && !img2Obj) return null;

        let h1 = img1Obj ? img1Obj.height * (maxW / img1Obj.width) : 0;
        let h2 = img2Obj ? img2Obj.height * (maxW / img2Obj.width) : 0;
        let totalH = h1 + h2 + (img1Obj && img2Obj ? gap : 0);

        let scale = 1;
        if (totalH > maxH) {
            scale = maxH / totalH;
        }

        let finalW = maxW * scale;
        let finalH1 = h1 * scale;
        let finalH2 = h2 * scale;
        let finalGap = gap * scale;

        let startX = margin + (maxW - finalW) / 2;
        let currentY = margin;

        if (img1Obj) {
            layout.img1 = { obj: img1Obj, x: startX, y: currentY, w: finalW, h: finalH1 };
            currentY += finalH1 + finalGap;
        }
        if (img2Obj) {
            layout.img2 = { obj: img2Obj, x: startX, y: currentY, w: finalW, h: finalH2 };
        }

        return layout;
    }

    async function downloadPNG() {
        if (!appState.box1.hasImage && !appState.box2.hasImage) {
            document.getElementById('status').innerText = "⚠️ Vui lòng chọn ít nhất 1 mặt CCCD!"; 
            return;
        }
        document.getElementById('status').innerText = "⏳ Đang ghép và xuất ảnh PNG...";

        const data1 = appState.box1.cropper ? appState.box1.cropper.getCroppedCanvas().toDataURL('image/png') : null;
        const data2 = appState.box2.cropper ? appState.box2.cropper.getCroppedCanvas().toDataURL('image/png') : null;

        let img1Obj = data1 ? await loadImage(data1) : null;
        let img2Obj = data2 ? await loadImage(data2) : null;

        let fname = document.getElementById('filename').value.trim() || "CCCD_ghep";

        // Tỷ lệ khổ giấy A4 (1240 x 1754 px)
        const pngW = 1240;
        const pngH = 1754;
        const layoutPNG = await calculateLayout(img1Obj, img2Obj, pngW, pngH, 80, 50);

        const cvs = document.createElement('canvas');
        cvs.width = pngW; 
        cvs.height = pngH;
        const ctx = cvs.getContext('2d');
        ctx.fillStyle = "#ffffff"; 
        ctx.fillRect(0, 0, pngW, pngH);
        
        if (layoutPNG.img1) ctx.drawImage(layoutPNG.img1.obj, layoutPNG.img1.x, layoutPNG.img1.y, layoutPNG.img1.w, layoutPNG.img1.h);
        if (layoutPNG.img2) ctx.drawImage(layoutPNG.img2.obj, layoutPNG.img2.x, layoutPNG.img2.y, layoutPNG.img2.w, layoutPNG.img2.h);
        
        const link = document.createElement('a');
        link.download = fname + ".png";
        link.href = cvs.toDataURL('image/png');
        link.click();

        document.getElementById('status').innerText = "✅ Đã tải xuống ảnh PNG thành công!";
    }
</script>
</body>
</html>
