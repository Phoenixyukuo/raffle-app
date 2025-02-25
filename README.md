<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TMU OGE 抽獎系統</title>
    <style>
        body {
            font-family: 'Noto Sans TC', sans-serif;
            max-width: 900px;
            margin: 0 auto;
            padding: 20px;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            overflow-x: hidden;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            margin-top: 20px;
        }
        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 20px;
            font-size: 2.5em;
        }
        #logo {
            max-width: 200px;
            display: block;
            margin: 0 auto 20px;
        }
        #participantList {
            width: 100%;
            height: 150px;
            padding: 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            resize: none;
            margin-bottom: 20px;
            transition: border-color 0.3s;
        }
        #participantList:focus {
            border-color: #3498db;
            outline: none;
        }
        #drawButton {
            padding: 12px 30px;
            font-size: 18px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            transition: transform 0.2s, background-color 0.2s;
            display: block;
            margin: 0 auto 20px;
        }
        #drawButton:hover {
            background-color: #2980b9;
            transform: scale(1.05);
        }
        #drawButton:disabled {
            background-color: #95a5a6;
            cursor: not-allowed;
            transform: none;
        }
        #result {
            margin-top: 30px;
            font-size: 28px;
            color: #e74c3c;
            text-align: center;
            font-weight: bold;
            animation: fadeIn 0.5s;
        }
        #participantsDisplay {
            margin-top: 20px;
            padding: 15px;
            background: #ecf0f1;
            border-radius: 8px;
            text-align: left;
            font-size: 16px;
        }
        #countdown {
            font-size: 24px;
            color: #2c3e50;
            margin-top: 20px;
            text-align: center;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body>
    <div class="container">
        <img id="logo" src="https://via.placeholder.com/200x100?text=TMU+OGE+Logo" alt="TMU OGE Logo">
        <h1>TMU OGE 即時抽獎系統</h1>
        <p>請輸入參與者名單（每行一位，最多20人）：</p>
        <textarea id="participantList" placeholder="張三
李四
王五..."></textarea>
        <button id="drawButton" onclick="startDraw()">開始抽獎</button>
        <div id="participantsDisplay"></div>
        <div id="countdown"></div>
        <div id="result"></div>
    </div>

    <script>
        let participants = [];
        const drawButton = document.getElementById('drawButton');
        const countdownDisplay = document.getElementById('countdown');

        // Load Google Fonts
        const link = document.createElement('link');
        link.href = 'https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700&display=swap';
        link.rel = 'stylesheet';
        document.head.appendChild(link);

        // Update participant list in real-time
        document.getElementById('participantList').addEventListener('input', function() {
            const input = this.value.trim();
            participants = input.split('\n').filter(name => name.trim() !== '');
            updateParticipantsDisplay();
        });

        // Display participants
        function updateParticipantsDisplay() {
            const display = document.getElementById('participantsDisplay');
            display.innerHTML = '<h3>目前參與者：</h3>' + 
                (participants.length > 0 ? participants.join(', ') : '尚無參與者');
            if (participants.length > 20) {
                alert('參與者不可超過20人！');
                participants = participants.slice(0, 20);
                document.getElementById('participantList').value = participants.join('\n');
            }
        }

        // Start draw with countdown
        function startDraw() {
            if (participants.length === 0) {
                alert('請先輸入參與者名單！');
                return;
            }

            drawButton.disabled = true;
            let countdown = 3;
            countdownDisplay.innerHTML = `抽獎倒數：${countdown}秒`;

            const interval = setInterval(() => {
                countdown--;
                if (countdown > 0) {
                    countdownDisplay.innerHTML = `抽獎倒數：${countdown}秒`;
                } else {
                    clearInterval(interval);
                    drawWinner();
                    countdownDisplay.innerHTML = '';
                    drawButton.disabled = false;
                }
            }, 1000);
        }

        // Draw the winner
        function drawWinner() {
            const winnerIndex = Math.floor(Math.random() * participants.length);
            const winner = participants[winnerIndex];
            
            document.getElementById('result').innerHTML = `恭喜得獎者：${winner}！`;
            
            participants.splice(winnerIndex, 1);
            document.getElementById('participantList').value = participants.join('\n');
            updateParticipantsDisplay();
        }
    </script>
</body>
</html>
