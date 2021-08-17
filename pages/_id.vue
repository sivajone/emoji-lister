<template>
  <v-container class="my-5">
    <section v-if="errored">
      <h1 class="text-center">{{error}}</h1>
    </section>
    <section v-else>
        
    <article
    v-if="loading"
    >
        <v-row justify="center">
            <v-sheet
                v-for="n in 20"
                :key="n"
                    class="d-flex ma-2"
            >
                    <v-skeleton-loader
                    min-width="256"
                    type="card"
                    ></v-skeleton-loader>
            </v-sheet>
        </v-row>
    </article>

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
  </v-container>
</template>

<script>
export default {
    inject: {
      theme: {
        default: { isDark: false },
      },
    },
    async asyncData({ params }) {
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
            title: '😏'
        }
    },
    mounted() {
        fetch(`https://discord.com/api/v9/guilds/${this.id}/emojis`, {
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
            if (data.length > 0) {
                this.emojis = data
            }
            else {
                throw new Error('204')
            }
        })
        .catch((error) => {
            this.errored = true;
            if (error.toString().endsWith('401')) {
                this.error = "Bad Token"
            }
            else if (error.toString().endsWith('404')) {
                this.error = "Server Not Found"
            }
            else if (error.toString().endsWith('204')) {
                this.error = "There are no emojis in this server 😔"
            }
            console.log(error)
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