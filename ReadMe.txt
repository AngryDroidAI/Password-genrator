<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Password Generator</title>
    <style>
        body {
            background-color: #121212;
            color: #00ff00;
            font-family: 'Courier New', Courier, monospace;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            transition: background-color 0.5s, color 0.5s;
        }
        button {
            background-color: #00ff00;
            color: #121212;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            margin-top: 20px;
        }
        textarea, input, select {
            background-color: #121212;
            color: #00ff00;
            border: 1px solid #00ff00;
            margin-top: 10px;
        }
        #strength {
            margin-top: 20px;
        }
        .history-item {
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h1>Password Generator</h1>
    <label for="length">Password Length:</label>
    <input type="number" id="length" value="20" min="8" max="100">
    <label for="characterSet">Character Set:</label>
    <select id="characterSet">
        <option value="all">All</option>
        <option value="lettersDigits">Letters & Digits</option>
        <option value="lettersOnly">Letters Only</option>
        <option value="digitsOnly">Digits Only</option>
    </select>
    <button onclick="generatePassword()">Generate Password</button>
    <textarea id="password" readonly></textarea>
    <button onclick="copyPassword()">Copy to Clipboard</button>
    <button onclick="toggleDarkMode()">Toggle Dark Mode</button>
    <p id="strength">Password Strength: </p>
    <h2>Password History</h2>
    <div id="history"></div>

    <script>
        function generatePassword() {
            const length = parseInt(document.getElementById('length').value);
            const characterSet = document.getElementById('characterSet').value;

            let alphabet = '';
            if (characterSet === 'all') {
                alphabet = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()_+-=[]{}|;:,.<>?';
            } else if (characterSet === 'lettersDigits') {
                alphabet = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
            } else if (characterSet === 'lettersOnly') {
                alphabet = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
            } else if (characterSet === 'digitsOnly') {
                alphabet = '0123456789';
            }

            let password = '';
            for (let i = 0; i < length; i++) {
                password += alphabet[Math.floor(Math.random() * alphabet.length)];
            }
            document.getElementById('password').value = password;
            checkStrength(password);
            addToHistory(password);
        }

        function copyPassword() {
            const password = document.getElementById('password');
            password.select();
            document.execCommand('copy');
            alert('Password copied to clipboard');
        }

        function checkStrength(password) {
            let strength = "Weak";
            if (password.length >= 8 && /[A-Z]/.test(password) && /[0-9]/.test(password) && /[a-z]/.test(password) && /[!@#$%^&*()_+\-=[\]{}|;:,.<>?]/.test(password)) {
                strength = "Strong";
            } else if (password.length >= 8) {
                strength = "Medium";
            }
            document.getElementById('strength').innerText = `Password Strength: ${strength}`;
        }

        function addToHistory(password) {
            const history = document.getElementById('history');
            const historyItem = document.createElement('div');
            historyItem.classList.add('history-item');
            historyItem.textContent = password;
            history.appendChild(historyItem);
        }

        function toggleDarkMode() {
            const body = document.body;
            if (body.style.backgroundColor === 'white') {
                body.style.backgroundColor = '#121212';
                body.style.color = '#00ff00';
            } else {
                body.style.backgroundColor = 'white';
                body.style.color = '#121212';
            }
        }
    </script>
</body>
</html>

