<template>
<div  class = "push_right">
    <h1>สร้างผู้ใช้งาน</h1>
    <form v-on:submit.prevent = "createUser">
        <p>Name : <input type="text" v-model="user.name"></p>
        <p>Lastname : <input type="text" v-model="user.lastname"></p>
        <p>Email : <input type="text" v-model="user.email"></p>
        <p>Password : <input type="text" v-model="user.password"></p>
        <p>
           <button type="submit">ตกลง</button>
           <button v-on:click="navigateTo('/users')"> ย้อนกลับ </button> </p>
        </p>
    </form>
    <hr>
    <div>
        <p>Name : {{user.name}}</p>
        <p>Lastname : {{user.lastname}}</p>
        <p>Email : {{user.email}}</p>
        <p>Password : {{user.password}}</p>
    </div>
</div>
</template>

<script>
import UsersService from '@/services/UsersService'

export default {
    data() {
        return {
            user: {
                name: '',
                lastname: '',
                email: '',
                password: '',
                status: 'active'
            }
        }
    },

    methods: {
         navigateTo (route) {
           this.$router.push(route)
       },

        async createUser() {
            try {
                await UsersService.post(this.user)
                this.$router.push({
                    name: 'users'
                })
            }catch (error) {
                console.log(error)
            }
        }
    }
}
</script>

<style scoped>
     .push_right{
    margin-top: 50px;
    margin-left: 470px;
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