<script setup>
const router = useRouter();
const route = useRoute();

// 串接 API 取得房型詳細資料
// API path : https://nuxr3.zeabur.app/api/v1/rooms/{id}
// 將資料渲染至下方的 div.room-page 區塊

console.log(route.params.id);
const {id} = route.params
const roomData = ref([]);
fetch(`https://nuxr3.zeabur.app/api/v1/rooms/${id}`)
  .then((response) => {
    if (!response.ok) {
      throw new Error("取得房型資料失敗");
    }
    return response.json();
  })
  .then((data) => {
    const { result } = data;
    roomData.value = result;
  })
  .catch((error) => {
    console.error("發生錯誤:", error);
  });

  const isProvide = function(isProvideBoolean = false){
    return isProvideBoolean? '提供' : '未提供'
  }
</script>

<template>
  <div>
    <h2>房型詳細頁面</h2>
    {{ roomData }}
    <div class="container">
      <button @click="router.go(-1)">回上一頁</button>
      <div class="row justify-content-center">
        <div class="col-md-6">
          <div class="room-page">
            <div class="room-header">
              <h1 class="room-name">{{ roomData.name }}</h1>
              <p class="room-description">
                {{ roomData.description }}
              </p>
            </div>

            <div class="room-gallery">
              <img
                :src="roomData.imageUrl"
                :alt="roomData.name"
                class="room-main-image"
              />
              <div class="room-image-list">
                <img
                  v-for="(item, ind) in roomData.imageUrlList"
                  :key="ind"
                  :src="item"
                  :alt="`圖片${ind + 1}`"
                />
              </div>
            </div>

            <div class="room-info">
              <div class="info-block">
                <h2>房間資訊</h2>
                <p>面積: {{ roomData.areaInfo }}</p>
                <p>床型: {{ roomData.bedInfo }}</p>
                <p>最多容納人數: {{ roomData.maxPeople }}</p>
                <p>價格: NT$ {{ roomData.price }}</p>
              </div>

              <div class="info-block">
                <h2>房間配置</h2>
                <ul v-for="(item,ind) in roomData.layoutInfo" :key="ind">
                  <li>{{ item.title }} : {{ isProvide(item.isProvide)}}</li>
                  <!-- <li>市景: 提供</li>
                  <li>獨立衛浴: 提供</li>
                  <li>樓層電梯: 提供</li> -->
                </ul>
              </div>

              <div class="info-block">
                <h2>房內設施</h2>
                <ul v-for="(item,ind) in roomData.facilityInfo" :key="ind">
                  <li>{{ item.title }} : {{ isProvide(item.isProvide)}}</li>
                  <!-- <li>平面電視: 提供</li>
                  <li>吹風機: 提供</li>
                  <li>冰箱: 提供</li>
                  <li>熱水壺: 提供</li>
                  <li>檯燈: 提供</li>
                  <li>衣櫥: 提供</li>
                  <li>書桌: 提供</li> -->
                </ul>
              </div>

              <div class="info-block">
                <h2>客房備品</h2>
                <ul v-for="(item,ind) in roomData.amenityInfo" :key="ind">
                  <li>{{ item.title }} : {{ isProvide(item.isProvide)}}</li>
                  <!-- <li>衛生紙: 提供</li>
                  <li>拖鞋: 提供</li>
                  <li>沐浴用品: 提供</li>
                  <li>刮鬍刀: 提供</li>
                  <li>刷牙用品: 提供</li>
                  <li>罐裝水: 提供</li>
                  <li>梳子: 提供</li> -->
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.room-page {
  max-width: 1200px;
  margin: 0 auto;
  background-color: #fff;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

.room-header {
  text-align: center;
  margin-bottom: 30px;
}

.room-name {
  font-size: 2rem;
  color: #333;
}

.room-description {
  font-size: 1rem;
  color: #666;
}

.room-gallery {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 30px;
}

.room-main-image {
  width: 100%;
  max-width: 600px;
  height: auto;
  border-radius: 10px;
  margin-bottom: 20px;
}

.room-image-list {
  display: flex;
  justify-content: center;
  gap: 10px;
}

.room-image-list img {
  width: 100px;
  height: auto;
  border-radius: 5px;
  cursor: pointer;
  transition: transform 0.3s;
}

.room-image-list img:hover {
  transform: scale(1.1);
}

.room-info {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.info-block {
  background-color: #f0f0f0;
  padding: 15px;
  border-radius: 10px;
}

.info-block h2 {
  font-size: 1.5rem;
  margin-bottom: 10px;
  color: #444;
}

.info-block p,
.info-block ul {
  font-size: 1rem;
  color: #555;
}

.info-block ul {
  list-style: none;
  padding-left: 0;
}

.info-block ul li {
  margin-bottom: 5px;
}
</style>