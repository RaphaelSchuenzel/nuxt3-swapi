<template lang="pug">
#character-list
    CharacterFilters(
        :characters="characters"
        :filters="filters"
        :enableSpeciesFilter="enableSpeciesFilter"
        @toggleSpeciesFilter="enableSpeciesFilter = !enableSpeciesFilter"
    )

    #characters.mt-12.grid.gap-5(
        class="sm:grid-cols-2 md:grid-cols-3"
    )
        NuxtLink.text-xl.mt-6.p-6.text-center.border-2.border-primary(
            class="hover:bg-primary hover:text-black"
            :to="getCharacterLink(character)"
            v-for="character in filteredCharacters"
        ) {{ character.name }}

    .flex.justify-center.mt-24
        a.px-8.py-4.bg-gray.cursor-pointer(
            :class="{ 'pointer-events-none opacity-50': !characters.next || loadingCharacters }"
            v-on:click="loadCharacters(characters.next)"
            v-if="characters.next"
        )
            span(
                v-if="characters.next && !loadingCharacters"
            ) Load More

            span(
                v-else
            ) Loading...
</template>

<script lang="ts">
import type { Characters } from '~/types'

const baseURL = `https://swapi.dev/api/`

export default defineNuxtComponent({
    data () {
        return {
            loadingCharacters: false,
            enableSpeciesFilter: false,
            filters: {
                films: [],
                species: []
            }
        }
    },
    async asyncData () {
        return {
            characters: await $fetch(`people`, { baseURL })
        }
    },
    computed: {
        filteredCharacters () {
            const enableSpeciesFilter = this.enableSpeciesFilter
            
            const filters = this.filters
            const characters = (this as any).characters.results

            const filteredCharacters = characters.filter((character:any) => {
                const isWithinFilmsFilter =
                    filters.films.some(film => character.films.includes(film))
                const isWithinSpeciesFilter =
                    filters.species.some(species => character.species.includes(species))

                return isWithinFilmsFilter && (enableSpeciesFilter ? isWithinSpeciesFilter : true)
                    ? character
                    : false
            })

            return filteredCharacters
        }
    },
    methods: {
        getCharacterLink: (character: any) => {
            if (character && character.url) {
                const match = character.url.match(`/people/([0-9]+)/`)

                return (match && match[1]) ? `/characters/${match[1]}` : false
            } else {
                return false
            }
        },
        async loadCharacters(next: string) {
            if (!this.loadingCharacters && next) {
                this.loadingCharacters = true

                const res: Characters = await $fetch(next)

                if (res) {
                    const characters = (this as any).characters
    
                    characters.next = res.next
                    characters.previous = res.previous
                    characters.results.push(...res.results)

                    return this.loadingCharacters = false
                } else {
                    return false
                }
            } else {
                return false
            }
        }
    }
})
</script>
