<template lang="pug">
.character.border-2.border-primary.p-8(
    class="md:p-16"
)
    h1.text-2xl {{ character.name }}

    .mt-16
        .items-center.mt-6(
            class="md:flex"
        )
            h2(
                class="md:w-1/6"
            ) Species
            
            .grid.gap-3.mt-4(
                class="md:grid-cols-2 md:mt-0"
            )
                .bg-gray.px-4.py-2(
                    class="md:ml-4"
                    v-if="!isArrayEmpty(character.species)"
                    v-for="species in character.species"
                )
                    CharacterResource(
                        :url="species"
                        attribute="name"
                    )

                div(
                    class="md:px-4 md:py-2 md:ml-4"
                    v-else
                ) N/A

        .items-center.mt-6(
            class="md:flex"
        )
            h2(
                class="md:w-1/6"
            ) Movies

            .grid.gap-3.mt-4(
                class="md:grid-cols-2 md:mt-0"
            )
                .bg-gray.px-4.py-2(
                    class="md:ml-4"
                    v-if="!isArrayEmpty(character.films)"
                    v-for="film in character.films"
                )
                    CharacterResource(
                        :url="film"
                        attribute="title"
                    )

                div(
                    class="md:px-4 md:py-2 md:ml-4"
                    v-else
                ) N/A

        .items-center.mt-6(
            class="md:flex"
        )
            h2(
                class="md:w-1/6"
            ) Spaceships

            .grid.gap-3.mt-4(
                class="md:grid-cols-2 md:mt-0"
            )
                .bg-gray.px-4.py-2(
                    class="md:ml-4"
                    v-if="!isArrayEmpty(character.starships)"
                    v-for="starship in character.starships"
                )
                    CharacterResource(
                        :url="starship"
                        attribute="name"
                    )

                div(
                    class="md:px-4 md:py-2 md:ml-4"
                    v-else
                ) N/A
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
