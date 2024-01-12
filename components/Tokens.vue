
<script setup>
import { computed } from 'vue'

const props = defineProps({
    format: String,
});

const tokenFormatClass = computed(() => `tokens-format-${props.format}`);
</script>

<template>
    <div :class="['Tokens', tokenFormatClass]">
        <slot></slot>
    </div>
</template>
