<template lang="pug">
span {{ data[props.attribute] }}
</template>

<script setup>
const props = defineProps({
    url: String,
    attribute: String
})

const { data } = await useFetch(props.url)
</script>
