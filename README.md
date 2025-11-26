<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <title>🎤 ردیاب شدت صدا</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      direction: rtl;
      background: #1b1b1b;
      color: white;
      text-align: center;
      padding-top: 60px;
    }

    h1 {
      font-weight: 300;
    }

    .bar {
      idth: 60%;
      height: 20px;
      margin: 30px auto;
      background: #333;
      border-radius: 12px;
      overflow: hidden;
    }

    .fill {
      height: 100%;
      width: 0%;
      background: linear-gradient(90deg, #00e5ff, #00ff8a);
      border-radius: 12px;
      transition: width 0.1s ease-out;
    }

    button {
      margin-top: 30px;
      padding: 12px 22px;
      font-size: 16px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      background: #00e5ff;
    }
    button:hover {
      opacity: 0.8;
    }
  </style>
</head>
<body>

  <h1>🎤 ردیابی شدت صدای محیط</h1>
  <p>روی دکمه زیر کلیک کنید و اجازه دسترسی به میکروفون بدهید.</p>

  <div class="bar">
    <div class="fill" id="fill"></div>
  </div>

  <button onclick="start()">شروع</button>

  <script>
    function start() {
      navigator.mediaDevices.getUserMedia({ audio: true })
        .then(stream => {
          const audioContext = new (window.AudioContext || window.webkitAudioContext)();
          const source = audioContext.createMediaStreamSource(stream);

          const analyser = audioContext.createAnalyser();
          analyser.fftSize = 512;

          const data = new Uint8Array(analyser.frequencyBinCount);
          source.connect(analyser);

          function update() {
            analyser.getByteFrequencyData(data);
            let value = data.reduce((a, b) => a + b) / data.length;

            let percent = Math.min(100, (value / 255) * 100);
            document.getElementById("fill").style.width = percent + "%";

            requestAnimationFrame(update);
          }
          update();
        })
        .catch(() => alert("دسترسی به میکروفون داده نشد."));
    }
  </script>

</body>
</html>
