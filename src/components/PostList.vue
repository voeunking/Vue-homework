<template>
    <div>
        <PostCart
            v-for="post in posts"
            :key="post.id"
            :id="post.id"
            :title="post.title"
            :nbLike="post.nbLike"
            @like="increasNblike"
        />
    </div>
</template>


<script setup>
import PostCart from './PostCart.vue';
import {ref} from 'vue'
const increasNblike = (payload)=>{
    posts.value.forEach(post => {
        if(post.id=== payload.id){
            post.nbLike++;
        }
    });
}
const posts = ref([
    {id:1, title: 'Hello king', nbLike:1},
    {id:2, title: 'Hello world', nbLike:2}
])

</script>

<style lang="scss" scoped>

</style>