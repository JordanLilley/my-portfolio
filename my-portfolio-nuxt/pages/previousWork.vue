<template>
<div class="bg-black h-screen px-6 py-12">
    <h2 class="text-white font-exo text-5xl ">
        PREVIOUS WORK
    </h2>
    <div class="flex space-x-2">
        <div v-for="project in projects">
         <img :src="project.image.url" alt="" style="height: 70vh;" class="object-cover">
         <p class="font-space-mono text-white text-sm">{{project.title}}</p>
        </div>
    </div>
</div>
</template>

<script>
import ethereumImg from '~/assets/ethereum.jpg'
export default {
    data() {
        return {
            projects: [
                {
                    image: {
                        url: ethereumImg
                        },
                        title: 'ETHEREUM CARBON CALCULATOR'
                }
            ]
        }
    }
}
</script>