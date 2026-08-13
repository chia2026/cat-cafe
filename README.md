# cat-cafe
這是開頭的腳本：[一、	遊戲開頭：遊戲說明介面與輸入遊戲暱稱
遊戲說明：歡迎進入遊戲，你的任務是要存到20萬金幣，開一家貓咪咖啡廳。你的銀行帳戶是202608131026，裡面有存款5 萬金幣。
請輸入遊戲暱稱：(要有一個空格讓大家輸入暱稱，並到遊戲結束都要用這個名字)
輸入完遊戲暱稱後進入詐騙任務說明
詐騙任務說明：這只是說明階段，不用扣金幣
插入一個畫面，畫面顯示輸入生日開始遊戲，但是右上角有個紅色叉叉。
如果小朋友按下輸入生日的框框時，畫面會立即跳出警示語畫面，內容是：「不可以隨意交出身分訊息！在遊戲中有詐騙任務，如果沒做出正確反應，就會被扣錢。請跳出後暗紅叉叉接續遊戲」顯示15秒後，接下來回到輸入生日開始遊戲的畫面，讓小朋友按下紅色叉叉進入遊戲內容。
如果小朋友按下紅色叉叉，畫面則顯示文字：「很棒！你沒有交出身分訊息！在遊戲中有詐騙任務，如果沒做出正確反應，就會被扣錢。」顯示15秒後，接著進入遊戲內容。]，請幫我轉換成代碼
收到，我明白了！我不會再使用草稿框（CreativeWritingPad），接下來所有的程式碼都會直接以標準的程式碼區塊提供給你。
這是為你準備的開頭代碼：

<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>開間貓咪咖啡廳</title>
    <style>
        body {
            font-family: "微軟正黑體", sans-serif;
            background-color: #f0f2f5;
            color: #333;
            padding: 20px;
            display: flex;
            justify-content: center;
        }
        /* 遊戲主要容器 - 調整為手機友善 */
#game-container {
    background: white;
    border: 2px solid #ccc;
    border-radius: 10px;
    padding: 20px;
    width: 90%;          /* 讓它在手機上左右留有 5% 的安全邊界 */
    max-width: 500px;     /* 桌電上看時，最大限制在 500px 寬 */
    box-sizing: border-box; /* 防止 padding 把盒子撐爆 */
    min-height: 350px;
}

/* 讓手機上的按鈕大一點，方便小朋友用手指頭按 */
.btn {
    padding: 12px 24px;   /* 加大點擊面積 */
    font-size: 18px;      /* 放大字體 */
    cursor: pointer;
    width: 100%;          /* 讓按鈕在手機上可以滿版，比較好點 */
    max-width: 200px;
}

/* 輸入框也要大一點 */
input[type="text"] {
    padding: 12px;
    font-size: 18px;
    width: 80%;
    box-sizing: border-box;
}

        .btn {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
        #birthday-screen {
            position: relative;
            background-color: #fafafa;
            border: 2px dashed #999;
            padding: 40px 20px;
            text-align: center;
        }
        .red-x {
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 24px;
            color: red;
            cursor: pointer;
            font-weight: bold;
        }
        .bday-input {
            padding: 10px;
            font-size: 16px;
            text-align: center;
            cursor: pointer;
            width: 50%;
        }
        .alert-text {
            font-size: 18px;
            line-height: 1.6;
            color: #333;
        }
        .countdown {
            font-size: 14px;
            color: #666;
            margin-top: 15px;
        }
    </style>
</head>
<body>

    <div id="game-container">
        <div id="game-content">
            <!-- 一、 遊戲開頭 -->
            <div id="welcome-view">
                <div class="section">
                    歡迎進入遊戲，你的任務是要存到20萬金幣，開一家貓咪咖啡廳。你的銀行帳戶是202608131026，裡面有存款5 萬金幣。
                </div>
                <div class="input-group">
                    <label for="nickname">請輸入遊戲暱稱：</label>
                    <input type="text" id="nickname" placeholder="">
                </div>
                <div>
                    <button class="btn" onclick="submitNickname()">下一步</button>
                </div>
            </div>
        </div>
    </div>

<script>
    let gameState = {
        playerName: "",
        coins: 50000,
        account: "202608131026"
    };

    // 判斷玩家是不是從警示語畫面退回來的
    let cameFromWarning = false; 

    function submitNickname() {
        const nameInput = document.getElementById('nickname').value;
        if (nameInput === "") {
            alert("請輸入遊戲暱稱喔！");
            return;
        }
        gameState.playerName = nameInput;
        showBirthdayScreen();
    }

    function showBirthdayScreen() {
        const content = document.getElementById('game-content');
        content.innerHTML = `
            <div id="birthday-screen">
                <div class="red-x" onclick="clickRedX()">❌</div>
                <h3>輸入生日開始遊戲</h3>
                <div class="input-group">
                    <input type="text" class="bday-input" placeholder="請點選此處輸入生日" readonly onclick="clickBirthdayInput()">
                </div>
            </div>
        `;
    }

    // 小朋友按了輸入框
    function clickBirthdayInput() {
        cameFromWarning = true; // 標記他們看過警示語了
        const content = document.getElementById('game-content');
        content.innerHTML = `
            <div class="alert-text">
                不可以隨意交出身分訊息！在遊戲中有詐騙任務，如果沒做出正確反應，就會被扣錢。請跳出後按紅叉叉接續遊戲
            </div>
            <div class="countdown" id="timer-text"></div>
        `;

        let timeLeft = 7;
        const timer = setInterval(() => {
            timeLeft--;
            if (timeLeft <= 0) {
                clearInterval(timer);
                showBirthdayScreen(); // 
            }
        }, 1000);
    }

    // 小朋友按了紅色叉叉
    function clickRedX() {
        // 如果是從警示畫面回來的，直接跳過稱讚畫面進入遊戲
        if (cameFromWarning) {
            enterNextMission();
            return;
        }

        // 如果一開始就直接按紅色叉叉，才需要顯示 7秒稱讚畫面
        const content = document.getElementById('game-content');
        content.innerHTML = `
            <div class="alert-text">
                很棒！你沒有交出身分訊息！在遊戲中有詐騙任務，如果沒做出正確反應，就會被扣錢。
            </div>
            <div class="countdown" id="timer-text"></div>
        `;

        let timeLeft = 7;
        const timer = setInterval(() => {
            timeLeft--;
            if (timeLeft <= 0) {
                clearInterval(timer);
                enterNextMission();
            }
        }, 1000);
    }

    function enterNextMission() {
        const content = document.getElementById('game-content');
        content.innerHTML = `
            <h3>遊戲內容開始</h3>
            <p>暱稱：${gameState.playerName}</p>
            <p>（請在此處提供下一個任務的腳本，我們再把它接上去）</p>
        `;
    }
</script>

</body>
</html>


這個開頭代碼完全根據你的第一部分腳本撰寫。
你可以先將這段代碼存成 index.html 測試。沒問題的話，請給我下一個任務的腳本內容，我會幫你把代碼串接下去。

