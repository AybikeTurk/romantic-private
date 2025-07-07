<template>
    <transition-group name="fade-up" tag="div" class="card-container">
        <div v-for="(note, index) in notes" :key="index" class="card" :class="{ flipped: flippedCards[index] }"
            @click="flipCard(index)">
            <div class="card-inner">
                <div class="card-front">💌</div>
                <div class="card-back">{{ note }}</div>
            </div>
        </div>
    </transition-group>

</template>

<script setup>
import { ref } from 'vue';
import confetti from 'canvas-confetti';

const notes = [
    "Sana her baktığımda kalbim yeniden çarpıyor 💗",
    "Sensizlik kelimelerin yetmediği bir eksiklik...",
    "Senin gülüşün, günümün en güzel kısmı 💕",
    "Birlikte geçirdiğimiz her an, en değerli hazinem.",
    "Sadece bir mesajın bile yüzümü güldürmeye yeter 😌",
    "Varlığın, karanlık günlerimin en parlak ışığı ✨",
    "Kalbim senin adını her atışta fısıldıyor 🔐",
    "Senin yanında geçen zaman, en güzel anılarıma dönüşüyor ⏳",
    "Gözlerine baktığımda her şey anlam kazanıyor 🌌",
    "Birlikte kurduğumuz hayaller, en değerli servetim 💭",
    "Seninle her gün, özel bir güne dönüşüyor 🎁",
    "Bir ömür değil, sonsuzluk bile seninle az gelir ♾️",
    "Kalbimde senin için ayrılmış bir köşe değil; koca bir dünya var 🌍",
    "Ellerin ellerimdeyken dünya durabilir, umurumda olmaz 🫶",
    "Geceleri yıldızlara değil, senin adını fısıldıyorum ⭐",
    "Uyurken bile aklımdasın… Rüyalarımda bile sen varsın 🌙",
    "Senin sevginden başka hiçbir şey beni bu kadar saramaz 💞",
    "En güzel şarkı bile senin sesin kadar huzur veremez 🎶",
    "İyi ki varsın… Her şeyden öte, iyi ki 'biz' varız 💫",
    "Senin varlığın, hayatımın en anlamlı şiiri oldu 📝"
];


const flippedCards = ref(notes.map(() => false));
const flipCard = (index) => {
    flippedCards.value[index] = !flippedCards.value[index];

    // Eğer kart yeni çevrildiyse konfeti başlat
    if (flippedCards.value[index]) {
        confetti({
            particleCount: 50,
            spread: 70,
            origin: { y: 0.6 },
            shapes: ['star'],
            scalar: 0.8,
            colors: ['#FFD700', '#FF69B4', '#ffffff'],
        });
    }
};

</script>

<style scoped>
.card-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 25px;
    padding: 40px 20px;
}

.card {
    width: 200px;
    height: 150px;
    perspective: 1000px;
    cursor: pointer;
}

.card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s;
    transform-style: preserve-3d;
    border-radius: 15px;
    box-shadow: 0 5px 15px rgba(255, 105, 180, 0.2);
}

.card.flipped .card-inner {
    transform: rotateY(180deg);
}

.card-front,
.card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 15px;
    font-size: 18px;
    padding: 20px;
}

.card-front {
    background: #ffe6eb;
    font-size: 40px;
}

.card-back {
    background: #fff0f6;
    transform: rotateY(180deg);
    color: #d63384;
    font-weight: bold;
    font-family: 'Dancing Script', cursive;
    font-size: 20px;
}

.fade-up-enter-from {
    opacity: 0;
    transform: translateY(20px);
}

.fade-up-enter-active {
    transition: all 0.6s ease;
}

.fade-up-enter-to {
    opacity: 1;
    transform: translateY(0);
}

/* 📱 Mobil Uyum */
@media (max-width: 600px) {
    .card {
        width: 70vw;
        height: 140px;
    }

    .card-front {
        font-size: 50px;
    }

    .card-back {
        font-size: 16px;
        padding: 15px;
    }

    .card-container {
        padding: 20px 10px;
        gap: 15px;
    }
}
</style>
