<template>
    <div class="app">
        <product-display />
    </div>
</template>

<script>
import ProductDisplay from './views/ProductDisplay.vue';

export default {
    name: 'App',
    components: {
        ProductDisplay,
    },
};
</script>
