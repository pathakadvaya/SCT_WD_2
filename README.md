<!DOCTYPE html>
<html>
<head>
  <title>Stopwatch</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
    }
    #stopwatch {
      font-size: 48px;
      font-weight: bold;
      margin-bottom: 20px;
    }
    #lap-times {
      margin-top: 20px;
    }
    .lap-time {
      margin-bottom: 10px;
    }
  </style>
</head>
<body>
  <h1>Stopwatch</h1>
  <div id="stopwatch">00:00:00</div>
  <button id="start-button">Start</button>
  <button id="pause-button" disabled>Pause</button>
  <button id="reset-button" disabled>Reset</button>
  <div id="lap-times"></div>

  <script>
    let startTime = 0;
    let currentTime = 0;
    let lapTimes = [];
    let intervalId = null;
    let isRunning = false;

    document.getElementById('start-button').addEventListener('click', startStopwatch);
    document.getElementById('pause-button').addEventListener('click', pauseStopwatch);
    document.getElementById('reset-button').addEventListener('click', resetStopwatch);

    function startStopwatch() {
      startTime = new Date().getTime();
      intervalId = setInterval(updateStopwatch, 10);
      isRunning = true;
      document.getElementById('start-button').disabled = true;
      document.getElementById('pause-button').disabled = false;
      document.getElementById('reset-button').disabled = false;
    }

    function pauseStopwatch() {
      clearInterval(intervalId);
      isRunning = false;
      document.getElementById('start-button').disabled = false;
      document.getElementById('pause-button').disabled = true;
    }

    function resetStopwatch() {
      startTime = 0;
      currentTime = 0;
      lapTimes = [];
      document.getElementById('stopwatch').innerHTML = '00:00:00';
      document.getElementById('lap-times').innerHTML = '';
      isRunning = false;
      document.getElementById('start-button').disabled = false;
      document.getElementById('pause-button').disabled = true;
      document.getElementById('reset-button').disabled = true;
    }

    function updateStopwatch() {
      currentTime = new Date().getTime() - startTime;
      let hours = Math.floor(currentTime / 3600000);
      let minutes = Math.floor((currentTime % 3600000) / 60000);
      let seconds = Math.floor((currentTime % 60000) / 1000);
      let milliseconds = currentTime % 1000;
      document.getElementById('stopwatch').innerHTML = `${pad(hours)}:${pad(minutes)}:${pad(seconds)}.${pad(milliseconds, 3)}`;
    }

    function lap() {
      let lapTime = currentTime;
      lapTimes.push(lapTime);
      let lapTimeElement = document.createElement('div');
      lapTimeElement.className = 'lap-time';
      lapTimeElement.innerHTML = `Lap ${lapTimes.length}: ${formatTime(lapTime)}`;
      document.getElementById('lap-times').appendChild(lapTimeElement);
    }

    function pad(number, length = 2) {
      return String(number).padStart(length, '0');
    }

    function formatTime(time) {
      let hours = Math.floor(time / 3600000);
      let minutes = Math.floor((time % 3600000) / 60000);
      let seconds = Math.floor((time % 60000) / 1000);
      let milliseconds = time % 1000;
      return `${pad(hours)}:${pad(minutes)}:${pad(seconds)}.${pad(milliseconds, 3)}`;
    }
  </script>
</body>
</html>
