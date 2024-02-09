<template>
  <NuxtLayout name="custom">
    <div>
      <!-- total user -->
      <div class="flex">
        <StatisticCard :Data="user" />
        <StatisticCard :Data="statisticMoney" />
      </div>
      <div>
        <div class="flex justify-end">
          <button type="button"
            class="flex justify-end px-4 py-2 text-sm font-medium text-white bg-gray-500 rounded-md hover:bg-gray-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75"
            @click="addUser()">
            Thêm Mới
          </button>
        </div>
        <div v-for="(post, index) in postData" :key="index">
          <PostCard :dataPost="post" @clickShowPopup="showPopup" @clickCloseAllPopup="closeAllPopup"
            @clickOnDelete="onDelete" @clickEdit="onEdit" />
        </div>
      </div>
    </div>
  </NuxtLayout>
</template>

<style scoped></style>
<script setup>
import { computed } from 'vue';
import { useDecodeTokenStore } from '#imports';
import StatisticCard from '~/components/StatisticCard.vue';
import PostCard from '~/components/PostCard.vue';
const router = useRouter()
const decoded = useDecodeTokenStore();

decoded.decodeToken;
var totalUser = computed(() => {
  return decoded?.totalUser || 0;
});
var user = {
  title: 'Total User',
  data: totalUser,
  color: 'red',
};
var statisticMoney = {
  title: 'Total Money',
  data: 0,
  color: 'blue',
};
const addUser = () => {
  router.push('/home/createPost');
};
const postData = [
  {
    idPost: "1",
    title: "sàádf",
    content: "sà",
    img: "https://d1hjkbq40fs2x4.cloudfront.net/2016-01-31/files/1045.jpg",
  },
  {
    idPost: "2",
    title: "sdfbà",
    content: "sà",
    img: "https://d1hjkbq40fs2x4.cloudfront.net/2016-01-31/files/1045.jpg",
  },
  {
    idPost: "3",
    title: "sàsdv",
    content: "sà",
    img: "https://d1hjkbq40fs2x4.cloudfront.net/2016-01-31/files/1045.jpg",
  }

];
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
const closeAllPopup = () => {
  console.log("closeAllPopup");
  var popups = document.querySelectorAll('.boxAction');
  let overlay = document.querySelector(".overlay");
  overlay.style.display = "none";
  popups.forEach(p => p.style.display = "none");
};

//edit
var isEdit = false;
var editData = null;
const onEdit = (id) => {
  console.log("edit", id);
  const data = postData.find(s => s.idPost === id);
  editData = {
    ...data
  };
  isEdit = true;
    router.push({
    path: '/home/createPost',
    query: { isEdit: isEdit, editData: JSON.stringify(editData) }
  });
  closeAllPopup();

};

</script>
