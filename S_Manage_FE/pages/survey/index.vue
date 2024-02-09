<script setup>

import axios from "axios";

import { notify } from '@kyvg/vue3-notification';
import SearchItem from "~/components/SearchItem.vue";
import voteResult from "~/components/voteResult.vue";
import createVote from "./createVote.vue";
import voteCard from "~/components/voteCard.vue";
const emits = defineEmits(['edit', 'delete', 'add']);

const config = useRuntimeConfig();
const API_BE = config.public.API_BASE_BE;

const userData = ref([]);
const searchKeyword = ref("");
const router = useRouter();

definePageMeta({
    layout: 'custom'
});


const closeAllPopup = () => {
    //console.log("closeAllPopup");
    var popups = document.querySelectorAll('.boxAction');
    let overlay = document.querySelector(".overlay");
    overlay.style.display = "none";
    popups.forEach(p => p.style.display = "none");
};
const autoClosePopup = (e) => {
    if (e.target.classList.contains('.boxAction'))
        return;
    closeAllPopup();
    console.log("autoClosePopup");
};
const showPopup = (id) => {
    console.log(id + " clicked!");
    closeAllPopup();
    var popup = document.querySelector("#action-" + id);
    let overlay = document.querySelector(".overlay");
    if (popup.dataset.display == "none") {
        popup.style.display = "block";
        popup.dataset.display = "block";
        overlay.style.display = "block";
    }
    else {
        popup.style.display = "none";
        popup.dataset.display = "none";
        overlay.style.display = "none";
    }
};

const onDelete = (id) => {
    closeAllPopup();
    axios
        .delete(`${API_BE}/api/v1/users/${id}`)
        .then((response) => {
            notify({
                title: "Delete Success",
                text: "User deleted",
                type: "success",
            });
            console.log(response.data);
            fetchData();
        })
        .catch((error) => {
            notify({
                title: "Delete Failed",
                text: error.response.data.message,
                type: "error",
            });
        });
};
const viewResult = (id => {
    closeAllPopup();
    router.push(`/survey/viewResult/${id}`);
})


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
const showModal = ref(false);

const addSurvey = () => {
    showModal.value = true;
};

var editSurvey = null;
var isEdit = false;

const onEdit = (id) => {
    const survey = surveys.find(s => s.id === id);

    editSurvey = {
        ...survey
    };
    isEdit = true;
    showModal.value = true;
    // console.log("bên này nè");
    // console.log(editSurvey);
    // console.log("bên này nè");
}

const closeSurvey = () => {
    showModal.value = false;
    isEdit = false;
};
</script>
<template>
    <div class="box-border flex items-center justify-center w-full p-0 m-0 ">
        <div class="w-3/5 ">
            <div class="flex justify-between pt-10 container-header ">
                <div class="relative">
                    <SearchItem v-model="searchKeyword" />
                </div>
                <button type="button"
                    class="flex justify-end px-4 py-2 text-sm font-medium text-white bg-gray-500 rounded-md hover:bg-gray-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75"
                    @click="addSurvey()">
                    Thêm Mới
                </button>
            </div>
            <div class="container-body">
                <div v-for="(survey, index) in surveys" :key="index">
                    <!-- <voteResult :voteFor="survey.voteFor" :options="survey.options.map(option => option.name)"
                        :result="survey.options.map(option => option.votes)" @edit="handleEdit" @delete="handleDelete"
                        @add="handleAdd" /> -->
                    <voteCard :dataSurvey="survey" @clickShowPopup="showPopup" @clickCloseAllPopup="closeAllPopup"
                        @clickOnDelete="onDelete" @clickEdit="onEdit" @clickViewResult="viewResult" />
                </div>
            </div>
            <div class="absolute inset-0 z-50 flex items-center justify-center bg-slate-800 bg-opacity-50" v-if="showModal">
                <div class="bg-white p-8 rounded-md">
                    <createVote @close="closeSurvey" :isEdit="isEdit" :dataSurvey="editSurvey" />
                </div>
            </div>
        </div>
    </div>
</template>
