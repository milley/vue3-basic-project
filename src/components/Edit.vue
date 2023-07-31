<script setup>
// 编辑
import axios from 'axios';
import { ref } from 'vue'
// 弹框开关
const dialogVisible = ref(false)
const form = ref({
  name: '',
  place: '',
  id: ''
})

const open = (row) => {
  //console.log(row);
  form.value.name = row.name
  form.value.place = row.place
  form.value.id = row.id
  dialogVisible.value = true
}

defineExpose({
  open
})


const emit = defineEmits([
  'on-update'
])

const onUpdate = async () => {
  await axios.patch(`/edit/${form.value.id}`, {
    name: form.value.name,
    place: form.value.place
  })

  dialogVisible.value = false

  emit('on-update')
}


</script>

<template>
  <el-dialog v-model="dialogVisible" title="编辑" width="400px">
    <el-form label-width="50px">
      <el-form-item label="姓名">
        <el-input placeholder="请输入姓名" v-model="form.name" />
      </el-form-item>
      <el-form-item label="籍贯">
        <el-input placeholder="请输入籍贯" v-model="form.place" />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="onUpdate">确认</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<style scoped>
.el-input {
  width: 290px;
}
</style>
