<template>
    <div class="password-generator">

        <div class="row">
            Amount <input type="number" v-model="amountOfCharacters">
        </div>

        <div class="row">
            Symbols <input type="checkbox" v-model="symbols">
        </div>

        <div class="row">
            Your generated password: <input type="text" v-model="generatedPassword" disabled>
        </div>

        <div class="row">
            <button @click="generatePassword()">Generate</button>
        </div>

    </div>
</template>

<script>
export default {
    name: 'PasswordGenerator',
    data() {
        return {
            generatedPassword: '',
            amountOfCharacters: 6,
            symbols: true
        }
    },
    methods: {
        generatePassword() {
            let password = '';
            let characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
            let charactersLength = characters.length;
            for (let i = 0; i < this.amountOfCharacters; i++) {
                password += characters.charAt(Math.floor(Math.random() * charactersLength));
            }

            this.generatedPassword = password;
        }
    },
    beforeMount() {
        this.generatePassword();
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

}
</style>