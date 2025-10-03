
<html>
<head>
  <meta charset="UTF-8">
  <title>作業繳交登記</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; padding: 20px; }
    input, button { padding: 8px; margin: 5px; font-size: 16px; }
    #msg { margin-top: 10px; font-weight: bold; color: green; }
  </style>
</head>
<body>
  <h2>📚 作業繳交登記</h2>
  <label>座號：<input type="number" id="seat" min="1" max="30"></label><br>
  <button onclick="mark('已繳交')">✅ 已繳交</button>
  <button onclick="mark('未繳交')">❌ 未繳交</button>

  <p id="msg"></p>

  <script>
    const API_URL = "https://script.google.com/macros/library/d/1fU2RCYb4eqo6AHR_eqTCYBwuKxH6n40sRD6ybTDj8Cr_3XJa4k8yDe1x/1"; // 換成你的 URL

    function mark(status) {
      const seat = document.getElementById("seat").value;
      if (!seat) { alert("請輸入座號！"); return; }
      fetch(API_URL, {
        method: "POST",
        body: JSON.stringify({ seat: seat, status: status }),
        headers: { "Content-Type": "application/json" }
      })
      .then(res => res.text())
      .then(msg => {
        document.getElementById("msg").innerText = "✅ 登記完成！";
        document.getElementById("seat").value = "";
      })
      .catch(err => {
        document.getElementById("msg").innerText = "⚠️ 發生錯誤：" + err;
      });
    }
  </script>
</body>
</html>
