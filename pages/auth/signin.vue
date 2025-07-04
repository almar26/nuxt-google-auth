<template>
    <!-- <div class="d-flex justify-center align-center" style="height: 100vh;">
        <v-card width="600" class="pa-10">

            <v-form class="text-center">
                <h1 class="mb-8">Login</h1>
                <v-text-field v-model="username" label="Username" prepend-inner-icon="mdi-account" outlined></v-text-field>
                <v-text-field v-model="password" type="password" label="Password" prepend-inner-icon="mdi-lock" outlined></v-text-field>
                <v-btn color="primary" @click="login()" :disabled="valid" :loading="valid" block>Log in</v-btn>
                <v-btn class="text-capitalize mt-6" color="red" @click="loginWithGoogle()" block><v-icon
                        left>mdi-google</v-icon> Sign in with google</v-btn>
            </v-form>
        </v-card>
    </div> -->
    <div class="d-flex align-center justify-center" style="height: 100vh">
        <v-card width="450" class="mx-auto card-border" outlined>
            <v-img src="../logo.png" max-height="150" contain></v-img>



            <v-form ref="form" v-model="valid" lazy-validation @submit.prevent="submit()" class="mx-15">
                <v-text-field v-model="username" :rules="usernameRules" label="Username"
                    prepend-inner-icon="mdi-account" outlined required></v-text-field>
                <v-text-field v-model="password" :rules="passwordRules" label="Password" type="password"
                    prepend-inner-icon="mdi-lock" outlined required></v-text-field>
                <v-btn :disabled="!valid" :loading="isLoading" color="primary" block type="submit">
                    Sign In
                </v-btn>
            </v-form>

            <div class="hr-sect mx-15 my-5">or sign in with</div>

            <v-row class="mx-12 mt-5">
                <v-col cols="6" class="mr-auto">
                    <v-btn outlined color="blue-grey" @click="loginWithGoogle()" block><v-icon color="red" left>mdi-google</v-icon> Google</v-btn>
                </v-col>
                <v-col cols="6">
                    <v-btn outlined color="blue-grey" block><v-icon color="blue" left>mdi-facebook</v-icon> Facebook</v-btn>
                </v-col>
            </v-row>

            <v-card-actions class="mb-10 mt-5 text-center">
                <v-spacer></v-spacer>
                <NuxtLink to="/auth/register" class="nuxt-link">Create an account</NuxtLink>
                <v-spacer></v-spacer>
            </v-card-actions>
        </v-card>
    </div>
</template>
<script>
export default {
    layout: "auth",
    middleware: 'guest',
    data() {
        return {
            valid: false,
            isLoading: false,
            username: "",
            password: "",
            usernameRules: [(v) => !!v || "Username is required"],
            passwordRules: [(v) => !!v || "Password is required"],
        }
    },
    methods: {
        async loginWithGoogle() {
            try {
                await this.$auth.loginWith('google')
            } catch (err) {
                console.error('Google login failed: ', err)
            }
        },

        async submit() {

            if (this.$refs.form.validate()) {
                this.isLoading = true;
                this.valid = false
                this.$auth.loginWith('local', {
                    data: {
                        identifier: this.username,
                        password: this.password,
                    }
                })
                    .then(res => {
                        console.log("User Profile: ", res.data.user)
                        this.isLoading = false;
                    })
                    .catch(error => {
                        console.log('Error')
                       
                        if (error.response.data) {
                            console.log(error.response.data.error.message)
                            alert(error.response.data.error.message)
                             this.valid = true;
                             this.isLoading = false;
                        }
                    })
            }

        }
    }
}
</script>

<style scoped>
.nuxt-link {
    text-decoration: none;
    padding-top: 5px;
}

.card-border {
    border-radius: 20px;
    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}

.hr-sect {
    display: flex;
    flex-basis: 100%;
    align-items: center;
    color: rgba(0, 0, 0, 0.9);
    margin: 8px 0;
}

.hr-sect::before,
.hr-sect::after {
    content: "";
    flex-grow: 1;
    background: rgba(0, 0, 0, 0.15);
    height: 1px;
    font-size: 0;
    line-height: 0;
    margin: 0 8px;
}
</style>