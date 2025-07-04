<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card class="logo py-4 d-flex justify-center">
        <div v-if="$auth.loggedIn">

          <div>

            <p>Local Authenticated: {{ $auth.strategy.name }}</p>
          </div>
          <div v-if="$auth.strategy.name === 'google'">
            <p>Welcome, {{ $auth.user.name }}</p>
            <img :src="$auth.user.picture" alt="User picture">
            <p>Google Authenticated</p>
            <v-btn @click="logout">Logout</v-btn>
          </div>
          <div v-else-if="$auth.strategy.name === 'local'">
            <p>Welcome, {{ $auth.user.username }}</p>
            <p>Email, {{ $auth.user.email }}</p>
            <p>Local Authenticated</p>
            <v-btn @click="logout">Logout</v-btn>
          </div>
        </div>

      </v-card>
      <v-card>
        <v-card-title class="headline">
          Welcome to the Vuetify + Nuxt.js template
        </v-card-title>
        <v-card-text>
          <p>Vuetify is a progressive Material Design component framework for Vue.js. It was designed to empower
            developers to create amazing applications.</p>
          <p>
            For more information on Vuetify, check out the <a href="https://vuetifyjs.com" target="_blank"
              rel="noopener noreferrer">
              documentation
            </a>.
          </p>
          <p>
            If you have questions, please join the official <a href="https://chat.vuetifyjs.com/" target="_blank"
              rel="noopener noreferrer" title="chat">
              discord
            </a>.
          </p>
          <p>
            Find a bug? Report it on the github <a href="https://github.com/vuetifyjs/vuetify/issues" target="_blank"
              rel="noopener noreferrer" title="contribute">
              issue board
            </a>.
          </p>
          <p>Thank you for developing with Vuetify and I look forward to bringing more exciting features in the future.
          </p>
          <div class="text-xs-right">
            <em><small>&mdash; John Leider</small></em>
          </div>
          <hr class="my-3">
          <a href="https://nuxtjs.org/" target="_blank" rel="noopener noreferrer">
            Nuxt Documentation
          </a>
          <br>
          <a href="https://github.com/nuxt/nuxt.js" target="_blank" rel="noopener noreferrer">
            Nuxt GitHub
          </a>
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="primary" nuxt to="/inspire">
            Continue
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',
  //middleware: ['auth'],
  async mounted() {
    if (!this.$auth.loggedIn) {
      console.log("Not logged in")
      //this.$router.push('/auth/signin')
    } else {
      console.log("You are logged in")
    }

    // Check if just logged in
    // if (this.$route.hash.includes('access_token')) {
    //   // wait for tokens to be parsed and stored
    //   await this.$auth.fetchUser()
    //   this.$router.replace({ path: '/'})
    // }
  },

  methods: {
    async logout() {
      await this.$auth.logout()
      this.$router.push('/auth/signin')
    }
  }

}
</script>
