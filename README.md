<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Retro90sHiphop Radio</title>
  <style>
    body {
      background: #000;
      color: #0f0;
      font-family: monospace;
      text-align: center;
      padding: 40px;
    }
    #player {
      width: 0;
      height: 0;
      visibility: hidden;
    }
    #songInfo {
      font-size: 20px;
      margin: 20px 0;
    }
    button {
      background: #0f0;
      color: #000;
      padding: 12px 20px;
      margin: 8px;
      border: none;
      cursor: pointer;
      font-size: 18px;
    }
    button:hover {
      background: #0c0;
    }
  </style>
</head>
<body>
  <h1>📻 Retro90sHiphop Radio</h1>
  <div id="songInfo">Loading your track...</div>
  <div class="controls">
    <button onclick="prevSong()">⏮️ Prev</button>
    <button onclick="playSong()">▶️ Play</button>
    <button onclick="pauseSong()">⏸️ Pause</button>
    <button onclick="nextSong()">⏭️ Next</button>
  </div>
  <div id="player"></div>

  <script>
    // YouTube API script
    const tag = document.createElement('script');
    tag.src = "https://www.youtube.com/iframe_api";
    document.head.appendChild(tag);

    const playlist = [
      { title: "Straight Outta Compton", artist: "N.W.A", id: "TMZi25Pq3T8" },
      { title: "F*** Tha Police", artist: "N.W.A", id: "51t1OsPSdBc" },
      { title: "All Eyez on Me", artist: "2Pac", id: "Ne_hVxwEGCg" },
      { title: "Hit 'Em Up", artist: "2Pac", id: "41qC3w3UUkU" },
      { title: "Hennessy", artist: "2Pac", id: "bZBpe3PV8QQ" },
      { title: "The Next Episode", artist: "Dr. Dre ft. Snoop", id: "QZXc39hT8t4" },
      { title: "Nuthin' But a G Thang", artist: "Dr. Dre ft. Snoop", id: "0F0CAEoF4XM" },
      { title: "Still D.R.E.", artist: "Dr. Dre ft. Snoop", id: "_CL6n0FJZpk" },
      { title: "The Real Slim Shady", artist: "Eminem", id: "eJO5HU_7_1w" },
      { title: "Without Me", artist: "Eminem", id: "YVkUvmDQ3HY" },
      { title: "Mockingbird", artist: "Eminem", id: "S9bCLPwzSC0" },
      { title: "Houdini", artist: "Eminem", id: "Ne5S8J5Q-Qs" },
      { title: "In Da Club", artist: "50 Cent", id: "5qm8PH4xAss" },
      { title: "Gangsta's Paradise", artist: "Coolio", id: "fPO76Jlnz6c" },
      { title: "Real Muthaphuckkin G's", artist: "Eazy-E", id: "RjizH3C-9ks" },
      { title: "Hello", artist: "Ice Cube", id: "fld7_PgtUL8" },
      { title: "It Was a Good Day", artist: "Ice Cube", id: "h4UqMyldS7Q" }
    ];

    let player;
    let shuffledPlaylist = [];
    let history = [];
    let currentIndex = -1;

    function shuffle(array) {
      const arr = array.slice();
      for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
      return arr;
    }

    function onYouTubeIframeAPIReady() {
      shuffledPlaylist = shuffle(playlist);
      currentIndex = 0;
      loadSong(shuffledPlaylist[currentIndex]);
    }

    function loadSong(song) {
      if (player) {
        player.loadVideoById(song.id);
      } else {
        player = new YT.Player('player', {
          videoId: song.id,
          playerVars: {
            autoplay: 1,
            controls: 0,
            showinfo: 0,
            modestbranding: 1,
            rel: 0,
            fs: 0
          },
          events: {
            onReady: () => {
              player.playVideo();
              updateSongInfo(song);
            },
            onStateChange: e => {
              if (e.data === YT.PlayerState.ENDED) nextSong();
            }
          }
        });
      }
      updateSongInfo(song);
    }

    function updateSongInfo(song) {
      document.getElementById('songInfo').innerText = `${song.artist} – "${song.title}"`;
    }

    function playSong() {
      if (player) player.playVideo();
    }

    function pauseSong() {
      if (player) player.pauseVideo();
    }

    function nextSong() {
      if (currentIndex < shuffledPlaylist.length - 1) {
        currentIndex++;
      } else {
        shuffledPlaylist = shuffle(playlist);
        currentIndex = 0;
      }
      loadSong(shuffledPlaylist[currentIndex]);
    }

    function prevSong() {
      if (currentIndex > 0) {
        currentIndex--;
        loadSong(shuffledPlaylist[currentIndex]);
      }
    }
  </script>
</body>
</html>
