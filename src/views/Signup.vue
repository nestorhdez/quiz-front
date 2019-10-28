<template>
    <div class="signup">
        <h1>Sign up</h1>
        <span v-if="error.status">{{error.message}}</span>
        <form>
            <label for="name">Name</label>
            <input v-model="user.name" type="name" id="name" placeholder="Name...">
            <label for="email">Email</label>
            <input v-model="user.email" type="email" id="email" placeholder="Email...">
            <label for="password">Password</label>
            <input v-model="user.password" type="password" id="password" placeholder="Password...">
            <button @click="signup">Sign up</button>
        </form>
        <router-link to="/login">Log in</router-link>
    </div>
</template>

<script>
export default {
    name: 'signup',
    data() {
        return{
        user: {
            name: '',
            email: '',
            password: ''
        },
        error: {
            status: false,
            message: ''
        }
        }
    },
    methods: {
        signup() {
        this.error.status = false;
        this.error.message = '';
        if(this.user.name == '' || this.user.email == '' || this.user.password == ''){
            return;
        }
        this.$axios.post(`${this.$url}/signup`, this.user)
            .then(res => localStorage.setItem('quiz', JSON.stringify(res.data) ) )
            .then(() => this.$router.replace('/') )
            .catch(err => {
                this.error.status = true;
                if(err.request.status != 400){
                    this.error.message = 'Conection error';
                    return;
                }
                const res = err.response.request.response;
                const msg = JSON.parse(res).errmsg;
                const passErr = JSON.parse(res).message;
                if(msg && msg.includes('duplicate') && msg.includes('email')) {
                    this.error.message = 'This email has already an account';
                }else if (msg && msg.includes('duplicate') && msg.includes('username')) {
                    this.error.message = 'This username already exists';
                }else if( passErr && passErr.includes('password') ){
                    this.error.message = 'The password is invalid';
                }
            });
    }
    },
    created() {
        localStorage.getItem('quiz') ? this.$router.replace('/') : '';
    }
}
</script>

<style scoped>

    h1 {
        margin-bottom: 40px;
    }

    span {
            color: #c54040;
        font-weight: 500;
        font-size: 1.2rem;
        margin-bottom: 20px;
        display: inline-block;
        }

    form {
        width: 69%;
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        text-align: left;
    }

    label, input {
        flex-basis: 100%;
    }

    label {
        margin-bottom: 10px;
        font-weight: 500;
    }

    input {
        margin-bottom: 20px;
        font-size: .9rem;
        padding: 10px 0;
        border: none;
        border-bottom: 2px solid #2c3e50;
    }

    button {
        flex-basis: 50%;
        background-color: #2c3e50;
        padding: 10px;
        margin: 0 auto;
        border: 1px solid #f9f8f8;
        color: white;
        font-size: .9rem;
        font-weight: 500;
        border-radius: 4px;
    }

    a {
        text-decoration: none;
        color: #2c3e50;
        margin-top: 20px;
        display: inline-block;
    }

</style>