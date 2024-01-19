<template>
    <div class="page-my-account">
        <h1 class="title">My Account</h1>
        <div class="button">
            <router-link to="/dashboard/my-account/edit-team" class="button is-loght">Edit Team</router-link>
            <button @click="logout()" class="button is-danger">Log Out</button>
        </div>
       
    </div>
    
</template>

<script>
import axios from 'axios';

    export default {
    name: "MyAccount",
    methods: {
        logout() {
            axios
                .post("/api/v1/token/logout/")
                .then( response => {
                    axios.defaults.headers.common["Authorization"] = ""

                    localStorage.removeItem("token")

                    this.$store.commit("removeToken")

                    this.$router.push("/")
                })
                .catch(error => {
                    if (error.response) {
                        for (const property in error.response.data){
                            this.errors.push('${property}: ${error.response.data[property]}')
                        }
                        console.log(JSON.stringify(error.response.data))
                    }else if (error.message) {
                        console.log(JSON.stringify(error.message))
                    } else {
                        console.log(JSON.stringify(error))
                    }
                })
        }
    }
}
</script>
