<!-- eslint-disable no-console -->
<script setup lang='ts'>
import { computed, ref } from 'vue'
import { NModal, NTabPane, NTabs } from 'naive-ui'
import General from './General.vue'
import Advanced from './Advanced.vue'
import { useAppStore, useAuthStore } from '@/store'
import { SvgIcon } from '@/components/common'
import NiGanMaMusic from '@/assets/ni_gan_ma.mp3'

interface Props {
  visible: boolean
}

interface Emit {
  (e: 'update:visible', visible: boolean): void
}

const props = defineProps<Props>()

const emit = defineEmits<Emit>()

const authStore = useAuthStore()

const isChatGPTAPI = computed<boolean>(() => !!authStore.isChatGPTAPI)

const active = ref('General')

const appStore = useAppStore()
const audioActive = computed(() => appStore.ikunAudio)
const audio = new Audio(NiGanMaMusic)
const show = computed({
  get() {
    if (props.visible === true && audioActive.value === true)
      audio.play()
    return props.visible
  },
  set(visible: boolean) {
    emit('update:visible', visible)
  },
})
</script>

<template>
  <NModal v-model:show="show" :auto-focus="false" preset="card" style="width: 95%; max-width: 640px">
    <div>
      <NTabs v-model:value="active" type="line" animated>
        <NTabPane name="General" tab="General">
          <template #tab>
            <SvgIcon class="text-lg" icon="ri:file-user-line" />
            <span class="ml-2">{{ $t('setting.general') }}</span>
          </template>
          <div class="min-h-[100px]">
            <General />
          </div>
        </NTabPane>
        <NTabPane v-if="isChatGPTAPI" name="Advanced" tab="Advanced">
          <template #tab>
            <SvgIcon class="text-lg" icon="ri:equalizer-line" />
            <span class="ml-2">{{ $t('setting.advanced') }}</span>
          </template>
          <div class="min-h-[100px]">
            <Advanced />
          </div>
        </NTabPane>
      </NTabs>
    </div>
  </NModal>
</template>
