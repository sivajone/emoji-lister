<template>
    <v-hover v-slot="{ hover }">
        <nuxt-link :to="'/' + id" style="text-decoration: none;">
            <v-card
                :elevation="hover ? 12 : 2"
                :class="{ 'on-hover': hover }"
                class="ma-2"
            >
            <v-container fluid>
                <v-img
                :src="src"
                height="256px"
                width="256px"
            />

                <v-card-title class="subtitle-2">
                    {{ name }}
                </v-card-title>
            </v-container>
            </v-card>
        </nuxt-link>
    </v-hover>
</template>

<script>
export default {
  props: {
        src: {
            type: String,
            default: null
        }, 
        name: {
            type: String,
            default: null
        }, 
        id: {
            type: String,
            default: null
        }
    },
};
</script>

<style>
.v-card {
  transition: transform .4s ease-in-out !important;
}

.v-card.on-hover {
  transform: scale(1.05);
 }

.show-btns {
  color: rgba(255, 255, 255, 1) !important;
}
</style>