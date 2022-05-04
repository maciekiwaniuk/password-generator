<template> 
    <div class="footer">
        Maciek Iwaniuk - {{ year }} © All rights reserved
    </div>
</template>

<script>
export default {
    name: 'TheFooter',
    data() {
        return {
            year: new Date().getFullYear()
        }
    }
}
</script>

<style lang="scss" scoped>
.footer {
    width: 100%;
    min-height: 10vh;
    font-size: 1rem;
}
</style>