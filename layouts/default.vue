<template>
  <v-app dark>
    <v-main>
      <Nuxt />
    </v-main>
    <v-footer padless>
      <v-row class="ma-0">
          <v-col class="d-flex justify-start py-1" align-self="center">
            SivajOne &copy; {{new Date().getFullYear()}}
          </v-col>
          <v-col class="d-flex justify-end py-1" align-self="center"> 
            <v-switch
            dense
            flat
              v-model="darkTheme"
              @change="changeTheme"
            />
          </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      // TODO localStorage
      darkTheme: true
    }
  },
  mounted() {
    this.$vuetify.theme.dark = this.darkTheme
  },
  methods: {
    changeTheme() {
      this.$vuetify.theme.dark = this.darkTheme
      // localStorage.setItem('darkTheme', this.darkTheme)
    }
  }
}
</script>