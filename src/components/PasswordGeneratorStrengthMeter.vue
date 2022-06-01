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
        this.$refs.bar.style.setProperty('--password-strength', this.passwordStrengthValue);
    },
    props: {
        generatedPassword: {
            type: String,
            validator(value) {
                return value.length < 1001;
            }
        }
    },
    watch: {
        generatedPassword(oldPassword, newPassword) {
            this.$refs.bar.style.setProperty('--password-strength', this.passwordStrengthValue);
        }
    },
    methods: {
        longerThan8Chars(string) { return string.length > 8; },
        longerThan16Chars(string) { return string.length > 16 },

        containsDigit(string) {  },
        containsSymbol(string) {  },
        containsSmallLetter(string) {  },
        containsBigLetter(string) {  },

        excludesRepetitions(string) {  },
    },
    computed: {
        /**
         * Strength of generated password as value (0 - 100+)
         */
        passwordStrengthValue() {
            let strength = 0;

            if (this.longerThan8Chars(this.generatedPassword)) strength += 10;
            if (this.longerThan16Chars(this.generatedPassword)) strength += 10;

            if (strength >= 100) return 100;
            return strength;
        },
        /**
         * Strength of generated password as string
         */
        passwordStrengthText() {
            let strength = this.passwordStrengthValue;

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
            let strength = this.passwordStrengthValue;

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
    width: 25rem;
    background-color: white;
    border-radius: 1rem;
    overflow: hidden;

    .bar {
        --password-strength: 0;

        position: absolute;
        left: 0;
        top: 0;
        height: 4rem;
        width: calc(var(--password-strength) * 1%);

        transition: ease width 0.5s;
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