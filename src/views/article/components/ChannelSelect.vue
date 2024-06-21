<script setup>
import { ref, defineProps, defineEmits } from 'vue'
import { artGetChannelsService } from '../../../api/article'

defineProps({
  modelValue: {
    type: [Number, String]
  }
})

const emit = defineEmits(['update:modelValue'])
const channelList = ref([])
const getChannelList = async () => {
  const res = await artGetChannelsService()
  channelList.value = res.data.data
}

getChannelList()
</script>

<template>
  <el-select
    :model-value="modelValue"
    @update:modelValue="emit('update:modelValue', $event)"
    style="width: 100px"
  >
    <el-option
      v-for="channel in channelList"
      :key="channel.id"
      :label="channel.cate_name"
      :value="channel.id"
    ></el-option>
  </el-select>
</template>
