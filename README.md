<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Generator</title>
    <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: #000;
            color: #333;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: #fff;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(255, 255, 255, 0.2);
            overflow: hidden;
            padding: 40px 30px;
        }

        .logo {
            text-align: center;
            margin-bottom: 20px;
        }

        .logo img {
            max-width: 180px;
            height: auto;
        }

        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
            font-size: 1rem;
        }

        .upload-box {
            border: 2px dashed #ddd;
            border-radius: 12px;
            padding: 40px 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-bottom: 20px;
            background: #f9f9f9;
        }

        .upload-box:hover {
            border-color: #667eea;
            background: #f0f4ff;
        }

        .upload-box.dragover {
            border-color: #667eea;
            background: #e6f0ff;
        }

        .upload-icon {
            font-size: 48px;
            margin-bottom: 15px;
            color: #667eea;
            line-height: 1;
        }

        .upload-text {
            font-size: 1.1rem;
            color: #333;
            margin-bottom: 5px;
        }

        .upload-hint {
            font-size: 0.9rem;
            color: #888;
        }

        input[type="file"] {
            display: none;
        }

        .file-info {
            margin-top: 15px;
            padding: 12px 15px;
            background: #f0f4ff;
            border-radius: 8px;
            display: none;
            text-align: left;
            border: 1px solid #e0e6ff;
        }

        .file-info.show {
            display: block;
            animation: fadeIn 0.3s ease;
        }

        .file-name {
            font-weight: 600;
            color: #333;
            display: block;
        }

        .file-size {
            color: #666;
            font-size: 0.85rem;
            display: block;
            margin-top: 5px;
        }

        .btn {
            background: linear-gradient(to right, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 14px 24px;
            border-radius: 50px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s ease;
            width: 100%;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
            margin-top: 10px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 7px 20px rgba(102, 126, 234, 0.6);
        }

        .btn:active {
            transform: translateY(0);
        }

        .btn:disabled {
            background: #ccc;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        .qr-section {
            text-align: center;
            margin-top: 30px;
            display: none;
        }

        .qr-section.show {
            display: block;
            animation: fadeInUp 0.5s ease;
        }

        .qr-title {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #333;
        }

        .qr-code {
            margin: 0 auto 25px;
            padding: 20px;
            background: #fff;
            border-radius: 12px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            display: inline-block;
            border: 1px solid #eee;
        }

        .qr-code img {
            border-radius: 8px;
            max-width: 100%;
            height: auto;
        }

        .btn-download {
            background: linear-gradient(to right, #48bb78, #38a169);
            box-shadow: 0 4px 15px rgba(72, 187, 120, 0.4);
        }

        .btn-download:hover {
            box-shadow: 0 7px 20px rgba(72, 187, 120, 0.6);
        }

        .loading {
            display: none;
            text-align: center;
            margin: 20px 0;
        }

        .loading.show {
            display: block;
        }

        .spinner {
            border: 4px solid rgba(102, 126, 234, 0.2);
            border-left-color: #667eea;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        .loading-text {
            color: #667eea;
            font-weight: 500;
        }

        .footer {
            text-align: center;
            margin-top: 30px;
            color: #fff;
            font-size: 0.9rem;
        }

        .footer a {
            color: #aaa;
            text-decoration: none;
            font-weight: 500;
            transition: color 0.3s ease;
        }

        .footer a:hover {
            color: #667eea;
        }

        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @media (max-width: 600px) {
            .container {
                padding: 30px 20px;
            }
            
            .logo img {
                max-width: 150px;
            }
            
            .upload-box {
                padding: 30px 15px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="logo">
            <img src="https://i.ibb.co/0y7FWjF1/image.png" alt="QR Code Generator Logo">
        </div>
        <p class="subtitle">Upload an image to generate a QR code that links to it</p>
        
        <div class="upload-section">
            <label class="upload-box" id="uploadBox">
                <div class="upload-icon">𓂃🖊</div>
                <div class="upload-text">Drag & Drop or Click to Upload</div>
                <div class="upload-hint">Supports JPG, PNG, GIF (Max 10MB)</div>
                <input type="file" id="imageInput" accept="image/*">
            </label>
            
            <div class="file-info" id="fileInfo">
                <span class="file-name" id="fileName"></span>
                <span class="file-size" id="fileSize"></span>
            </div>
            
            <button class="btn" id="generateBtn" onclick="uploadAndGenerateQR()" disabled>
                Generate QR Code
            </button>
            
            <div class="loading" id="loading">
                <div class="spinner"></div>
                <p class="loading-text">Uploading image and generating QR code...</p>
            </div>
        </div>
        
        <div class="qr-section" id="qrSection">
            <h2 class="qr-title">Your QR Code</h2>
            <div class="qr-code" id="qrCode"></div>
            <a href="#" id="downloadLink" download="qr-code.png">
                <button class="btn btn-download">Download QR Code</button>
            </a>
        </div>
    </div>
    
    <div class="footer">
        Made with ❤️ by Armeen | Powered by <a href="https://imgbb.com/" target="_blank">ImgBB</a>
    </div>

    <script>
        // Use a demo API key (in a real app, this would be handled server-side)
        const imgbbAPIKey = '1cf8feb97ba0aa50a1ee75cef54488ec';
        
        const uploadBox = document.getElementById('uploadBox');
        const fileInput = document.getElementById('imageInput');
        const fileInfo = document.getElementById('fileInfo');
        const fileName = document.getElementById('fileName');
        const fileSize = document.getElementById('fileSize');
        const generateBtn = document.getElementById('generateBtn');
        const loading = document.getElementById('loading');
        const qrSection = document.getElementById('qrSection');
        const qrCode = document.getElementById('qrCode');
        const downloadLink = document.getElementById('downloadLink');

        // File input change handler
        fileInput.addEventListener('change', function() {
            if (this.files && this.files[0]) {
                const file = this.files[0];
                updateFileInfo(file);
                generateBtn.disabled = false;
            }
        });

        // Drag and drop handlers
        uploadBox.addEventListener('dragover', (e) => {
            e.preventDefault();
            uploadBox.classList.add('dragover');
        });

        uploadBox.addEventListener('dragleave', () => {
            uploadBox.classList.remove('dragover');
        });

        uploadBox.addEventListener('drop', (e) => {
            e.preventDefault();
            uploadBox.classList.remove('dragover');
            
            if (e.dataTransfer.files.length > 0) {
                fileInput.files = e.dataTransfer.files;
                const file = e.dataTransfer.files[0];
                updateFileInfo(file);
                generateBtn.disabled = false;
            }
        });

        // Update file info display
        function updateFileInfo(file) {
            fileName.textContent = file.name;
            fileSize.textContent = formatFileSize(file.size);
            fileInfo.classList.add('show');
        }

        // Format file size
        function formatFileSize(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }

        // Main function to upload image and generate QR code
        async function uploadAndGenerateQR() {
            const file = fileInput.files[0];
            if (!file) {
                alert('Please select an image!');
                return;
            }

            // Check file size (10MB limit)
            if (file.size > 10 * 1024 * 1024) {
                alert('File size exceeds 10MB limit. Please choose a smaller image.');
                return;
            }

            // Show loading state
            loading.classList.add('show');
            generateBtn.disabled = true;

            const reader = new FileReader();
            reader.onload = async function() {
                const base64 = reader.result.split(',')[1];
                const formData = new FormData();
                formData.append('key', imgbbAPIKey);
                formData.append('image', base64);

                try {
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
                                dark: '#2d3748',
                                light: '#ffffff'
                            }
                        }, (err, qrUrl) => {
                            if (err) {
                                console.error(err);
                                alert('Error generating QR code');
                                loading.classList.remove('show');
                                generateBtn.disabled = false;
                                return;
                            }
                            
                            // Display QR code
                            qrCode.innerHTML = `<img src="${qrUrl}" alt="QR Code">`;
                            downloadLink.href = qrUrl;
                            
                            // Show QR section
                            qrSection.classList.add('show');
                            
                            // Hide loading
                            loading.classList.remove('show');
                        });
                    } else {
                        alert('Upload failed: ' + (data.error?.message || 'Unknown error'));
                        loading.classList.remove('show');
                        generateBtn.disabled = false;
                    }
                } catch(err) {
                    console.error(err);
                    alert('Upload failed. Please check your connection and try again.');
                    loading.classList.remove('show');
                    generateBtn.disabled = false;
                }
            }
            
            reader.readAsDataURL(file);
        }
    </script>
</body>
</html>
