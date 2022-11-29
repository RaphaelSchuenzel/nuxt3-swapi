<template lang="pug">
.character
    h1 {{ character.name }}

    .mt-6(
        v-if="!isArrayEmpty(character.species)"
    )
        h2 Species
        
        p(
            v-for="species in character.species"
        )
            CharacterResource(
                :url="species"
                attribute="name"
            )

    .mt-4(
        v-if="!isArrayEmpty(character.films)"
    )
        h2 Movies

        p(
            v-for="film in character.films"
        )
            CharacterResource(
                :url="film"
                attribute="title"
            )

    .mt-4(
        v-if="!isArrayEmpty(character.starships)"
    )
        h2 Spaceships

        p(
            v-for="starship in character.starships"
        )
            CharacterResource(
                :url="starship"
                attribute="name"
            )
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
    },
    methods: {
        isArrayEmpty: (arr: string[]) => {
            return (Array.isArray(arr) && arr.length) ? false : true
        }
    }
})
</script>

<style lang="sass" scoped></style>
