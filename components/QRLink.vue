<style lang="scss">
.qrlink {
    margin-right: auto;
    margin-left: auto;
    display: flex;
    flex-direction: column;
    align-items: center;
}
.qrlink > a.qrlink-a {
    text-decoration: none;
    border: none;
}
.qrlink > a.qrlink-a:hover {
    border: none;
}
</style>

<script setup>
import { ref, onMounted } from "vue";
import QRCode from "qrcode";

const props = defineProps({
    url: String,
});
const canvas = ref("");
onMounted(() => {
    QRCode.toCanvas(
        canvas.value,
        props.url,
        error => {
            if (error) {
                console.error(`Unable to create QR Code for ${props.url}`);
                console.error(error);
            }
        }
    );
});
</script>

<template>
    <div class="qrlink">
        <a :href="url" class="qrlink-a">
            <canvas ref="canvas"></canvas>
        </a>
    </div>
</template>
