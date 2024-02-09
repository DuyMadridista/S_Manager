<template>
    <div>
        <h1>{{ voteFor }} </h1>
        <div v-for="(option, index) in options" :key="index" class="option">
            <span>{{ option }}</span>
            <div class="progress-bar" :style="{ width: result[index] + '%' }"></div>
            <span>{{ result[index] }}%</span>
            <!-- <button class=" m-1 bg-yellow-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="editOption(index)">Edit</button>
            <button class="  bg-red-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="deleteOption(index)">Delete</button> -->
        </div> 

        <!-- Button for adding a new option -->
        <button @click="addNewOption" class="bg-green-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Add New Option</button>
    </div>
</template>

<script setup>
const { props, emit } = defineProps(['voteFor', 'options', 'result']);

const editOption = (index) => {
    emit('edit', index);
};

const deleteOption = (index) => {
    emit('delete', index);
};

const addNewOption = () => {
    // Emit the 'add' event to indicate the request for adding a new option
    emit('add');
};
</script>

<style scoped>
.option {
    margin-bottom: 10px;
    display: flex;
    align-items: center;
}

.progress-bar {
    background-color: #4caf50;
    height: 20px;
    margin: 0 10px;
}
</style>
