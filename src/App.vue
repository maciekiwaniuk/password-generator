<template>
    <Header/>
    <PasswordGenerator/>
    <Footer/>
</template>

<script>
import Header from '@/components/Header.vue'
import PasswordGenerator from '@/components/PasswordGenerator.vue'
import Footer from '@/components/Footer.vue'

export default {
    name: 'App',
    components: {
        Header,
        PasswordGenerator,
        Footer
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;300;700&display=swap');

/** Variables **/
$background-color: black;
$border-color: white;
$app-bg-color: rgb(124, 111, 217);

/** Set default font-size to 62,5% so 1rem = 10px **/
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Montserrat', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

body {
    background-color: $background-color;
}

#app {
    display: grid;
    grid-auto-rows: 1fr 6fr 0.5fr;
    min-height: 100vh;
    text-align: center;

    background-color: $app-bg-color;

    /** Size of app **/
    width: 100vw;
    margin: 0 0 0 0;
    @media (min-width: 40rem) {
        grid-auto-rows: 1fr 6fr 0.3fr;
        width: 80vw;
        margin: 0 10vw 0 10vw;

        border-left: solid $border-color 0.2rem;
        border-right: solid $border-color 0.2rem;
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
