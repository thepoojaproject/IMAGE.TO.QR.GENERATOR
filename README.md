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
            --success: #4cc9f0;
            --error: #f72585;
            --gray: #6c757d;
            --border-radius: 12px;
            --box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            --transition: all 0.3s ease;
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
            background: #000000;
            color: var(--dark);
            padding: 20px;
            line-height: 1.6;
        }

        .container {
            max-width: 800px;
            width: 100%;
            margin: 0 auto;
            padding: 30px;
            background: #ffffff;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            margin-top: 20px;
            border: 1px solid #e0e0e0;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
        }

        .logo {
            max-width: 300px;
            margin: 0 auto 20px;
            display: block;
        }

        .subtitle {
            color: #666;
            font-size: 1.1rem;
            max-width: 600px;
            margin: 0 auto 20px;
        }

        .upload-section {
            margin-bottom: 30px;
        }

        .upload-box {
            border: 2px dashed #ddd;
            border-radius: var(--border-radius);
            padding: 40px 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
            background: #f9f9f9;
            text-align: center;
            margin-bottom: 20px;
            position: relative;
            overflow: hidden;
        }

        .upload-box:hover {
            border-color: var(--primary);
            background: #f0f4ff;
        }

        .upload-box.active {
            border-color: var(--primary);
            background: #f0f4ff;
        }

        .upload-icon {
            font-size: 48px;
            margin-bottom: 15px;
            color: var(--primary);
        }

        .upload-text {
            font-size: 1.2rem;
            margin-bottom: 10px;
            color: var(--dark);
        }

        .upload-subtext {
            font-size: 0.9rem;
            color: #888;
        }

        .file-info {
            margin-top: 15px;
            padding: 10px 15px;
            background: rgba(67, 97, 238, 0.1);
            border-radius: 8px;
            font-size: 0.9rem;
            display: none;
            color: #333;
            border-left: 3px solid var(--primary);
        }

        input[type="file"] {
            display: none;
        }

        .button-group {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 20px;
            flex-wrap: wrap;
        }

        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: var(--transition);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        button:hover {
            background: var(--primary-dark);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        button:disabled {
            background: #aaa;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        button.secondary {
            background: transparent;
            color: var(--primary);
            border: 2px solid var(--primary);
        }

        button.secondary:hover {
            background: rgba(67, 97, 238, 0.1);
        }

        #qrContainer {
            margin-top: 30px;
            text-align: center;
            display: none;
        }

        #qrContainer.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        #qrCode {
            max-width: 250px;
            border-radius: var(--border-radius);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
            margin: 0 auto 20px;
            display: block;
            padding: 15px;
            background: white;
        }

        .qr-info {
            margin-top: 20px;
            padding: 15px;
            background: #f9f9f9;
            border-radius: var(--border-radius);
            font-size: 0.9rem;
            text-align: left;
            max-width: 500px;
            margin: 20px auto;
            color: #333;
            border: 1px solid #e0e0e0;
        }

        .qr-info a {
            color: var(--primary);
            text-decoration: none;
            word-break: break-all;
        }

        .qr-info a:hover {
            text-decoration: underline;
        }

        .loading {
            display: none;
            text-align: center;
            margin: 20px 0;
        }

        .loading.active {
            display: block;
        }

        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: var(--primary);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        .error {
            display: none;
            background: rgba(247, 37, 133, 0.1);
            color: var(--error);
            padding: 15px;
            border-radius: var(--border-radius);
            margin: 20px 0;
            text-align: center;
            border-left: 3px solid var(--error);
        }

        .error.active {
            display: block;
        }

        .footer {
            margin-top: 40px;
            text-align: center;
            color: #aaa;
            font-size: 0.9rem;
            padding: 20px;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }
            
            .logo {
                max-width: 250px;
            }
            
            .button-group {
                flex-direction: column;
                width: 100%;
            }
            
            button {
                width: 100%;
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <img src="https://i.ibb.co/0y7FWjF1/image.png" alt="QR Code Generator Logo" class="logo">
            <p class="subtitle">Upload any image and generate a QR code that links directly to your uploaded image</p>
        </header>

        <div class="upload-section">
            <label class="upload-box" id="uploadBox">
                <div class="upload-icon">𓂃🖊</div>
                <div class="upload-text">Drag & Drop or Click to Upload</div>
                <div class="upload-subtext">Supports JPG, PNG, GIF, and other image formats</div>
                <input type="file" id="imageInput" accept="image/*">
            </label>
            
            <div class="file-info" id="fileInfo"></div>
            
            <div class="button-group">
                <button id="generateBtn" onclick="uploadAndGenerateQR()">
                    <span>Generate QR Code</span>
                </button>
                <button class="secondary" onclick="resetAll()">
                    <span>Reset</span>
                </button>
            </div>
        </div>

        <div class="loading" id="loading">
            <div class="spinner"></div>
            <p>Uploading image and generating QR code...</p>
        </div>

        <div class="error" id="error"></div>

        <div id="qrContainer">
            <img id="qrCode" src="" alt="QR Code">
            <div class="qr-info">
                <p><strong>Image URL:</strong> <a id="imageUrl" href="" target="_blank"></a></p>
            </div>
            <div class="button-group">
                <a id="downloadLink" href="" download="qr-code.png">
                    <button>
                        <span>Download QR Code</span>
                    </button>
                </a>
            </div>
        </div>
    </div>

    <div class="footer">Made with ❤️ by Armeen | Powered by ImgBB</div>

    <script>
        const imgbbAPIKey = '1cf8feb97ba0aa50a1ee75cef54488ec';
        const uploadBox = document.getElementById('uploadBox');
        const fileInput = document.getElementById('imageInput');
        const fileInfo = document.getElementById('fileInfo');
        const generateBtn = document.getElementById('generateBtn');
        const loading = document.getElementById('loading');
        const error = document.getElementById('error');
        const qrContainer = document.getElementById('qrContainer');
        const qrCode = document.getElementById('qrCode');
        const imageUrl = document.getElementById('imageUrl');
        const downloadLink = document.getElementById('downloadLink');

        // Set initial state
        generateBtn.disabled = true;

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

        // File input change
        fileInput.addEventListener('change', handleFileSelection);

        function handleFileSelection() {
            const file = fileInput.files[0];
            if (!file) return;
            
            // Check if file is an image
            if (!file.type.match('image.*')) {
                showError('Please select a valid image file (JPG, PNG, GIF, etc.)');
                resetFileInput();
                return;
            }
            
            // Check file size (max 10MB)
            if (file.size > 10 * 1024 * 1024) {
                showError('File size too large. Please select an image smaller than 10MB.');
                resetFileInput();
                return;
            }
            
            // Display file info
            const fileSize = (file.size / (1024 * 1024)).toFixed(2);
            fileInfo.innerHTML = `
                <strong>Selected file:</strong> ${file.name}<br>
                <strong>Size:</strong> ${fileSize} MB<br>
                <strong>Type:</strong> ${file.type}
            `;
            fileInfo.style.display = 'block';
            
            // Enable generate button
            generateBtn.disabled = false;
            
            // Hide any previous error
            hideError();
            
            // Hide previous QR code
            qrContainer.classList.remove('active');
        }

        function resetFileInput() {
            fileInput.value = '';
            fileInfo.style.display = 'none';
            generateBtn.disabled = true;
        }

        function resetAll() {
            resetFileInput();
            hideError();
            qrContainer.classList.remove('active');
            uploadBox.classList.remove('active');
        }

        function showError(message) {
            error.textContent = message;
            error.classList.add('active');
        }

        function hideError() {
            error.classList.remove('active');
        }

        async function uploadAndGenerateQR() {
            const file = fileInput.files[0];
            if (!file) {
                showError('Please select an image first!');
                return;
            }

            // Show loading, hide error
            loading.classList.add('active');
            hideError();
            generateBtn.disabled = true;

            try {
                // Convert file to base64
                const base64 = await readFileAsBase64(file);
                
                // Upload to ImgBB
                const imageUrlValue = await uploadToImgBB(base64);
                
                // Generate QR code
                const qrCodeUrl = await generateQRCode(imageUrlValue);
                
                // Display results
                displayResults(qrCodeUrl, imageUrlValue);
                
            } catch (err) {
                console.error(err);
                showError('An error occurred. Please try again.');
            } finally {
                loading.classList.remove('active');
                generateBtn.disabled = false;
            }
        }

        function readFileAsBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = () => resolve(reader.result.split(',')[1]);
                reader.onerror = () => reject(reader.error);
                reader.readAsDataURL(file);
            });
        }

        async function uploadToImgBB(base64) {
            const formData = new FormData();
            formData.append('key', imgbbAPIKey);
            formData.append('image', base64);

            const response = await fetch('https://api.imgbb.com/1/upload', {
                method: 'POST',
                body: formData
            });

            const data = await response.json();
            
            if (data.success) {
                return data.data.url;
            } else {
                throw new Error(data.error?.message || 'Upload failed');
            }
        }

        function generateQRCode(url) {
            return new Promise((resolve, reject) => {
                QRCode.toDataURL(url, { 
                    width: 300,
                    margin: 2,
                    color: {
                        dark: '#4361ee',
                        light: '#ffffff'
                    }
                }, (err, qrUrl) => {
                    if (err) reject(err);
                    else resolve(qrUrl);
                });
            });
        }

        function displayResults(qrCodeUrl, imageUrlValue) {
            qrCode.src = qrCodeUrl;
            imageUrl.href = imageUrlValue;
            imageUrl.textContent = imageUrlValue;
            downloadLink.href = qrCodeUrl;
            qrContainer.classList.add('active');
            
            // Scroll to QR code
            qrContainer.scrollIntoView({ behavior: 'smooth' });
        }
    </script>
</body>
</html>
