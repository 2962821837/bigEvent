<script setup>
import { ref } from 'vue'
import { artDelChannelService, artGetChannelsService } from '@/api/article'
import ChannelEdit from './components/ChannelEdit.vue'
const channelList = ref([])
const dialog = ref()

const loading = ref(false)
const getChannelList = async () => {
  loading.value = true
  const res = await artGetChannelsService()
  // console.log(res.data.data)
  channelList.value = res.data.data
  loading.value = false
}
getChannelList()
// console.log(channelList.value)

import { Delete, Edit } from '@element-plus/icons-vue'
// import { ElMessage, ElMessageBox } from 'element-plus'
const onEditChannel = (row) => {
  dialog.value.open(row)
}

const onDelChannel = async (row) => {
  await ElMessageBox.confirm('你确认删除该分类信息吗？', '温馨提示', {
    type: 'warning',
    confirmButtonText: '确认',
    cancelButtonText: '取消'
  })
  await artDelChannelService(row.id)
  ElMessage({ type: 'success', message: '删除成功' })
  getChannelList()
}

const onAddChannel = () => {
  dialog.value.open({})
}

const onSuccess = () => {
  getChannelList()
}
</script>

<template>
  <PageContainer title="文章分类">
    <template #extra>
      <el-button type="primary" @click="onAddChannel">添加分类</el-button>
    </template>
    <el-table :data="channelList" style="width: 100%" v-loading="loading">
      <el-table-column prop="date" label="序号" width="100" type="index">
      </el-table-column>
      <el-table-column prop="cate_name" label="分类名称"></el-table-column>
      <el-table-column prop="cate_alias" label="分类别名"></el-table-column>
      <el-table-column prop="address" label="操作" width="100">
        <template #default="{ row }">
          <el-button
            :icon="Edit"
            circle
            plain
            type="primary"
            @click="onEditChannel(row)"
          ></el-button>
          <el-button
            :icon="Delete"
            circle
            plain
            type="danger"
            @click="onDelChannel(row)"
          ></el-button>
        </template>
      </el-table-column>
      <template #empty>
        <el-empty description="没有数据"></el-empty>
      </template>
    </el-table>
    <ChannelEdit ref="dialog" @success="onSuccess"></ChannelEdit>
  </PageContainer>
</template>

<style></style>
