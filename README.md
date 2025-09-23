
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Image to QR Generator</title>
<script src="https://cdn.jsdelivr.net/npm/qrcode/build/qrcode.min.js"></script>
<style>
  body {
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 50px 20px;
    background: #f5f5f5;
    color: #333;
    min-height: 100vh;
    justify-content: flex-start;
  }

  h1 { margin-bottom: 30px; font-size: 1.8rem; }

  .upload-box {
    border: 2px dashed #ccc;
    border-radius: 10px;
    width: 300px;
    height: 150px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    margin-bottom: 20px;
    text-align: center;
    transition: border-color 0.2s;
    background: #fff;
  }

  .upload-box:hover { border-color: #333; }

  input[type="file"] { display: none; }

  button {
    background: #333;
    color: #fff;
    border: none;
    padding: 10px 20px;
    border-radius: 8px;
    cursor: pointer;
    margin: 5px auto;
    transition: background 0.2s;
    display: block;
  }

  button:hover { background: #555; }

  #qrContainer {
    margin-top: 20px;
    text-align: center;
  }

  #qrContainer img {
    max-width: 250px;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    display: block;
    margin: 0 auto 15px;
  }

  .footer {
    margin-top: auto;
    padding-top: 30px;
    font-size: 0.9rem;
    color: #666;
    text-align: center;
  }

  @media (max-width: 400px) {
    .upload-box { width: 90%; height: 120px; }
    button { width: 90%; }
    #qrContainer img { max-width: 90%; }
  }
</style>
</head>
<body>

<h1>Image to QR Generator</h1>

<label class="upload-box" id="uploadBox">
  Drag & Drop or Click to Upload
  <input type="file" id="imageInput" accept="image/*">
</label>

<button onclick="uploadAndGenerateQR()">Generate QR Code</button>

<div id="qrContainer"></div>

<div class="footer">Made with ❤️ by Armeen</div>

<script>
const imgbbAPIKey = '1cf8feb97ba0aa50a1ee75cef54488ec';
const uploadBox = document.getElementById('uploadBox');
const fileInput = document.getElementById('imageInput');

uploadBox.addEventListener('dragover', (e) => { e.preventDefault(); uploadBox.style.borderColor = '#333'; });
uploadBox.addEventListener('dragleave', () => { uploadBox.style.borderColor = '#ccc'; });
uploadBox.addEventListener('drop', (e) => { e.preventDefault(); uploadBox.style.borderColor = '#ccc'; if(e.dataTransfer.files.length>0) fileInput.files = e.dataTransfer.files; });

async function uploadAndGenerateQR() {
    const file = fileInput.files[0];
    if (!file) { alert('Please select an image!'); return; }

    const reader = new FileReader();
    reader.onload = async function() {
        const base64 = reader.result.split(',')[1];
        const formData = new FormData();
        formData.append('key', imgbbAPIKey);
        formData.append('image', base64);

        try {
            const response = await fetch('https://api.imgbb.com/1/upload', { method:'POST', body: formData });
            const data = await response.json();
            if(data.success){
                const imageUrl = data.data.url;
                QRCode.toDataURL(imageUrl, { width: 250 }, (err, qrUrl)=>{
                    if(err){ console.error(err); return; }
                    document.getElementById('qrContainer').innerHTML = `
                        <img src="${qrUrl}" alt="QR Code">
                        <a href="${qrUrl}" download="qr.png">
                          <button>Download QR</button>
                        </a>
                    `;
                });
            } else { alert('Upload failed: ' + data.error.message); }
        } catch(err) { console.error(err); alert('Upload failed.'); }
    }
    reader.readAsDataURL(file);
}
</script>

</body>
</html>
