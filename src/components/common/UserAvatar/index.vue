<script setup lang='ts'>
import { computed } from 'vue'
import { NAvatar } from 'naive-ui'
import { useUserStore } from '@/store'
import defaultAvatar from '@/assets/avatar.jpg'
import { isString } from '@/utils/is'

const userStore = useUserStore()

const userInfo = computed(() => userStore.userInfo)
</script>

<template>
  <div class="flex items-center overflow-hidden">
    <div class="w-10 h-10 overflow-hidden rounded-full shrink-0">
      <template v-if="isString(userInfo.avatar) && userInfo.avatar.length > 0">
        <NAvatar
          class="defaultAvatar"
          size="large"
          round
          :src="userInfo.avatar"
          :fallback-src="defaultAvatar"
        />
      </template>
      <template v-else>
        <NAvatar class="defaultAvatar" size="large" round :src="defaultAvatar" />
      </template>
    </div>
    <div class="flex-1 min-w-0 ml-2">
      <h2 class="overflow-hidden font-bold text-md text-ellipsis whitespace-nowrap">
        <!-- {{ userInfo.name ?? '小黑子' }} -->
        {{ $t('common.username') }}
      </h2>
      <p class="overflow-hidden text-xs text-gray-500 text-ellipsis whitespace-nowrap">
        <!-- <span
          v-if="isString(userInfo.description) && userInfo.description !== ''"
          v-html="userInfo.description"
        /> -->
        {{ $t('common.userDescription') }}
      </p>
    </div>
  </div>
</template>

<style>
.defaultAvatar{
  transform: rotateY(360deg);
  transition: transform 1s ease-in-out;
  animation: spin 2s linear infinite; /* 添加无限循环的旋转动画 */
}
@keyframes spin {
  from {
    transform: rotateY(0deg);
  }
  to {
    transform: rotateY(360deg);
  }
}
</style>
