<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>📚 作業繳交登記</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; padding: 30px; }
    input, button { padding: 10px; margin: 8px; font-size: 18px; }
    #msg { margin-top: 15px; font-weight: bold; }
  </style>
</head>
<body>
  <h2>📚 作業繳交登記系統</h2>

  <label>座號：<input type="number" id="seat" min="1" max="50"></label><br>
  <button onclick="mark('已繳交')">✅ 已繳交</button>
  <button onclick="mark('未繳交')">❌ 未繳交</button>

  <p id="msg"></p>

  <script>
    const API_URL = "https://script.google.com/macros/s/AKfycbxvSt8eUEVxkG53IZoFlhnAhvOOvL91kLXEwkpqtswIETkkRBZBp3sHkSZMUIZG4PZ-/exec";

    function mark(status) {
      const seat = document.getElementById("seat").value;
      if (!seat) {
        alert("請輸入座號！");
        return;
      }

      fetch(API_URL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ studentId: seat, status: status })
      })
      .then(res => res.json())
      .then(data => {
        document.getElementById("msg").innerText = `✅ ${data.studentId} 號登記完成：${data.record}`;
        document.getElementById("seat").value = "";
      })
      .catch(err => {
        document.getElementById("msg").innerText = "⚠️ 發生錯誤：" + err;
      });
    }
  </script>
</body>
</html>
