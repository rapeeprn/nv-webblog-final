<template>
    <div class="push_right">
        <h1> User Login </h1>
        <form v-on:submit.prevent="onLogin">
            <p>Username: <input type="text" v-model="email" /></p>
            <p>Password: <input type="password" v-model="password" /></p>
            <p><button type="submit">Login </button></p>
            <dev class = "error" v-if="error">{{error}}</dev>
        </form>
    </div>
</template>

<script>
import AuthenService from '@/services/AuthenService'

export default {
    data () {
        return {
            email: '',
            password: ''
        };
    },
    
    methods:{
        async onLogin () {
            try {
                const response = await AuthenService.login({
                    email: this.email,
                    password: this.password,
                    error: null
                });
                this.$store.dispatch('setToken',response.data.token)
                this.$store.dispatch('setUser',response.data.user)
                this.$router.push({
                   name: 'blogs'
                })
                console.log(response)
            }catch (error) {
                console.log(error)
                this.error = error.response.data.error
                this.email = ''
                this.password = ''
            }
        },
    },
}
</script>

<style scoped>
.error{
    color: red;
}
.push_right{
    margin-top: 50px;
    margin-left: 700px;
}
.nv-navbar {
  background-color: #DCDCDC;
  width: 100%;
  padding: 10px 0px 10px 0px;
}
.nv-navbar .nav {
  list-style: none;
  margin: 0;
  padding: 0;
  float: left;
}
.nv-navbar .nav li {
  float: left;
}
.nv-navbar .nav li a {
  padding: 10px;
  text-decoration: none;
  color: #778890;
  font-weight: bold;
}
.nv-navbar .nav li a:hover {
  padding: 10px;
  text-decoration: none;
  color: black;
}
.nv-navbar .nav li a.router-link-active {
  background-color: #708090;
  color: black;
}
.clearfix {
  clear: left;
}
</style>
