<script setup>
import Edit from './components/Edit.vue'
import axios from 'axios'
import { ref, onMounted } from 'vue'

// 列表渲染
const list = ref([])
const getList = async () => {
  const res = await axios.get('/list')
  //console.log(res)
  list.value = res.data
}

onMounted(() => {
  getList();
})


// 删除功能
const onDeleted = async (id) => {
  console.log(id)
  await axios.delete(`/del/${id}`)
  getList()
}

// TODO: 编辑功能
const editRef = ref(null)

const onEdit = (row) => {
  editRef.value.open(row)
}
</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{row}">
          <el-button type="primary" @click="onEdit(row)" link>编辑</el-button>
          <el-button type="danger" @click="onDeleted(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="editRef" @on-update="getList" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
