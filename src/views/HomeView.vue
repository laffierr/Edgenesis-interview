<template>
  <div class="home">
    <div class="add">
      <el-button type="primary" @click="handleAdd">增加</el-button>
    </div>

    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="date" label="Date" width="180" />
      <el-table-column prop="name" label="Name" width="180" />
      <el-table-column prop="address" label="Address" />
      <el-table-column fixed="right" label="Operations" width="150">
        <template #default="scope">
          <el-button type="primary" size="small" @click="handleEdit(scope.$index, scope.row)"
            >编辑</el-button
          >
          <el-button type="primary" size="small" @click="handleDelete(scope.$index, scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <el-dialog v-model="dialogVisible" :title="title" width="60%">
      <el-form :model="form" label-width="80px" class="demo-form-inline">
        <el-form-item label="日期" prop="date">
          <el-input v-model="form.date"></el-input>
        </el-form-item>
        <el-form-item label="姓名" prop="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="地址" prop="address">
          <el-input v-model="form.address"></el-input>
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="handleSubmit('form')"> 确 定 </el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>
<script setup lang="ts">
import axios from 'axios'
import { ref, reactive } from 'vue'
const tableData = ref([])
const dialogVisible = ref(false)
const form = reactive({
  date: '',
  name: '',
  address: '',
})
const title = ref('添加')
const active = ref('')
const getSearch = () => {
  axios.get('https://www.fastmock.site/mock/5903235a886ef1aa2f446afbe614596e/api/search')
}

getSearch()
const handleAdd = () => {
  title.value = '添加'
  form.date = ''
  form.name = ''
  form.address = ''

  dialogVisible.value = true
}

const handleSubmit = (formName) => {
  if (formName === 'form') {
    if (title.value === '添加') {
      tableData.value.push({ ...form })
    } else if (title.value === '编辑') {
      const editedIndex = tableData.value.findIndex(item => item.date === form.date)
      if (editedIndex !== -1) {
        tableData.value[editedIndex] = { ...form }
      }
    }
  }

  dialogVisible.value = false
}

const handleEdit = (index, data) => {
  title.value = '编辑'
  form.date = data.date
  form.name = data.name
  form.address = data.address
  dialogVisible.value = true
}

const handleDelete = (index, data) => {
  const deleteIndex = tableData.value.findIndex(item => item.date === data.date)
  if (deleteIndex !== -1) {
    tableData.value.splice(deleteIndex, 1)
  }
}

</script>
<style scoped>
.add {
  overflow: hidden;
  margin-bottom: 24px;
}
.add button {
  float: right;
}
</style>
