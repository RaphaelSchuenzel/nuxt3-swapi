<template lang="pug">
#character-list
    div.text-center.pt-24.pb-32
        h1.text-2xl
            span.text-primary Star Wars 
            span Character List

    #characters.grid.grid-cols-5.gap-5
        .character.mt-6.p-6.text-center(
            v-for="character in data.results"
        )
            NuxtLink(
                :to="getCharacterUrl(character.url)"
            )
                span.text-xl.character-text {{ character.name }}

    .flex.justify-center.mt-24
        .px-8.py-4.border-2.border-primary(
            :class="{ 'pointer-events-none saturate-0 opacity-50': !data.next || loadingCharacters }"
            v-on:click="loadCharacters(data.next)"
            v-if="data.next"
        )
            span(
                v-if="data.next && !loadingCharacters"
            ) Load More

            span(
                v-else
            ) Loading...
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

<style lang="sass" scoped>
.character
    .character-text
        transform: rotate3d(1,0,0,12deg) translateY(-20px)
</style>