<script>
    import Data from './Data';
    export default {
        name: 'Rest',
        data() {
            return {
                currentString: Data[Math.floor(Math.random()*Data.length)],
                animateForward: true,
                animateBackward: false,
                timer: 5*60
            }
        },
        mounted() {
            this.$emit('rest')
            setTimeout(() => {
                this.$emit('restart')
            }, this.timer*1000)
            setTimeout(() => {
                this.animateBackward = true
                this.animateForward = false
            }, 18000)
            setTimeout(() => {
                this.animateForward = true
                this.animateBackward = false
            }, 20000)
            setInterval(() => {
                this.currentString = Data[Math.floor(Math.random()*Data.length)]
                setTimeout(() => {
                    this.animateBackward = true
                    this.animateForward = false
                }, 18000)
                setTimeout(() => {
                    this.animateBackward = false
                    this.animateForward = true
                }, 20000)
            }, 20000)
        }
    }
</script>

<template>
    <div class="text" v-bind:class="{'animateForward': animateForward, 'animateBackward': animateBackward}">
        {{ currentString }}
    </div>
</template>

<style scoped>
    .animateForward{
        animation: animateForward 2s;
    }
    @keyframes animateForward{
        0%{
            opacity: 0%;
        }
        100%{
            opacity: 100%;
        }
    }
    .animateBackward{
        animation: animateBackward 2s;
    }
    @keyframes animateBackward{
        0%{
            opacity: 100%;
        }
        100%{
            opacity: 0%;
        }
    }
    .text{
        width: 70vw;
        margin-left: 15vw;
        display: flex;
        align-items: center;
        justify-content: center;
        position: absolute;
        top: 40vh;
        color: white;
        font-family: 'Titillium Web', sans-serif;
        font-size: 4rem;
        text-align: center;
        cursor: default;
    }
</style>