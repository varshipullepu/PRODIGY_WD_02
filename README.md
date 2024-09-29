<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stopwatch</title>
  <style>
    body {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      font-family: Arial, sans-serif;
    }
    .timer {
      font-size: 48px;
      margin-bottom: 20px;
    }
    .buttons {
      margin: 10px;
    }
    button {
      padding: 10px 20px;
      font-size: 16px;
      margin: 5px;
    }
    .laps {
      margin-top: 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <div class="timer">00:00:00</div>
  <div class="buttons">
    <button id="start">Start</button>
    <button id="pause">Pause</button>
    <button id="reset">Reset</button>
    <button id="lap">Lap</button>
  </div>
  <div class="laps"></div>

  <script>
    let startTime, updatedTime, difference, tInterval, isRunning = false;
    let lapTimes = [];
    
    const timerDisplay = document.querySelector('.timer');
    const lapsDisplay = document.querySelector('.laps');

    function startTimer() {
      if (!isRunning) {
        startTime = Date.now() - (difference || 0);
        tInterval = setInterval(updateTime, 1000);
        isRunning = true;
      }
    }

    function pauseTimer() {
      clearInterval(tInterval);
      isRunning = false;
    }

    function resetTimer() {
      clearInterval(tInterval);
      isRunning = false;
      difference = 0;
      lapTimes = [];
      timerDisplay.textContent = '00:00:00';
      lapsDisplay.innerHTML = '';
    }

    function updateTime() {
      updatedTime = Date.now() - startTime;
      let hours = Math.floor((updatedTime % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      let minutes = Math.floor((updatedTime % (1000 * 60 * 60)) / (1000 * 60));
      let seconds = Math.floor((updatedTime % (1000 * 60)) / 1000);
      
      timerDisplay.textContent = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
    }

    function recordLap() {
      lapTimes.push(timerDisplay.textContent);
      lapsDisplay.innerHTML = lapTimes.map((lap, index) => `<p>Lap ${index + 1}: ${lap}</p>`).join('');
    }

    document.getElementById('start').addEventListener('click', startTimer);
    document.getElementById('pause').addEventListener('click', pauseTimer);
    document.getElementById('reset').addEventListener('click', resetTimer);
    document.getElementById('lap').addEventListener('click', recordLap);
  </script>

</body>
</html>
