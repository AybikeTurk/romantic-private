<template>
    <div v-for="heart in heartRain" :key="heart.id" class="falling-heart" :style="{
        left: heart.left + '%',
        animationDelay: heart.delay + 's',
        animationDuration: heart.duration + 's',
        fontSize: heart.size + 'px'
    }">
        {{ heart.emoji }}
    </div>

    <!-- Mektup Zarfı -->
    <div v-if="!letterOpened" class="envelope-container">
        <div class="envelope-controls">
            <button @click="skipAnimation" class="skip-btn">❌</button>
        </div>
        <div class="envelope-wrapper" @click="openLetter">
            <div class="envelope" :class="{ 'envelope-animate': showEnvelope }">
                <div class="envelope-front"></div>
                <div class="envelope-back"></div>
                <div class="envelope-flap"></div>
                <div class="heart">💌</div>
                <div class="stamp">❤️</div>
            </div>
            <div class="instruction" v-if="showEnvelope">
                <span>Mektubun için tıkla!</span>
            </div>
        </div>
    </div>

    <!-- Açılan Mektup -->
    <transition name="card">
        <div v-if="letterOpened" class="card-container">
            <div class="love-card">
                <div class="card-inner" :class="{ 'card-open': cardOpen }">
                    <div class="card-front">
                        <div class="seal" @click="cardOpen = true">✉️</div>
                        <h2>Sevgilime</h2>
                        <div class="heart-pulse">💖</div>
                    </div>
                    <div class="card-back">
                        <h3>Sevgilim,</h3>
                        <p class="message">{{ message }}</p>
                        <div class="signature">Seni seven...</div>
                        <button @click="closeCard" class="close-btn">❤️ Kapat</button>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script setup>
import { ref, onMounted, defineEmits } from 'vue';
import confetti from 'canvas-confetti';

const emit = defineEmits(['close']);
const heartRain = ref(false);

const showEnvelope = ref(false);
const letterOpened = ref(false);
const cardOpen = ref(false);
const message = ref(`

Dün seni kırdığım için çok üzgünüm. Bilmeni isterim ki, kalbini incitmek hiç istemezdim. Hatalıydım ve tüm içtenliğimle senden özür diliyorum.

Sen benim için sadece bir sevgili değil, hayatımın en güzel parçasısın. Senin gülüşünle aydınlanıyor günlerim, sesinle huzur buluyorum. Ne zaman moralim bozuk olsa, aklıma senin varlığın geliyor ve her şey bir anda güzelleşiyor.

Birlikte yaşadığımız her an, kalbimde silinmeyecek izler bırakıyor. Seni düşündükçe kendimi dünyanın en şanslı insanı gibi hissediyorum. Seni sevmek, nefes almak gibi; vazgeçilmez ve tarifsiz…

Dilerim ki kalbindeki kırgınlığı silebilirim. Seni çok seviyorum ve her zaman seveceğim.

Aşkla,`);


onMounted(() => {
    setTimeout(() => {
        showEnvelope.value = true;
    }, 500);
});

const skipAnimation = (e) => {
    e.stopPropagation();
    showEnvelope.value = false;
    setTimeout(() => {
        letterOpened.value = true;
        window.scrollTo({ top: 0, behavior: 'smooth' }); // sayfanın başına kaydır
        emit('close'); // App.vue'deki showLetter'ı false yapacak
    }, 300);
};

const startHeartRain = () => {
    heartRain.value = [];
    const heartCount = 30; // toplam kalp sayısı
    for (let i = 0; i < heartCount; i++) {
        heartRain.value.push({
            id: i,
            left: Math.random() * 100, // rastgele % sol pozisyon
            delay: Math.random() * 2,  // rastgele gecikme
            duration: 3 + Math.random() * 2, // 3-5sn arasında rastgele hız
            size: 20 + Math.random() * 20, // 20-40px arası boyut
            emoji: ['💖', '❤️', '💘', '💕', '💗'][Math.floor(Math.random() * 5)]
        });
    }
    setTimeout(() => heartRain.value = [], 5000); // 5sn sonra temizle
};

const openLetter = () => {
    showEnvelope.value = false;
    letterOpened.value = true;
    setTimeout(() => {
        cardOpen.value = true;
        startHeartRain();
    }, 500);
};


const closeCard = () => {
    cardOpen.value = false;
    setTimeout(() => {
        letterOpened.value = false;
        showEnvelope.value = true;
    }, 500);
};
</script>

<style scoped>
/* Temel Stiller */
:root {
    --pink-primary: #ff6b8b;
    --pink-dark: #ff4757;
    --pink-light: #ff9a9e;
    --cream: #fff5f5;
}

* {
    box-sizing: border-box;
}

/* Zarf Konteyneri */
.envelope-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: #000000;
    background-image: url("https://www.transparenttextures.com/patterns/asfalt-light.png");
    /* This is mostly intended for prototyping; please download the pattern and re-host for production environments. Thank you! */
    z-index: 1000;
    overflow: hidden;
}

.envelope-controls {
    position: absolute;
    top: 20px;
    right: 20px;
    z-index: 1001;
}

.skip-btn {
    background: rgba(255, 255, 255, 0.8);
    border: none;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    font-size: 16px;
    cursor: pointer;
    transition: all 0.3s;
}

.skip-btn:hover {
    background: white;
    transform: scale(1.1);
}

/* Zarf Tasarımı */
.envelope-wrapper {
    position: relative;
    cursor: pointer;
    transition: transform 0.3s;
}

.envelope {
    position: relative;
    width: 200px;
    height: 120px;
    transition: all 0.5s ease;
    filter: drop-shadow(0 10px 20px rgba(255, 105, 180, 0.3));
}

.envelope-animate {
    animation: float 3s ease-in-out infinite;
}

.envelope-front,
.envelope-back,
.envelope-flap {
    position: absolute;
    border-radius: 2px;
}

.envelope-front {
    width: 100%;
    height: 100%;
    background: var(--pink-primary);
    clip-path: polygon(0% 0%, 100% 0%, 100% 70%, 50% 100%, 0% 70%);
    z-index: 3;
}

.envelope-back {
    width: 98%;
    height: 98%;
    top: 1%;
    left: 1%;
    background: var(--pink-dark);
    clip-path: polygon(0% 70%, 50% 100%, 100% 70%, 100% 100%, 0% 100%);
    z-index: 2;
}

.envelope-flap {
    width: 98%;
    height: 50%;
    top: 0;
    left: 1%;
    background: var(--pink-light);
    clip-path: polygon(0% 0%, 50% 50%, 100% 0%);
    transform-origin: top;
    z-index: 4;
    transition: transform 0.5s;
}

.envelope-wrapper:hover .envelope-flap {
    transform: rotateX(180deg);
}

.heart {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 40px;
    z-index: 5;
    animation: pulse 1.5s infinite;
}

.stamp {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 24px;
    z-index: 6;
    transform: rotate(15deg);
    opacity: 0.9;
}

.instruction {
    margin-top: 30px;
    text-align: center;
}

.instruction span {
    display: inline-block;
    background: rgba(255, 255, 255, 0.9);
    padding: 8px 20px;
    border-radius: 50px;
    color: var(--pink-dark);
    font-weight: bold;
    animation: fadeInOut 2s infinite;
    box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
}

/* Kart Tasarımı */
.card-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background: rgba(0, 0, 0, 0.7);
    z-index: 1000;
    padding: 20px;
}

.love-card {
    perspective: 1000px;
    width: 100%;
    max-width: 400px;
    max-height: 90vh;
}

.card-inner {
    position: relative;
    width: 100%;
    min-height: 500px;
    transition: transform 0.8s;
    transform-style: preserve-3d;
}

.card-open {
    transform: rotateY(180deg);
}

.card-front,
.card-back {
    position: relative;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border-radius: 15px;
    padding: 30px;
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}

.card-front {
    background: linear-gradient(135deg, var(--pink-light) 0%, var(--cream) 100%);
    color: white;
}

.card-back {
    position: absolute;
    top: 0;
    left: 0;
    background: white;
    transform: rotateY(180deg);
    color: #333;
    overflow-y: auto;
    /* dikey kaydırma */
    max-height: 90vh;
    /* ekran yüksekliğine göre sınırla */
    -webkit-overflow-scrolling: touch;
}

.seal {
    position: absolute;
    top: -20px;
    right: -20px;
    background: white;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 30px;
    box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    z-index: 10;
}

.heart-pulse {
    font-size: 60px;
    margin: 30px 0;
    animation: heartbeat 1.5s infinite;
}

.message {
    font-size: 16px;
    line-height: 1.8;
    margin: 0px 0px;
    white-space: pre-line;
    text-align: center;
}

.signature {
    margin-top: 30px;
    font-style: italic;
    align-self: flex-end;
}

.close-btn {
    background: var(--pink-primary);
    color: black;
    border: none;
    padding: 12px 30px;
    border-radius: 50px;
    cursor: pointer;
    margin-top: 30px;
    font-size: 16px;
    transition: all 0.3s;
    box-shadow: 0 3px 10px rgba(255, 105, 180, 0.3);
}

.close-btn:hover {
    background: var(--pink-dark);
    transform: translateY(-3px);
}

.falling-heart {
    position: fixed;
    top: -50px;
    z-index: 9999;
    pointer-events: none;
    animation-name: heartFall;
    animation-timing-function: linear;
}

@keyframes heartFall {
    0% {
        transform: translateY(0);
        opacity: 1;
    }

    100% {
        transform: translateY(110vh);
        opacity: 0;
    }
}

/* Animasyonlar */
@keyframes float {
    0% {
        transform: translateY(0px) rotate(-2deg);
    }

    50% {
        transform: translateY(-20px) rotate(2deg);
    }

    100% {
        transform: translateY(0px) rotate(-2deg);
    }
}

@keyframes pulse {
    0% {
        transform: scale(1);
        opacity: 0.8;
    }

    50% {
        transform: scale(1.1);
        opacity: 1;
    }

    100% {
        transform: scale(1);
        opacity: 0.8;
    }
}

@keyframes heartbeat {
    0% {
        transform: scale(0.9);
    }

    50% {
        transform: scale(1.1);
    }

    100% {
        transform: scale(0.9);
    }
}

@keyframes fadeInOut {
    0% {
        opacity: 0.7;
        transform: scale(0.95);
    }

    50% {
        opacity: 1;
        transform: scale(1.05);
    }

    100% {
        opacity: 0.7;
        transform: scale(0.95);
    }
}

.card-enter-active,
.card-leave-active {
    transition: all 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55);
}

.card-enter-from,
.card-leave-to {
    opacity: 0;
    transform: scale(0.8) rotate(10deg);
}

.card-back::-webkit-scrollbar {
    width: 8px;
    /* scrollbar genişliği */
}

.card-back::-webkit-scrollbar-track {
    background: #ffeef8;
    /* scroll track rengi (pembe ton) */
    border-radius: 4px;
}

.card-back::-webkit-scrollbar-thumb {
    background: var(--pink-primary);
    /* scroll thumb rengi */
    border-radius: 4px;
}

.card-back::-webkit-scrollbar-thumb:hover {
    background: var(--pink-dark);
    /* hover durumunda daha koyu ton */
}

/* Responsive Tasarım */
@media (max-width: 768px) {
    .envelope {
        width: 150px;
        height: 90px;
    }

    .heart {
        font-size: 30px;
    }

    .stamp {
        font-size: 18px;
    }

    .instruction span {
        font-size: 14px;
        padding: 6px 15px;
    }

    .love-card {
        max-width: 90%;
        min-height: 400px;
    }

    .card-front,
    .card-back {
        padding: 20px;
        min-height: 400px;
    }

    .message {
        font-size: 14px;
    }

    .close-btn {
        padding: 10px 25px;
        font-size: 14px;
    }
}

@media (max-width: 480px) {
    .skip-btn {
        width: 35px;
        height: 35px;
        font-size: 14px;
    }

    .envelope {
        width: 120px;
        height: 70px;
    }

    .heart-pulse {
        font-size: 40px;
        margin: 20px 0;
    }
}
</style>