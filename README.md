<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Geonation Minecraft Server</title>

  <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">

  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Press Start 2P', cursive;
      background: linear-gradient(135deg, #0b3d0b, #000);
      color: #fff;
      min-height: 100vh;
      overflow-x: hidden;
    }

    a { text-decoration: none; }

    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px;
    }

    .logo {
      font-size: 20px;
      cursor: pointer;
      transition: transform 0.3s;
    }

    .logo:hover { transform: scale(1.1); }

    .vip-btn {
      margin-left: 15px;
      padding: 10px 16px;
      border-radius: 20px;
      background: #1faa1f;
      color: #000;
      font-size: 12px;
      cursor: pointer;
      transition: transform 0.3s, box-shadow 0.3s;
    }

    .vip-btn:hover {
      transform: scale(1.1);
      box-shadow: 0 0 10px #1faa1f;
    }

    .socials {
      display: flex;
      gap: 12px;
    }

    .social-btn {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 10px 18px;
      border-radius: 25px;
      font-size: 12px;
      font-weight: bold;
      transition: transform 0.3s, box-shadow 0.3s;
    }

    .tiktok {
      background: #ff0050;
      color: #000;
    }

    .discord {
      background: #5865F2;
      color: #000;
    }

    .social-btn:hover {
      transform: scale(1.1);
      box-shadow: 0 0 12px rgba(255,255,255,0.6);
    }

    .page {
      display: none;
      text-align: center;
      padding: 80px 20px;
      animation: fadeSlide 0.6s ease;
    }

    .page.active { display: block; }

    @keyframes fadeSlide {
      from { opacity: 0; transform: translateY(40px); }
      to { opacity: 1; transform: translateY(0); }
    }

    h1 {
      font-size: 48px;
      margin-bottom: 60px;
      text-shadow: 4px 4px #000;
      animation: pop 0.8s ease;
    }

    @keyframes pop {
      from { transform: scale(0.7); opacity: 0; }
      to { transform: scale(1); opacity: 1; }
    }

    .server-info {
      margin-top: 120px;
      font-size: 14px;
      line-height: 2.2;
    }

    .status { color: #00ff00; }

    .ranks {
      display: flex;
      justify-content: center;
      gap: 40px;
      flex-wrap: wrap;
      margin-top: 80px;
    }

    .rank-box {
      background: rgba(0,0,0,0.6);
      border: 2px solid #1faa1f;
      border-radius: 20px;
      width: 220px;
      padding: 30px 20px;
      transition: transform 0.3s, box-shadow 0.3s;
    }

    .rank-box:hover {
      transform: translateY(-10px) scale(1.05);
      box-shadow: 0 0 20px #1faa1f;
    }

    .rank-box h2 {
      margin-bottom: 20px;
      color: #1faa1f;
    }

    .price { font-size: 14px; }
  </style>
</head>

<body>

<!-- Sounds -->
<audio id="clickSound" src="https://assets.mixkit.co/sfx/preview/mixkit-game-click-1114.mp3"></audio>
<audio id="pageSound" src="https://assets.mixkit.co/sfx/preview/mixkit-game-level-completed-2059.mp3"></audio>

<header>
  <div>
    <span class="logo" onclick="playPageSound(); showPage('home')">Geonation</span>
    <button class="vip-btn" onclick="playPageSound(); showPage('vip')">VIP Ranks</button>
  </div>

  <div class="socials">
    <a class="social-btn tiktok" onclick="playClickSound()" href="https://www.tiktok.com/@mc_itzzmg" target="_blank">
      TikTok
    </a>

    <a class="social-btn tiktok" onclick="playClickSound()" href="https://www.tiktok.com/@mastrasss._" target="_blank">
      TikTok 2
    </a>

    <a class="social-btn discord" onclick="playClickSound()" href="https://discord.gg/tcvtMVpj" target="_blank">
      Discord
    </a>
  </div>
</header>

<section id="home" class="page active">
  <h1>Geonation</h1>

  <div class="server-info">
    <p>Server Name: Geonation</p>
    <p>IP: 91.197.6.16:22976</p>
    <p>Status: <span class="status" id="serverStatus">Checking...</span></p>
  </div>
</section>

<section id="vip" class="page">
  <h1>VIP Ranks</h1>

  <div class="ranks">
    <div class="rank-box">
      <h2>VIP</h2>
      <p class="price">Price: 10 GEL</p>
    </div>

    <div class="rank-box">
      <h2>+VIP</h2>
      <p class="price">Price: 20 GEL</p>
    </div>
  </div>
</section>

<script>
  const clickSound = document.getElementById('clickSound');
  const pageSound = document.getElementById('pageSound');

  function playClickSound() {
    clickSound.currentTime = 0;
    clickSound.play();
  }

  function playPageSound() {
    pageSound.currentTime = 0;
    pageSound.play();
  }

  async function checkServerStatus() {
    try {
      const res = await fetch('https://api.mcstatus.io/v2/status/java/91.197.6.16:22976');
      const data = await res.json();
      const statusEl = document.getElementById('serverStatus');

      if (data.online) {
        statusEl.textContent = 'ONLINE';
        statusEl.style.color = '#00ff00';
      } else {
        statusEl.textContent = 'OFFLINE';
        statusEl.style.color = 'red';
      }
    } catch {
      const statusEl = document.getElementById('serverStatus');
      statusEl.textContent = 'ERROR';
      statusEl.style.color = 'orange';
    }
  }

  checkServerStatus();

  function showPage(pageId) {
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.getElementById(pageId).classList.add('active');
  }
</script>

</body>
</html>
