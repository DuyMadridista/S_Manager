<template>
    <div class="w-full">
        <div class="w-20  cursor-pointer flex items-center">
            <font-awesome-icon :icon="['fas', 'chevron-left']" />
            <button class="btn-SignUp hover:opacity-90 w-10 text-black font-bold border-b-2 border-black text-left ml-1"
                @click="$emit('close')">Back</button>
        </div>
        <div class="">
            <form @submit.prevent="saveUser" class="w-100">
                <div class="flex justify-between">
                    <div>
                        <div class="input p-2 mb-2 relative">
                            <label for="" class="text-base text-black font-semibold">Title</label>
                            <input
                                class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2 pr-12 mt-2 text-base"
                                v-model="surveyData.title" type="text" placeholder="Title"
                                :class="{ 'is-invalid': errorValue.title }" />
                            <div class="invalid-feedback text-left text-red-500" v-if="errorValue.title">{{ errorValue.title
                            }}</div>
                        </div>
                        <!-- Display added options -->
                        <div v-for="(option, index) in surveyData.options" :key="index" class="input p-2 mb-2 relative">
                            <label for="" class="text-base text-black font-semibold">Option {{ index + 1 }}</label>
                            <input
                                class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2 pr-12 mt-2 text-base"
                                v-model="surveyData.options[index].name" type="text" :placeholder="'Option ' + (index + 1)"
                                :class="{ 'is-invalid': errorValue.options[index] }" />
                            <div class="invalid-feedback text-left text-red-500" v-if="errorValue.options[index]">{{
                                errorValue.options[index] }}</div>
                        </div>
                        <div class="flex justify-center">
                            <button type="button" @click="addOption"
                                class="btn-SignIn hover:opacity-90 bg-green-500 text-black font-bold py-2 px-10 rounded mr-8">Add
                                option</button>
                        </div>
                    </div>
                </div>
                <div class="flex justify-center mt-8">
                    <button type="submit"
                        class="btn-SignIn hover:opacity-90 bg-green-500 text-black font-bold py-2 px-10 rounded mr-8">Submit</button>
                </div>
            </form>
        </div>
    </div>
</template>

<script setup>
import { ref, defineProps, defineEmits, onMounted } from 'vue';

const emit = defineEmits(['close']);

const props = defineProps({
    dataSurvey: {
        type: Object,
        default: null
    },
    isEdit: {
        type: Boolean,
        default: false
    }
});
const dataSurvey = props.dataSurvey
const isEdit = props.isEdit
const surveyData = ref({
    id: null,
    title: "",
    description: "",
    options: [],
});

const errorValue = ref({
    id: null,
    title: "",
    description: "",
    options: [],
});

const addOption = () => {
    surveyData.value.options.push({ name: "", vote: 0 });
};

const validate = () => {
    let isValue = true;

    if (surveyData.value.title === "") {
        isValue = false;
        errorValue.value.title = "Title is required";
    } else {
        errorValue.value.title = "";
    }

    for (let i = 0; i < surveyData.value.options.length; i++) {
        if (surveyData.value.options[i] === "") {
            isValue = false;
            errorValue.value.options[i] = "Option " + (i + 1).toString() + " is required";
        } else {
            errorValue.value.options[i] = "";
        }
    }

    return isValue;
};

const saveUser = () => {
    if (validate()) {
        console.log("Survey Data:", surveyData.value);
        emit('close');
        // Bạn có thể thực hiện gửi dữ liệu surveyData.value lên server ở đây
    }
};

// Nếu đang ở chế độ chỉnh sửa, gán dữ liệu của survey cần chỉnh sửa vào surveyData
onMounted(() => {
    console.log("data");
    console.log(dataSurvey.title);
    console.log(isEdit);
    if (isEdit && dataSurvey) {
        surveyData.value.id = dataSurvey.id;
        surveyData.value.title = dataSurvey.title;
        surveyData.value.description = dataSurvey.description;
        surveyData.value.options = [...dataSurvey.options];

        console.log("survey");
        console.log(surveyData.value);
    }
})
</script>
