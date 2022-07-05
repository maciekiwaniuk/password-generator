<template>
    <div class="password-generator">

        <div class="row">
            <label for="numberOfCharacters">Number</label>
            <button
                class="number-button increase-number-button"
                @click="changeNumberOfCharacters(-1);"
            >-</button>

            <input
                class="number-of-characters-text-field"
                type="number" 
                v-model="numberOfCharacters"
                @change="validateNumberOfCharacters();"
                min="0"
                pattern="\d*"
            >

            <button
                class="number-button decrease-number-button"
                @click="changeNumberOfCharacters(1);"
            >+</button>
        </div>

        <div class="row">
            <label for="symbols">Symbols</label>
            <input 
                class="checkbox"
                type="checkbox" 
                v-model="symbols"
            >
        </div>

        <div class="row">
            <label for="digits">Digits</label>
            <input 
                class="checkbox"
                type="checkbox" 
                v-model="digits"
            >
        </div>

        <div class="row">
            <label for="smallLetters">Small letters</label>
            <input 
                class="checkbox"
                type="checkbox" 
                v-model="smallLetters"
            >
        </div>

        <div class="row">
            <label for="bigLetters">Big letters</label>
            <input 
                class="checkbox"
                type="checkbox" 
                v-model="bigLetters"
            >
        </div>

        <div class="row" :class="{ greyedText: !possibilityToGeneratePasswordFromUniqueCharacters }">
            <label for="excludeRepetitions">Exclude repetitions</label>
            <input 
                class="checkbox"
                type="checkbox" 
                @click="handleExcludeRepetitionsClick($event);"
                v-model="excludeRepetitions"
            >
        </div>

        <div class="row">
            <label for="generatedPassword">Password</label>
            <input 
                type="text" 
                class="generated-password" 
                v-model="generatedPassword" 
                @click="copyToClipboard(generatedPassword);"
                readonly
            >
        </div>

        <div class="row">
            <PasswordGeneratorStrengthMeter :generatedPassword="generatedPassword"/>
        </div>

        <div class="row">
            <button class="generate-button" @click="regeneratePassword();">Generate</button>
        </div>

    </div>
</template>

<script>
import PasswordGeneratorStrengthMeter from '@/components/PasswordGeneratorStrengthMeter.vue';

export default {
    name: 'PasswordGenerator',
    components: {
        PasswordGeneratorStrengthMeter
    },  
    beforeCreate() {
        // Creates and saves default value of options to local storage if values weren't set yet
        if (localStorage.getItem('numberOfCharacters') === null) localStorage.setItem('numberOfCharacters', 6);
        if (localStorage.getItem('symbols') === null) localStorage.setItem('symbols', true);
        if (localStorage.getItem('digits') === null) localStorage.setItem('digits', true);
        if (localStorage.getItem('smallLetters') === null) localStorage.setItem('smallLetters', true);
        if (localStorage.getItem('bigLetters') === null) localStorage.setItem('bigLetters', true);
        if (localStorage.getItem('excludeRepetitions') === null) localStorage.setItem('excludeRepetitions', false);
    },
    data() {
        return {
            numberOfCharacters: localStorage.getItem('numberOfCharacters'),
            symbols: localStorage.getItem('symbols') == 'true',
            digits: localStorage.getItem('digits') == 'true',
            smallLetters: localStorage.getItem('smallLetters') == 'true',
            bigLetters: localStorage.getItem('bigLetters') == 'true',
            excludeRepetitions: localStorage.getItem('excludeRepetitions') == 'true'
        }
    },
    watch: {
        numberOfCharacters(value) {
            // zero or more digits
            let regex = /\d+/,
                number = value;

            // if number doesn't contain only digits or is too big
            if (!regex.test(number) || number > 1000) {
                number = this.getValidatedNumberOfCharacters(number);
                this.numberOfCharacters = number;
            }

            localStorage.setItem('numberOfCharacters', number);
        },
        symbols(value) {
            localStorage.setItem('symbols', value);
        },
        digits(value) {
            localStorage.setItem('digits', value);
        },
        smallLetters(value) {
            localStorage.setItem('smallLetters', value);
        },
        bigLetters(value) {
            localStorage.setItem('bigLetters', value);
        },
        excludeRepetitions(value) {
            localStorage.setItem('excludeRepetitions', value);
        }
    },  
    methods: {
        /**
         * Returns validated number of characters
         */
        getValidatedNumberOfCharacters(numberOfCharacters) {
            // if number of characters is passed as a string - return last saved value of number
            if (typeof numberOfCharacters == 'string') return localStorage.getItem('numberOfCharacters')

            return numberOfCharacters > 1000 ? 1000 : numberOfCharacters;            
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
         * Modifies number of characters
         */
        changeNumberOfCharacters(number) {
            if (this.numberOfCharacters == 0 && number == -1) return;
            if (this.numberOfCharacters == 1000 && number == 1) return;


            if (this.numberOfCharacters == '') {
                this.numberOfCharacters = 0;
                return;
            }
            this.numberOfCharacters = parseInt(this.numberOfCharacters) + number;
        },
        /**
         * Focuses regenerating password via changing number of characters
         */
        regeneratePassword() {
            this.numberOfCharacters -= 1;
            this.numberOfCharacters += 1;
        },
        /**
         * Copies password to clipboard
         */
        copyToClipboard(password) {
            navigator.clipboard.writeText(password);

            this.$swal({
                title: 'Password has been copied',
                icon: 'success',
                confirmButtonText: 'OK'
            });
        },
        /**
         * Handles click on exclude repetitions option
         */
        handleExcludeRepetitionsClick(event) {
            if (!this.possibilityToGeneratePasswordFromUniqueCharacters) {
                // block possibility to select checkbox
                event.preventDefault();
            }
        }
    },
    computed: {
        /**
         * String of characters possible to generate password from
         */
        charactersToGeneratePassword() {
            let characters = '';

            if (this.symbols) characters += `${this.getSymbols()}`;
            if (this.digits) characters += `${this.getDigits()}`;
            if (this.smallLetters) characters += `${this.getSmallLetters()}`;
            if (this.bigLetters) characters += `${this.getBigLetters()}`;

            return characters;
        },
        /**
         * Possibility to generate password from unique characters
         */
        possibilityToGeneratePasswordFromUniqueCharacters() {
            return this.charactersToGeneratePassword.length >= this.numberOfCharacters;
        },
        /**
         * Final generated password
         */
        generatedPassword() {
            let password = '',
                generatedCharacters = 0;

            while (generatedCharacters != this.numberOfCharacters) {
                let generatedRandomCharacter = this.charactersToGeneratePassword.charAt(Math.floor(Math.random() * this.charactersToGeneratePassword.length));

                // if (exclude repetitions option is selected) && (password already contains generated character) &&
                //    (specific number of characters is possible to generated only from unique characters)
                if (this.excludeRepetitions &&
                    password.includes(generatedRandomCharacter) && this.possibilityToGeneratePasswordFromUniqueCharacters) continue;

                generatedCharacters += 1;
                password += generatedRandomCharacter;
            }

            return password;
        }
    }
}
</script>

<style lang="scss" scoped>
$disabled-color: rgb(182, 179, 179);

/** Hide arrows inside number input **/
input[type='number'] { -moz-appearance: textfield; }
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button { -webkit-appearance: none; }

.password-generator {
    font-size: 1.6rem;

    display: grid;
    grid-auto-columns: 1fr;
    grid-auto-rows: 1fr;

    @media (min-width: 40rem) {
        font-size: 2rem;
    }

    .row {
        display: flex;
        justify-content: center;
        align-items: center;

        color: black;
        transition: ease color 0.7s,
                    ease font-size 0.2s;
    }
    .row:hover {
        font-size: 110%;

        transition: ease font-size 0.2s;
    }

    .generated-password {
        width: 13rem;
        height: 3rem;
        margin-left: 0.2rem;
        font-size: 1.2rem;
        padding-left: 0.5rem;
        border-radius: 1rem;
        outline: none;

        border: solid black 0.2rem;
        background-color: white;
        color: black;

        transition: ease color 0.7s,
                    ease background-color 0.7s,
                    ease border 0.7s;
    }
    .generated-password:hover {
        cursor: pointer;

        color: white;
        background-color: black;
        border: solid white 0.2rem;

        transition: ease color 0.7s,
                    ease background-color 0.7s,
                    ease border 0.7s;
    }

    .generate-button {
        width: 14rem;
        height: 4rem;
        font-size: 1.6rem;
        font-weight: 700;
        border-radius: 1.5rem;

        border: solid black 0.2rem;
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
    
    .number-of-characters-text-field {
        width: 4rem;
        height: 1.8rem;
        margin-top: 0.2rem;
        font-size: 1.3rem;
        border: solid black 0.15rem;
        text-align: center;
        outline: none;
    }
    .number-button {
        width: 1.5rem;
        height: 1.8rem;
        margin: 0;
        margin-top: 0.2rem;
        font-weight: 700;
        font-size: 1rem;

        background-color: white;
        color: black;

        transition: ease color 0.4s,
                    ease background-color 0.4s,
                    ease border 0.4s;
    }
    .number-button:hover {
        cursor: pointer;
        
        color: white;
        background-color: black;

        transition: ease color 0.4s,
                    ease background-color 0.4s,
                    ease border 0.4s;
    }
    .increase-number-button {
        border-top-left-radius: 0.5rem;
        border-bottom-left-radius: 0.5rem;
        margin-left: 0.2rem;
    }
    .decrease-number-button {
        border-top-right-radius: 0.5rem;
        border-bottom-right-radius: 0.5rem;
    }

    .greyedText {
        color: $disabled-color !important;
        text-decoration: line-through;

        transition: ease color 0.7s;
    }

}
</style>