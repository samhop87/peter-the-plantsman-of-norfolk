<script>
import { ref, onMounted, onUnmounted } from 'vue'

export default {
    setup() {
        const background = ref(null)
        const first = ref(null)
        const second = ref(null)

        const handleScroll = (evt) => {
            const scrollY = window.scrollY
            // decreases as user scrolls
            first.value.style.opacity =
                (100 - (scrollY + window.innerHeight - first.value.offsetHeight)) / 100
            // increases as user scrolls
            second.value.style.opacity =
                (scrollY + window.innerHeight - second.value.offsetTop) / 100

            const maxBackgroundSize = 120
            const backgroundSize = scrollY / (maxBackgroundSize - 100) // increases as user scrolls

            // zoom the background at a slower rate
            background.value.style.transform =
                'scale(' + (100 + backgroundSize * 0.4) / 100 + ')'
        }

        onMounted(() => {
            document.addEventListener('scroll', handleScroll)
        })

        onUnmounted(() => {
            document.removeEventListener('scroll', handleScroll)
        })

        return {
            background,
            first,
            second,
        }
    },
}
</script>

<template>
    <div class="root">
        <img class="background w-full" ref="background" src="../../../public/images/towngarden.jpg" />
        <div class="section section-1" ref="first">
            <div>
                <img src="../../../public/images/logo.png" />
            </div>
        </div>
        <div class="section section-2" ref="second">
            <div>
                <h2>Here's more info</h2>
                <p>Lorem ipsum dolor, sit amet consectetur adipisicing elit...</p>
            </div>
        </div>
        <div class="section section-3">
            <h2>New Section</h2>
            <p>Content for the new section...</p>
        </div>
    </div>
</template>

<style scoped>
img.background {
    /* Positioning */
    position: fixed;
    top: 0;
    left: 0;
}

.section {
    min-height: 100vh;
    position: relative;
}

.section > div {
    position: fixed;
    color: white;
    /* centers this div */
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}

.section-1 {
    margin-bottom: 500px; /* determines the gap between our two sections */
    font-size: 4em;
}

.section-2 {
    opacity: 0; /* defaults to 0 because it's not visible */
}

.section-2 div {
    background-color: rgba(255, 255, 255, 0.7);
    color: black;
    text-align: center;
    padding: 50px;
    max-width: 300px;
}

.section-2 h2 {
    font-size: 2em;
    margin-bottom: 40px;
}

.section-2 p {
    line-height: 150%;
}

.section-3 {
    min-height: 100vh;
    background-color: white;
    color: black;
    padding: 50px;
    text-align: center;
}

.section-3 h2 {
    font-size: 2em;
    margin-bottom: 40px;
}

.section-3 p {
    line-height: 150%;
}
</style>
