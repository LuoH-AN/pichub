<template>
  <div :class="alertClasses">
    <div class="flex">
      <div class="flex-shrink-0">
        <!-- Alert Icon -->
        <svg v-if="color === 'red'" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd" />
        </svg>
        <svg v-else-if="color === 'green'" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
        </svg>
        <svg v-else-if="color === 'blue'" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd" />
        </svg>
        <svg v-else class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M8.257 3.099c.765-1.36 2.722-1.36 3.486 0l5.58 9.92c.75 1.334-.213 2.98-1.742 2.98H4.42c-1.53 0-2.493-1.646-1.743-2.98l5.58-9.92zM11 13a1 1 0 11-2 0 1 1 0 012 0zm-1-8a1 1 0 00-1 1v3a1 1 0 002 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
        </svg>
      </div>
      <div class="ml-3">
        <h3 v-if="title" class="text-sm font-medium" :class="titleColor">
          {{ title }}
        </h3>
        <div class="text-sm" :class="descriptionColor">
          <p>{{ description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  title: {
    type: String,
    default: ''
  },
  description: {
    type: String,
    default: ''
  },
  color: {
    type: String,
    default: 'red',
    validator: (value) => ['red', 'green', 'blue', 'yellow'].includes(value)
  },
  variant: {
    type: String,
    default: 'subtle',
    validator: (value) => ['solid', 'subtle', 'outline'].includes(value)
  }
})

const alertClasses = computed(() => {
  const baseClasses = 'rounded-lg p-4'

  const variantClasses = {
    solid: {
      red: 'bg-red-600 text-white',
      green: 'bg-green-600 text-white',
      blue: 'bg-blue-600 text-white',
      yellow: 'bg-yellow-600 text-white'
    },
    subtle: {
      red: 'bg-red-50 text-red-800',
      green: 'bg-green-50 text-green-800',
      blue: 'bg-blue-50 text-blue-800',
      yellow: 'bg-yellow-50 text-yellow-800'
    },
    outline: {
      red: 'border border-red-200 text-red-800',
      green: 'border border-green-200 text-green-800',
      blue: 'border border-blue-200 text-blue-800',
      yellow: 'border border-yellow-200 text-yellow-800'
    }
  }

  return `${baseClasses} ${variantClasses[props.variant][props.color]}`
})

const titleColor = computed(() => {
  return props.variant === 'solid' ? 'text-white' : `text-${props.color}-800`
})

const descriptionColor = computed(() => {
  return props.variant === 'solid' ? 'text-white' : `text-${props.color}-700`
})
</script>