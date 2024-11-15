<!-- <template>
    <div>
        <h2>Sign up</h2>
        <form @submit.prevent="register">
            <div>
                <label for="registerName">Name:</label>
                <input type="text" id="registerName" v-model="registerName">
            </div>
            <div>
                <label for="registerAge">Age:</label>
                <input type="text" id="registerAge" v-model="registerAge">
            </div>
            <button type="submit">Sign up</button>
        </form>

        <h2>Sign in</h2>
        <form @submit.prevent="login">
            <div>
                <label for="loginName">Name:</label>
                <input type="text" id="loginName" v-model="loginName">
            </div>
            <button type="submit">Sign in</button>
        </form>
    </div>
</template> -->

<template>
    <div>
        <h2>Sign up</h2>
        <form @submit.prevent="register">
            <div>
                <label for="registerName">Name:</label>
                <input type="text" id="registerName" v-model="registerName">
            </div>
            <div>
                <label for="registerAge">Age:</label>
                <input type="text" id="registerAge" v-model="registerAge">
            </div>
            <button type="submit">Sign up</button>
        </form>

        <h2>Sign in</h2>
        <form @submit.prevent="login">
            <div>
                <input v-model="loginName" placeholder="Enter your name" />
                <button @click="login">Login</button>
                <p>{{ message }}</p> <!-- Message displayed here -->
            </div>
        </form>
    </div>
</template>
    
<script>
import axios from 'axios';

export default {
    name: 'AuthComponent',

    data() {
        return {
            registerName: '',
            registerAge: '',
            loginName: '',
            message: '',
        };
    },
    methods: {
        async register() {
            try {
                const response = await axios.post('http://127.0.0.1:8000/users/', {
                    name: this.registerName,
                    age: Number(this.registerAge),
                });

                console.log('Register success: ' + response.data);             
            } catch (error) {
                console.error('Registration failed:', error);
            }
        },
        async login() {
            try {
                await axios.get(`http://127.0.0.1:8000/users/${this.loginName}`);

                this.message = 'You are authorized';
                this.$emit('handleAuth');
            } catch (error) {
                // Перевіряємо, чи помилка пов'язана з відповіддю сервера та має статус 404
                if (error.response && error.response.status === 404) {
                // Користувача не знайдено
                    this.message = 'This user does not exist in the system. Try again or register yourself.';
                } else if (error.response) {
                // Якщо є інші коди помилок від сервера, окрім 404
                    this.message = `Server error: ${error.response.status}. Please try again later.`;
                } else {
                // Якщо це помилка мережі або інша непередбачена помилка
                    this.message = 'Network error. Please check your connection and try again.';
                }
            } 
        },
    },
};
</script>
    
<style>

</style>
    