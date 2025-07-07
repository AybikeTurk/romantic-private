<template>
    <div class="audio-player-wrapper">
        <!-- Yüzen Balonlar -->
        <div v-for="(balloon, i) in balloons" :key="i" class="balloon" :style="balloonStyle(balloon)">
            ❤️
        </div>

        <!-- Kalp Atışı Efekti -->
        <div class="heartbeat" :class="{ 'heartbeat-active': isPlaying }">
            💖
        </div>

        <!-- Ana Player -->
        <div class="audio-player" :class="{ 'player-pulse': isPlaying }">
            <button @click="togglePlay" class="play-button">
                {{ isPlaying ? '⏸' : '▶' }}
            </button>

            <div class="song-display">
                <transition name="slide-fade" mode="out-in">
                    <div :key="currentSong.title" class="song-info">
                        <div class="song-title">{{ currentSong.title }}</div>
                        <div class="song-artist">{{ currentSong.artist }}</div>
                    </div>
                </transition>

                <div class="progress-bar" @click="seek">
                    <div class="progress" :style="{ width: progress + '%' }"></div>
                </div>
            </div>

            <button @click="showPlaylist = !showPlaylist" class="playlist-button">
                ♫
            </button>
        </div>

        <!-- Playlist (Açılır Kapanır) -->
        <transition name="popup">
            <div v-if="showPlaylist" class="playlist">
                <div v-for="(song, i) in playlist" :key="i" @click="playSong(i)"
                    :class="{ 'active': i === currentSongIndex }">
                    {{ song.title }} - {{ song.artist }}
                </div>
            </div>
        </transition>

        <audio ref="audio" @timeupdate="updateProgress"></audio>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

// Şarkı Listesi
const playlist = [
    { title: "İntizar", artist: "Elmira Rehimova", src: "/songs/intizar.mp3" },
    { title: "Bana Sen Lazımsın", artist: "Rafet El Roman", src: "/songs/BanaSenLazimsin.mp3" },
    { title: "Geceyi Sana Yazdım", artist: "Onur Akın", src: "/songs/GeceyiSanaYazdim.mp3" },

];

// Player State
const audio = ref(null);
const isPlaying = ref(false);
const currentSongIndex = ref(0);
const currentSong = ref(playlist[0]);
const progress = ref(0);
const showPlaylist = ref(false);

// Animasyonlar
const balloons = ref(Array(10).fill().map(() => ({
    x: Math.random() * 100,
    y: Math.random() * 100,
    size: Math.random() * 20 + 10,
    speed: Math.random() * 2 + 1
})));

// Balon Animasyonu
const balloonStyle = (balloon) => ({
    left: `${balloon.x}%`,
    top: `${balloon.y}%`,
    fontSize: `${balloon.size}px`,
    animation: `float ${balloon.speed}s infinite alternate ease-in-out`
});

// Müzik Kontrolleri
const togglePlay = () => {
    if (isPlaying.value) {
        audio.value.pause();
    } else {
        audio.value.play().catch(e => console.error("Play error:", e));
    }
    isPlaying.value = !isPlaying.value;
};

const playSong = (index) => {
    currentSongIndex.value = index;
    currentSong.value = playlist[index];
    audio.value.src = currentSong.value.src;
    audio.value.load();
    if (isPlaying.value) audio.value.play();
};

const updateProgress = () => {
    progress.value = (audio.value.currentTime / audio.value.duration) * 100;
};

const seek = (e) => {
    const percent = e.offsetX / e.target.clientWidth;
    audio.value.currentTime = percent * audio.value.duration;
};
// AudioPlayer.vue (script setup)
const playAudio = () => {
    if (audio.value && !isPlaying.value) {
        audio.value.play()
            .then(() => {
                isPlaying.value = true;
            })
            .catch(err => console.warn("Autoplay engellendi:", err));
    }
};
// TANIMDAN SONRA expose ET
defineExpose({
    playAudio
});
// Balonları Hareket Ettir
onMounted(() => {
    audio.value.src = currentSong.value.src;
    setInterval(() => {
        balloons.value = balloons.value.map(b => ({
            ...b,
            y: (b.y - 0.05) % 100
        }));
    }, 50);
});
</script>

<style scoped>
/* Ana Container */
.audio-player-wrapper {
    position: fixed;
    bottom: 20px;
    right: 20px;
    z-index: 1000;
}

/* Yüzen Balonlar */
.balloon {
    position: fixed;
    pointer-events: none;
    z-index: -1;
    opacity: 0.7;
    filter: drop-shadow(0 0 5px rgba(255, 105, 180, 0.7));
}

@keyframes float {
    0% {
        transform: translateY(0) rotate(0deg);
    }

    100% {
        transform: translateY(-20px) rotate(5deg);
    }
}

/* Kalp Atışı */
.heartbeat {
    position: fixed;
    bottom: 80px;
    right: 50px;
    font-size: 30px;
    opacity: 0;
    transform: scale(0.5);
    transition: all 0.3s;
}

.heartbeat-active {
    animation: heartbeat 1.5s infinite;
}

@keyframes heartbeat {
    0% {
        transform: scale(0.8);
        opacity: 0.8;
    }

    50% {
        transform: scale(1.2);
        opacity: 1;
    }

    100% {
        transform: scale(0.8);
        opacity: 0.8;
    }
}

/* Ana Player */
.audio-player {
    background: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
    padding: 15px 20px;
    border-radius: 50px;
    display: flex;
    align-items: center;
    gap: 15px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    transition: all 0.3s;
}

.player-pulse {
    animation: pulse 2s infinite;
}

@keyframes pulse {
    0% {
        box-shadow: 0 0 0 0 rgba(255, 105, 180, 0.7);
    }

    70% {
        box-shadow: 0 0 0 15px rgba(255, 105, 180, 0);
    }

    100% {
        box-shadow: 0 0 0 0 rgba(255, 105, 180, 0);
    }
}

/* Butonlar */
.play-button {
    background: #ff6b8b;
    border: none;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    font-size: 16px;
    color: white;
    cursor: pointer;
    transition: all 0.3s;
}

.play-button:hover {
    transform: scale(1.1);
}

/* Şarkı Bilgisi */
.song-display {
    flex: 1;
    min-width: 150px;
}

.song-title {
    font-weight: bold;
    font-size: 14px;
}

.song-artist {
    font-size: 12px;
    opacity: 0.8;
}

/* Progress Bar */
.progress-bar {
    height: 4px;
    background: rgba(255, 255, 255, 0.3);
    border-radius: 2px;
    margin-top: 8px;
    cursor: pointer;
}

.progress {
    height: 100%;
    background: white;
    border-radius: 2px;
    transition: width 0.1s;
}

.playlist-button {
    background: linear-gradient(135deg, #ff6b8b, #ff9a9e);
    border: none;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    font-size: 18px;
    color: white;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 5px 15px rgba(255, 105, 180, 0.4);
    transition: all 0.3s ease;
}

.playlist-button:hover {
    transform: scale(1.15) rotate(-10deg);
    box-shadow: 0 8px 20px rgba(255, 105, 180, 0.6);
}

/* Playlist */
.playlist {
    position: absolute;
    bottom: 70px;
    right: 0;
    background: white;
    border-radius: 15px;
    padding: 10px;
    width: 200px;
    max-height: 300px;
    overflow-y: auto;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
}

.playlist div {
    padding: 8px 12px;
    border-radius: 8px;
    cursor: pointer;
}

.playlist div:hover {
    background: #ffe6eb;
}

.playlist div.active {
    background: #ff6b8b;
    color: white;
}

/* Animasyonlar */
.slide-fade-enter-active {
    transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
    transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
    transform: translateX(20px);
    opacity: 0;
}

.popup-enter-active,
.popup-leave-active {
    transition: all 0.3s ease;
}

.popup-enter-from,
.popup-leave-to {
    transform: translateY(10px);
    opacity: 0;
}
</style>