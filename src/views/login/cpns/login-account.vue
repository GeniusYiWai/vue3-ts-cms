<template>
  <div class="login-account">
    <el-form label-width="60px" :rules="rules" :model="account" ref="formRef">
      <el-form-item label="账号" prop="name">
        <el-input v-model="account.name" />
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="account.password" show-password />
      </el-form-item>
    </el-form>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'
import { useStore } from 'vuex'
import { ElForm } from 'element-plus'
import localCache from '@/utils/cache'

export default defineComponent({
  setup() {
    const store = useStore()

    const account = reactive({
      name: localCache.getCache('name') ?? '',
      password: localCache.getCache('password') ?? ''
    })
    const rules = {
      name: [
        {
          required: true,
          message: '用户名是必传内容~',
          trigger: 'blur'
        },
        {
          pattern: /^[a-z0-9]{5,10}$/,
          message: '用户名必须是5~10个字母或者数字~',
          trigger: 'blur'
        }
      ],
      password: [
        {
          required: true,
          message: '密码是必传内容~',
          trigger: 'blur'
        },
        {
          pattern: /^[a-z0-9]{3,}$/,
          message: '用户名必须是3位以上的字母或者数字~',
          trigger: 'blur'
        }
      ]
    }

    const formRef = ref<InstanceType<typeof ElForm>>()

    const loginAction = (isKeepPassword: boolean) => {
      formRef.value?.validate((valid) => {
        if (valid) {
          // 1.判断是否需要记住密码
          if (isKeepPassword) {
            // 本地缓存
            localCache.setCache('name', account.name)
            localCache.setCache('password', account.password)
          } else {
            localCache.deleteCache('name')
            localCache.deleteCache('password')
          }

          // 2.开始进行登录验证
          store.dispatch('login/accountLoginAction', { ...account })
        }
      })
    }

    return {
      account,
      rules,
      loginAction,
      formRef
    }
  }
})
</script>

<style scoped></style>
