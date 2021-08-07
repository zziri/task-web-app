<template>
    <header>
        <h1>Make Your Tasks</h1>
        <div class="googleLoginContainer" v-if="isAuthorized === false">
            <span class="googleLoginBtn" @click="googleLoginHandler">Google Login</span>
        </div>
        <section><ul></ul></section>
    </header>
</template>

<script>
export default {
    props: ['isAuthorized'],
    methods: {
        googleLoginHandler() {
            console.log("clicked Google Login")
            var userInfo = new Object()
            this.$gAuth.signIn()
            .then((result) => {
                userInfo.email = result.Ts.Et
                userInfo.name = result.Ts.Me
                userInfo.accessToken = result.Zb.access_token
                if (this.$gAuth.isAuthorized) {
                    this.$emit('googleLogin', userInfo);
                } else {
                    this.$emit('googleLogin', null);
                }
            })
        }
    }
}
</script>

<style>
    body {
        text-align: center;
        background-color: #F6F6F8;
    }
    input {
        border-style: groove;
        width: 200px;
    }
    button {
        border-style: groove;
    }
    .shadow {
        box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
    }
    .googleLoginContainer {
        width: 8.5rem;
        height: 50px;
        line-height: 50px;
        background-color: white;
        border-radius: 5px;
        margin: 0 auto;
    }
    .googleLoginBtn {
        color: #e20303;
        display: black;
    }
</style>