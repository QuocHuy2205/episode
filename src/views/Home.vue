<!-- buổi 7 -->
<!-- <template>
  <div>
    <h2><strong>Danh sách</strong></h2>
    <div v-if="dang_tai" class="loading">Đang tải...</div>
    <ul v-else>
      <li v-for="nguoi in danh_sach_nguoi_dung" :key="nguoi.id" style="margin-bottom: 10px">
        <p><strong>Tên:</strong> {{ nguoi.name }}</p>
        <p style="margin-left: 20px">
          <em>Email: {{ nguoi.email }}</em>
        </p>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const danh_sach_nguoi_dung = ref([])
const dang_tai = ref(false)

const lay_danh_sach_nguoi_dung = async () => {
  dang_tai.value = true
  try {
    const phan_hoi = await axios.get('https://jsonplaceholder.typicode.com/users')
    danh_sach_nguoi_dung.value = phan_hoi.data
  } catch (loi) {
    console.error('Lỗi khi tải danh sách:', loi)
  }
  dang_tai.value = false
}

onMounted(() => {
  lay_danh_sach_nguoi_dung()
})
</script>

<style scoped>
.loading {
  font-style: italic;
}
ul {
  list-style-type: disc;
  padding-left: 20px;
}
</style> -->
<template>
  <div>
    <h2><strong>Danh sách</strong></h2>

    <!-- Thanh tìm kiếm -->
    <input
      type="text"
      v-model="tu_khoa"
      placeholder="Nhập tên hoặc email để tìm..."
      @input="bat_dau_tim"
      style="margin-bottom: 15px; padding: 5px; width: 300px"
    />


    <div v-if="dang_tai" class="loading">Đang tải...</div>
    <div v-else-if="dang_tim" class="loading">Đang tìm kiếm...</div>

    <ul v-else>
      <li v-for="nguoi in danh_sach_loc" :key="nguoi.id" style="margin-bottom: 10px">
        <p><strong>Tên:</strong> {{ nguoi.name }}</p>
        <p style="margin-left: 20px">
          <em>Email: {{ nguoi.email }}</em>
        </p>
      </li>
      <li v-if="danh_sach_loc.length === 0">
        <em>Không tìm thấy kết quả</em>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import axios from 'axios'

// Biến state
const danh_sach_nguoi_dung = ref([])
const dang_tai = ref(false) 
const dang_tim = ref(false) 
const tu_khoa = ref('')
let timeout_id = null

const lay_danh_sach_nguoi_dung = async () => {
  dang_tai.value = true
  try {
    const phan_hoi = await axios.get('https://jsonplaceholder.typicode.com/users')
    danh_sach_nguoi_dung.value = phan_hoi.data
  } catch (loi) {
    console.error('Lỗi khi tải danh sách:', loi)
  }
  dang_tai.value = false
}

const bat_dau_tim = () => {
  dang_tim.value = true
  clearTimeout(timeout_id)
  timeout_id = setTimeout(() => {
    dang_tim.value = false
  }, 300) 
}

const danh_sach_loc = computed(() => {
  if (!tu_khoa.value) return danh_sach_nguoi_dung.value
  const keyword = tu_khoa.value.toLowerCase()
  return danh_sach_nguoi_dung.value.filter(
    (nguoi) =>
      nguoi.name.toLowerCase().includes(keyword) || nguoi.email.toLowerCase().includes(keyword),
  )
})

onMounted(() => {
  lay_danh_sach_nguoi_dung()
})
</script>
