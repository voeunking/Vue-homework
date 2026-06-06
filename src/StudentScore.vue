<template>
    <div>
        <h1>Student Score</h1>
        <p>Hi {{ StudentName }}</p>
        <p>Here is total score: {{ Total }}</p>
        <p> Here is Average: {{ Average }}</p>
        <p>Status: {{ status }}</p>
        

    </div>
</template>

<script setup>
import {ref, computed, watch} from 'vue'
const StudentName = ref('king')
const Score= ref([
    {subject: "English", score:20},
    {subject: "PL", score:30},
    {subject: "Vue", score:50},
])

const Total = computed(()=>{
    let total = 0;
    Score.value.forEach(item=>{
        total += item.score
    })
    return total
});

const passMake = ref(50)
const Average = computed(()=>{
    return Total.value/Score.value.length
})

const status = computed(()=>{
    if (Average >passMake){
        return 'Pass'

    }else{
        return 'Fail' 
    }
})

watch(Score.value,(newValue) => {
    localStorage.setItem('score', JSON.stringify(newValue))
  },
  { deep: true }
)
</script>

