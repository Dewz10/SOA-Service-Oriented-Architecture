<template>
    <div class="vue-tempalte">
        <div class="row justify-content-center">
            <div class="col-md-4">
                <!-- Content goes here -->

                <form @submit.prevent="saveData">
                    <h3 class="row justify-content-center">Sign Up</h3>
                    <div class="form-group">
                        <label for="text">First Name</label>
                        <input type="text" class="form-control form-control-lg" v-model="user.userFirstname"  placeholder="First Name"  required>
                    </div>
                    <div class="form-group">
                        <label for="text">Last Name</label>
                        <input type="text" class="form-control form-control-lg" v-model="user.userLastname"  placeholder="Last Name" required>
                    </div>
                    <div class="form-group">
                        <label for="text">Phone</label>
                        <input type="text" class="form-control form-control-lg" v-model="user.userPhonenumber"   placeholder="Phone" required>
                    </div>
                    <div class="form-group">
                        <label for="text">Username</label>
                        <input type="text" class="form-control form-control-lg" v-model="user.userUsername"  placeholder="Username" required>
                    </div>
                    <div class="form-group">
                        <label for="text">Password</label>
                        <input type="password" class="form-control form-control-lg" v-model="inputPassword"  placeholder="Password" required>
                    </div>
                    <div class="form-group row justify-content-center m-3">
                        <button type="submit" class="btn btn-dark btn-lg btn-block">Sign up</button>

                        <p class="forgot-password text-right mt-2 mb-4">
                            Already registered
                            <router-link to="/login"> sign in?</router-link>
                        </p>
                    </div>
                </form>
                <!-- <h3>{{ user }}</h3> -->
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios';
import bcrypt from 'bcryptjs';

export default {
    name: "SignUpPage",
    props: {
        msg: String
    },
    data() {
        return {
            user: {
                userUsername: "",
                userPassword: "",
                userFirstname: "",
                userLastname: "",
                userPhonenumber: "",
                userType: "USER"
            },
            inputPassword: null,
        }
    },
    methods: {
        async saveData() {
            const saltRounds = 10;
            const hashedPassword = await bcrypt.hash(this.inputPassword, saltRounds);
            this.user.userPassword = hashedPassword;
            await axios.post("http://localhost:8080/api/v1/users/", this.user)
                .then(
                    ({ data }) => {
                        // alert("saved success!!!");
                        this.user.userUsername = '';
                        this.userPassword = '';
                        this.user.userFirstname = '',
                            this.userLastname = '',
                            this.userPhonenumber = '',
                            alert("Sign up success!");
                        // localStorage.setItem("user-info", JSON.stringify(data));
                        this.$router.push({ name: "HomePage" })
                        console.log(data);
                    }
                );
        },

        mounted() {
            let user = localStorage.getItem('user-info');
            if (user) {
                this.$router.push({ name: "HomePage" })
            }
        }
        // handleSubmitForm() {
        //     let apiURL = 'http://localhost:8080/api/v1/users/';

        //     axios.post(apiURL, this.user).then((e) => {
        //         console.log(e.data);
        //         this.$router.push('/view');
        //         this.user = {
        //             userUsername: this.user.userUsername,
        //             userPassword: this.userPassword,
        //             userFirstname: this.user.userFirstname,
        //             userLastname: this.userLastname,
        //             userPhonenumber: this.userPhonenumber
        //         }
        //     }).catch(error => {
        //         console.log(error);
        //     })
        // }
    }
}
</script>

<style scoped>
* {
    box-sizing: border-box;
}

body {
    background: #2554FF !important;
    min-height: 100vh;
    display: flex;
    font-weight: 400;
}

body,
html,
.App,
.vue-tempalte,
.vertical-center {
    width: 100%;
    height: 100%;
}

.navbar-light {
    background-color: #ffffff;
    box-shadow: 0px 14px 80px rgba(34, 35, 58, 0.2);
}

.vertical-center {
    display: flex;
    text-align: left;
    justify-content: center;
    flex-direction: column;
}

.inner-block {
    width: 450px;
    margin: auto;
    background: #ffffff;
    box-shadow: 0px 14px 80px rgba(34, 35, 58, 0.2);
    padding: 40px 55px 45px 55px;
    border-radius: 15px;
    transition: all .3s;
}

.vertical-center .form-control:focus {
    border-color: #2554FF;
    box-shadow: none;
}

.vertical-center h3 {
    text-align: center;
    margin: 0;
    line-height: 1;
    padding-bottom: 20px;
}

label {
    font-weight: 500;
}

.forgot-password,
.forgot-password a {
    text-align: right;
    font-size: 17px;
    padding-top: 10px;
    color: #7a7a7a;
    margin: 0;
}

.forgot-password a {
    color: #2554FF;
}

.social-icons {
    text-align: center;
    font-family: "Open Sans";
    font-weight: 300;
    font-size: 1.5em;
    color: #222222;
}

.social-icons ul {
    list-style: none;
    margin: 0;
    padding: 0;
}

.social-icons ul li {
    display: inline-block;
    zoom: 1;
    width: 65px;
    vertical-align: middle;
    border: 1px solid #e3e8f9;
    font-size: 15px;
    height: 40px;
    line-height: 40px;
    margin-right: 5px;
    background: #f4f6ff;
}

.social-icons ul li a {
    display: block;
    font-size: 1.4em;
    margin: 0 5px;
    text-decoration: none;
}

.social-icons ul li a i {
    -webkit-transition: all 0.2s ease-in;
    -moz-transition: all 0.2s ease-in;
    -o-transition: all 0.2s ease-in;
    -ms-transition: all 0.2s ease-in;
    transition: all 0.2s ease-in;
}

.social-icons ul li a:focus i,
.social-icons ul li a:active i {
    transition: none;
    color: #222222;
}
</style>
