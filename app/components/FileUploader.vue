<template>
  <div
    class="relative border-2 border-dashed border-gray-300 rounded-lg p-6 transition-all duration-200"
    :class="{
      'border-blue-400 bg-blue-50': isDragging,
      'hover:border-gray-400': !isDragging
    }"
    @dragenter.prevent="handleDragEnter"
    @dragover.prevent="handleDragOver"
    @dragleave.prevent="handleDragLeave"
    @drop.prevent="handleDrop"
  >
    <!-- 上传区域 - 整个区域可点击 -->
    <div
      class="text-center cursor-pointer"
      @click="triggerFileInput"
    >
      <CloudArrowUpIcon
        class="w-12 h-12 text-gray-400 mx-auto mb-4"
      />
      <p class="text-lg font-medium text-gray-700 mb-2">
        {{ isDragging ? '释放文件以上传' : '拖拽文件到此处或点击选择' }}
      </p>
      <p class="text-sm text-gray-500">
        支持多个文件，最大 {{ maxFileSize }}MB
      </p>

      <!-- 隐藏的文件输入 -->
      <input
        ref="fileInput"
        type="file"
        multiple
        :accept="acceptedFileTypes"
        class="hidden"
        @change="handleFileSelect"
      />
    </div>

    <!-- 拖拽遮罩 -->
    <div
      v-if="isDragging"
      class="absolute inset-0 bg-blue-100 bg-opacity-50 rounded-lg flex items-center justify-center"
    >
      <div class="text-center">
        <ArrowDownTrayIcon class="w-8 h-8 text-blue-600 mb-2" />
        <p class="text-blue-700 font-medium">释放文件以上传</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { CloudArrowUpIcon, FolderOpenIcon, ArrowDownTrayIcon } from '@heroicons/vue/24/outline'
import Button from '~/components/ui/Button.vue'
import Icon from '~/components/ui/Icon.vue'

const props = defineProps({
  maxFileSize: {
    type: Number,
    default: 10 // MB
  },
  acceptedFileTypes: {
    type: String,
    default: '*' // Accept all file types
  },
  maxFiles: {
    type: Number,
    default: 10
  }
})

const emit = defineEmits(['files-selected', 'error'])

const fileInput = ref(null)
const isDragging = ref(false)

// 触发文件选择
const triggerFileInput = () => {
  fileInput.value?.click()
}

// 处理文件选择
const handleFileSelect = (event) => {
  const files = Array.from(event.target.files)
  processFiles(files)
}

// 处理拖拽进入
const handleDragEnter = (e) => {
  e.preventDefault()
  isDragging.value = true
}

// 处理拖拽悬停
const handleDragOver = (e) => {
  e.preventDefault()
  isDragging.value = true
}

// 处理拖拽离开
const handleDragLeave = (e) => {
  e.preventDefault()
  // 检查是否真的离开了上传区域
  if (!e.currentTarget.contains(e.relatedTarget)) {
    isDragging.value = false
  }
}

// 处理文件拖拽
const handleDrop = (e) => {
  e.preventDefault()
  isDragging.value = false

  const files = Array.from(e.dataTransfer.files)
  processFiles(files)
}

// 处理文件验证和选择
const processFiles = (files) => {
  // 检查文件数量
  if (files.length > props.maxFiles) {
    emit('error', `一次最多只能上传 ${props.maxFiles} 个文件`)
    return
  }

  // 验证文件大小
  const validFiles = []
  const maxSizeBytes = props.maxFileSize * 1024 * 1024

  for (const file of files) {
    if (file.size > maxSizeBytes) {
      emit('error', `文件 "${file.name}" 超过最大限制 ${props.maxFileSize}MB`)
      continue
    }
    validFiles.push(file)
  }

  if (validFiles.length > 0) {
    emit('files-selected', validFiles)
  }

  // 清空文件输入
  if (fileInput.value) {
    fileInput.value.value = ''
  }
}
</script>

<style scoped>
/* 拖拽时的视觉反馈 */
.border-dashed {
  transition: all 0.2s ease-in-out;
}
</style>