<template lang="pug">
div
    h1 {{ character.name }}

    div
        label Species
        p {{ character.species }}

    div
        label Movies
        p {{ character.films }}

    div
        label Spaceships
        p {{ character.starships }}
</template>

<script lang="ts">
const baseURL = `https://swapi.dev/api/`

export default defineNuxtComponent({
    async asyncData () {
        const route = useRoute()
        const character = await $fetch(`people/${route.params.id}/`, { baseURL })

        return {
            character
        }
    }
})
</script>

<style lang="sass" scoped></style>