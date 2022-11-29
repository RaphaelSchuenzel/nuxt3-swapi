<template lang="pug">
p {{ data[props.attribute] }}
</template>

<script setup>
const props = defineProps({
    url: {
        type: String,
    },
    attribute: {
        type: String,
    }
})

const { data } = await useFetch(props.url)
</script>
