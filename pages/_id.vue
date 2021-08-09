<template>
  <div class="container">
    <section v-if="errored">
      <h1>Something bad happened, try again later...</h1>
    </section>
    <section v-else>
        
      <v-progress-linear
        v-if="loading"
        indeterminate
        reverse
      ></v-progress-linear>

      <article>
        <v-row justify="center">
            <v-card v-for="emoji in emojis" :key="emoji.id" cols="auto" class="ma-2">
                <v-container fluid>
                    <v-row justify="center">
                        <v-col cols="auto">
                            <v-img
                                v-if="emoji.animated"
                                :src="
                                'https://cdn.discordapp.com/emojis/' + emoji.id + '.gif?size=128'
                                "
                                height="128"
                                width="128"
                            ></v-img>
                            <v-img
                                v-else
                                :src="
                                'https://cdn.discordapp.com/emojis/' + emoji.id + '.png?size=128'
                                "
                                height="128px"
                                width="128px"
                            ></v-img>
                        </v-col>

                        <v-col
                        cols="auto"
                        class="text-center pl-0"
                        >
                            <v-row
                                class="flex-column ma-0"
                                justify="center"
                            >
                                
                                <v-col class="px-0">
                                    <v-btn icon>
                                        <a v-if="emoji.animated" 
                                        :href="'https://cdn.discordapp.com/emojis/' + emoji.id + '.gif?size=2048'"
                                        target="_blank"
                                            ><v-icon>mdi-download</v-icon>
                                        </a>
                                        <a v-else 
                                        :href="
                                        'https://cdn.discordapp.com/emojis/' + emoji.id + '.webp?size=2048'"
                                        target="_blank"
                                            ><v-icon>mdi-download</v-icon>
                                        </a>
                                    </v-btn>
                                </v-col>                             
                            </v-row>
                        </v-col>
                    </v-row>
                </v-container>
            </v-card>
        </v-row>
      </article>
    </section>
  </div>
</template>

<script>
export default {
    async asyncData({ params }) {
        const id = await params.id // When calling /abc the slug will be "abc"
        return { id }
    },
    head() {
        return {
        title: this.id
        }
    },
    data() {
        return {
        errored: false,
        loading: true,
        emojis: null,
        token: sessionStorage.getItem("token"),
        };
    },
    mounted() {
        fetch(`https://discordapp.com/api/guilds/${this.id}/emojis`, {
            headers: {
            "Content-Type": "application/json",
            Authorization: `${this.token}`,
            },
        })
        .then(response => 
            response.json()
        )
        .then(data => {
            console.log(data)
            this.emojis = data
        })
        .catch((error) => {
            console.log(error);
            this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
};
</script>
