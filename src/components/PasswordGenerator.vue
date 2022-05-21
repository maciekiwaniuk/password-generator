<template>
    <div class="password-generator">

        <div class="row">
            <label for="amountOfCharacters">Amount</label>
            <input
                class="amount-of-characters-text-field"
                type="number" 
                @input="saveOptionToLocalStorage('amountOfCharacters', amountOfCharacters); checkAmountOfCharacters();" 
                v-model="amountOfCharacters"
            >
        </div>

        <div class="row">
            <label for="symbols">Symbols</label>
            <input 
                class="checkbox"
                type="checkbox" 
                @change="saveOptionToLocalStorage('symbols', symbols);" 
                v-model="symbols"
            >
        </div>

        <div class="row">
            <label for="digits">Digits</label>
            <input 
                class="checkbox"
                type="checkbox" 
                @change="saveOptionToLocalStorage('digits', digits);" 
                v-model="digits"
            >
        </div>

        <div class="row">
            <label for="smallLetters">Small letters</label>
            <input 
                class="checkbox"
                type="checkbox" 
                @change="saveOptionToLocalStorage('smallLetters', smallLetters);" 
                v-model="smallLetters"
            >
        </div>

        <div class="row">
            <label for="bigLetters">Big letters</label>
            <input 
                class="checkbox"
                type="checkbox" 
                @change="saveOptionToLocalStorage('bigLetters', bigLetters);" 
                v-model="bigLetters"
            >
        </div>

        <div class="row">
            <label for="similarCharacters">Exclude similar characters</label>
            <input 
                class="checkbox"
                type="checkbox" 
                @change="saveOptionToLocalStorage('similarCharacters', similarCharacters);" 
                v-model="similarCharacters"
            >
        </div>

        <div class="row">
            <label for="generatedPassword">Generated password</label>
            <input 
                type="text" 
                class="generated-password" 
                v-model="generatedPassword" 
                disabled
            >
        </div>

        <div class="row">
            <button class="generate-button" @click="regeneratePassword();">Generate</button>
        </div>

    </div>
</template>

<script>
export default {
    name: 'PasswordGenerator',
    beforeCreate() {
        // Creates and saves default value of options to local storage if values weren't set yet
        if (localStorage.getItem('amountOfCharacters') === null) localStorage.setItem('amountOfCharacters', 6);
        if (localStorage.getItem('symbols') === null) localStorage.setItem('symbols', true);
        if (localStorage.getItem('digits') === null) localStorage.setItem('digits', true);
        if (localStorage.getItem('smallLetters') === null) localStorage.setItem('smallLetters', true);
        if (localStorage.getItem('bigLetters') === null) localStorage.setItem('bigLetters', true);
        if (localStorage.getItem('similarCharacters') === null) localStorage.setItem('similarCharacters', false);
    },
    data() {
        return {
            amountOfCharacters: localStorage.getItem('amountOfCharacters'),
            symbols: localStorage.getItem('symbols') == 'true',
            digits: localStorage.getItem('digits') == 'true',
            smallLetters: localStorage.getItem('smallLetters') == 'true',
            bigLetters: localStorage.getItem('bigLetters') == 'true',
            similarCharacters: localStorage.getItem('similarCharacters') == 'true'
        }
    },
    methods: {
        /**
         * Checks amount of characters
         */
        checkAmountOfCharacters() {
            if (this.amountOfCharacters > 1000) {
                this.amountOfCharacters = 1000

                this.saveOptionToLocalStorage(
                    'amountOfCharacters',
                    1000
                );
            }
        },
        /**
         * Returns symbols as string
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
        },
        /**
         * Saves selected option to local storage
         */
        saveOptionToLocalStorage(name, value) {
            localStorage.setItem(name, value);
        }
    },
    computed: {
        generatedPassword() {
            let password = '',
                characters = '';

            if (this.symbols) characters += `${this.getSymbols()}`;
            if (this.digits) characters += `${this.getDigits()}`;
            if (this.smallLetters) characters += `${this.getSmallLetters()}`;
            if (this.bigLetters) characters += `${this.getBigLetters()}`;

            let amountOfPossibleUniqueCharacters = characters.length,
                generatedCharacters = 0;

            while (generatedCharacters != this.amountOfCharacters) {
                let generatedRandomCharacter = characters.charAt(Math.floor(Math.random() * characters.length));

                // check if excluding similar characters option is selected, password already contains generated character
                // and selected amount of characters is possible to generate unique characters
                if (this.similarCharacters &&
                    password.includes(generatedRandomCharacter) && amountOfPossibleUniqueCharacters >= this.amountOfCharacters) continue;

                generatedCharacters += 1;
                password += generatedRandomCharacter;
            }

            return password;
        }
    }
}
</script>

<style lang="scss" scoped>
.password-generator {
    font-size: 2rem;

    display: grid;
    grid-auto-columns: 1fr;
    grid-auto-rows: 1fr;

    .row {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .generated-password {
        width: 13rem;
        height: 3rem;
        margin-left: 0.2rem;
        font-size: 1.2rem;
        color: black;
    }

    .generate-button {
        width: 14rem;
        height: 4rem;
        font-size: 1.6rem;
        font-weight: 700;
        border-radius: 1.5rem;

        border: solid black 0.2rem;;
        background-color: white;
        color: black;

        transition: ease color 0.7s,
                    ease background-color 0.7s,
                    ease border 0.7s;
    }
    .generate-button:hover {
        cursor: pointer;

        color: white;
        background-color: black;
        border: solid white 0.2rem;

        transition: ease color 0.7s,
                    ease background-color 0.7s,
                    ease border 0.7s;
    }

    .checkbox {
        margin-left: 0.3rem;
        margin-top: 0.3rem;
        width: 1.1rem;
        height: 1.1rem;
    }
    .checkbox:hover {
        cursor: pointer;
    }
    
    .amount-of-characters-text-field {
        width: 5rem;
        height: 1.5rem;
        margin-top: 0.2rem;
        margin-left: 0.2rem;
        font-size: 1.3rem;
    }

}
</style>