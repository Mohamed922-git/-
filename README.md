<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>📚 جدول المذاكرة التفاعلي</title>
  <style>
    body {
      margin: 0;
      font-family: 'Cairo', sans-serif;
      background: linear-gradient(to right, #e0eafc, #cfdef3);
      backdrop-filter: blur(10px);
      color: #333;
    }
    .container {
      max-width: 1000px;
      margin: auto;
      padding: 20px;
      background: rgba(255, 255, 255, 0.3);
      border-radius: 20px;
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    }

    h1 {
      text-align: center;
      color: #333;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
      backdrop-filter: blur(10px);
    }

    th, td {
      border: 1px solid #ccc;
      padding: 10px;
      text-align: center;
    }

    input[type="text"], select {
      width: 100%;
      padding: 5px;
      border: none;
      border-radius: 5px;
      text-align: center;
    }

    .color-select option {
      font-weight: bold;
    }

    .controls {
      text-align: center;
      margin-top: 20px;
    }

    button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 16px;
      border: none;
      border-radius: 10px;
      background: rgba(255, 255, 255, 0.6);
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
      cursor: pointer;
    }

    .timer {
      text-align: center;
      font-size: 18px;
      margin-top: 25px;
      background: rgba(255,255,255,0.2);
      padding: 20px;
      border-radius: 15px;
    }

    #countdown {
      font-size: 24px;
      font-weight: bold;
      margin-top: 10px;
    }

  </style>
</head>
<body>
  <div class="container">
    <h1>📚 جدول المذاكرة من السبت للخميس</h1>
    <table>
      <tr>
        <th>اليوم</th>
        <th>المادة</th>
        <th>الوقت</th>
        <th>ملاحظات</th>
        <th>لون</th>
      </tr>
      <script>
        const days = ['السبت','الأحد','الإثنين','الثلاثاء','الأربعاء','الخميس'];
        const colors = ['aqua', 'red', 'purple', 'black', 'blue', 'pink'];
        days.forEach(day => {
          document.write(`<tr>
            <td>${day}</td>
            <td><input type="text" id="subject-${day}"></td>
            <td><input type="text" id="time-${day}"></td>
            <td><input type="text" id="note-${day}"></td>
            <td>
              <select id="color-${day}" class="color-select" onchange="updateRowColor('${day}')">
                ${colors.map(c => `<option value="${c}" style="color:${c}">${c}</option>`).join('')}
              </select>
            </td>
          </tr>`);
        });

        function updateRowColor(day) {
          const color = document.getElementById(`color-${day}`).value;
          document.getElementById(`subject-${day}`).style.color = color;
          document.getElementById(`time-${day}`).style.color = color;
          document.getElementById(`note-${day}`).style.color = color;
        }

        function resetSchedule() {
          days.forEach(day => {
            document.getElementById(`subject-${day}`).value = '';
            document.getElementById(`time-${day}`).value = '';
            document.getElementById(`note-${day}`).value = '';
            document.getElementById(`color-${day}`).selectedIndex = 0;
            updateRowColor(day);
          });
        }

        function shareToWhatsApp() {
          let text = "📚 جدول مذاكرتي:\n";
          days.forEach(day => {
            const subject = document.getElementById(`subject-${day}`).value;
            const time = document.getElementById(`time-${day}`).value;
            const note = document.getElementById(`note-${day}`).value;
            if(subject || time || note){
              text += `\n📅 ${day}:\n📘 المادة: ${subject || '-'}\n⏰ الوقت: ${time || '-'}\n📝 ملاحظات: ${note || '-'}\n`;
            }
          });
          const url = `https://wa.me/?text=${encodeURIComponent(text)}`;
          window.open(url, '_blank');
        }
      </script>
    </table>

    <div class="controls">
      <button onclick="resetSchedule()">إعادة تعيين</button>
      <button onclick="shareToWhatsApp()">مشاركة على واتساب</button>
    </div>

    <div class="timer">
      <label>⏳ حدد وقت المذاكرة (بالدقائق): </label>
      <input type="number" id="studyDuration" placeholder="مثلاً: 25">
      <br><br>
      <button onclick="startTimer()">ابدأ</button>
      <button onclick="stopTimer()">إنهاء</button>
      <div id="countdown"></div>
    </div>
  </div>

  <script>
    let timerInterval;
    function startTimer() {
      const minutes = parseInt(document.getElementById('studyDuration').value);
      if (isNaN(minutes) || minutes <= 0) {
        alert('من فضلك أدخل عدد دقائق صحيح.');
        return;
      }
      let time = minutes * 60;
      updateCountdown(time);
      timerInterval = setInterval(() => {
        time--;
        updateCountdown(time);
        if (time <= 0) {
          clearInterval(timerInterval);
          document.getElementById("countdown").innerText = "✅ انتهى الوقت!";
          alert("🎉 انتهى وقت المذاكرة!");
        }
      }, 1000);
    }

    function stopTimer() {
      clearInterval(timerInterval);
      document.getElementById("countdown").innerText = "⏹ تم إيقاف المؤقت.";
    }

    function updateCountdown(seconds) {
      const min = Math.floor(seconds / 60).toString().padStart(2, '0');
      const sec = (seconds % 60).toString().padStart(2, '0');
      document.getElementById("countdown").innerText = `${min}:${sec}`;
    }
  </script>
</body>
</html>
