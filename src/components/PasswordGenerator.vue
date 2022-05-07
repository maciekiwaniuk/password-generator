<template>
    <div class="password-generator">

        <div class="row">
            Amount <input type="number" v-model="amountOfCharacters">
        </div>

        <div class="row">
            Symbols <input type="checkbox" v-model="symbols">
        </div>

        <div class="row">
            Digits <input type="checkbox" v-model="digits">
        </div>

        <div class="row">
            Small letters <input type="checkbox" v-model="smallLetters">
        </div>

        <div class="row">
            Big letters <input type="checkbox" v-model="bigLetters">
        </div>

        <div class="row">
            Generated password: <input type="text" class="generatedPassword" v-model="generatedPassword" disabled>
        </div>

        <div class="row">
            <button class="generateButton" @click="regeneratePassword(); getSymbols();">Generate</button>
        </div>

    </div>
</template>

<script>
export default {
    name: 'PasswordGenerator',
    data() {
        return {
            amountOfCharacters: 6,
            symbols: true,
            digits: true,
            smallLetters: true,
            bigLetters: true
        }
    },
    methods: {
        /**
         * Return symbols as string
         */
        getSymbols() {
            // specials chars in ascii - (from -> to)
            // (33 -> 47), (58 -> 64), (91 -> 96), (123 -> 126)
            let symbols = '';
            for (let code = 33; code <= 47; code++) symbols += `${String.fromCharCode(code)}`;
            for (let code = 58; code <= 64; code++) symbols += `${String.fromCharCode(code)}`;
            for (let code = 91; code <= 96; code++) symbols += `${String.fromCharCode(code)}`;
            for (let code = 123; code <= 126; code++) symbols += `${String.fromCharCode(code)}`;
            return symbols;
        },
        /**
         * Returns digits as string
         */
        getDigits() {
            // digits in ascii - (48 -> 57)
            let digits = '';
            for (let code = 48; code <= 57; code++) digits += `${String.fromCharCode(code)}`;
            return digits;
        },
        /**
         * Returns small letters as string
         */
        getSmallLetters() {
            // small letters in ascii - (97 -> 122)
            let smallLetters = '';
            for (let code = 97; code <= 122; code++) smallLetters += `${String.fromCharCode(code)}`;
            return smallLetters;
        },
        /**
         * Returns big letters as string
         */
        getBigLetters() {
            // big letters in ascii - (65 -> 90)
            let bigLetters = '';
            for (let code = 65; code <= 90; code++) bigLetters += `${String.fromCharCode(code)}`;
            return bigLetters;
        },
        /**
         * Focuses regenerating password via changing amount of characters
         */
        regeneratePassword() {
            this.amountOfCharacters -= 1;
            this.amountOfCharacters += 1;
        }
    },
    computed: {
        generatedPassword() {
            let password = '',
                characters = '';

            if (this.symbols) characters += `${this.getSymbols()}`;
            if (this.digits) characters += `${this.getDigits()}`;
            if (this.smallLetters) characters = `${this.getSmallLetters()}`;
            if (this.bigLetters) characters = `${this.getBigLetters()}`;

            for (let i = 0; i < this.amountOfCharacters; i++) {
                password += characters.charAt(Math.floor(Math.random() * characters.length));
            }

            return password;
        }
    }
}
</script>

<style lang="scss" scoped>
.password-generator {
    font-size: 2rem;
    background-color: rgb(135, 107, 107);

    display: grid;
    grid-auto-columns: 1fr;
    grid-auto-rows: 1fr;

    .row {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .generatedPassword {
        width: 13rem;
        height: 3rem;
        font-size: 1.2rem;
        color: black;
    }

    .generateButton {
        width: 14rem;
        height: 4rem;
        font-size: 1.6rem;
        font-weight: 700;
        border-radius: 15px;
        border: solid black 2px;
        background-color: white;
        color: black;
    }
    .generateButton:hover {
        cursor: pointer;
        background-color: rgb(240, 233, 206);
    }

}
</style>