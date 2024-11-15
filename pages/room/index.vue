<script setup>
const roomsList = ref([]);

// const data = await $fetch("https://nuxr3.zeabur.app/api/v1/rooms");
// console.log(data);
// roomsList.value = data.result;

fetch("https://nuxr3.zeabur.app/api/v1/rooms")
  .then((response) => {
    if (!response.ok) {
      throw new Error("取得房型資料失敗");
    }
    return response.json();
  })
  .then((data) => {
    const { result } = data;
    roomsList.value = result;
  })
  .catch((error) => {
    console.error("發生錯誤:", error);
  });
// 使用 fetch 或 axios 串接 前台房型 API ( GET )
// apiUrl : https://nuxr3.zeabur.app/api/v1/rooms
// response 回傳後，將資料寫入 roomsList 變數
// 使用 roomsList 變數在下方 template 渲染列表
const route = useRoute();
const router = useRouter();
</script>
<template>
  <div>
    <h2>房型 index {{ route.fullPath }}</h2>
    <div class="container mt-4">
      <div class="row justify-content-center">
        <div
          class="col-8 col-md-6 col-lg-3"
          v-for="room in roomsList"
          :key="room._id"
        >
          <div class="card h-100 shadow-sm" @click="router.push('/room/_id')">
            <img :src="room.imageUrl" class="card-img-top" alt="Room Image" />
            <div class="card-body d-flex flex-column">
              <h3 class="card-title">{{ room.name }}</h3>
              <p class="card-text flex-grow-1">{{ room.description }}</p>
              <ul class="list-unstyled">
                <li><strong>面積:</strong> {{ room.areaInfo }}</li>
                <li><strong>床型:</strong> {{ room.bedInfo }}</li>
                <li><strong>最大容納人數:</strong> {{ room.maxPeople }}</li>
                <li><strong>價格:</strong> {{ room.price }}</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
.card-img-top {
  object-fit: cover;
  height: 200px;
  max-width: 100%;
}
</style>