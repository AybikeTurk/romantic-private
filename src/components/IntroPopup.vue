<template>
    <transition name="fade">
        <div class="intro-popup" v-if="visible">
            <div class="popup-content">
                <p>{{ message }}</p>
                <button @click="close">Devam Et 💖</button>
            </div>
        </div>
    </transition>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const visible = ref(true);
const message = `Hoş geldin canım... 🌙  
Bu küçük siteyi sadece seni gülümsetmek için hazırladım.  
Keyifle gezmeni dilerim 💕`;

const close = () => {
    visible.value = false;
};

onMounted(() => {
    // Otomatik kapanması için(opsiyonel):
    setTimeout(() => close(), 7000);
});
</script>

<style scoped>
.intro-popup {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
    padding: 20px;
    box-sizing: border-box;
}

.popup-content {
    background: #fff0f6;
    padding: 30px;
    border-radius: 20px;
    text-align: center;
    box-shadow: 0 10px 30px rgba(255, 105, 180, 0.3);
    font-size: 18px;
    white-space: pre-line;
    max-width: 600px;
    width: 100%;
}

/* Buton */
button {
    margin-top: 20px;
    padding: 10px 20px;
    background: #ff6b8b;
    border: none;
    border-radius: 10px;
    color: white;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s ease;
}

button:hover {
    background: #ff3b6b;
    transform: scale(1.05);
}

/* Fade animation */
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

/* 📱 Mobil uyumluluk */
@media (max-width: 480px) {
    .popup-content {
        font-size: 16px;
        padding: 20px;
    }

    button {
        font-size: 14px;
        padding: 8px 16px;
    }
}
</style>
