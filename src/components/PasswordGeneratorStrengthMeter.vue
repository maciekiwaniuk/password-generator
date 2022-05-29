<template>
    <div class="strength-meter">
        <div
            :class="{ 
                veryWeakText: passwordStrength == 'veryWeak',
                weakText: passwordStrength == 'weak',
                mediumText: passwordStrength == 'medium',
                strongText: passwordStrength == 'strong',
                veryStrongText: passwordStrength == 'veryStrong',
                unhackableText: passwordStrength == 'unhackable'
            }"
        >
            Weak - {{ generatedPassword }}
        </div>
    </div>
</template>

<script>
export default {
    name: 'PasswordGeneratorStrengthMeter',
    data() {
        return {
            
        }
    },
    props: {
        generatedPassword: {
            type: String,
            validator(value) {
                return value.length < 1001;
            }
        }
    },
    methods: {

    },
    computed: {
        /**
         * Strength of generated password in string
         */
        passwordStrength() {
            return 'veryWeak';
        },
        /**
         * Text value of generated password strength
         */
        passwordStrengthText() {
            if (this.passwordStrength == 'veryWeak') return 'Very weak';
            else if (this.passwordStrength == 'weak') return 'Weak';
            else if (this.passwordStrength == 'medium') return 'Medium';
            else if (this.passwordStrength == 'strong') return 'Strong';
            else if (this.passwordStrength == 'veryStrong') return 'Very strong';
            else if (this.passwordStrength == 'unhackable') return 'Unhackable';
            else return '??';
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
    border: solid black 0.2rem;
    border-radius: 1rem;
}

</style>