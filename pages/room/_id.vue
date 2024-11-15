<script setup>
const route = useRoute()
const router = useRouter()
</script>
<template>
  <div>
    <h2>房型詳細頁面 {{ route.fullPath }}</h2>
    <button type="button"  @click="router.go(-1)">回到上一頁</button>
  </div>
</template>
<style scoped></style>