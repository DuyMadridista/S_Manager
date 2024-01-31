<template>
  <NuxtLayout name="custom">
    <!-- <Sidebar />
        <div class="main ml-[255px]">
            <Header />
            <Footer />
        </div> -->
    <div class="flex">
      <!-- total user -->
      <StatisticCard :Data="user" />
      <StatisticCard :Data="statisticMoney" />
      <div>
        <button
          type="button"
          class="flex justify-end px-4 py-2 text-sm font-medium text-white bg-gray-500 rounded-md hover:bg-gray-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75"
          @click="addUser()"
        >
          Thêm Mới
        </button>
      </div>
    </div>
  </NuxtLayout>
</template>

<style scoped></style>
<script setup>
import { computed } from 'vue';
import { useDecodeTokenStore } from '#imports';
import StatisticCard from '~/components/StatisticCard.vue';
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
  //   console.log(authStore.isAuth);
  router.push('/home/createPost');
};
</script>
