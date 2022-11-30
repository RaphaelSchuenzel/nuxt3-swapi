<template lang="pug">
#app
    NuxtLoadingIndicator(
        color="#FFE81F"
    )

    main.container.max-w-3xl.mx-auto.text-white.py-24
        .text-center.pt-8.pb-32
            h1.text-2xl
                NuxtLink(
                    to="/"
                )
                    span.text-primary Star Wars 
                    span Characters

        NuxtPage
</template>

<style lang="sass">
html, body
    background: url('https://www.publicdomainpictures.net/pictures/60000/nahled/stars-in-night-sky.jpg'), black

.page-enter-from,.page-leave-to
    opacity: 0
    transform: translateY(-20px)
</style>