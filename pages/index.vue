<template>
    <div class="d-flex flex-column container">
      <h1>Emoji lister</h1>
      <v-text-field
            v-model="token"
            label="token"
            outlined
            type="password"
      />
      <v-btn
        color="primary"
        @click="getServers"
        elevation="2"
        rounded
        x-large
      >Get servers</v-btn>

      <section v-if="errored">
        <h1>Something bad happened, try again later...</h1>
      </section>

      <section v-else>

        <v-progress-linear
          v-if="loading"
          indeterminate
          reverse
        ></v-progress-linear>

        <article v-for="server in servers" :key="server.id">
          <v-row justify="center">
            <Card
              v-if="server.icon.startsWith('a_')"
              :id="server.id"
              :src="
                'https://cdn.discordapp.com/icons/' +
                server.id +
                '/' +
                server.icon +
                '.gif?size=256'
              "
              :name="server.name"
            />
            <Card v-else
              :id="server.id"
              :src="
                'https://cdn.discordapp.com/icons/' +
                server.id +
                '/' +
                server.icon +
                '.webp?size=256'
              "
              :name="server.name"
            />
          </v-row>
        </article>
      </section>
    </div>
</template>

<script>
import Card from "@/components/Card";
import "@fontsource/montserrat";

export default {
  name: "Home",
  head() {
    return {
      title: 'Home'
    }
  },
  components: {
    Card,
  },
  data() {
    return {
      token: null,
      servers: null,
      loading: null,
      errored: false,
    };
  },
  methods: {
    getServers() {
      this.loading = true;
        fetch("https://discordapp.com/api/users/@me/guilds", {
          headers: {
            "Content-Type": "application/json",
            Authorization: `${this.token}`,
          },
        })
        .then(response => 
          response.json()
        )
        .then(data => {
          this.servers = data;
        })
        .catch((error) => {
          console.error(error)
          this.errored = true;
        })
        .finally(() => {
          this.loading = false;
          sessionStorage.setItem("token", this.token);
        });
    },
  },
};
</script>

<style lang="scss">
.container {
  font-family: 'Montserrat';
  & > h1 {
    text-align: center;
  }
}
.v-input input {
  text-align: center;
}
</style>