<template>
  <div class="user">
    <div class="search">
      <yw-form v-bind="modalConfig" />
    </div>
    <page-search />
    <page-content ref="pageContentRef" pageName="users"></page-content>
    <page-modal ref="pageModalRef" pageName="users"></page-modal>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import YwForm from '@/base-ui/form'
import { modalConfig } from './config/modal.config'
export default defineComponent({
  name: 'user',
  components: {
    YwForm
  },
  setup() {
    return {
      modalConfig
    }
  }
})
</script>
<style scoped lang="less"></style>
