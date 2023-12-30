<script setup>
import { onMounted, ref } from 'vue'
import Edit from './components/Edit.vue'
import axios from 'axios';

// TODO: 列表渲染
// 声明响应式list->调用接口获取数据->后端数据赋值给list->绑定到table组件
const list = ref([])
const getList = async () => {
  // 接口调用
  const res = await axios.get('/list')
  //交给list
  list.value = res.data
}

onMounted(() => getList())

// TODO: 删除功能
// 获取当前行的id->调用接口删除->重新获取列表
const onDelete = async (id) => {
  //console.log(id)
  await axios.delete(`/del/${id}`)
  // 重新获取列表
  getList()
}



// TODO: 编辑功能
// 打开弹窗-> 回填数据->更新数据
const editRef = ref(null)
const onEdit = (row) => {
  // 打开弹窗
  // 回填数据
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
        <template #default="{ row }">
          <el-button type="primary" @click="onEdit(row)" link>编辑</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link>删除</el-button>
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
