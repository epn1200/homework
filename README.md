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

    async function mark(status) {
      const seat = document.getElementById("seat").value;
      if (!seat) {
        alert("請輸入座號！");
        return;
      }

      try {
        const res = await fetch(API_URL, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ studentId: seat, status: status })
        });

        // 先拿文字
        const text = await res.text();

        let data;
        try {
          data = JSON.parse(text); // 嘗試解析 JSON
        } catch(err) {
          // 回傳不是 JSON，顯示原始內容
          document.getElementById("msg").innerText = "⚠️ API 回傳不是 JSON:\n" + text;
          return;
        }

        if (data.status === "ok") {
          document.getElementById("msg").innerText = `✅ ${data.studentId} 號登記完成`;
          document.getElementById("seat").value = "";
        } else {
          document.getElementById("msg").innerText = "⚠️ 登記失敗: " + JSON.stringify(data);
        }

      } catch(err) {
        document.getElementById("msg").innerText = "⚠️ 發生錯誤：" + err;
      }
    }
  </script>
</body>
</html>
