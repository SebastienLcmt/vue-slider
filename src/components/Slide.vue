<template>
    <div class="slide">
        <transition name="slide">
            <slot />
        </transition>
    </div>
</template>

<script>
    export default {
        name: 'Slide',
    }
</script>

<style scoped>
    .slide-enter-active, /* Transition entre enter from et to*/
    .slide-leave-active { /* Transition entre leave from et to*/
        transition: opacity 0.6s ease-in-out;
    }

    .slide-enter-from, /* Opacity à 0 au début */
    .slide-leave-to {
        opacity: 0;
    }

    .slide-enter-to,
    .slide-leave-from {
        opacity: 1;
    }

</style>