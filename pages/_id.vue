<template>
  <div class="container">
    <section v-if="errored">
      <h1>{{error}}</h1>
    </section>
    <section v-else>
        
      <v-progress-linear
        v-if="loading"
        indeterminate
        reverse
      ></v-progress-linear>

      <article>
        <v-row justify="center">
            <v-card 
            v-for="emoji in emojis" 
            :key="emoji.id" 
            cols="auto" 
            class="d-flex ma-1"
            >
                <v-container class="d-flex">
                    <v-row justify="center" align="center">
                        <v-col cols="auto">
                            <v-img
                                v-if="emoji.animated"
                                :src="
                                'https://cdn.discordapp.com/emojis/' + emoji.id + '.gif?size=128'
                                "

                            />
                            <v-img
                                v-else
                                :src="
                                'https://cdn.discordapp.com/emojis/' + emoji.id + '.png?size=128'
                                "
                            />
                        </v-col>

                        <v-col
                        class="flex-column text-center pl-0"
                        cols="auto"
                        justify="center"
                        align="center"
                        >
                            <v-row
                                class="flex-column ma-0"
                                justify="center"
                                align="center"
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
    props: {
        name: {
            type: String,
            default: undefined
        }
    },
    async asyncData({ params}) {
        const id = await params.id // When calling /abc the slug will be "abc"
        return { id }
    },
    data() {
        return {
            errored: false,
            error: null,
            loading: true,
            emojis: null,
            token: sessionStorage.getItem("token"),
        };
    },
    head() {
        return {
            title: this.name
        }
    },
    mounted() {
        fetch(`https://discordapp.com/api/guilds/${this.id}/emojis`, {
            headers: {
            "Content-Type": "application/json",
            Authorization: `${this.token}`,
            },
        })
        .then(response => {
            if (response.ok)  {
                return response.json()
            }

            else if (response.status === 401) {
                throw new Error('401')
            }

            else if (response.status === 404) {
                throw new Error('404')
            }
        })
        .then(data => {
            this.emojis = data
        })
        .catch((error) => {
            this.errored = true;
            if (error.toString().endsWith('401')) {
                this.error = "401 Bad Token"
            }
            else if (error.toString().endsWith('404')) {
                this.error = "401 Server Not Found"
            }
        })
        .finally(() => (this.loading = false));
    },
};
</script>

<style>
.v-image {
    min-width: 128px;
    min-height: 100%;
}
</style>