<template>
    <v-container class="d-flex flex-column items-center">
      <h1 class="text-center">Emoji lister</h1>
      <v-text-field
        v-model="token"
        class="mt-3"
        label="token"
        outlined
        type="password"
      />
      <v-btn
        color="primary"
        elevation="2"
        rounded
        x-large
        :loading="loading"
        @click="getServers"
      >Get servers</v-btn>

      <section v-if="errored">
        <h1>Something bad happened, try again later...</h1>
      </section>

      <section v-else>

        <article class="py-10">
          <v-row justify="center">
              <Card
                v-for="server in servers"
                :id="server.id"
                :key="server.id"
                :src="
                  'https://cdn.discordapp.com/icons/' +
                  server.id +
                  '/' +
                  server.icon + 
                  checkType(server.icon) + 
                  '?size=256'
                "
                :name="server.name"
              />
          </v-row>
        </article>
      </section>
    </v-container>
</template>

<script>
import Card from "@/components/Card";

export default {
  name: "Home",
  components: {
    Card,
  },
  data() {
    return {
      token: sessionStorage.getItem('token') || null,
      servers: null,
      loading: null,
      errored: false,
    };
  },
  head() {
    return {
      title: 'Home'
    }
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
          this.servers = data
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
    checkType(hash) {
      if (hash.toString().startsWith('a_')) {
        return '.gif'
      }
      else {
        return  '.webp'
      }
    }
  },
};
</script>

<style lang="scss">
@keyframes floating {
  0% {
    transform: rotate(5deg) scale(1);;
  }
  50% {
    transform: rotate(-5deg) scale(1.2);
  }
  100% {
    transform: rotate(5deg) scale(1);
  }
}
.container {
  font-family: 'Montserrat', sans-serif;
  & > h1 {
    animation: floating 10s infinite ease-in-out;
  }
}
.v-input input {
  text-align: center;
}
</style>