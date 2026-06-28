<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Special Celebration System</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Urbanist:wght@400;600;700;800&family=Dancing+Script:wght@700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body { background-color: #000000; color: #ffffff; font-family: 'Urbanist', sans-serif; overflow: hidden; height: 100vh; }

        /* ================= 1. OVERLAY GERBANG UTAMA ================= */
        #overlay-gate {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background: #000000; z-index: 10000; display: flex;
            flex-direction: column; justify-content: center; align-items: center;
            text-align: center; padding: 20px;
        }
        .gate-btn {
            background: transparent; color: #ff3377; border: 2px solid #ff3377;
            padding: 15px 35px; font-size: 1rem; font-weight: 800; letter-spacing: 3px;
            text-transform: uppercase; border-radius: 4px; cursor: pointer;
            box-shadow: 0 0 15px rgba(255, 51, 119, 0.2);
            transition: all 0.3s ease; display: flex; align-items: center; gap: 10px;
        }
        .gate-btn:hover {
            background: #ff3377; color: #000000; box-shadow: 0 0 30px rgba(255, 51, 119, 0.6);
        }
        .gate-text {
            color: #8492a6; font-size: 0.8rem; margin-top: 15px; letter-spacing: 1px;
        }

        /* ================= 2. CELEBRATION LOADING SCREEN (CYBER PINK MATRIX) ================= */
        #loading-screen {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background: #000000; z-index: 9999; display: flex;
            justify-content: center; align-items: center;
            transition: opacity 1s ease, visibility 1s; opacity: 1;
        }
        #matrix-canvas { position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1; }
        .matrix-mask {
            position: absolute; top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle, rgba(0,0,0,0.7) 0%, rgba(0,0,0,0.9) 80%, #000000 100%);
            z-index: 2;
        }
        
        .loading-content {
            position: relative; z-index: 3; display: flex; flex-direction: column;
            align-items: center; text-align: center; padding: 20px; pointer-events: none;
            width: 100%; max-width: 600px;
        }
        
        .glow-aura {
            position: absolute; width: 300px; height: 300px;
            background: radial-gradient(circle, rgba(255, 51, 119, 0.2) 0%, transparent 70%);
            animation: pulseGlow 3s infinite alternate; z-index: -1;
        }

        .hbd-text {
            font-family: 'Dancing Script', cursive;
            font-size: 3.5rem; color: #ffffff; margin-bottom: 10px;
            text-shadow: 0 0 10px #ff3377, 0 0 20px #ff3377, 0 0 40px #ff0055;
            animation: textFloat 2.5s ease-in-out infinite alternate, neonFlicker 4s infinite;
        }

        .sub-wish {
            font-size: 1.1rem; font-weight: 700; color: #00e5ff;
            letter-spacing: 2px; text-transform: uppercase;
            text-shadow: 0 0 8px rgba(0, 229, 255, 0.6);
            margin-bottom: 25px; opacity: 0; animation: fadeInDelayed 1.5s 1s forwards;
        }

        .progress-container {
            width: 220px; height: 4px; background: rgba(255, 255, 255, 0.1);
            border-radius: 10px; overflow: hidden; position: relative;
            box-shadow: 0 0 10px rgba(255, 51, 119, 0.3);
        }
        .progress-bar {
            height: 100%; width: 0%;
            background: linear-gradient(90deg, #ff3377, #00e5ff);
            box-shadow: 0 0 15px #ff3377;
            animation: fillProgress 7.5s linear forwards;
        }

        /* ================= 3. HALAMAN UTAMA (MAIN APP CARD) ================= */
        #main-app {
            display: none; height: 100vh; width: 100vw;
            display: flex; justify-content: center; align-items: center;
            background: radial-gradient(circle at center, #0f0c20 0%, #05020a 100%);
            padding: 20px; opacity: 0; transition: opacity 1.5s ease;
        }
        
        .main-card {
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 51, 119, 0.15);
            backdrop-filter: blur(15px);
            padding: 40px 30px; border-radius: 24px;
            text-align: center; max-width: 450px; width: 100%;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5), inset 0 0 20px rgba(255, 51, 119, 0.05);
            animation: cardEntrance 1.2s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }

        .icon-box {
            width: 70px; height: 70px; background: rgba(255, 51, 119, 0.1);
            border-radius: 50%; display: flex; justify-content: center; align-items: center;
            margin: 0 auto 20px; color: #ff3377; font-size: 1.8rem;
            border: 1px solid rgba(255, 51, 119, 0.3);
            box-shadow: 0 0 20px rgba(255, 51, 119, 0.2);
        }

        .main-card h2 {
            font-size: 1.8rem; font-weight: 800; margin-bottom: 15px;
            letter-spacing: 0.5px; background: linear-gradient(45deg, #ffffff, #ffb3cc);
            -webkit-background-clip: text; -webkit-text-fill-color: transparent;
        }

        .sweet-message {
            font-size: 0.95rem; color: #b0bddb; line-height: 1.7;
            margin-bottom: 25px; font-weight: 400;
        }

        .smile-btn {
            background: linear-gradient(90deg, #ff3377, #e6005c); color: #ffffff;
            border: none; padding: 14px 28px; font-size: 0.85rem; font-weight: 700;
            letter-spacing: 1px; border-radius: 30px; cursor: pointer;
            box-shadow: 0 4px 15px rgba(255, 51, 119, 0.4);
            transition: all 0.3s ease; display: inline-flex; align-items: center; gap: 8px;
        }
        .smile-btn:hover {
            transform: translateY(-2px); box-shadow: 0 6px 25px rgba(255, 51, 119, 0.6);
        }

        /* ================= ANIMASI KEYFRAMES ================= */
        @keyframes pulseGlow { 0% { transform: scale(0.8); opacity: 0.5; } 100% { transform: scale(1.2); opacity: 1; } }
        @keyframes textFloat { 0% { transform: translateY(0px); } 100% { transform: translateY(-10px); } }
        @keyframes fadeInDelayed { 100% { opacity: 1; } }
        @keyframes fillProgress { 100% { width: 100%; } }
        @keyframes cardEntrance { 0% { transform: scale(0.8) translateY(30px); opacity: 0; } 100% { transform: scale(1) translateY(0); opacity: 1; } }
        @keyframes neonFlicker {
            0%, 19%, 21%, 23%, 25%, 54%, 56%, 100% { text-shadow: 0 0 10px #ff3377, 0 0 20px #ff3377, 0 0 40px #ff0055; }
            20%, 24%, 55% { text-shadow: none; }
        }
    </style>
</head>
<body>

    <div id="overlay-gate">
        <button class="gate-btn" onclick="startCelebration()"><i class="fa-solid fa-gift"></i> OPEN MY GIFT</button>
        <div class="gate-text">Click to load secure database & visual interaction</div>
    </div>

    <div id="loading-screen">
        <canvas id="matrix-canvas"></canvas>
        <div class="matrix-mask"></div>
        <div class="loading-content">
            <div class="glow-aura"></div>
            <h1 class="hbd-text">Happy Birthday!</h1>
            <div class="sub-wish">May Your Days Be Bright</div>
            <div class="progress-container">
                <div class="progress-bar"></div>
            </div>
        </div>
    </div>

    <div id="main-app">
        <div class="main-card">
            <div class="icon-box">
                <i class="fa-solid fa-heart"></i>
            </div>
            <h2>A Special Wish For You</h2>
            <p class="sweet-message">
                On this beautiful day, I just wanted to remind you that the world is a much better, brighter, and happier place just because you are in it. May your smile never fade, your laughter stay loud, and all your hidden prayers be answered. 
                <br><br>
                Thank you for being you! Cheers to another amazing year of your life! ✨
            </p>
            <button class="smile-btn" onclick="giveExtraSmile()">
                <i class="fa-regular fa-face-smile-wink"></i> Click For Extra Smile
            </button>
        </div>
    </div>

    <script>
        let matrixInterval = null;

        function startCelebration() {
            document.getElementById('overlay-gate').style.display = 'none';
            startCyberMatrix();

            // Pemicu Suara Robot Sapaan Panjang (Bahasa Inggris)
            if ('speechSynthesis' in window) {
                let speechText = "Access granted. System initialized. Happy birthday to you! May your days be filled with endless joy and brilliant smiles. Enjoy your day.";
                let speech = new SpeechSynthesisUtterance(speechText);
                
                speech.lang = 'en-US';
                speech.pitch = 1.0; 
                speech.rate = 0.95; 
                speech.volume = 1.0;

                window.speechSynthesis.speak(speech);
            }

            // Transisi Loading Screen 7.5 Detik ke Kartu Ucapan Utama
            setTimeout(() => {
                if (matrixInterval) clearInterval(matrixInterval);
                const loader = document.getElementById('loading-screen');
                const mainApp = document.getElementById('main-app');
                
                loader.style.opacity = '0';
                setTimeout(() => {
                    loader.style.display = 'none';
                    mainApp.style.display = 'flex';
                    setTimeout(() => { mainApp.style.opacity = '1'; }, 50);
                }, 1000);
            }, 7500);
        }

        // MATRIX ENGINE KILAU CYBER PINK
        const canvas = document.getElementById('matrix-canvas');
        const ctx = canvas.getContext('2d');
        function resizeCanvas() { canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
        resizeCanvas(); window.addEventListener('resize', resizeCanvas);

        const matrixChars = "01101HBD✨❤️★⚡01";
        const charArray = matrixChars.split("");
        const fontSize = 14;
        let columns = canvas.width / fontSize;
        const dropPositions = [];
        for (let i = 0; i < columns; i++) { dropPositions[i] = Math.random() * -100; }

        function drawMatrix() {
            ctx.fillStyle = "rgba(0, 0, 0, 0.05)"; ctx.fillRect(0, 0, canvas.width, canvas.height);
            ctx.font = "bold " + fontSize + "px monospace";
            
            for (let i = 0; i < dropPositions.length; i++) {
                const randomChar = charArray[Math.floor(Math.random() * charArray.length)];
                const xPos = i * fontSize; const yPos = dropPositions[i] * fontSize;
                
                if (Math.random() > 0.97) { 
                    ctx.fillStyle = "#ffffff"; ctx.shadowBlur = 10; ctx.shadowColor = "#ffffff"; 
                } else if (Math.random() > 0.7) {
                    ctx.fillStyle = "#00e5ff"; ctx.shadowBlur = 4; ctx.shadowColor = "#00e5ff";
                } else { 
                    ctx.fillStyle = "#ff3377"; ctx.shadowBlur = 5; ctx.shadowColor = "#ff3377"; 
                }
                
                ctx.fillText(randomChar, xPos, yPos); ctx.shadowBlur = 0;
                if (yPos > canvas.height && Math.random() * 100 > 98) { dropPositions[i] = 0; }
                dropPositions[i] += 1;
            }
        }
        
        function startCyberMatrix() { matrixInterval = setInterval(drawMatrix, 25); }

        function giveExtraSmile() {
            alert("Sistem Mendeteksi: Senyuman kamu terdeteksi sangat manis! Tetaplah bahagia hari ini dan seterusnya ya! 😉✨");
        }
    </script>
</body>
</html>
