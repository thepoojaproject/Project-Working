<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Scanner</title>
    <script src="https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background-color: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 500px;
            overflow: hidden;
        }

        .header {
            background: linear-gradient(to right, #6a11cb, #2575fc);
            color: white;
            padding: 20px;
            text-align: center;
        }

        .header h1 {
            font-size: 24px;
            margin-bottom: 5px;
        }

        .header p {
            font-size: 14px;
            opacity: 0.9;
        }

        .scanner-container {
            padding: 20px;
            text-align: center;
        }

        #video-container {
            position: relative;
            width: 100%;
            margin: 0 auto 20px;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        #video {
            width: 100%;
            height: 300px;
            background-color: #f0f0f0;
            display: block;
        }

        #canvas {
            display: none;
        }

        .scan-area {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 200px;
            height: 200px;
            border: 2px solid #2575fc;
            border-radius: 10px;
            box-shadow: 0 0 0 1000px rgba(0, 0, 0, 0.3);
        }

        .scan-line {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background-color: #2575fc;
            animation: scan 2s linear infinite;
        }

        @keyframes scan {
            0% { top: 0; }
            50% { top: 100%; }
            100% { top: 0; }
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 20px;
        }

        button {
            padding: 12px 20px;
            border: none;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        #start-btn {
            background-color: #2575fc;
            color: white;
        }

        #stop-btn {
            background-color: #ff4757;
            color: white;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
        }

        button:active {
            transform: translateY(0);
        }

        button:disabled {
            background-color: #cccccc;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        .result-container {
            padding: 15px;
            background-color: #f8f9fa;
            border-radius: 10px;
            margin-top: 20px;
            text-align: left;
        }

        .result-container h3 {
            color: #2575fc;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        #result {
            word-break: break-all;
            padding: 10px;
            background-color: white;
            border-radius: 5px;
            min-height: 50px;
            border: 1px solid #e0e0e0;
        }

        .instructions {
            margin-top: 20px;
            padding: 15px;
            background-color: #f0f7ff;
            border-radius: 10px;
            font-size: 14px;
            color: #555;
        }

        .instructions h3 {
            color: #2575fc;
            margin-bottom: 10px;
        }

        .instructions ul {
            padding-left: 20px;
        }

        .instructions li {
            margin-bottom: 8px;
        }

        .status {
            margin: 10px 0;
            padding: 10px;
            border-radius: 5px;
            text-align: center;
            font-weight: 500;
        }

        .status.success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .status.error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .status.info {
            background-color: #cce7ff;
            color: #004085;
            border: 1px solid #b3d7ff;
        }

        .footer {
            text-align: center;
            padding: 15px;
            font-size: 12px;
            color: #777;
            border-top: 1px solid #eee;
        }

        @media (max-width: 600px) {
            .container {
                border-radius: 10px;
            }
            
            .scan-area {
                width: 180px;
                height: 180px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>QR Code Scanner</h1>
            <p>Scan any QR code with your camera</p>
        </div>
        
        <div class="scanner-container">
            <div id="video-container">
                <video id="video" playsinline></video>
                <div class="scan-area">
                    <div class="scan-line"></div>
                </div>
                <canvas id="canvas"></canvas>
            </div>
            
            <div id="status" class="status info">Click "Start Scanning" to begin</div>
            
            <div class="controls">
                <button id="start-btn">
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                        <path d="M8 5v14l11-7z"/>
                    </svg>
                    Start Scanning
                </button>
                <button id="stop-btn" disabled>
                    <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                        <path d="M6 6h12v12H6z"/>
                    </svg>
                    Stop
                </button>
            </div>
            
            <div class="result-container">
                <h3>
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
                        <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
                    </svg>
                    Scan Result
                </h3>
                <div id="result">No QR code scanned yet</div>
            </div>
            
            <div class="instructions">
                <h3>How to use:</h3>
                <ul>
                    <li>Click "Start Scanning" to activate your camera</li>
                    <li>Allow camera access when prompted</li>
                    <li>Point your camera at a QR code</li>
                    <li>Hold steady until the code is detected</li>
                    <li>Click "Stop" to end the scanning session</li>
                </ul>
            </div>
        </div>
        
        <div class="footer">
            QR Code Scanner &copy; 2023 | Uses jsQR library
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const video = document.getElementById('video');
            const canvas = document.getElementById('canvas');
            const context = canvas.getContext('2d');
            const startBtn = document.getElementById('start-btn');
            const stopBtn = document.getElementById('stop-btn');
            const resultDiv = document.getElementById('result');
            const statusDiv = document.getElementById('status');
            
            let stream = null;
            let scanning = false;
            let animationFrame = null;
            
            // Update status message
            function updateStatus(message, type = 'info') {
                statusDiv.textContent = message;
                statusDiv.className = `status ${type}`;
            }
            
            // Start scanning
            startBtn.addEventListener('click', async function() {
                try {
                    updateStatus('Requesting camera access...', 'info');
                    
                    // Request camera access
                    stream = await navigator.mediaDevices.getUserMedia({ 
                        video: { 
                            facingMode: "environment",
                            width: { ideal: 1280 },
                            height: { ideal: 720 }
                        } 
                    });
                    
                    video.srcObject = stream;
                    
                    // Wait for video to be ready
                    video.addEventListener('loadedmetadata', function() {
                        // Set canvas size to match video
                        canvas.width = video.videoWidth;
                        canvas.height = video.videoHeight;
                        
                        // Start scanning
                        scanning = true;
                        startBtn.disabled = true;
                        stopBtn.disabled = false;
                        updateStatus('Scanning for QR codes...', 'info');
                        
                        // Start the scanning loop
                        scanQRCode();
                    });
                    
                } catch (error) {
                    console.error('Error accessing camera:', error);
                    updateStatus('Error accessing camera: ' + error.message, 'error');
                }
            });
            
            // Stop scanning
            stopBtn.addEventListener('click', function() {
                scanning = false;
                startBtn.disabled = false;
                stopBtn.disabled = true;
                
                if (stream) {
                    stream.getTracks().forEach(track => track.stop());
                    stream = null;
                }
                
                if (animationFrame) {
                    cancelAnimationFrame(animationFrame);
                }
                
                updateStatus('Scanning stopped. Click "Start Scanning" to begin again.', 'info');
            });
            
            // QR Code scanning function
            function scanQRCode() {
                if (!scanning) return;
                
                // Draw current video frame to canvas
                if (video.readyState === video.HAVE_ENOUGH_DATA) {
                    context.drawImage(video, 0, 0, canvas.width, canvas.height);
                    
                    // Get image data from canvas
                    const imageData = context.getImageData(0, 0, canvas.width, canvas.height);
                    
                    // Try to decode QR code
                    const code = jsQR(imageData.data, imageData.width, imageData.height);
                    
                    if (code) {
                        // QR code found!
                        updateStatus('QR code detected!', 'success');
                        resultDiv.textContent = code.data;
                        
                        // Draw bounding box on canvas (optional)
                        drawBoundingBox(code.location);
                        
                        // Stop scanning after successful detection
                        setTimeout(() => {
                            stopBtn.click();
                        }, 2000);
                        
                        return;
                    }
                }
                
                // Continue scanning
                animationFrame = requestAnimationFrame(scanQRCode);
            }
            
            // Draw bounding box around detected QR code
            function drawBoundingBox(location) {
                context.strokeStyle = '#2575fc';
                context.lineWidth = 2;
                context.beginPath();
                context.moveTo(location.topLeftCorner.x, location.topLeftCorner.y);
                context.lineTo(location.topRightCorner.x, location.topRightCorner.y);
                context.lineTo(location.bottomRightCorner.x, location.bottomRightCorner.y);
                context.lineTo(location.bottomLeftCorner.x, location.bottomLeftCorner.y);
                context.closePath();
                context.stroke();
            }
        });
    </script>
</body>
</html>
