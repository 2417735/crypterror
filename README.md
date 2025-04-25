# crypterror

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Cryptography Tool</title>
  <link rel="stylesheet" href="style.css" />
  <style>
    body {
      background-color: #000;
      color: #fff;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0;
      padding: 0;
      animation: fadeIn 1s ease;
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    .container {
      max-width: 900px;
      margin: 30px auto;
      padding: 20px;
      background: #111;
      border-radius: 8px;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.2);
      transition: all 0.3s ease;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
      font-size: 2rem;
      animation: bounceIn 1s ease;
    }
    @keyframes bounceIn {
      0% { transform: scale(0.9); opacity: 0; }
      60% { transform: scale(1.1); opacity: 1; }
      100% { transform: scale(1); }
    }
    textarea {
      width: 100%;
      height: 100px;
      padding: 10px;
      background: #222;
      color: #fff;
      border: 1px solid #444;
      border-radius: 5px;
      margin-bottom: 10px;
      resize: vertical;
      transition: all 0.2s;
    }
    textarea:focus {
      border-color: #666;
      background: #1c1c1c;
    }
    .controls, .output-section, .input-section {
      margin-bottom: 20px;
    }
    select, input[type="text"] {
      padding: 8px;
      margin-right: 10px;
      border-radius: 4px;
      border: none;
      background: #222;
      color: white;
    }
    button {
      padding: 10px 15px;
      border: none;
      border-radius: 4px;
      background-color: #444;
      color: white;
      cursor: pointer;
      transition: background 0.3s;
    }
    button:hover {
      background-color: #666;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Cryptography Tool</h1>
    <div class="input-section">
      <textarea id="inputText" placeholder="Enter text or upload a file..."></textarea>
      <input type="file" id="fileInput" accept=".txt" onchange="loadFileText(event)" />
    </div>

    <div class="controls">
      <select id="algorithmSelect">
        <option value="rot13">ROT13</option>
        <option value="caesar">Caesar Cipher</option>
        <option value="vigenere">Vigenère Cipher</option>
        <option value="transposition">Transposition Cipher</option>
        <option value="aes">AES</option>
        <option value="rsa">RSA</option>
        <option value="all">All Algorithms</option>
      </select>
      <input type="text" id="keyInput" placeholder="Enter key (if required)" />
      <button onclick="encryptText()">Encrypt</button>
      <button onclick="decryptText()">Decrypt</button>
    </div>

    <div class="output-section">
      <textarea id="outputText" placeholder="Output will appear here..."></textarea>
      <button onclick="downloadOutput()">Download Output</button>
    </div>
  </div>

  <script>
    function rot13(str) {
      return str.replace(/[a-zA-Z]/g, function (c) {
        return String.fromCharCode(
          c.charCodeAt(0) + (c.toLowerCase() < 'n' ? 13 : -13)
        );
      });
    }

    function caesarEncrypt(str, shift = 3) {
      return str.replace(/[a-zA-Z]/g, function (c) {
        const base = c <= 'Z' ? 65 : 97;
        return String.fromCharCode(((c.charCodeAt(0) - base + shift) % 26) + base);
      });
    }

    function caesarDecrypt(str, shift = 3) {
      return caesarEncrypt(str, 26 - shift);
    }

    function encryptText() {
      const input = document.getElementById('inputText').value;
      const algorithm = document.getElementById('algorithmSelect').value;
      const key = document.getElementById('keyInput').value;
      let output = '';

      if (!input.trim()) {
        alert('Please enter or upload text.');
        return;
      }

      if (algorithm === 'rot13') {
        output = '[ROT13 ENCRYPTED]: ' + rot13(input);
      } else if (algorithm === 'caesar') {
        const shift = parseInt(key);
        if (isNaN(shift)) {
          alert('Please enter a numeric key for Caesar Cipher.');
          return;
        }
        output = '[Caesar ENCRYPTED]: ' + caesarEncrypt(input, shift);
      } else if (algorithm === 'all') {
        const shift = parseInt(key);
        if (isNaN(shift)) {
          alert('Please enter a numeric key for batch Caesar Cipher.');
          return;
        }
        output += '[ROT13 ENCRYPTED]: ' + rot13(input) + '\n';
        output += '[Caesar ENCRYPTED]: ' + caesarEncrypt(input, shift) + '\n';
        output += '[Vigenère ENCRYPTED]: (Coming soon)\n';
        output += '[Transposition ENCRYPTED]: (Coming soon)\n';
        output += '[AES ENCRYPTED]: (Coming soon)\n';
        output += '[RSA ENCRYPTED]: (Coming soon)\n';
      } else {
        output = '(' + algorithm + ') encryption coming soon...';
      }

      document.getElementById('outputText').value = output;
    }

    function decryptText() {
      const input = document.getElementById('inputText').value;
      const algorithm = document.getElementById('algorithmSelect').value;
      const key = document.getElementById('keyInput').value;
      let output = '';

      if (!input.trim()) {
        alert('Please enter or upload text.');
        return;
      }

      if (algorithm === 'rot13') {
        output = '[ROT13 DECRYPTED]: ' + rot13(input);
      } else if (algorithm === 'caesar') {
        const shift = parseInt(key);
        if (isNaN(shift)) {
          alert('Please enter a numeric key for Caesar Cipher.');
          return;
        }
        output = '[Caesar DECRYPTED]: ' + caesarDecrypt(input, shift);
      } else if (algorithm === 'all') {
        const shift = parseInt(key);
        if (isNaN(shift)) {
          alert('Please enter a numeric key for batch Caesar Cipher.');
          return;
        }
        output += '[ROT13 DECRYPTED]: ' + rot13(input) + '\n';
        output += '[Caesar DECRYPTED]: ' + caesarDecrypt(input, shift) + '\n';
        output += '[Vigenère DECRYPTED]: (Coming soon)\n';
        output += '[Transposition DECRYPTED]: (Coming soon)\n';
        output += '[AES DECRYPTED]: (Coming soon)\n';
        output += '[RSA DECRYPTED]: (Coming soon)\n';
      } else {
        output = '(' + algorithm + ') decryption coming soon...';
      }

      document.getElementById('outputText').value = output;
    }

    function downloadOutput() {
      const text = document.getElementById('outputText').value;
      const blob = new Blob([text], { type: 'text/plain' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'output.txt';
      a.click();
      URL.revokeObjectURL(url);
    }

    function loadFileText(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
          document.getElementById('inputText').value = e.target.result;
        };
        reader.readAsText(file);
      }
    }
  </script>
</body>
</html>
