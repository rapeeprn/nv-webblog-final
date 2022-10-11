<template>
    <div class="push_right">
        <h1>แสดงข้อมูล</h1>
        <p>ID : {{ user.id }} </p>
        <p>Name : {{user.name}} </p>
        <p>Lastname : {{user.lastname}}</p>
        <p>Email : {{user.email}}</p>
        <p>Password : {{user.password}}</p>
        <p> 
            <button v-on:click="navigateTo('/user/edit/'+user.id)">แก้ไขข้อมูล</button>
            <button v-on:click="navigateTo('/users')"> ย้อนกลับ </button> 
        </p>
    </div>
</template>

<script>
import UsersService from '@/services/UsersService'

export default {
    data() {
        return {
            user: null,
        };
    },

    async created() {
        try {
            let userId = this.$route.params.userId;
            this.user = (await UsersService.show(userId)).data
        }catch (error) {
            console.log(error);
        }
    },
    
    methods: {
       navigateTo (route) {
           this.$router.push(route)
       },
    }
};
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