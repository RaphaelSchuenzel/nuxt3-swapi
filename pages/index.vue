<template lang="pug">
#characters
    .character(
        v-for="character in data.results"
    )
        NuxtLink(
            :to="getCharacterUrl(character.url)"
        ) {{ character.name }}
</template>

<script lang="ts">
const baseURL = `https://swapi.dev/api/`

export default defineNuxtComponent({
    async asyncData () {
        return {
            data: await $fetch(`people`, { baseURL })
        }
    },
    methods: {
        getCharacterUrl: (url: string) => {
            const match = url.match(`/people/([0-9]+)/`)

            return (match && match[1]) ? `/characters/${match[1]}` : false
        }
    }
})
</script>

<style lang="sass" scoped></style>