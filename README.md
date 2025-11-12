<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Retro QR Code Generator</title>
  <script src="https://cdn.jsdelivr.net/npm/qrcode/build/qrcode.min.js"></script>
  <style>
    /* === Retro Theme Colors === */
    :root {
      --bg: #f4f1e9;
      --panel: #fffaf0;
      --border: #b8b4a8;
      --text: #222;
      --accent: #2c2c2c;
      --button: #e0dccc;
      --button-hover: #cfcab8;
      --success: #1a8917;
      --error: #b00020;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: var(--bg);
      font-family: "Courier New", monospace;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-start;
      min-height: 100vh;
      color: var(--text);
      padding: 20px;
    }

    h1 {
      font-size: 1.5rem;
      letter-spacing: 1px;
      text-transform: uppercase;
      background: var(--accent);
      color: #fefefe;
      padding: 10px 20px;
      border: 2px solid var(--border);
      box-shadow: 2px 2px 0 #000;
    }

    .container {
      width: 100%;
      max-width: 700px;
      background: var(--panel);
      border: 3px solid var(--border);
      box-shadow: 6px 6px 0 #000;
      margin-top: 30px;
      padding: 25px;
      text-align: center;
    }

    .upload-box {
      border: 2px dashed var(--border);
      padding: 30px;
      margin-bottom: 15px;
      background: #fffef8;
      cursor: pointer;
      transition: background 0.2s;
    }

    .upload-box:hover {
      background: #f5f2e9;
    }

    .upload-box.active {
      background: #ede9da;
      border-color: #000;
    }

    .upload-icon {
      font-size: 2rem;
      margin-bottom: 10px;
    }

    .upload-text {
      font-size: 1rem;
    }

    input[type="file"] {
      display: none;
    }

    .file-info {
      font-size: 0.9rem;
      color: var(--accent);
      margin-top: 8px;
    }

    .image-preview {
      margin: 15px auto;
      max-width: 200px;
      border: 2px solid var(--border);
      box-shadow: 2px 2px 0 #000;
      display: none;
    }

    button {
      background: var(--button);
      border: 2px solid var(--border);
      box-shadow: 2px 2px 0 #000;
      color: var(--text);
      font-family: inherit;
      font-weight: bold;
      padding: 10px 20px;
      margin: 8px;
      cursor: pointer;
      transition: background 0.2s;
    }

    button:hover {
      background: var(--button-hover);
    }

    button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }

    .loading {
      display: none;
      margin-top: 15px;
      font-size: 0.9rem;
    }

    .message {
      display: none;
      margin: 15px 0;
      padding: 10px;
      border: 2px solid var(--border);
      box-shadow: 2px 2px 0 #000;
      font-weight: bold;
    }

    .success {
      background: #e5f7e2;
      color: var(--success);
    }

    .error {
      background: #f9e0e0;
      color: var(--error);
    }

    #qrContainer {
      margin-top: 25px;
      display: none;
    }

    .qr-code {
      border: 4px solid var(--border);
      background: #fff;
      box-shadow: 4px 4px 0 #000;
      margin-bottom: 15px;
    }

    .footer {
      margin-top: 30px;
      font-size: 0.85rem;
      color: var(--accent);
      border-top: 2px solid var(--border);
      padding-top: 10px;
      text-align: center;
    }

    @media (max-width: 600px) {
      .container {
        padding: 15px;
      }
      h1 {
        font-size: 1.2rem;
      }
    }
  </style>
</head>
<body>
  <h1>QR Code Generator</h1>

  <div class="container">
    <label class="upload-box" id="uploadBox">
      <div class="upload-icon">𓂃🖊</div>
      <div class="upload-text">Click or Drag & Drop Image</div>
      <div class="file-info" id="fileInfo"></div>
      <input type="file" id="imageInput" accept="image/*" />
    </label>

    <img id="imagePreview" class="image-preview" alt="Preview" />

    <div class="message" id="message"></div>

    <button id="generateBtn" onclick="uploadAndGenerateQR()">Generate QR</button>

    <div class="loading" id="loading">Uploading... Please wait ⏳</div>

    <div id="qrContainer">
      <img id="qrCode" class="qr-code" alt="QR Code" />
      <a id="downloadLink" download="qr-code.png">
        <button>⬇ Download QR</button>
      </a>
    </div>
  </div>

  <div class="footer">
    Made with ❤️ By Armeen
  </div>

  <script>
    const imgbbAPIKey = "1cf8feb97ba0aa50a1ee75cef54488ec";
    const uploadBox = document.getElementById("uploadBox");
    const fileInput = document.getElementById("imageInput");
    const fileInfo = document.getElementById("fileInfo");
    const imagePreview = document.getElementById("imagePreview");
    const generateBtn = document.getElementById("generateBtn");
    const qrContainer = document.getElementById("qrContainer");
    const qrCode = document.getElementById("qrCode");
    const downloadLink = document.getElementById("downloadLink");
    const loading = document.getElementById("loading");
    const message = document.getElementById("message");

    // Drag & Drop
    uploadBox.addEventListener("dragover", (e) => {
      e.preventDefault();
      uploadBox.classList.add("active");
    });

    uploadBox.addEventListener("dragleave", () => {
      uploadBox.classList.remove("active");
    });

    uploadBox.addEventListener("drop", (e) => {
      e.preventDefault();
      uploadBox.classList.remove("active");
      if (e.dataTransfer.files.length > 0) {
        fileInput.files = e.dataTransfer.files;
        handleFileSelection();
      }
    });

    fileInput.addEventListener("change", handleFileSelection);

    function handleFileSelection() {
      const file = fileInput.files[0];
      if (!file) return;

      if (file.size > 5 * 1024 * 1024) {
        showMessage("❌ File exceeds 5MB", "error");
        resetFileInput();
        return;
      }

      if (!file.type.match("image.*")) {
        showMessage("❌ Only image files allowed", "error");
        resetFileInput();
        return;
      }

      fileInfo.textContent = `📄 ${file.name}`;
      const reader = new FileReader();
      reader.onload = (e) => {
        imagePreview.src = e.target.result;
        imagePreview.style.display = "block";
      };
      reader.readAsDataURL(file);
      hideMessage();
    }

    function resetFileInput() {
      fileInput.value = "";
      fileInfo.textContent = "";
      imagePreview.style.display = "none";
    }

    async function uploadAndGenerateQR() {
      const file = fileInput.files[0];
      if (!file) {
        showMessage("⚠️ Select an image first!", "error");
        return;
      }

      loading.style.display = "block";
      generateBtn.disabled = true;
      hideMessage();

      try {
        const base64 = await fileToBase64(file);
        const formData = new FormData();
        formData.append("key", imgbbAPIKey);
        formData.append("image", base64.split(",")[1]);

        const response = await fetch("https://api.imgbb.com/1/upload", {
          method: "POST",
          body: formData,
        });

        const data = await response.json();

        if (data.success) {
          const imageUrl = data.data.url;

          QRCode.toDataURL(
            imageUrl,
            { width: 200, margin: 1, color: { dark: "#000", light: "#fff" } },
            (err, qrUrl) => {
              if (err) {
                showMessage("QR generation failed", "error");
                return;
              }
              qrCode.src = qrUrl;
              downloadLink.href = qrUrl;
              qrContainer.style.display = "block";
              showMessage("✅ QR Code Ready!", "success");
            }
          );
        } else {
          throw new Error("Upload failed");
        }
      } catch (err) {
        showMessage("❌ Upload Error", "error");
      } finally {
        loading.style.display = "none";
        generateBtn.disabled = false;
      }
    }

    function fileToBase64(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = () => resolve(reader.result);
        reader.onerror = (error) => reject(error);
        reader.readAsDataURL(file);
      });
    }

    function showMessage(text, type) {
      message.textContent = text;
      message.className = `message ${type}`;
      message.style.display = "block";
    }

    function hideMessage() {
      message.style.display = "none";
    }
  </script>
</body>
</html>
