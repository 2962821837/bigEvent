<script setup>
import { userUpdateInfoService } from '@/api/user'
import PageContainer from '@/components/PageContainer.vue'
import { useUserStore } from '@/stores'
import { ElMessage } from 'element-plus'
import { ref } from 'vue'

const formRef = ref()
const {
  user: { id, username, nickname, email },
  getUser
} = useUserStore()

const form = ref({
  id,
  username,
  nickname,
  email
})

const rules = {
  nickname: [
    { required: true, message: '请输入用户昵称', trigger: 'blur' },
    {
      pattern: /^\S{2,10}/,
      message: '昵称必须是2-10位的非空字符串',
      trigger: 'blur'
    }
  ],
  email: [
    { required: true, message: '请输入用户邮箱', trigger: 'blur' },
    { type: 'email', message: '邮箱格式不正确', trigger: 'blur' }
  ]
}

const submitForm = async () => {
  const valid = await formRef.value.validate()
  if (valid) {
    await userUpdateInfoService(form.value)
    await getUser()
    ElMessage.success('修改成功')
  }
}
</script>

<template>
  <page-container title="基本资料">
    <el-form :model="form" :rules="rules" ref="formRef" label-width="100px">
      <el-form-item label="登录名称">
        <el-input v-model="form.username" disabled></el-input>
      </el-form-item>
      <el-form-item label="用户昵称" prop="nickname">
        <el-input v-model="form.nickname"></el-input>
      </el-form-item>
      <el-form-item label="用户邮箱" prop="email">
        <el-input v-model="form.email"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm"> 提交修改</el-button>
      </el-form-item>
    </el-form>
  </page-container>
</template>
