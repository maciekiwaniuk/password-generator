<template>
    <div class="strength-meter">
        <div class="bar" :class="barColorClass" ref="bar"></div>

        <div class="bar-text">{{ passwordStrengthText }}</div>
    </div>
</template>

<script>
export default {
    name: 'PasswordGeneratorStrengthMeter',
    mounted() {
        this.$refs.bar.style.setProperty('--password-strength', this.passwordStrengthTotalValue);
    },
    props: {
        generatedPassword: {
            type: String,
            validator: function (value) {
                return value.length < 1001;
            }
        }
    },
    watch: {
        generatedPassword(oldPassword, newPassword) {
            this.$refs.bar.style.setProperty('--password-strength', this.passwordStrengthTotalValue);
        }
    },
    methods: {
        stringLongerThanSpecificNumberOfChars(string, number) { return string.length >= number; },

        containsDigit(string) { return string.match(/[\d]+/) },
        containsSymbol(string) { return string.match(/[!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~]+/) },
        containsSmallLetter(string) { return string.match(/[a-z]+/) },
        containsBigLetter(string) { return string.match(/[A-Z]+/) },

        excludesRepetitions(string) { 
            let chars = '';

            for (let i = 0; i <= string.length; i++) {
                if (chars.includes(string[i])) return false;
                chars += string[i];
            }

            return true;
        },
    },
    computed: {
        /**
         * Strength of generated password from length
         */
        passwordStrengthFromLength() {
            // every 4 characters strength increases by 5
            return Math.round((this.generatedPassword.length / 4) * 5);
        },
        /**
         * Strength of generated password from containing digits
         */
        passwordStrengthFromDigits() {
            let strength = 0;
            if (this.containsDigit(this.generatedPassword)) {
                // every 5 characters when password contains digits
                // strength increases by 3
                strength += Math.round((this.generatedPassword.length / 5) * 3)
            }

            // return strength from containing digits but max value is 20
            return strength > 20 ? 20 : strength;       
        },
        /**
         * Strength of generated password from containing symbols
         */
        passwordStrengthFromSymbols() {
            let strength = 0;
            if (this.containsSymbol(this.generatedPassword)) {
                strength += Math.round((this.generatedPassword.length / 5) * 3)
            }

            return strength > 20 ? 20 : strength;      
        },
        /**
         * Strength of generated password from containing small letters
         */
        passwordStrengthFromSmallLetters() {
            let strength = 0;
            if (this.containsSmallLetter(this.generatedPassword)) {
                strength += Math.round((this.generatedPassword.length / 5) * 3)
            }

            return strength > 20 ? 20 : strength;      
        },
        /**
         * Strength of generated password from containing big letters
         */
        passwordStrengthFromBigLetters() {
            let strength = 0;
            if (this.containsBigLetter(this.generatedPassword)) {
                strength += Math.round((this.generatedPassword.length / 5) * 3)
            }

            return strength > 20 ? 20 : strength;       
        },
        /**
         * Strength of generated password from containing specific characters
         */
        passwordStrengthFromContainingSpecificCharacters() {
            let strength = 0;

            strength += this.passwordStrengthFromDigits;
            strength += this.passwordStrengthFromSymbols;
            strength += this.passwordStrengthFromSmallLetters;
            strength += this.passwordStrengthFromBigLetters;

            return strength;
        },
        /**
         * Strength of generated password as value (0 - 100+)
         */
        passwordStrengthTotalValue() {
            let strength = 0,
                passwordLength = this.generatedPassword.length;

            strength += this.passwordStrengthFromLength;
            strength += this.passwordStrengthFromContainingSpecificCharacters;

            if (passwordLength >= 8 && this.excludesRepetitions(this.generatedPassword)) strength += 10;

            if (strength >= 100) return 100;
            return strength;
        },
        /**
         * Strength of generated password as string
         */
        passwordStrengthText() {
            let strength = this.passwordStrengthTotalValue;

            if (strength < 20) return 'Very weak';
            else if (strength < 30) return 'Weak';
            else if (strength < 50) return 'Medium';
            else if (strength < 70) return 'Strong';
            else if (strength < 90) return 'Very strong';
            else return 'Unhackable';
        },
        /**
         * Password strength bar background color class
         */
        barColorClass() {
            let strength = this.passwordStrengthTotalValue;

            if (strength < 20) return 'veryWeakBackground';
            else if (strength < 30) return 'weakBackground';
            else if (strength < 50) return 'mediumBackground';
            else if (strength < 70) return 'strongBackground';
            else if (strength < 90) return 'veryStrongBackground';
            else return 'unhackableBackground';
        }
    }
}
</script>

<style lang="scss" scoped>
.strength-meter {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 1.8rem;
    height: 3rem;
    width: 20rem;
    background-color: white;
    border-radius: 1rem;
    overflow: hidden;

    @media (min-width: 40rem) {
        width: 25rem;
    }

    .bar {
        --password-strength: 0;

        position: absolute;
        left: 0;
        top: 0;
        height: 4rem;
        width: calc(var(--password-strength) * 1%);

        transition: ease width 0.5s,
                    ease background-color 0.4s;
    }

    .bar-text {
        z-index: 1;
        font-weight: 700;
    }

    .veryWeakBackground { background-color: red; }
    .weakBackground { background-color: rgb(161, 49, 49); }
    .mediumBackground { background-color: rgb(242, 173, 43); }
    .strongBackground { background-color: rgb(46, 207, 46); }
    .veryStrongBackground { background-color: rgb(27, 187, 27); }
    .unhackableBackground { background-color: rgb(7, 70, 7); }
}

</style>