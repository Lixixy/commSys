<script lang="ts" setup>
import {useRouter} from "vue-router";

const router = useRouter();

function goHome() {
  router.push('/')
}
</script>
<template>
  <div>
    <el-result
      icon="warning"
      title="404"
      sub-title="你找的页面走丢了~"
    >
      <template #extra>
        <el-button type="primary" @click="goHome">返回首页</el-button>
      </template>
    </el-result>
  </div>
</template>