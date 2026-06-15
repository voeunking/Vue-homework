<template>
    <div class="card">
        <h2>name:{{ props.name }}</h2>
        <p>Role: {{ props.role }}</p>
        <p>Points: {{ props.points }}</p>
    </div>
</template>

<script setup>
const props = defineProps({
    name: {type: String, required:true},
    role: {type: String, default:'student'},
    points: {type: Number, default:0},
})

</script>

<style lang="scss" scoped>

</style>