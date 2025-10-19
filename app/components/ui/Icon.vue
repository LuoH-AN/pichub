<template>
  <component :is="iconComponent" :class="iconClasses" v-if="iconComponent" />
  <span v-else class="w-4 h-4 bg-gray-300 rounded"></span>
</template>

<script setup>
import { computed } from 'vue'

// Import Heroicons
import {
  CloudArrowUpIcon,
  FolderOpenIcon,
  ArrowDownTrayIcon,
  TrashIcon,
  XMarkIcon,
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

const props = defineProps({
  name: {
    type: String,
    required: true
  },
  class: {
    type: String,
    default: ''
  }
})

const iconMap = {
  'i-heroicons-cloud-arrow-up': CloudArrowUpIcon,
  'i-heroicons-folder-open': FolderOpenIcon,
  'i-heroicons-arrow-down-tray': ArrowDownTrayIcon,
  'i-heroicons-trash': TrashIcon,
  'i-heroicons-x-mark': XMarkIcon,
  'i-heroicons-document-text': DocumentTextIcon,
  'i-heroicons-table': TableCellsIcon,
  'i-heroicons-presentation-chart': PresentationChartBarIcon,
  'i-heroicons-photo': PhotoIcon,
  'i-heroicons-video': VideoCameraIcon,
  'i-heroicons-musical-note': MusicalNoteIcon,
  'i-heroicons-archive-box': ArchiveBoxIcon,
  'i-heroicons-document': DocumentIcon,
  'i-heroicons-cloud': CloudIcon
}

const iconComponent = computed(() => {
  const component = iconMap[props.name]
  console.log(`Icon component lookup: ${props.name} -> ${component ? 'FOUND' : 'NOT FOUND'}`)
  return component || DocumentIcon
})

const iconClasses = computed(() => {
  return props.class
})
</script>