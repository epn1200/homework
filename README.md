<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>作業登記系統</title>
</head>
<body>
  <h2>作業登記系統</h2>
  <video id="camera" width="300" height="200" style="display:none;"></video>
  
  <form id="submitForm">
    <label>座號：</label>
    <input type="text" id="studentId" required>
    <button type="submit">登記</button>
  </form>

  <script src="https://cdn.jsdelivr.net/npm/jsqr/dist/jsQR.js"></script>
  <script>
    const video = document.getElementById('camera');
    const studentIdInput = document.getElementById('studentId');
    const form = document.getElementById('submitForm');
    const API_URL = "[YOUR_GOOGLE_SCRIPT_URL](https://script.google.com/macros/s/AKfycbyR86BoMxyPYGLTNzZQMD1471HtuilyLWAhVWIjOceRNL9UUGmE54vUY5JnvozGMwVP/exec)"; // ← 換成你的 GAS 網址

    // 開啟鏡頭
    navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } })
      .then(stream => {
        video.srcObject = stream;
        video.setAttribute("playsinline", true);
        video.play();
        scan();
      });

    function scan() {
      const canvas = document.createElement("canvas");
      const context = canvas.getContext("2d");
      requestAnimationFrame(scan);

      if (video.readyState === video.HAVE_ENOUGH_DATA) {
        canvas.width = video.videoWidth;
        canvas.height = video.videoHeight;
        context.drawImage(video, 0, 0, canvas.width, canvas.height);
        const imageData = context.getImageData(0, 0, canvas.width, canvas.height);

        const code = jsQR(imageData.data, canvas.width, canvas.height);
        if (code) {
          studentIdInput.value = code.data;
          form.requestSubmit();
        }
      }
    }

    // 表單送出
    form.addEventListener("submit", async (e) => {
      e.preventDefault();
      const studentId = studentIdInput.value;
      try {
        let res = await fetch(API_URL, {
          method: "POST",
          body: JSON.stringify({ studentId }),
        });
        let text = await res.text();
        alert("登記結果：" + text);
      } catch (err) {
        alert("送出失敗：" + err);
      }
      studentIdInput.value = "";
    });
  </script>
</body>
</html>
