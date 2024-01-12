<style lang="scss">
    span.Token {
        padding: 0.2rem;
        margin: 0 0.2rem 0 0.2rem;
        border: 0.2rem transparent solid;
        display: inline-block;
    }

    .tokens-format-cut {
        .token-type-Ident,
        .token-type-Punct,
        .token-type-Literal,
        .token-type-Delimiter {
            border-bottom-color: brown;
        }
    }

    .tokens-format-decorate {
        .token-type-Group {
            border-color: black black transparent black;
        }
        .token-type-Ident {
            border-bottom-color: blue;
        }
        .token-type-Punct {
            border-bottom-color: black;
        }
        .token-type-Literal {
            border-bottom-color: brown;
        }
        .token-type-Delimiter {
            border-bottom-color: green;
        }
    }
</style>

<script setup>
import { computed } from 'vue'

const props = defineProps({
    type: String,
});

const tokenTypeClass = computed(() => `token-type-${props.type}`);
</script>

<template>
    <span :class="['Token', tokenTypeClass]">
        <slot></slot>
    </span>
</template>
