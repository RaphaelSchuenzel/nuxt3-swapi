<template lang="pug">
#app
    main.container.mx-auto.text-white
        NuxtLoadingIndicator(
            color="#FFE81F"
        )

        slot
</template>

<style lang="sass">
html, body
    background: #000
</style>