<template lang="pug">
#character-filters.border-2.border-gray
    .flex.justify-between.px-8.py-6.cursor-pointer(
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

    .px-8.pt-4.pb-8(
        v-show="showFilters"
    )
        #film-filters.mt-6(
            class="md:flex"
        )
            p(
                class="md:w-1/6"
            ) Movies

            .grid.gap-6.mt-4(
                class="md:grid-cols-2 md:mt-0"
            )
                .bg-gray.px-4.py-2(
                    v-for="film in films"
                )
                    input(
                        type="checkbox"
                        :value="film"
                        v-model="filters.films"
                    )

                    CharacterResource.ml-3(
                        :url="film"
                        attribute="title"
                    )

        #species-filters.mt-12(
            class="md:flex"
        )
            p(
                class="md:w-1/6"
            ) Species

            .grid.gap-3.mt-4(
                class="md:grid-cols-2 md:mt-0"
            )
                .bg-gray.px-4.py-2(
                    v-for="species in species"
                )
                    input(
                        type="checkbox"
                        :value="species"
                        v-model="filters.species"
                    )

                    CharacterResource.ml-3(
                        :url="species"
                        attribute="name"
                    )
</template>

<script lang="ts">
import type { Characters } from '~/types'
import type { PropType } from 'vue'

export default defineNuxtComponent({
    props: {
        characters: {
            type: Object as PropType<Characters>,
            required: true,
        },
        filters: {
            type: Object,
            required: true,
        }
    },
    data () {
        return {
            showFilters: false,
        }
    },
    computed: {
        films() {
            return this.getFilterOptions('films')
        },
        species() {
            return this.getFilterOptions('species')
        }
    },
    methods: {
        getFilterOptions(key: string) {
            const props = this.$props

            if (props && key) {
                const options = [...new Set(
                    props.characters.results.flatMap(
                        character => character[key].map(
                            option => option
                        )
                    )
                )]

                options.sort((a: string, b: string) => {
                    const identifierA = a.match(`/${key}/([0-9]+)/`)[1]
                    const identifierB = b.match(`/${key}/([0-9]+)/`)[1]

                    if (identifierA && identifierB) {
                        return parseInt(identifierA) - parseInt(identifierB)
                    }
                })

                this.filters[key] = [...options]

                return options
            } else {
                return false
            }
        }
    }
})
</script>
