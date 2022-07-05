<template> 
    <div class="footer">
        Maciek Iwaniuk - {{ year }} © All rights reserved
    </div>
</template>

<script>
export default {
    name: 'Footer',
    data() {
        return {
            year: new Date().getFullYear()
        }
    }
}
</script>

<style lang="scss" scoped>
.footer {
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>