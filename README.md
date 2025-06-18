<!DOCTYPE html>
<html lang="ru">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Бесконечная Капча</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.5);
            overflow: hidden;
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 40px 30px;
            text-align: center;
        }

        h1 {
            color: white;
            margin-bottom: 30px;
            font-size: 2.5rem;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }

        .captcha-container {
            position: relative;
            margin: 30px 0;
            padding: 30px;
            background: rgba(0, 0, 0, 0.2);
            border-radius: 15px;
            border: 2px solid rgba(255, 255, 255, 0.1);
        }

        .captcha-display {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 120px;
            margin-bottom: 20px;
            position: relative;
            overflow: hidden;
        }

        .captcha-text {
            font-size: 3rem;
            font-weight: bold;
            letter-spacing: 8px;
            color: white;
            position: relative;
            z-index: 2;
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            transform: skew(-5deg);
        }

        .captcha-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background:
                repeating-linear-gradient(45deg,
                    rgba(255, 255, 255, 0.1) 0px,
                    rgba(255, 255, 255, 0.1) 10px,
                    rgba(0, 0, 0, 0.1) 10px,
                    rgba(0, 0, 0, 0.1) 20px),
                radial-gradient(circle, transparent 20%, rgba(0, 0, 0, 0.3) 100%);
            z-index: 1;
        }

        .captcha-noise {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image:
                url("data:image/svg+xml,%3Csvg width='100' height='100' viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M11 18c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm48 25c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm-43-7c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm63 31c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM34 90c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm56-76c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM12 86c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm28-65c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm23-11c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-6 60c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm29 22c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zM32 63c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm57-13c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-9-21c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM60 91c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM35 41c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM12 60c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2z' fill='%23ffffff' fill-opacity='0.1' fill-rule='evenodd'/%3E%3C/svg%3E");
            opacity: 0.5;
            z-index: 1;
        }

        .input-group {
            display: flex;
            gap: 10px;
            margin-top: 25px;
        }

        input {
            flex: 1;
            padding: 15px 20px;
            border-radius: 50px;
            border: none;
            background: rgba(0, 0, 0, 0.3);
            color: white;
            font-size: 1.2rem;
            text-align: center;
            outline: none;
            transition: all 0.3s;
            border: 2px solid rgba(255, 255, 255, 0.1);
        }

        input:focus {
            border-color: #ff69b4;
            box-shadow: 0 0 15px rgba(255, 105, 180, 0.3);
        }

        button {
            padding: 15px 25px;
            border-radius: 50px;
            border: none;
            background: rgba(255, 105, 180, 0.3);
            color: white;
            font-size: 1.1rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s;
            border: 2px solid rgba(255, 255, 255, 0.1);
        }

        button:hover {
            background: rgba(255, 105, 180, 0.5);
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }

        button:active {
            transform: translateY(1px);
        }

        .timer {
            margin-top: 20px;
            color: rgba(255, 255, 255, 0.7);
            font-size: 1rem;
        }

        .message {
            margin-top: 25px;
            padding: 15px;
            border-radius: 10px;
            font-size: 1.2rem;
            transition: all 0.3s;
            opacity: 0;
            height: 0;
            overflow: hidden;
        }

        .success {
            background: rgba(40, 167, 69, 0.3);
            color: #28a745;
            opacity: 1;
            height: auto;
        }

        .error {
            background: rgba(220, 53, 69, 0.3);
            color: #dc3545;
            opacity: 1;
            height: auto;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 25px;
        }

        .counter {
            position: absolute;
            top: 15px;
            right: 15px;
            background: rgba(255, 105, 180, 0.3);
            color: white;
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.9rem;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>Бесконечная Капча</h1>

        <div class="captcha-container">
            <div class="counter">Попытки: <span id="attempts">0</span></div>

            <div class="captcha-display">
                <div class="captcha-overlay"></div>
                <div class="captcha-noise"></div>
                <div class="captcha-text" id="captchaText">ABCD</div>
            </div>

            <div class="input-group">
                <input type="text" id="userInput" placeholder="Введите текст с картинки" maxlength="6">
                <button id="checkBtn">Проверить</button>
            </div>

            <div class="timer">Автообновление через: <span id="countdown">30</span> сек</div>

            <div class="message" id="message"></div>

            <div class="controls">
                <button id="refreshBtn">Обновить капчу</button>
                <button id="autoRefreshToggle">Автообновление: Вкл</button>
            </div>
        </div>
    </div>

    <script>
        // Элементы интерфейса
        const captchaText = document.getElementById('captchaText');
        const userInput = document.getElementById('userInput');
        const checkBtn = document.getElementById('checkBtn');
        const refreshBtn = document.getElementById('refreshBtn');
        const countdown = document.getElementById('countdown');
        const message = document.getElementById('message');
        const autoRefreshToggle = document.getElementById('autoRefreshToggle');
        const attemptsDisplay = document.getElementById('attempts');

        // Состояние приложения
        let currentCaptcha = '';
        let autoRefresh = true;
        let countdownValue = 30;
        let countdownInterval;
        let attempts = 0;

        // Генерация случайной капчи
        function generateCaptcha() {
            const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZabcdefghjkmnpqrstuvwxyz23456789';
            let result = '';

            for (let i = 0; i < 6; i++) {
                result += chars.charAt(Math.floor(Math.random() * chars.length));
            }

            // Применяем искажения к каждому символу
            let distortedText = '';
            for (let i = 0; i < result.length; i++) {
                const char = result.charAt(i);
                const rotation = (Math.random() * 20 - 10).toFixed(1);
                const scale = (0.8 + Math.random() * 0.4).toFixed(2);
                const skew = (Math.random() * 10 - 5).toFixed(1);

                distortedText += `
                    <span style="
                        display: inline-block;
                        transform: rotate(${rotation}deg) scale(${scale}) skew(${skew}deg);
                        margin: 0 2px;
                    ">
                        ${char}
                    </span>
                `;
            }

            captchaText.innerHTML = distortedText;
            currentCaptcha = result;
            userInput.value = '';
            message.className = 'message';
            resetCountdown();

            // Добавляем анимацию обновления
            captchaText.style.animation = 'none';
            setTimeout(() => {
                captchaText.style.animation = 'captchaRefresh 0.5s ease';
            }, 10);
        }

        // Проверка введенной капчи
        function checkCaptcha() {
            const userText = userInput.value.trim();
            attempts++;
            attemptsDisplay.textContent = attempts;

            if (userText.toLowerCase() === currentCaptcha.toLowerCase()) {
                showMessage('Правильно! Капча обновлена', 'success');
                generateCaptcha();
            } else {
                showMessage('Неверно! Попробуйте снова', 'error');
                userInput.value = '';
                userInput.focus();
            }
        }

        // Показ сообщения
        function showMessage(text, type) {
            message.textContent = text;
            message.className = `message ${type}`;

            setTimeout(() => {
                message.className = 'message';
            }, 3000);
        }

        // Сброс таймера
        function resetCountdown() {
            countdownValue = 30;
            countdown.textContent = countdownValue;

            if (countdownInterval) {
                clearInterval(countdownInterval);
            }

            if (autoRefresh) {
                countdownInterval = setInterval(() => {
                    countdownValue--;
                    countdown.textContent = countdownValue;

                    if (countdownValue <= 0) {
                        generateCaptcha();
                    }
                }, 1000);
            }
        }

        // Переключение автообновления
        function toggleAutoRefresh() {
            autoRefresh = !autoRefresh;
            autoRefreshToggle.textContent = `Автообновление: ${autoRefresh ? 'Вкл' : 'Выкл'}`;

            if (autoRefresh) {
                resetCountdown();
            } else {
                clearInterval(countdownInterval);
                countdown.textContent = 'выкл';
            }
        }

        // Инициализация
        function init() {
            generateCaptcha();

            // События
            checkBtn.addEventListener('click', checkCaptcha);
            refreshBtn.addEventListener('click', generateCaptcha);
            autoRefreshToggle.addEventListener('click', toggleAutoRefresh);

            // Проверка по Enter
            userInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    checkCaptcha();
                }
            });

            // Добавляем CSS для анимации
            const style = document.createElement('style');
            style.textContent = `
                @keyframes captchaRefresh {
                    0% { opacity: 0; transform: scale(0.8); }
                    100% { opacity: 1; transform: scale(1); }
                }
            `;
            document.head.appendChild(style);
        }

        // Запуск приложения
        init();
    </script>
</body>

</html>
