<script setup>

import axios from "axios";

import { notify } from '@kyvg/vue3-notification';

import UserCard from "~/components/UserCard.vue";
import SearchItem from "~/components/SearchItem.vue";

const emits = defineEmits(['clickShowPopup', 'clickCloseAllPopup', 'clickOnDelete', 'clickEditUser', 'update:searchKeyword']);

const config = useRuntimeConfig();
const API_BE = config.public.API_BASE_BE;

const userData = ref([]);
const searchKeyword = ref("");
const router = useRouter();

definePageMeta({
    layout: 'custom'
});
const closeAllPopup = () => {
    console.log("closeAllPopup");
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


const fetchData = async () => {
    try {
        const response = await axios.get(`${API_BE}/api/v1/users`);
        return userData.value = response.data;
    }
    catch (error) {
        return [];
    }
};

const filteredUsers = computed(() => {
    if (!searchKeyword.value) {
        return userData.value;
    }
    const keyword = searchKeyword.value.toLowerCase();
    return userData.value.filter((user) => {
        if (user) {
            const name = (user.name || '').toString(); // Convert to string
            const email = (user.email || '').toString(); // Convert to string
            const age = (user.age || '').toString(); // Convert to string
            const gender = (user.gender || '').toString(); // Convert to string
            const role = (user.role || '').toString(); // Convert to string

            return (
                name.toLowerCase().includes(keyword) ||
                email.toLowerCase().includes(keyword) ||
                age.toLowerCase().includes(keyword) ||
                gender.toLowerCase().includes(keyword) ||
                role.toLowerCase().includes(keyword)
            );
        }

        return false;
    });
});

onMounted(() => {
    // isLogin();
    fetchData();
});

const editUser = (id) => {
    closeAllPopup();
    router.push(`/users/${id}`);
};
const addUser = () => {
    //   console.log(authStore.isAuth);
    router.push('/users/createUser');
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
                    @click="addUser()">
                    Thêm Mới
                </button>
            </div>
            <div class="container-body">
                <div v-for="user in filteredUsers" :key="user.idUser">
                    <UserCard :DataUser="user" @clickShowPopup="showPopup" @clickCloseAllPopup="closeAllPopup"
                        @clickOnDelete="onDelete" @clickEditUser="editUser" />
                </div>
            </div>
        </div>
    </div>
</template>
