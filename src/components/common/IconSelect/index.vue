<script setup lang="ts">
import { icons } from './icons'

interface Props {
  disabled?: boolean
}
const props = withDefaults(defineProps<Props>(), {
  disabled: false,
})

const value = defineModel('value', { type: String })
const searchValue = ref('')
const showPopover = ref(false)

const { t } = useI18n()

const iconList = computed(() => icons.filter(item => item.includes(searchValue.value)))

function handleSelectIcon(icon: string) {
  value.value = icon
  showPopover.value = false
}
</script>

<template>
  <n-popover v-model:show="showPopover" placement="bottom" trigger="click" :disabled="props.disabled">
    <template #trigger>
      <n-input :value="value" readonly :placeholder="t('components.iconSelector.inputPlaceholder')">
        <template #suffix>
          <nova-icon :icon="value" />
        </template>
      </n-input>
    </template>
    <template #header>
      <n-input v-model:value="searchValue" type="text" :placeholder="t('components.iconSelector.searchPlaceholder')" />
    </template>
    <div class="w-400px">
      <div v-if="iconList.length > 0" class="grid grid-cols-9 h-auto overflow-auto gap-1">
        <div
          v-for="(item, index) in iconList" :key="index" class="border border-gray-200 m-2px p-5px flex-center"
          @click="handleSelectIcon(item)"
        >
          <nova-icon :icon="item" :size="24" />
        </div>
      </div>
      <n-empty v-else class="w-full" />
    </div>
  </n-popover>
</template>

<style scoped></style>
