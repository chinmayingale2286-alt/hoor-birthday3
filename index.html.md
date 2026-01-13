<!DOCTYPE html>  
<html lang="en">  
<head>  
<meta charset="UTF-8">  
<title>Happy 18th Birthday Hoor</title>  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
  
<style>  
body {  
  margin: 0;  
  background: radial-gradient(circle at top, #0b0f2b, #050711);  
  color: #fff;  
  font-family: 'Poppins', sans-serif;  
  text-align: center;  
  overflow-x: hidden;  
}  
  
.container {  
  min-height: 100vh;  
  display: flex;  
  align-items: center;  
  justify-content: center;  
}  
  
.card {  
  background: rgba(255,255,255,0.08);  
  backdrop-filter: blur(18px);  
  border-radius: 30px;  
  padding: 40px;  
  max-width: 900px;  
  width: 100%;  
  box-shadow: 0 30px 80px rgba(0,0,0,0.6);  
}  
  
h1 {  
  font-size: 3em;  
  margin-bottom: 10px;  
}  
  
h2 {  
  font-weight: 300;  
  opacity: 0.85;  
}  
  
/* Cake */  
.cake-container {  
  margin: 40px auto;  
  position: relative;  
}  
  
.cake {  
  width: 260px;  
}  
  
.candles {  
  position: absolute;  
  top: -30px;  
  left: 50%;  
  transform: translateX(-50%);  
  display: flex;  
  gap: 15px;  
}  
  
.flame {  
  width: 12px;  
  height: 25px;  
  background: radial-gradient(circle, #ffd166, #ff7b00);  
  border-radius: 50%;  
  animation: flicker 0.15s infinite alternate;  
}  
  
.flame.off {  
  display: none;  
}  
  
@keyframes flicker {  
  from { transform: scaleY(1); }  
  to { transform: scaleY(1.3); }  
}  
  
/* Fireworks canvas */  
canvas {  
  position: fixed;  
  top: 0;  
  left: 0;  
  pointer-events: none;  
}  
  
/* Cats */  
.cats {  
  display: flex;  
  justify-content: center;  
  gap: 20px;  
  margin-top: 25px;  
}  
  
.cats img {  
  width: 150px;  
  border-radius: 20px;  
}  
  
/* Message */  
#instruction {  
  margin-top: 20px;  
  font-size: 1.1em;  
  opacity: 0.9;  
}  
</style>  
</head>  
  
<body>  
  
<audio id="music" loop>  
  <source src="birthday.mp3" type="audio/mpeg">  
</audio>  
  
<canvas id="fireworks"></canvas>  
  
<div class="container">  
  <div class="card">  
    <h1>Happy 18th Birthday, Hoor</h1>  
    <h2>Make a wish… and blow out the candles 🎂</h2>  
  
    <div class="cake-container">  
      <img class="cake" src="https://i.imgur.com/8fK4h6Y.png">  
      <div class="candles">  
        <div class="flame"></div>  
        <div class="flame"></div>  
        <div class="flame"></div>  
      </div>  
    </div>  
  
    <p id="instruction">Blow towards your phone 🎤✨</p>  
  
    <div class="cats">  
      <img src="https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif">  
      <img src="https://media.giphy.com/media/VbnUQpnihPSIgIXuZv/giphy.gif">  
    </div>  
  </div>  
</div>  
  
<script>  
/* MICROPHONE BLOW DETECTION */  
let blown = false;  
  
navigator.mediaDevices.getUserMedia({ audio: true }).then(stream => {  
  const audioContext = new AudioContext();  
  const mic = audioContext.createMediaStreamSource(stream);  
  const analyser = audioContext.createAnalyser();  
  mic.connect(analyser);  
  
  const data = new Uint8Array(analyser.frequencyBinCount);  
  
  function detectBlow() {  
    analyser.getByteFrequencyData(data);  
    let volume = data.reduce((a,b)=>a+b) / data.length;  
  
    if (volume > 50 && !blown) {  
      blown = true;  
      blowCandles();  
    }  
    requestAnimationFrame(detectBlow);  
  }  
  detectBlow();  
});  
  
/* Candle blow */  
function blowCandles() {  
  document.querySelectorAll('.flame').forEach(f => f.classList.add('off'));  
  document.getElementById("instruction").innerText = "✨ Wish granted ✨";  
  document.getElementById("music").play();  
  launchFireworks();  
}  
  
/* FIREWORKS */  
const canvas = document.getElementById("fireworks");  
const ctx = canvas.getContext("2d");  
canvas.width = innerWidth;  
canvas.height = innerHeight;  
  
function launchFireworks() {  
  for (let i = 0; i < 25; i++) {  
    setTimeout(() => explode(  
      Math.random()*canvas.width,  
      Math.random()*canvas.height/2  
    ), i*150);  
  }  
}  
  
function explode(x, y) {  
  for (let i = 0; i < 50; i++) {  
    ctx.fillStyle = `hsl(${Math.random()*360},100%,60%)`;  
    ctx.fillRect(x + Math.random()*80-40, y + Math.random()*80-40, 3, 3);  
  }  
  setTimeout(()=>ctx.clearRect(0,0,canvas.width,canvas.height),300);  
}  
  
window.onresize = () => {  
  canvas.width = innerWidth;  
  canvas.height = innerHeight;  
};  
</script>  
  
</body>  
</html>  
