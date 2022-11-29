<template lang="pug">
span {{ data[props.attribute] }}
</template>

<script setup lang="ts">
const props = defineProps({
    url: String,
    attribute: String
})

const { data } = await useFetch(props.url)
</script>
