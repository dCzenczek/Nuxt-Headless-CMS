<template>
  <v-app-bar
    app
    elevate-on-scroll
    dark
  >
    <nuxt-link to="/" exact class="home-page-navigation">
      <img class="mx-3" :src="logo" alt="logo">
      {{ pageName }}
    </nuxt-link>

    <v-spacer />

    <Navigation />

    <LanguageSwitcher />
  </v-app-bar>
</template>

<script>
import LanguageSwitcher from '@/components/ui/LanguageSwitcher'
import Navigation from '@/components/ui/Navigation'
import { mapGetters, mapState } from 'vuex'

export default {
  name: 'AppHeader',

  components: {
    LanguageSwitcher,
    Navigation
  },

  computed: {
    ...mapState({
      pageName: state => state.config.pageName
    }),
    ...mapGetters({
      homePageRoute: 'config/getHomePageRoute',
      logo: 'config/getLogo'
    })
  }
}
</script>

<style lang="scss" scoped>
.home-page-navigation {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  text-decoration: none;
  color: white;
}
</style>
