<template lang="pug">
#characters.grid.grid-cols-5.gap-2
    .character(
        v-for="character in data.results"
    )
        NuxtLink(
            :to="getCharacterUrl(character.url)"
        ) {{ character.name }}

    .mt-4(
        v-on:click="loadCharacters(data.next)"
        v-if="data.next && !loadingCharacters"
    ) Load more
</template>

<script lang="ts">
const baseURL = `https://swapi.dev/api/`

export default defineNuxtComponent({
    data () {
        return {
            loadingCharacters: false,
        }
    },
    async asyncData () {
        return {
            data: await $fetch(`people`, { baseURL })
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

                const res = await $fetch(next)

                if (res) {
                    this.data.next = res.next
                    this.data.previous = res.previous
                    this.data.results.push(...res.results)

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

<style lang="sass" scoped></style>