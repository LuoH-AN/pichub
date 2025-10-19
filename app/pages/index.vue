<template>
  <div class="min-h-screen bg-gray-50 flex items-center justify-center p-4">
    <div class="max-w-4xl w-full">
      <!-- 页面标题 -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-gray-900 mb-2">文件上传</h1>
        <p class="text-gray-600">支持拖拽和点击上传，轻松管理您的文件</p>
      </div>

      <!-- 文件上传区域 -->
      <div class="mb-6">
        <FileUploader
          @files-selected="handleFilesSelected"
          @error="handleError"
          :max-file-size="50"
          :max-files="10"
          accepted-file-types="*"
        />
      </div>

      <!-- 错误提示 -->
      <Alert
        v-if="errorMessage"
        color="red"
        variant="subtle"
        title="上传错误"
        :description="errorMessage"
        class="mb-6"
      />

      <!-- 已选择的文件列表 -->
      <div v-if="selectedFiles.length > 0" class="mb-6">
        <div class="flex items-center justify-between mb-4">
          <div class="flex items-center space-x-2">
            <DocumentTextIcon class="w-5 h-5 text-blue-500" />
            <h2 class="text-lg font-medium text-gray-800">
              文件列表 ({{ selectedFiles.length }})
            </h2>
          </div>
          <Button
            color="gray"
            variant="ghost"
            size="sm"
            @click="clearFiles"
            title="清除所有"
          >
            <TrashIcon class="w-4 h-4" />
          </Button>
        </div>

        <div class="space-y-2">
          <div
            v-for="(file, index) in selectedFiles"
            :key="index"
            class="flex items-center justify-between p-3 border border-gray-200 rounded-md hover:bg-gray-50 transition-colors bg-white"
          >
            <div class="flex items-center space-x-4 flex-1 min-w-0">
              <div class="flex-shrink-0">
                <component
                  :is="getFileIcon(file.name)"
                  class="w-8 h-8 text-blue-500"
                />
              </div>
              <div class="flex-1 min-w-0">
                <p class="font-medium text-gray-900 truncate" :title="file.name">{{ file.name }}</p>
                <div class="flex items-center space-x-2 text-sm text-gray-500">
                  <span>{{ formatFileSize(file.size) }}</span>
                  <span>•</span>
                  <span>{{ getFileType(file.name) }}</span>
                </div>
              </div>
            </div>
            <div class="flex items-center space-x-2 flex-shrink-0 ml-4">
              <Button
                color="blue"
                variant="outline"
                size="sm"
                @click="downloadFile(file)"
                title="下载"
              >
                <ArrowDownTrayIcon class="w-4 h-4" />
              </Button>
              <Button
                color="red"
                variant="outline"
                size="sm"
                @click="removeFile(index)"
                title="删除"
              >
                <XMarkIcon class="w-4 h-4" />
              </Button>
            </div>
          </div>
        </div>

        <!-- 文件统计 -->
        <div class="flex items-center justify-between text-sm text-gray-600 mt-4 pt-4 border-t border-gray-200">
          <span>总文件数: {{ selectedFiles.length }}</span>
          <span>总大小: {{ formatFileSize(totalFileSize) }}</span>
        </div>
      </div>

      <!-- 空状态 -->
      <div v-else-if="!errorMessage" class="text-center py-12">
        <CloudIcon class="w-20 h-20 text-gray-300 mx-auto mb-4" />
        <h3 class="text-lg font-medium text-gray-900 mb-2">还没有选择任何文件</h3>
        <p class="text-gray-500">使用上方的上传区域选择文件</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import {
  ArrowDownTrayIcon,
  XMarkIcon,
  TrashIcon,
  DocumentTextIcon,
  TableCellsIcon,
  PresentationChartBarIcon,
  PhotoIcon,
  VideoCameraIcon,
  MusicalNoteIcon,
  ArchiveBoxIcon,
  DocumentIcon,
  CloudIcon
} from '@heroicons/vue/24/outline'
import Button from '~/components/ui/Button.vue'
import Icon from '~/components/ui/Icon.vue'
import Card from '~/components/ui/Card.vue'
import Alert from '~/components/ui/Alert.vue'

const selectedFiles = ref([])
const errorMessage = ref('')

// 计算总文件大小
const totalFileSize = computed(() => {
  return selectedFiles.value.reduce((total, file) => total + file.size, 0)
})

// 处理文件选择
const handleFilesSelected = (files) => {
  // 过滤掉重复的文件（基于文件名和大小）
  const existingFiles = new Set(
    selectedFiles.value.map(f => `${f.name}-${f.size}`)
  )
  
  const newFiles = files.filter(file =>
    !existingFiles.has(`${file.name}-${file.size}`)
  )
  
  if (newFiles.length > 0) {
    selectedFiles.value = [...selectedFiles.value, ...newFiles]
  }
  
  errorMessage.value = ''
}

// 处理错误
const handleError = (error) => {
  errorMessage.value = error
}

// 删除单个文件
const removeFile = (index) => {
  selectedFiles.value.splice(index, 1)
}

// 清除所有文件
const clearFiles = () => {
  selectedFiles.value = []
  errorMessage.value = ''
}

// 下载文件（模拟）
const downloadFile = (file) => {
  // 创建下载链接
  const url = URL.createObjectURL(file)
  const a = document.createElement('a')
  a.href = url
  a.download = file.name
  document.body.appendChild(a)
  a.click()
  document.body.removeChild(a)
  URL.revokeObjectURL(url)
}

// 格式化文件大小
const formatFileSize = (bytes) => {
  if (bytes === 0) return '0 Bytes'
  const k = 1024
  const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB']
  const i = Math.floor(Math.log(bytes) / Math.log(k))
  return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i]
}

// 获取文件图标
const getFileIcon = (filename) => {
  const extension = filename.split('.').pop().toLowerCase()
  const iconMap = {
    pdf: DocumentTextIcon,
    doc: DocumentTextIcon,
    docx: DocumentTextIcon,
    xls: TableCellsIcon,
    xlsx: TableCellsIcon,
    ppt: PresentationChartBarIcon,
    pptx: PresentationChartBarIcon,
    jpg: PhotoIcon,
    jpeg: PhotoIcon,
    png: PhotoIcon,
    gif: PhotoIcon,
    mp4: VideoCameraIcon,
    avi: VideoCameraIcon,
    mp3: MusicalNoteIcon,
    wav: MusicalNoteIcon,
    zip: ArchiveBoxIcon,
    rar: ArchiveBoxIcon,
    txt: DocumentIcon,
    default: DocumentIcon
  }
  return iconMap[extension] || iconMap.default
}

// 获取文件类型
const getFileType = (filename) => {
  const extension = filename.split('.').pop().toLowerCase()
  const typeMap = {
    pdf: 'PDF 文档',
    doc: 'Word 文档',
    docx: 'Word 文档',
    xls: 'Excel 表格',
    xlsx: 'Excel 表格',
    ppt: 'PPT 演示文稿',
    pptx: 'PPT 演示文稿',
    jpg: '图片文件',
    jpeg: '图片文件',
    png: '图片文件',
    gif: '图片文件',
    mp4: '视频文件',
    avi: '视频文件',
    mp3: '音频文件',
    wav: '音频文件',
    zip: '压缩文件',
    rar: '压缩文件',
    txt: '文本文件',
    default: '其他文件'
  }
  return typeMap[extension] || typeMap.default
}
</script>
