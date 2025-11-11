
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Minimal Retro Calendar</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;600&display=swap');

    body {
      background-color: #f2e9de;
      font-family: 'IBM Plex Mono', monospace;
      color: #222;
      letter-spacing: 0.5px;
    }

    .calendar-container {
      background: #fffaf3;
      border: 2px solid #222;
      border-radius: 6px;
      box-shadow: 4px 4px 0 #222;
      padding: 1rem;
      max-width: 360px;
      margin: 2rem auto;
    }

    .calendar-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 2px solid #222;
      padding-bottom: 0.5rem;
      margin-bottom: 1rem;
    }

    button {
      background: none;
      border: 1px solid #222;
      font-size: 12px;
      padding: 4px 8px;
      cursor: pointer;
      transition: background 0.2s ease;
    }

    button:hover {
      background: #222;
      color: #fffaf3;
    }

    #monthYear {
      font-weight: 600;
      font-size: 14px;
      text-transform: uppercase;
    }

    .weekdays {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      font-size: 11px;
      text-align: center;
      font-weight: 600;
      border-bottom: 1px dashed #888;
      padding-bottom: 6px;
      margin-bottom: 6px;
    }

    .day {
      min-width: 38px;
      min-height: 38px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 1px;
      border-radius: 4px;
      cursor: pointer;
      border: 1px solid transparent;
      transition: background-color 0.1s ease;
    }

    .day:hover {
      background: #e9e1d0;
    }

    .today {
      border: 1px solid #000;
      background: #dcd3c2;
      font-weight: bold;
    }

    .empty {
      opacity: 0.3;
      pointer-events: none;
    }

    .footer {
      text-align: center;
      font-size: 11px;
      color: #555;
      margin-top: 1rem;
    }
  </style>
</head>
<body>
  <div class="calendar-container">
    <div class="calendar-header">
      <button id="prevBtn">◀</button>
      <div>
        <div id="monthYear"></div>
      </div>
      <button id="nextBtn">▶</button>
    </div>

    <div class="weekdays">
      <div>Sun</div><div>Mon</div><div>Tue</div><div>Wed</div><div>Thu</div><div>Fri</div><div>Sat</div>
    </div>

    <div id="calendarDays" class="grid grid-cols-7 gap-1 text-center"></div>

    <div class="flex justify-center mt-3">
      <button id="todayBtn">Today</button>
    </div>

    <p class="footer">Made in ❤️ By Armeen </p>
  </div>

  <script>
    const monthYear = document.getElementById('monthYear');
    const calendarDays = document.getElementById('calendarDays');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const todayBtn = document.getElementById('todayBtn');

    let activeDate = new Date();

    function normalize(d) {
      return new Date(d.getFullYear(), d.getMonth(), d.getDate());
    }

    function renderCalendar() {
      const year = activeDate.getFullYear();
      const month = activeDate.getMonth();

      monthYear.innerText = activeDate.toLocaleDateString('en-US', { month: 'long', year: 'numeric' });

      const firstDayIndex = new Date(year, month, 1).getDay();
      const lastDate = new Date(year, month + 1, 0).getDate();

      calendarDays.innerHTML = '';

      for (let i = 0; i < firstDayIndex; i++) {
        const empty = document.createElement('div');
        empty.className = 'empty';
        calendarDays.appendChild(empty);
      }

      const today = normalize(new Date());

      for (let d = 1; d <= lastDate; d++) {
        const cell = document.createElement('div');
        cell.className = 'day';
        cell.innerText = d;
        const cellDate = normalize(new Date(year, month, d));

        if (cellDate.getTime() === today.getTime()) {
          cell.classList.add('today');
        }

        cell.addEventListener('click', () => {
          alert(`Selected: ${cellDate.toLocaleDateString()}`);
        });

        calendarDays.appendChild(cell);
      }
    }

    prevBtn.addEventListener('click', () => {
      activeDate.setMonth(activeDate.getMonth() - 1);
      renderCalendar();
    });

    nextBtn.addEventListener('click', () => {
      activeDate.setMonth(activeDate.getMonth() + 1);
      renderCalendar();
    });

    todayBtn.addEventListener('click', () => {
      activeDate = new Date();
      renderCalendar();
    });

    document.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowLeft') { activeDate.setMonth(activeDate.getMonth() - 1); renderCalendar(); }
      if (e.key === 'ArrowRight') { activeDate.setMonth(activeDate.getMonth() + 1); renderCalendar(); }
    });

    renderCalendar();
  </script>
</body>
</html>
