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

/** variables **/
$background-color: rgb(219, 217, 186);

/** set default font-size to 62,5% so 1rem = 10px **/
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Montserrat', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-size: 62,5%;
}

body {
    background-color: $background-color;
}

#app {
    display: grid;
    grid-auto-rows: 2fr 8fr 0.5fr;
    min-height: 100vh;
    text-align: center;

    /** Size of app **/
    width: 100vw;
    margin: 0 0 0 0;
    @media (min-width: 40rem) {
        width: 80vw;
        margin: 0 10vw 0 10vw;
    }
    @media (min-width: 60rem) {
        width: 60vw;
        margin: 0 20vw 0 20vw;
    }
    @media (min-width: 80rem) {
        width: 50vw;
        margin: 0 25vw 0 25vw;
    }
    @media (min-width: 100rem) {
        width: 40vw;
        margin: 0 30vw 0 30vw;
    }
}


</style>
