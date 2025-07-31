<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zoom会議リアルタイム要約</title>
    <script src="https://appssdk.zoom.us/sdk.min.js"></script>
    <style>
        body {
            font-family: 'Segoe UI', sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        .container {
            max-width: 1000px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #4a5568;
            margin-bottom: 30px;
        }
        .controls {
            text-align: center;
            margin: 20px 0;
        }
        .btn {
            padding: 12px 24px;
            margin: 0 10px;
            border: none;
            border-radius: 25px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .btn-start {
            background: linear-gradient(45deg, #48bb78, #38a169);
            color: white;
        }
        .btn-stop {
            background: linear-gradient(45deg, #f56565, #e53e3e);
            color: white;
        }
        .btn-clear {
            background: linear-gradient(45deg, #718096, #4a5568);
            color: white;
        }
        .panel {
            background: white;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            padding: 20px;
            margin: 20px 0;
            min-height: 150px;
        }
        .status {
            text-align: center;
            padding: 15px;
            border-radius: 10px;
            margin: 20px 0;
            background: #f8f9fa;
            border-left: 4px solid #667eea;
        }
        .transcript-text {
            font-size: 16px;
            line-height: 1.6;
            white-space: pre-wrap;
        }
        .summary-text {
            font-size: 14px;
            line-height: 1.5;
            background: #f7fafc;
            padding: 15px;
            border-radius: 8px;
        }
        .claude-prompt {
            width: 100%;
            height: 150px;
            padding: 15px;
            border: 2px solid #e2e8f0;
            border-radius: 8px;
            font-family: monospace;
            font-size: 14px;
        }
        .copy-btn {
            background: #667eea;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎤 Zoom会議リアルタイム要約システム</h1>
        
        <div class="status" id="status">
            ステータス: 待機中
        </div>
        
        <div class="controls">
            <button class="btn btn-start" onclick="startRecording()">🎤 録音開始</button>
            <button class="btn btn-stop" onclick="stopRecording()" disabled>⏹️ 録音停止</button>
            <button class="btn btn-clear" onclick="clearAll()">🗑️ クリア</button>
        </div>
        
        <div class="panel">
            <h3>📝 リアルタイム文字起こし</h3>
            <div class="transcript-text" id="transcript">音声認識を開始してください...</div>
        </div>
        
        <div class="panel">
            <h3>🔍 キーワード抽出</h3>
            <div class="summary-text" id="summary">キーワードはここに表示されます...</div>
        </div>
        
        <div class="panel hidden" id="claudePanel">
            <h3>📋 Claude用プロンプト</h3>
            <textarea class="claude-prompt" id="claudePrompt" readonly></textarea>
            <button class="copy-btn" onclick="copyPrompt()">📋 コピー</button>
            <p style="color:#666; font-size:14px;">
                👆 このプロンプトをClaude MAXに貼り付けて要約を取得してください
            </p>
        </div>
    </div>

    <script>
        let recognition;
        let isRecording = false;
        let transcriptBuffer = "";
        let zoomSdk;
        
        // Zoom SDK初期化
        async function initializeZoomApp() {
            try {
                zoomSdk = new ZoomSdk();
                await zoomSdk.config({
                    capabilities: [
                        'getMeetingContext',
                        'getMeetingParticipants'
                    ]
                });
                
                const meetingContext = await zoomSdk.getMeetingContext();
                document.getElementById('status').textContent = 
                    `ステータス: Zoom会議 "${meetingContext.meetingTopic || '会議'}" に接続済み`;
                
            } catch (error) {
                console.log('Zoom外での使用:', error);
                document.getElementById('status').textContent = 'ステータス: Zoom外での使用（テストモード）';
            }
        }
        
        // 音声認識初期化
        function initSpeechRecognition() {
            if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
                alert('このブラウザは音声認識に対応していません。Chrome または Edge をお使いください。');
                return false;
            }
            
            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            recognition = new SpeechRecognition();
            
            recognition.continuous = true;
            recognition.interimResults = true;
            recognition.lang = 'ja-JP';
            
            recognition.onstart = function() {
                document.getElementById('status').textContent = 'ステータス: 🔴 録音中...';
            };
            
            recognition.onresult = function(event) {
                let interimTranscript = '';
                let finalTranscript = '';
                
                for (let i = event.resultIndex; i < event.results.length; i++) {
                    const transcript = event.results[i][0].transcript;
                    if (event.results[i].isFinal) {
                        finalTranscript += transcript;
                    } else {
                        interimTranscript += transcript;
                    }
                }
                
                if (finalTranscript) {
                    transcriptBuffer += finalTranscript + ' ';
                    extractKeywords(transcriptBuffer);
                }
                
                const currentText = transcriptBuffer + interimTranscript;
                document.getElementById('transcript').textContent = currentText;
            };
            
            recognition.onerror = function(event) {
                console.error('音声認識エラー:', event.error);
            };
            
            recognition.onend = function() {
                if (isRecording) {
                    setTimeout(() => {
                        if (isRecording) recognition.start();
                    }, 100);
                } else {
                    document.getElementById('status').textContent = 'ステータス: 停止';
                }
            };
            
            return true;
        }
        
        // 録音開始
        function startRecording() {
            if (!recognition && !initSpeechRecognition()) return;
            
            isRecording = true;
            recognition.start();
            
            document.querySelector('.btn-start').disabled = true;
            document.querySelector('.btn-stop').disabled = false;
        }
        
        // 録音停止
        function stopRecording() {
            if (recognition) {
                isRecording = false;
                recognition.stop();
                
                document.querySelector('.btn-start').disabled = false;
                document.querySelector('.btn-stop').disabled = true;
                
                if (transcriptBuffer.trim()) {
                    generateClaudePrompt(transcriptBuffer);
                }
            }
        }
        
        // 全クリア
        function clearAll() {
            transcriptBuffer = "";
            document.getElementById('transcript').textContent = "音声認識を開始してください...";
            document.getElementById('summary').textContent = "キーワードはここに表示されます...";
            document.getElementById('claudePanel').classList.add('hidden');
            document.getElementById('status').textContent = 'ステータス: 待機中';
        }
        
        // キーワード抽出
        function extractKeywords(text) {
            const keywords = [
                '決定', '結論', '合意', '承認', '却下',
                '次回', '期限', '締切', '予定', 'スケジュール',
                '予算', '費用', '金額', '価格',
                '責任者', '担当者', '担当', 'リーダー',
                '課題', '問題', 'リスク', '懸念',
                'アクション', 'TODO', 'やること', '宿題'
            ];
            
            let summary = '';
            let found = false;
            
            keywords.forEach(keyword => {
                if (text.includes(keyword)) {
                    const sentences = text.split('。').filter(s => s.includes(keyword));
                    sentences.forEach(sentence => {
                        if (sentence.trim()) {
                            summary += `• ${keyword}: ${sentence.trim()}。\n`;
                            found = true;
                        }
                    });
                }
            });
            
            if (!found) {
                summary = '重要なキーワードが見つかりませんでした';
            }
            
            document.getElementById('summary').textContent = summary;
        }
        
        // Claude用プロンプト生成
        function generateClaudePrompt(text) {
            const prompt = `以下の会議音声テキストを議事録として整理してください：

${text}

【整理してほしい項目】
・会議の概要
・主な議論ポイント
・決定事項
・アクションアイテム（誰が何をいつまでに）
・次回会議の予定`;

            document.getElementById('claudePrompt').value = prompt;
            document.getElementById('claudePanel').classList.remove('hidden');
        }
        
        // プロンプトコピー
        function copyPrompt() {
            const textarea = document.getElementById('claudePrompt');
            textarea.select();
            document.execCommand('copy');
            
            const button = document.querySelector('.copy-btn');
            const originalText = button.textContent;
            button.textContent = '✅ コピー完了！';
            button.style.background = '#48bb78';
            
            setTimeout(() => {
                button.textContent = originalText;
                button.style.background = '#667eea';
            }, 2000);
        }
        
        // 初期化
        window.onload = async function() {
            await initializeZoomApp();
            
            if (!('webkitSpeechRecognition' in window) && !('SpeechRecognition' in window)) {
                document.getElementById('status').textContent = 'ステータス: 音声認識非対応ブラウザ';
                document.querySelector('.btn-start').disabled = true;
            }
        };
    </script>
</body>
</html>
