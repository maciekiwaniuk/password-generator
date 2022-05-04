<template>
    <TheHeader/>
    <PasswordGenerator/>
    <TheFooter/>
</template>

<script>
import TheHeader from '@/components/TheHeader.vue'
import PasswordGenerator from '@/components/PasswordGenerator.vue'
import TheFooter from '@/components/TheFooter.vue'

export default {
    name: 'App',
    components: {
        TheHeader,
        PasswordGenerator,
        TheFooter
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;300;700&display=swap');

/** set default font-size to 62,5% so 1rem = 10px **/
body {
    font-family: 'Montserrat', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-size: 62,5%;
    background-color: rgb(219, 217, 186);
}

#app {
    text-align: center;
}
</style>
