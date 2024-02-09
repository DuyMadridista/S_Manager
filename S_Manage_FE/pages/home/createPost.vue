<template>
    <div class="px-20 py-10">
        <div class="w-20  cursor-pointer flex items-center">
            <font-awesome-icon :icon="['fas', 'chevron-left']" />
            <button class="btn-SignUp hover:opacity-90 w-10 text-black font-bold border-b-2 border-black text-left ml-1 "
                @click="Cancel()">
                Back
            </button>
        </div>

        <div class="flex ">

            <form @submit.prevent="saveUser()" class=" w-full pr-20 ">
                <div class=" flex justify-between">
                    <div class=" w-3/5 ">
                        <div class="input  p-2 mb-4 w-3/4 relative">
                            <label for="" class="text-base text-black font-semibold">
                                <Title></Title>
                            </label>
                            <input
                                class="bg-gray-200 focus:outline-none placeholder:text-slate-500 w-full border-2 rounded px-3 py-2  pr-12 mt-2 text-base"
                                name="title" v-model="postData.title" type="text" placeholder="Title" @blur="validate()"
                                :class="{ 'is-invalid': errorValue.title }" />

                            <div class="invalid-feedback text-left text-red-500 " v-if="errorValue.title"> {{
                                errorValue.title }} </div>
                        </div>
                        <div class="content">
                            <QuillEditor theme="snow" name="content" v-model:content="postData.content" content-type="html"
                                @blur="validate()"
                                style="width: 100%; border: 2px solid #ccc; border-radius: 5px;heigh:500px;" :options="{
                                    modules: {
                                        toolbar: [
                                            ['bold', 'italic', 'underline', 'strike'],        // các nút định dạng văn bản
                                            [{ 'list': 'ordered' }, { 'list': 'bullet' }],    // danh sách đánh số và danh sách chấm
                                            [{ 'indent': '-1' }, { 'indent': '+1' }],          // thay đổi lề văn bản
                                            ['image', 'link', 'video'],                       // chèn hình ảnh, liên kết, video
                                            ['clean']                                        // xóa định dạng
                                        ]
                                    },
                                }" :class="{ 'is-invalid': errorValue.content }" />
                            <div class="invalid-feedback text-left text-red-500 " v-if="errorValue.content"> {{
                                errorValue.content }} </div>
                        </div>
                    </div>
                    <div class="w-2/5 flex flex-col justify-start items-center">
                        <div>
                            <img id="imageDisplay" alt="img" class="w-[500px] h-[600px] rounded-3xl shadow-xl ">
                            <input type="file" id="imageInput" name="img">
                        </div>
                    </div>
                </div>
                <div class="flex justify-center mt-6 w-3/4">
                    <button type="submit"
                        class="btn-SignIn hover:opacity-90 bg-green-500 text-black font-bold py-2 px-10 rounded mr-8">
                        Submit
                    </button>
                </div>
            </form>

        </div>

    </div>
</template>
<script setup>

import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';
import { notify } from '@kyvg/vue3-notification';
import { QuillEditor } from '@vueup/vue-quill'
import '@vueup/vue-quill/dist/vue-quill.snow.css';
import displayIMG from '../../../middleware/displayIMG';

definePageMeta({
    layout: 'admin'
});

const router = useRouter();
const route = useRoute();
const userId = route.params.id;

const config = useRuntimeConfig();
const URL_BE = config.public.API_BASE_BE;

// const dataRole = ref([]);
// const props = defineProps({
//     editData: {
//         type: Object,
//         default: null
//     },
//     isEdit: {
//         type: Boolean,
//         default: false
//     }
// });
// const editData = props.editData
// const isEdit = props.isEdit


const postData = ref({
    id_post: "",
    title: "",
    content: "",
    img: ""
});

const errorValue = ref({
    id_post: "",
    title: "",
    content: "",
});

const validate = () => {
    let isValue = true;

    if (postData.value.title == "") {
        isValue = false;
        errorValue.value.title = "Title is required";
    } else {
        errorValue.value.title = "";
    }

    if (postData.value.content == "") {
        isValue = false;
        errorValue.value.content = "Content is required";
    } else {
        errorValue.value.content = "";
    }
    return isValue;
}

// const getRole = () => {
//     axios
//         .get(`${URL_BE}/api/v1/author/get_all_role`)
//         .then((res) => {
//             return dataRole.value = res.data
//         })
//         .catch((error) => {
//             console.log(error);
//         })
// }



// onMounted(async () => {
//     displayIMG("imageInput", "imageDisplay");
//     getRole();
// });


const saveUser = () => {
    console.log("submit nè");
    if (validate()) {

        // Lấy tệp avatar từ input
        const imgInput = document.getElementById('imageInput');
        const selectedImg = imgInput.files[0];

        const data = postData.value;
        console.log(data);
        console.log(selectedImg);

        // Tạo FormData để chứa dữ liệu và tệp avatar
        const formData = new FormData();

        if (selectedImg) {
            formData.append('img', selectedImg);
        }

        for (const key in data) {
            formData.append(key, data[key]);
        }
        console.log(formData);

        // axios
        //     .post(`${URL_BE}/api/v1/users`, formData)
        //     .then((response) => {
        //         console.log(response);
        //         notify({
        //             type: 'success',
        //             title: 'Create success',
        //             text: 'Create success',
        //         });
        //         Cancel();
        //     })
        //     .catch((error) => {
        //         console.log(error);
        //     });
    }
};

const Cancel = () => {
    router.push('/home');
}
onMounted(() => {
    const isEdit = route.query.isEdit;
    const editData = JSON.parse(route.query.editData);
    console.log(isEdit);
    console.log(editData);
    if (isEdit && editData) {
        postData.value.id_post = editData.id_post;
        postData.value.title = editData.title;
        postData.value.content = editData.content;
        postData.value.img = editData.img;
        console.log("data");
        console.log(postData.value);
    }
})
</script>