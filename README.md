<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Free online tool to convert images to JPG, PNG, BMP, GIF, and WEBP formats instantly.">
    <title>Free Image Converter Tool | Convert Images in Seconds</title>
    <!-- Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #161978;
            --background: #f7f7fa;
            --text-color: #030412;
        }

        .dark-mode {
            --background: #121212;
            --text-color: #ffffff;
        }

        * {
            box-sizing: border-box;
            transition: background 0.5s, color 0.5s;
        }

        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background:  #edf5f2;
            color: var(--text-color);
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            margin-bottom: 40px;
        }

        .converter-box {
            background: #12de63;
            padding: 30px;
            border-radius: 12px;
            text-align: center;
        }

        #uploadSection {
            border: 2px dashed var(--primary-color);
            padding: 30px;
            margin: 20px 0;
            cursor: pointer;
        }

        .button {
            background: var(--primary-color);
            color: white;
            padding: 12px 24px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
            margin: 10px;
        }

        .button:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }

        #progressBar {
            width: 100%;
            height: 20px;
            background: #d9c0ba;
            border-radius: 10px;
            margin: 20px 0;
            display: none;
        }

        #progressFill {
            width: 0%;
            height: 100%;
            background: #e33c12;
            border-radius: 10px;
            transition: width 0.3s;
        }

        .notification {
            display: none;
            padding: 15px;
            margin: 20px 0;
            border-radius: 6px;
        }

        .success {
            background: #d9c0ba;
            color: #155724;
        }

        .dark-mode-toggle {
            position: fixed;
            top: 20px;
            right: 20px;
            cursor: pointer;
        }

        .social-share {
            margin: 30px 0;
            text-align: center;
        }

        .social-share a {
            margin: 0 10px;
            font-size: 24px;
            color: var(--text-color);
        }

        /* Responsive Design */
        @media (max-width: 600px) {
            .converter-box {
                padding: 15px;
            }
            #uploadSection {
                padding: 15px;
            }
        }
    </style>
</head>
<body>
    <div class="dark-mode-toggle">
        <i class="fas fa-moon" id="darkModeToggle"></i>
    </div>

    <div class="container">
        <div class="header">
            <h1>Free Image Converter Tool</h1> 
            <p>Convert images to JPG, PNG, BMP, GIF, WEBP in seconds!</p>
        </div>

        <div class="converter-box">
            <div id="uploadSection" onclick="document.getElementById('imageUpload').click()">
                <i class="fas fa-cloud-upload-alt fa-3x"></i>
                <p>Click to upload or drag and drop</p>
                <input type="file" id="imageUpload" accept="image/*" hidden>
            </div>

            <select id="formatSelect" class="button">
                <option value="jpg">JPG</option>
                <option value="png">PNG</option>
                <option value="webp">WEBP</option>
              <option value="Gif">GIF</option>
            </select>

            <button class="button" id="convertButton">Convert Now</button>

            <div id="progressBar">
                <div id="progressFill"></div>
            </div>

            <div class="notification success" id="successNotification">
                Image uploaded successfully!
            </div>

            <a id="downloadLink" class="button" style="display: none;">
                <i class="fas fa-download"></i> Download Image
            </a>
        </div>

        <!-- FAQ Section -->
        <div class="faq">
            <h2>FAQ</h2>
            <details>
                <summary>What is the best format for web images?</summary>
                <p>WEBP is recommended for web images due to its superior compression and quality balance.</p>
            </details>
            <!-- Add more FAQ items -->
        </div>

        <!-- Social Sharing -->
        <div class="social-share">
            <a href="#" title="Share on Twitter"><i class="fab fa-twitter"></i></a>
            <a href="#" title="Share on Facebook"><i class="fab fa-facebook"></i></a>
            <!-- Add more social links -->
        </div>
    </div>

    <script>
        // Dark Mode Toggle
        document.getElementById('darkModeToggle').addEventListener('click', function() {
            document.body.classList.toggle('dark-mode');
            this.classList.toggle('fa-moon');
            this.classList.toggle('fa-sun');
        });

        // Image Conversion Logic
        document.getElementById('convertButton').addEventListener('click', convertImage);
        
        function convertImage() {
            const file = document.getElementById('imageUpload').files[0];
            const format = document.getElementById('formatSelect').value;
            
            if (!file) {
                alert('Please select an image first!');
                return;
            }

            // Show progress bar
            document.getElementById('progressBar').style.display = 'block';

            const reader = new FileReader();
            
            reader.onprogress = function(e) {
                if (e.lengthComputable) {
                    const percent = (e.loaded / e.total) * 100;
                    document.getElementById('progressFill').style.width = percent + '%';
                }
            };

            reader.onload = function(e) {
                const img = new Image();
                img.src = e.target.result;
                
                img.onload = function() {
                    const canvas = document.createElement('canvas');
                    canvas.width = img.width;
                    canvas.height = img.height;
                    
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(img, 0, 0);
                    
                    // Convert to selected format
                    let mimeType = 'image/jpeg';
                    if (format === 'png') mimeType = 'image/png';
                    if (format === 'webp') mimeType = 'image/webp';
                     if (format ==='Gif')  mimeType = 'image/Gif';

                    const convertedData = canvas.toDataURL(mimeType, 0.9);
                    
                    // Show download button
                    const downloadLink = document.getElementById('downloadLink');
                    downloadLink.href = convertedData;
                    downloadLink.download = `converted_image.${format}`;
                    downloadLink.style.display = 'inline-block';
                    
                    // Show success notification
                    document.getElementById('successNotification').style.display = 'block';
                    document.getElementById('progressBar').style.display = 'none';
                };
            };

            reader.readAsDataURL(file);
        }
    </script>
</body>
</html>
