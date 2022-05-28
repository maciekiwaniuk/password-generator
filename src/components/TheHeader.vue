<template>
    <div class="header">
        Password generator
    </div>
</template>

<script>
export default {
    name: 'TheHeader'
}
</script>

<style lang="scss" scoped>
.header {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.9rem;
    font-weight: 700;
    
    @media (min-width: 40rem) {
        font-size: 2.4rem;

    }

    transition: ease font-size 0.2s;
}
</style>