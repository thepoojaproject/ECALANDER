
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Responsive Calendar</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .day {
      min-width: 44px;
      min-height: 44px;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      margin: 3px;
      border-radius: 8px;
      cursor: pointer;
      transition: transform .12s ease, background-color .12s ease;
      user-select: none;
    }
    .day:hover { transform: translateY(-3px); background-color: rgba(0,0,0,0.04); }
    .today {
      background-color: #2563eb; /* Tailwind blue-600 */
      color: white;
      font-weight: 600;
      box-shadow: 0 6px 18px rgba(37,99,235,0.18);
    }
    /* subtle disabled placeholders */
    .empty { opacity: 0.12; pointer-events: none; height: 44px; border-radius: 8px; }
  </style>
</head>
<body class="bg-slate-50 min-h-screen flex flex-col items-center justify-center p-4">
  <div class="w-full max-w-md">
    <div class="bg-white rounded-2xl shadow-lg p-5">
      <div class="flex items-center justify-between gap-2 mb-4">
        <div class="flex items-center gap-2">
          <button id="prevBtn" aria-label="Previous month" class="p-2 rounded-lg hover:bg-slate-100">
            ◀
          </button>
        </div>

        <div class="text-center">
          <div id="monthYear" class="text-lg font-semibold"></div>
          <div id="subtitle" class="text-xs text-gray-500">Current month & year shown here</div>
        </div>

        <div class="flex items-center gap-2">
          <button id="todayBtn" class="px-3 py-1 rounded-lg bg-slate-100 hover:bg-slate-200 text-sm">Today</button>
          <button id="nextBtn" aria-label="Next month" class="p-2 rounded-lg hover:bg-slate-100">
            ▶
          </button>
        </div>
      </div>

      <div class="grid grid-cols-7 text-center font-medium text-sm text-slate-600 mb-2">
        <div>Sun</div><div>Mon</div><div>Tue</div><div>Wed</div><div>Thu</div><div>Fri</div><div>Sat</div>
      </div>

      <div id="calendarDays" class="grid grid-cols-7 gap-1 text-center"></div>
    </div>

    <!-- Footer text -->
    <p class="text-center text-sm text-gray-500 mt-3">
      Made with ❤️ by <span class="font-semibold text-blue-600">Armeen</span>
    </p>
  </div>

  <script>
    const monthYear = document.getElementById('monthYear');
    const calendarDays = document.getElementById('calendarDays');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const todayBtn = document.getElementById('todayBtn');

    // activeDate is the month being displayed (day-of-month doesn't matter)
    let activeDate = new Date();

    // normalize time to avoid timezone edge cases
    function normalize(d) {
      return new Date(d.getFullYear(), d.getMonth(), d.getDate());
    }

    function renderCalendar() {
      const year = activeDate.getFullYear();
      const month = activeDate.getMonth();

      // display "Month Year" like "September 2025"
      monthYear.innerText = activeDate.toLocaleDateString('en-US', { month: 'long', year: 'numeric' });

      // first day weekday (0=Sun..6=Sat) and last date of month
      const firstDayIndex = new Date(year, month, 1).getDay();
      const lastDate = new Date(year, month + 1, 0).getDate();

      calendarDays.innerHTML = '';

      // add placeholders for days before the 1st
      for (let i = 0; i < firstDayIndex; i++) {
        const empty = document.createElement('div');
        empty.className = 'empty';
        calendarDays.appendChild(empty);
      }

      const today = normalize(new Date());

      for (let d = 1; d <= lastDate; d++) {
        const cell = document.createElement('div');
        cell.className = 'day';
        cell.tabIndex = 0;
        cell.innerText = d;

        const cellDate = normalize(new Date(year, month, d));

        if (cellDate.getTime() === today.getTime()) {
          cell.classList.add('today');
          cell.setAttribute('aria-current', 'date');
        }

        // click handler
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

    // keyboard navigation
    document.addEventListener('keydown', (e) => {
      if (e.key === 'ArrowLeft') { activeDate.setMonth(activeDate.getMonth() - 1); renderCalendar(); }
      if (e.key === 'ArrowRight') { activeDate.setMonth(activeDate.getMonth() + 1); renderCalendar(); }
    });

    // initial render
    renderCalendar();
  </script>
</body>
</html>
