<style lang="scss">
    .logos {
        display: flex;
        justify-content: space-evenly;
        max-height: 100px;
        width: 60%;
        margin: 1em auto 1em auto;
    }
    .logos > * {
        flex-basis: fit-content;
    }
</style>

<template>
    <div class="logos">
        <slot></slot>
    </div>
</template>
