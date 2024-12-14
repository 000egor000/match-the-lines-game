<template>
    <div :class="{ active: active, circle: true }" :id="position.id" @click="connectCircles"
        :style="{ left: position.x + 'px', top: position.y + 'px' }">
        {{ position.id }}
    </div>
</template>

<script>
export default {
    props: {
        position: {
            type: Object,
            required: true,
        },
        active: {
            type: Boolean,
            default: false, // Изменено на false для большей ясности
            required: true
        }
    },

    methods: {
        connectCircles() {
            this.$emit('circle-clicked', this.position);
            this.$emit('change-active');
        },
    },
}
</script>

<style scoped>
.circle {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background-color: blue;
    position: absolute;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #fff;
}

.active {
    transform: scale(1.1);
    border: 2px solid red;
}
</style>
