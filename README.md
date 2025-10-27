<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Generator</title>
    <script src="https://cdn.jsdelivr.net/npm/qrcode/build/qrcode.min.js"></script>
    <style>
        :root {
            --primary: #4361ee;
            --primary-dark: #3a56d4;
            --secondary: #7209b7;
            --light: #f8f9fa;
            --dark: #212529;
            --gray: #6c757d;
            --success: #4cc9f0;
            --error: #f72585;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            color: var(--dark);
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
            background: white;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            padding: 40px;
            margin-top: 30px;
            text-align: center;
        }
        
        .logo {
            max-width: 300px;
            margin: 0 auto 15px;
            display: block;
        }
        
        .subtitle {
            color: var(--gray);
            margin-bottom: 30px;
            font-size: 1.1rem;
        }
        
        .upload-section {
            margin-bottom: 30px;
        }
        
        .upload-box {
            border: 2px dashed #bdc3c7;
            border-radius: 12px;
            width: 100%;
            max-width: 400px;
            height: 200px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            margin: 0 auto 20px;
            text-align: center;
            transition: all 0.3s ease;
            background: #f8f9fa;
            position: relative;
            overflow: hidden;
        }
        
        .upload-box:hover {
            border-color: var(--primary);
            background: #edf2ff;
        }
        
        .upload-box.active {
            border-color: var(--primary);
            background: #e8f4fe;
        }
        
        .upload-icon {
            font-size: 48px;
            color: var(--primary);
            margin-bottom: 15px;
            line-height: 1;
        }
        
        .upload-text {
            font-size: 1.1rem;
            color: var(--gray);
            margin-bottom: 5px;
        }
        
        .upload-hint {
            font-size: 0.85rem;
            color: var(--gray);
        }
        
        .file-info {
            margin-top: 10px;
            font-size: 0.9rem;
            color: var(--primary);
            display: none;
        }
        
        input[type="file"] {
            display: none;
        }
        
        .btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            margin: 10px 5px;
            box-shadow: 0 4px 6px rgba(67, 97, 238, 0.3);
        }
        
        .btn:hover {
            background: var(--primary-dark);
            transform: translateY(-2px);
            box-shadow: 0 6px 8px rgba(67, 97, 238, 0.4);
        }
        
        .btn:active {
            transform: translateY(0);
        }
        
        .btn:disabled {
            background: var(--gray);
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .btn-secondary {
            background: var(--secondary);
            box-shadow: 0 4px 6px rgba(114, 9, 183, 0.3);
        }
        
        .btn-secondary:hover {
            background: #6511a0;
            box-shadow: 0 6px 8px rgba(114, 9, 183, 0.4);
        }
        
        .btn-icon {
            margin-right: 8px;
        }
        
        #qrContainer {
            margin-top: 30px;
            text-align: center;
            display: none;
        }
        
        .qr-code {
            max-width: 250px;
            border-radius: 12px;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
            margin: 0 auto 20px;
            border: 8px solid white;
        }
        
        .image-preview {
            max-width: 200px;
            max-height: 150px;
            border-radius: 8px;
            margin: 15px auto;
            display: none;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }
        
        .loading {
            display: none;
            margin: 20px auto;
        }
        
        .spinner {
            width: 40px;
            height: 40px;
            border: 4px solid rgba(67, 97, 238, 0.2);
            border-radius: 50%;
            border-top-color: var(--primary);
            animation: spin 1s linear infinite;
            margin: 0 auto;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .message {
            padding: 12px;
            border-radius: 8px;
            margin: 15px 0;
            display: none;
        }
        
        .success {
            background: rgba(76, 201, 240, 0.2);
            color: #0a7ea3;
            border-left: 4px solid var(--success);
        }
        
        .error {
            background: rgba(247, 37, 133, 0.1);
            color: var(--error);
            border-left: 4px solid var(--error);
        }
        
        .footer {
            margin-top: 40px;
            padding-top: 20px;
            font-size: 0.9rem;
            color: var(--gray);
            text-align: center;
            border-top: 1px solid #e9ecef;
            width: 100%;
        }
        
        .instructions {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            margin: 30px 0;
            text-align: left;
        }
        
        .instructions h3 {
            color: var(--primary);
            margin-bottom: 10px;
        }
        
        .instructions ol {
            padding-left: 20px;
        }
        
        .instructions li {
            margin-bottom: 8px;
            color: var(--gray);
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 25px;
            }
            
            .logo {
                max-width: 250px;
            }
            
            .upload-box {
                height: 180px;
            }
            
            .btn {
                width: 100%;
                margin: 5px 0;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <img src="https://i.ibb.co/0y7FWjF1/image.png" alt="QR Code Generator" class="logo">
        <p class="subtitle">Upload an image and generate a QR code for its URL</p>
        
        <div class="upload-section">
            <label class="upload-box" id="uploadBox">
                <div class="upload-icon">𓂃🖊</div>
                <div class="upload-text">Drag & Drop or Click to Upload</div>
                <div class="upload-hint">Supports JPG, PNG, GIF (Max 5MB)</div>
                <div class="file-info" id="fileInfo"></div>
                <input type="file" id="imageInput" accept="image/*">
            </label>
            
            <img id="imagePreview" class="image-preview" alt="Image Preview">
            
            <div class="message" id="message"></div>
            
            <button id="generateBtn" class="btn" onclick="uploadAndGenerateQR()">
                <span class="btn-icon">⚡</span> Generate QR Code
            </button>
            
            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p>Uploading image and generating QR code...</p>
            </div>
        </div>
        
        <div id="qrContainer">
            <h2>Your QR Code</h2>
            <img id="qrCode" class="qr-code" alt="QR Code">
            <a id="downloadLink" download="qr-code.png">
                <button class="btn btn-secondary">
                    <span class="btn-icon">⬇️</span> Download QR Code
                </button>
            </a>
        </div>
        
        <div class="instructions">
            <h3>How It Works</h3>
            <ol>
                <li>Upload an image using the drag & drop area or click to browse</li>
                <li>Click "Generate QR Code" to upload your image and create a QR code</li>
                <li>Download your QR code to share with others</li>
                <li>When someone scans the QR code, they'll be directed to your image</li>
            </ol>
        </div>
    </div>
    
    <div class="footer">
        Made with ❤️ by Armeen
    </div>

    <script>
        const imgbbAPIKey = '1cf8feb97ba0aa50a1ee75cef54488ec';
        const uploadBox = document.getElementById('uploadBox');
        const fileInput = document.getElementById('imageInput');
        const fileInfo = document.getElementById('fileInfo');
        const imagePreview = document.getElementById('imagePreview');
        const generateBtn = document.getElementById('generateBtn');
        const qrContainer = document.getElementById('qrContainer');
        const qrCode = document.getElementById('qrCode');
        const downloadLink = document.getElementById('downloadLink');
        const loading = document.getElementById('loading');
        const message = document.getElementById('message');

        // Drag and drop functionality
        uploadBox.addEventListener('dragover', (e) => {
            e.preventDefault();
            uploadBox.classList.add('active');
        });
        
        uploadBox.addEventListener('dragleave', () => {
            uploadBox.classList.remove('active');
        });
        
        uploadBox.addEventListener('drop', (e) => {
            e.preventDefault();
            uploadBox.classList.remove('active');
            if (e.dataTransfer.files.length > 0) {
                fileInput.files = e.dataTransfer.files;
                handleFileSelection();
            }
        });

        // File input change handler
        fileInput.addEventListener('change', handleFileSelection);

        function handleFileSelection() {
            const file = fileInput.files[0];
            if (!file) return;
            
            // Check file size (max 5MB)
            if (file.size > 5 * 1024 * 1024) {
                showMessage('File size exceeds 5MB limit. Please choose a smaller file.', 'error');
                resetFileInput();
                return;
            }
            
            // Check file type
            if (!file.type.match('image.*')) {
                showMessage('Please select an image file (JPG, PNG, GIF, etc.)', 'error');
                resetFileInput();
                return;
            }
            
            // Show file info
            fileInfo.textContent = `Selected: ${file.name} (${(file.size / 1024).toFixed(1)} KB)`;
            fileInfo.style.display = 'block';
            
            // Show image preview
            const reader = new FileReader();
            reader.onload = function(e) {
                imagePreview.src = e.target.result;
                imagePreview.style.display = 'block';
            };
            reader.readAsDataURL(file);
            
            // Enable generate button
            generateBtn.disabled = false;
            hideMessage();
        }
        
        function resetFileInput() {
            fileInput.value = '';
            fileInfo.style.display = 'none';
            imagePreview.style.display = 'none';
            generateBtn.disabled = true;
        }
        
        async function uploadAndGenerateQR() {
            const file = fileInput.files[0];
            if (!file) {
                showMessage('Please select an image first!', 'error');
                return;
            }

            // Show loading state
            loading.style.display = 'block';
            generateBtn.disabled = true;
            hideMessage();
            
            try {
                // Convert file to base64
                const base64 = await fileToBase64(file);
                
                // Upload to ImgBB
                const formData = new FormData();
                formData.append('key', imgbbAPIKey);
                formData.append('image', base64.split(',')[1]);
                
                const response = await fetch('https://api.imgbb.com/1/upload', {
                    method: 'POST',
                    body: formData
                });
                
                const data = await response.json();
                
                if (data.success) {
                    const imageUrl = data.data.url;
                    
                    // Generate QR code
                    QRCode.toDataURL(imageUrl, { 
                        width: 250, 
                        margin: 2,
                        color: {
                            dark: '#4361ee',
                            light: '#ffffff'
                        }
                    }, (err, qrUrl) => {
                        if (err) {
                            console.error(err);
                            showMessage('Failed to generate QR code. Please try again.', 'error');
                            return;
                        }
                        
                        // Display QR code
                        qrCode.src = qrUrl;
                        downloadLink.href = qrUrl;
                        qrContainer.style.display = 'block';
                        
                        // Show success message
                        showMessage('QR code generated successfully!', 'success');
                        
                        // Scroll to QR code
                        qrContainer.scrollIntoView({ behavior: 'smooth' });
                    });
                } else {
                    throw new Error(data.error?.message || 'Upload failed');
                }
            } catch (err) {
                console.error(err);
                showMessage('Upload failed. Please try again.', 'error');
            } finally {
                // Hide loading state
                loading.style.display = 'none';
                generateBtn.disabled = false;
            }
        }
        
        function fileToBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result);
                reader.onerror = error => reject(error);
                reader.readAsDataURL(file);
            });
        }
        
        function showMessage(text, type) {
            message.textContent = text;
            message.className = `message ${type}`;
            message.style.display = 'block';
        }
        
        function hideMessage() {
            message.style.display = 'none';
        }
    </script>
</body>
</html>
