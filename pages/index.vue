<template lang="pug">
#character-list
    CharacterFilters(
        :characters="characters"
        :filters="filters"
    )

    #characters.mt-12.grid.gap-5(
        class="sm:grid-cols-2 md:grid-cols-3 xl:grid-cols-4"
    )
        NuxtLink.text-xl.mt-6.p-6.text-center.border-2.border-primary(
            :to="getCharacterUrl(character.url)"
            v-for="character in filteredCharacters"
        ) {{ character.name }}

    .flex.justify-center.mt-24
        .px-8.py-4.bg-gray.cursor-pointer(
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
            return (this as any).characters.results
        }
    },
    methods: {
        getCharacterUrl: (url: string) => {
            const match = url.match(`/people/([0-9]+)/`)

            return (match && match[1]) ? `/characters/${match[1]}` : false
        },
        async loadCharacters(next: string) {
            if (!this.loadingCharacters && next) {
                this.loadingCharacters = true

                const res: Characters = await $fetch(next)

                if (res) {
                    (this as any).characters
                        .next = res.next
                        .previous = res.previous
                        .results.push(...res.results)

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
