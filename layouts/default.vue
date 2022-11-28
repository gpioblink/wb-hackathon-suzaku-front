<template>
  <v-app :style="{ background: '#F5F5F5' }">
    <v-main>
      <v-toolbar>
        <nuxt-link to="/" v-slot="{navigate}" class="c-p" custom>
          <v-toolbar-title class="grey--text text--darken-2">
            <span role="link" @click="navigate">IoT旅行通信システム</span>
          </v-toolbar-title>
        </nuxt-link>
        <v-spacer></v-spacer>
        <!-- <v-btn text to="/">
          <span>デバイスと接続</span>
        </v-btn> -->
      </v-toolbar>

      <v-container class="spacing-playground pa-6">
        <Nuxt/>
      </v-container>

    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }} Team Suzaku / @gpioblink</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js'
    }
  }
}
</script>

<style scoped>
.c-p {
  cursor: pointer;
}
</style>