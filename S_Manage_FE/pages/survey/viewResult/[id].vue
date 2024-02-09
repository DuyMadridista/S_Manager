<template>
    <div class="box-border flex items-center justify-center w-full p-0 m-0 ">
        <div class="w-3/5 ">
            <div class="px-20 py-10">
                <div class="flex items-center w-20 cursor-pointer">
                    <font-awesome-icon :icon="['fas', 'chevron-left']" />
                    <button
                        class="btn-SignUp hover:opacity-90 w-10 text-[04364A] font-bold border-b-2 border-[04364A] text-left ml-1"
                        @click="Cancel()">
                        Back
                    </button>
                </div>
                <div class="container-body">
                    <voteResult :voteFor="data.title" :options="data.options.map(option => option.name)"
                        :result="data.options.map(option => option.votes)" @add="handleAdd" />
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
import { ref, onMounted, defineEmits } from 'vue';
import voteResult from '@/components/voteResult.vue';
import { useRouter } from 'vue-router';
import { useRoute } from 'vue-router';
const route = useRoute();
const router = useRouter();
definePageMeta({
    layout: 'custom'
});
const id = route.params.id;
const surveys = [{
    id: 1,
    title: "Best Framework",
    description: " alaa",
    options: [
        { name: "Vue", votes: 5 },
        { name: "React", votes: 2 },
        { name: "Angular", votes: 70 },
    ],
},
{
    id: 2,
    title: "Best Language",
    description: " alaa",
    options: [
        { name: "JavaScript", votes: 0 },
        { name: "Python", votes: 0 },
        { name: "Java", votes: 0 },
    ],
},
{
    id: 3,
    title: "Best Database",
    description: " alaa",
    options: [
        { name: "MongoDB", votes: 0 },
        { name: "MySQL", votes: 0 },
        { name: "PostgreSQL", votes: 0 },
    ],
},
];
var data = ref({
    id: 0,
    title: "",
    description: "",
    options: [
    ],
});
const getData = (id) => {

    data.value = surveys.find(survey => survey.id == id);
};
const Cancel = () => {
    router.push('/survey');
}
onMounted(async () => {


    console.log('id', id);
    if (id != 0) {
        getData(id);
    }
});
</script>>