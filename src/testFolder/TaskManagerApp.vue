
<template>
    <div class="w-8/12 m-auto px-24 py-12 border border-gray-300 rounded-xl">
        <h1 class="text-2xl text-center font-bold uppercase"> Task Management</h1>
        <form action="">
            <label for="tast-tile"> Task Tile:</label><br>
            <input type="text" class="px-4 w-full mt-2 py-2 border berder-gray-200 rounded-md">

            <label for="priority"> Task priority:</label>
            <select name="" id="" class="px-4 w-full mt-2 py-2 border berder-gray-200 rounded-md">
                <option value=""> choose Task</option>
                <option value="high">High</option>
                <option value="meduim">Meduim</option>
                <option value="low">Low</option>
            </select>
            <button class=" w-full py-2 bg-blue-300 text-white rounded-md mt-4">Add Task</button>
           
        </form>
        <div class=" flex justify-between items-center mt-8">
            <p class="text-gray-600 text-md"> All tasks listend below:</p>
            <button class=" px-2 py-2 bg-blue-300 text-white rounded-md mt-4">Create Task</button>

        </div>

    </div>
</template>

<script setup>

</script>

<style lang="scss" scoped>

</style>