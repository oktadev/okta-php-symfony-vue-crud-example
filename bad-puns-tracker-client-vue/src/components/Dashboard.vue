<template>
    <section class="section">
        <div class="container">
            <nav class="navbar" role="navigation" aria-label="main navigation">
                <div class="navbar-menu is-active buttons">
                    <router-link to="/" tag="button" id='home-button' class="button is-link"> Home </router-link>
                    <router-link to="/movies" v-if='authenticated' tag="button" id='home-button' class="button is-link"> Movies </router-link>
                    <button class="button is-link" v-if='authenticated' v-on:click='logout' id='logout-button'> Logout </button>
                    <button v-else v-on:click='login' id='login-button' class="button is-link"> Login </button>
                </div>
            </nav>
            <router-view></router-view>
        </div>
    </section>
</template>

<script>
export default {
    data: function () {
        return {
            authenticated: false
        }
    },
    created () {
        this.isAuthenticated()
    },
    watch: {
        // Everytime the route changes, check for auth status
        '$route': 'isAuthenticated'
    },
    methods: {
        async isAuthenticated () {
            this.authenticated = await this.$auth.isAuthenticated()
        },
        login () {
            this.$auth.loginRedirect('/')
        },
        async logout () {
            await this.$auth.logout()
            await this.isAuthenticated()

            // Navigate back to home
            this.$router.push({ path: '/' })
        }
    }
}
</script>