<template>
  <div v-if="displayHeader()">
    <v-app-bar app clipped-left flat fixed>
      <v-toolbar-title>
        <nuxt-link to="/" class="link-title">
          <div>
            Jakub Gania Software
          </div>
        </nuxt-link>
      </v-toolbar-title>
      <v-spacer />
      <v-app-bar-nav-icon
        class="nav-icon"
        @click.native="$emit('switchNavigationDrawer')"
      />
    </v-app-bar>
  </div>
</template>

<script>
export default {
  mounted() {
    this.displayHeader()
  },
  methods: {
    displayHeader() {
      return this.$route.name !== 'index'
    }
  }
}
</script>

<style scoped>
.link-title {
  text-decoration: none;
  font-family: 'Audiowide', cursive;
  color: #000;
  font-size: 24px;
}
.nav-icon {
  color: #000;
}

@media only screen and (max-width: 600px) {
  .link-title {
    font-size: 12px;
  }
}
</style>
