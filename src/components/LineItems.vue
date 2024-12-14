<template>
    <svg class="lines" v-if="lines.length">
        <line v-for="(line, index) in lines" :key="index" :x1="line.start ? line.start.x + circleRadius : 0"
            :y1="line.start ? line.start.y + circleRadius : 0"
            :x2="line.end ? line.end.x + circleRadius : line.start.x + circleRadius"
            :y2="line.end ? line.end.y + circleRadius : line.start.y + circleRadius" stroke="red" stroke-width="2" />
    </svg>
</template>

<script>
export default {
    props: {
        lines: {
            type: Array,
            default: () => [],
            required: true
        },
        circleRadius: {
            type: Number,
            default: 25,
            required: true
        }
    },
}
</script>

<style scoped>
svg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
}
</style>
