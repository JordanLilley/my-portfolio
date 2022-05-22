<template>
<div class="bg-black h-screen px-6 py-12">
    <h2 class="text-white font-exo text-5xl pb-4 opacity-0" ref="title" style="transform: translateY(30px)">
        PREVIOUS WORK
    </h2>
    <div class="flex space-x-4">
        <div ref="projects" v-for="project in projects" class="w-full opacity-0" style="transform: translateX(30px);">
         <nuxt-link to="/">
            <img :src="project.image.url" alt="" style="height: 70vh;" class="object-cover">
            <p class="font-space-mono text-white text-sm">{{project.title}}</p>
         </nuxt-link>
        </div>
    </div>
</div>
</template>

<script>
import gsap from 'gsap'
import ethereumImg from '~/assets/ethereum.jpg'
import diceImg from '~/assets/dice.jpg'
export default {
    data() {
        return {
            projects: [
                {
                    image: {
                        url: ethereumImg
                        },
                        title: 'ETHEREUM CARBON CALCULATOR'
                },
                {
                    image: {
                        url: diceImg
                        },
                        title: 'Blackjack'
                }
            ]
        }
    },
    mounted() {
        gsap.to(this.$refs.title, {
            opacity: 1,
            duration: 2,
            y: 0,
            ease: 'expo'
        })

        gsap.to(this.$refs.projects, {
            opacity: 1,
            duration: 2,
            stagger: 0.1,
            x: 0,
            ease: 'expo'
        })
    }
}
</script>