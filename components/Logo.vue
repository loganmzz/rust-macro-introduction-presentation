<style lang="scss">
    .logo > img {
        height: 100%;
        margin: auto;
    }
</style>

<script setup>
import { computed } from 'vue'

const props = defineProps({
    name: String,
});

const imgSrc = computed(() => `/img/logos/${props.name}.png`);
</script>

<template>
    <div class="logo"><img :src="imgSrc"></div>
</template>
