<template>
    <div> 
        <div class="login-card">
             <div class="content-right">
                <h3 class="content-right__text-bold"> create an account </h3>
                <transition name="blank">
                    <h5 class="error" v-if="isError"> please, do not leave any fields blank </h5>
                </transition>
                <form class="auth-form" @submit.prevent="register()">
                    <input type="text" placeholder="name" class="input" v-model="profile">
                    <input type="email" placeholder="email" class="input" v-model="email">
                    <input type="password" placeholder="password" class="input" v-model="password">
                    <button class="btn-auth btn-auth-colored">sign up </button>
                </form>
            </div>
            <div class="content-left">
                <h3 class="content-left__text-bold"> Hello, Friend </h3>
                <p class="content-left__text-small">enter your personal details and start your polling now</p>
                <button class="btn-auth btn-auth-transparent" @click="$router.push({name:'login'})">sign in </button>
            </div>
        </div>
    </div>
</template>


<script>
import {login, signup } from '../api'
import { setAuth } from '../helpers'
export default {
    data() {
        return  {
            email: '',
            password: '',
            profile: '',
            isError: false
        }
    },
    methods: {
        async register() {
            let credentials = {email:this.email, password:this.password, profile:this.profile};
            try {
                let signupData = await signup(credentials)
                let loginData = await login(credentials)
                setAuth(loginData)
                this.$router.push({name: 'pollContainer'})
            }
            catch(err) {
                setTimeout(()=> this.isError = false, 3000)
                 this.isError = true;
            } 
        }
    }
}    
</script>






<style >
.error {
    padding: 4rem;
    border-radius: 4px;
    background-color: #fce4ec;
    color: #d81b60;
    margin: 2rem 0;
    font-weight: 200;
    font-size: 1rem;
}

.blank-enter-active, .blank-leave-active {
    transition: all 1s;
}
.blank-enter, .blank-leave-to {
    opacity: 0;
    transform: translateY(-2rem)
}
.blank-leave-active {
    position: absolute;
}
</style>

