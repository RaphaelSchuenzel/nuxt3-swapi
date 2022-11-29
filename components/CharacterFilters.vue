<template lang="pug">
#character-filters.bg-gray
    .flex.justify-between.px-8.py-4.cursor-pointer(
        v-on:click="showFilters = !showFilters"
    )
        h2.text-lg Filters

        p (click to 
            span(
                v-if="!showFilters"
            ) show)
            span(
                v-else-if="showFilters"
            ) hide)

    div(
        v-if="showFilters"
    )
        #film-filters
            p Movies

            .film-filter(
                v-for="film in films"
            )
                input(
                    type="checkbox"
                    :value="film"
                    v-model="filmFilter"
                )

                CharacterResource(
                    :url="film"
                    attribute="title"
                )

            p {{ filmFilter }}
</template>

<script setup lang="ts">
const props = defineProps({
    characters: Object
})

const showFilters = false

const films = [...new Set(
    props.characters.results.flatMap(
        character => character.films.map(film => film))
    )
]

films.sort((a: string, b: string) => {
    const identifierA = a.match(`/films/([0-9]+)/`)[1]
    const identifierB = b.match(`/films/([0-9]+)/`)[1]

    if (identifierA && identifierB) {
        return parseInt(identifierA) - parseInt(identifierB)
    }
})

const filmFilter = []
</script>
