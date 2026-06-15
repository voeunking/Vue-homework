<template>
    <div class="py-5 border">
        <h1>Cart</h1>
        <p>title: {{ props.title }}</p>
        <p>Like Numbers: {{ props.nbLike }}</p>
        <button @click="handleLike()" class="btn bg-gray-200 px-4 py-2 rounded-xl">Like</button>

    </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';
const emits = defineEmits(["like"]);
const handleLike = ()=>{
    emits("like",{id: props.id})
}
const props = defineProps({
    id:{
        type: Number,
        reqiured: true
    },
    title:{
        type: String,
        reqiured: true
    },
    nbLike:{
        type: Number,
        reqiured:true
        
    }
})

</script>

<style lang="scss" scoped>

</style>