<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <title>西元轉民國與日期差計算</title>
  <style>
    body {
      font-family: sans-serif;
      padding: 2em;
      background-color: #f0f4f8;
    }
    input, button {
      font-size: 1em;
      margin: 0.5em 0;
      padding: 0.4em;
      width: 200px;
    }
    label {
      display: block;
      margin-top: 1em;
    }
  </style>
</head>
<body>
  <h2>📅 日期換算（手動輸入）</h2>

  <label>輸入日期1（格式：YYYY/MM/DD）：</label>
  <input type="text" id="date1" placeholder="例如：2025/05/05">

  <label>輸入日期2（格式：YYYY/MM/DD）：</label>
  <input type="text" id="date2" placeholder="例如：2027/08/15">

  <br><button onclick="convertDates()">轉換與計算</button>

  <h3>結果：</h3>
  <p id="result1"></p>
  <p id="result2"></p>
  <p id="daysDiff"></p>

  <script>
    function parseDate(dateStr) {
      const parts = dateStr.split('/');
      if (parts.length !== 3) return null;

      const year = parseInt(parts[0], 10);
      const month = parseInt(parts[1], 10) - 1; // JS 月份從 0 開始
      const day = parseInt(parts[2], 10);

      const date = new Date(year, month, day);
      return isNaN(date.getTime()) ? null : date;
    }

    function convertToROC(date) {
      const year = date.getFullYear();
      const month = date.getMonth() + 1;
      const day = date.getDate();

      if (year < 1912) {
        return "（民國尚未建立）";
      }

      const rocYear = year - 1911;
      return `民國 ${rocYear} 年 ${month} 月 ${day} 日`;
    }

    function calculateDateDiff(start, end) {
      if (start > end) [start, end] = [end, start]; // 確保 start 在前

      let yearDiff = end.getFullYear() - start.getFullYear();
      let monthDiff = end.getMonth() - start.getMonth();
      let dayDiff = end.getDate() - start.getDate();

      if (dayDiff < 0) {
        // 借上個月的天數
        const temp = new Date(end.getFullYear(), end.getMonth(), 0); // 上個月的最後一天
        dayDiff += temp.getDate();
        monthDiff--;
      }

      if (monthDiff < 0) {
        monthDiff += 12;
        yearDiff--;
      }

      return `${yearDiff} 年 ${monthDiff} 月 ${dayDiff} 天`;
    }

    function convertDates() {
      const input1 = document.getElementById('date1').value.trim();
      const input2 = document.getElementById('date2').value.trim();

      const date1 = parseDate(input1);
      const date2 = parseDate(input2);

      if (!date1 || !date2) {
        alert("請輸入有效的西元日期（格式：YYYY/MM/DD）");
        return;
      }

      document.getElementById('result1').textContent = `日期1：${input1} → ${convertToROC(date1)}`;
      document.getElementById('result2').textContent = `日期2：${input2} → ${convertToROC(date2)}`;

      const detailDiff = calculateDateDiff(date1, date2);
      document.getElementById('daysDiff').textContent = `兩日期相差 ${detailDiff}`;
    }
  </script>
</body>
</html>
