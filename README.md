<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2 Puanlık Quiz Uygulaması</title>
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        .quiz-container {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 600px;
            text-align: center;
            transition: all 0.3s ease;
        }
        h1 {
            color: #2c3e50;
            margin-bottom: 20px;
        }
        .question-counter {
            font-size: 14px;
            color: #7f8c8d;
            margin-bottom: 15px;
        }
        button.option {
            display: block;
            width: 100%;
            margin: 10px 0;
            padding: 12px;
            cursor: pointer;
            background: #ecf0f1;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            transition: all 0.2s;
        }
        button.option:hover {
            background: #d6eaf8;
            transform: translateY(-2px);
        }
        #next-btn {
            margin-top: 25px;
            padding: 12px 25px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s;
        }
        #next-btn:hover {
            background: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(41, 128, 185, 0.4);
        }
        #next-btn:disabled {
            background: #bdc3c7;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        .selected {
            background: #a0d8ef !important;
            border-left: 5px solid #3498db !important;
        }
        .correct {
            background: #2ecc71 !important;
            color: white;
        }
        .wrong {
            background: #e74c3c !important;
            color: white;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
            min-height: 27px;
        }
        #final-result {
            animation: fadeIn 0.8s ease;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .progress-container {
            width: 100%;
            height: 8px;
            background: #ecf0f1;
            border-radius: 4px;
            margin: 20px 0;
            overflow: hidden;
        }
        #progress-bar {
            height: 100%;
            background: #3498db;
            width: 0%;
            transition: width 0.4s ease;
        }
        .timer {
            font-size: 18px;
            color: #e74c3c;
            font-weight: bold;
            margin-top: 10px;
        }
        .score-display {
            font-size: 20px;
            margin: 15px 0;
            color: #2c3e50;
        }
        #restart-btn {
            margin-top: 30px;
            padding: 12px 30px;
            font-size: 16px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }
        #restart-btn:hover {
            background: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(41, 128, 185, 0.4);
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div class="question-counter" id="counter">Soru 1/<span id="total-questions">0</span></div>
        <div class="progress-container">
            <div id="progress-bar"></div>
        </div>
        <div class="score-display">Puan: <span id="score">0</span>/<span id="max-score">0</span></div>
        <h1 id="question">Soru Yükleniyor...</h1>
        <div id="options"></div>
        <button id="next-btn" disabled>Sonraki Soru</button>
        <div id="result"></div>
        <div class="timer" id="timer" style="display: none;"></div>
    </div>

    <script>
        // Temel soru bankası
        let questionBank = [
            {
                question: "Türkiye'nin başkenti neresidir?",
                options: ["İstanbul", "Ankara", "İzmir", "Bursa"],
                answer: 1,
                explanation: "Ankara, 13 Ekim 1923'ten beri Türkiye'nin başkentidir."
            },
            {
                question: "Hangisi bir meyve değildir?",
                options: ["Elma", "Havuç", "Muz", "Çilek"],
                answer: 1,
                explanation: "Havuç, sebze kategorisinde yer alan bir kök bitkidir."
            },
            {
                question: "2 + 2 kaçtır?",
                options: ["3", "4", "5", "6"],
                answer: 1,
                explanation: "Temel matematik işlemine göre 2 + 2 = 4'tür."
            },
            {
                question: "Hangi gezegen 'Kızıl Gezegen' olarak bilinir?",
                options: ["Venüs", "Mars", "Jüpiter", "Satürn"],
                answer: 1,
                explanation: "Mars, yüzeyindeki demir oksitten dolayı kırmızı görünür."
            },
            {
                question: "Türkiye'nin en uzun nehri hangisidir?",
                options: ["Sakarya", "Kızılırmak", "Fırat", "Dicle"],
                answer: 2,
                explanation: "Fırat Nehri, 2.800 km uzunluğuyla Türkiye'nin en uzun nehridir."
            }
            // Yeni soruları buraya ekleyebilirsiniz
        ];

        // Oyun durumu
        let questions = [];
        let currentQuestion = 0;
        let score = 0;
        let userAnswer = null;
        let timerInterval;

        // Soruları karıştır
        function shuffleQuestions() {
            // Soruların bir kopyasını oluştur
            let shuffled = [...questionBank];
            
            // Soru sırasını karıştır
            shuffled = shuffled.sort(() => Math.random() - 0.5);
            
            // Her sorunun şıklarını karıştır (doğru cevabın indexini güncelle)
            shuffled.forEach(q => {
                const correctAnswer = q.options[q.answer];
                q.options = q.options.sort(() => Math.random() - 0.5);
                q.answer = q.options.indexOf(correctAnswer);
            });
            
            return shuffled;
        }

        // Oyunu başlat
        function startGame() {
            questions = shuffleQuestions();
            currentQuestion = 0;
            score = 0;
            
            document.getElementById('total-questions').textContent = questions.length;
            document.getElementById('max-score').textContent = questions.length * 2;
            document.getElementById('score').textContent = score;
            
            showQuestion();
        }

        function showQuestion() {
            clearInterval(timerInterval);
            document.getElementById('timer').style.display = 'none';
            
            const question = questions[currentQuestion];
            document.getElementById('question').textContent = question.question;
            document.getElementById('counter').textContent = `Soru ${currentQuestion + 1}/${questions.length}`;
            document.getElementById('progress-bar').style.width = `${((currentQuestion + 1) / questions.length) * 100}%`;
            
            const optionsDiv = document.getElementById('options');
            optionsDiv.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.className = 'option';
                button.textContent = option;
                button.onclick = () => selectOption(index);
                optionsDiv.appendChild(button);
            });
            
            document.getElementById('next-btn').disabled = true;
            document.getElementById('result').textContent = '';
        }

        function selectOption(index) {
            userAnswer = index;
            const buttons = document.querySelectorAll('.option');
            buttons.forEach(btn => {
                btn.classList.remove('selected');
                btn.disabled = false;
            });
            buttons[index].classList.add('selected');
            document.getElementById('next-btn').disabled = false;
        }

        function startTimer(duration, display, callback) {
            let timer = duration;
            display.style.display = 'block';
            display.textContent = `Sonraki soruya geçiş: ${timer}s`;
            
            timerInterval = setInterval(function () {
                display.textContent = `Sonraki soruya geçiş: ${--timer}s`;
                
                if (timer <= 0) {
                    clearInterval(timerInterval);
                    display.style.display = 'none';
                    callback();
                }
            }, 1000);
        }

        document.getElementById('next-btn').addEventListener('click', () => {
            const question = questions[currentQuestion];
            const buttons = document.querySelectorAll('.option');
            
            buttons.forEach(btn => btn.disabled = true);
            
            if (userAnswer === question.answer) {
                score += 2; // Her doğru cevap 2 puan
                buttons[userAnswer].classList.add('correct');
                document.getElementById('result').innerHTML = `
                    ✔ Doğru! +2 Puan <small style="display:block;font-weight:normal">${question.explanation}</small>
                `;
            } else {
                buttons[userAnswer].classList.add('wrong');
                buttons[question.answer].classList.add('correct');
                document.getElementById('result').innerHTML = `
                    ✖ Yanlış! 0 Puan <small style="display:block;font-weight:normal">Doğru cevap: ${question.options[question.answer]}<br>${question.explanation}</small>
                `;
            }
            
            document.getElementById('score').textContent = score;
            document.getElementById('next-btn').disabled = true;
            
            // 4 saniye timer başlat
            startTimer(4, document.getElementById('timer'), () => {
                if (currentQuestion < questions.length - 1) {
                    currentQuestion++;
                    showQuestion();
                } else {
                    showFinalResult();
                }
            });
        });

        function showFinalResult() {
            const totalPossibleScore = questions.length * 2;
            document.querySelector('.quiz-container').innerHTML = `
                <div id="final-result">
                    <h1>Test Tamamlandı! 🎉</h1>
                    <p style="font-size: 24px; margin: 30px 0;">Puan: <strong>${score}/${totalPossibleScore}</strong></p>
                    <p style="color: #7f8c8d;">${getResultMessage(score, totalPossibleScore)}</p>
                    <button id="restart-btn">Tekrar Başlat</button>
                </div>
            `;
            
            document.getElementById('restart-btn').addEventListener('click', startGame);
        }

        function getResultMessage(score, total) {
            const percentage = (score / total) * 100;
            if (percentage >= 90) return "Mükemmel! Neredeyse tam puan! 🌟";
            if (percentage >= 70) return "Çok iyi! Harika bir performans! 👏";
            if (percentage >= 50) return "İyi iş çıkardın! Daha iyisini yapabilirsin! 👍";
            return "Daha fazla çalışmaya ihtiyacın var. Pes etme! 💪";
        }

        // Oyunu ilk başlatma
        startGame();
    </script>
</body>
</html>
