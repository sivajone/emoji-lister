<template>
    <v-col cols="auto">
        <v-card
            class="ma-5"
            ols="auto"
        >
            <v-img
            :src="src"
            height="256px"
            width="256px"
            />

            <v-card-title class="subtitle-2">
                {{ name }}
            </v-card-title>

            <v-card-actions>
                <v-btn
                    text
                    color="deep-purple accent-4"
                    :to="'/' + id"
                >
                    Go
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-col>
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
