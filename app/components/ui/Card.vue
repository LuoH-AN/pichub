<template>
  <div :class="cardClasses">
    <!-- Header -->
    <div v-if="$slots.header" class="px-6 py-4 border-b border-gray-200">
      <slot name="header" />
    </div>

    <!-- Body -->
    <div class="px-6 py-4">
      <slot />
    </div>

    <!-- Footer -->
    <div v-if="$slots.footer" class="px-6 py-4 border-t border-gray-200 bg-gray-50">
      <slot name="footer" />
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  shadow: {
    type: String,
    default: 'md',
    validator: (value) => ['none', 'sm', 'md', 'lg'].includes(value)
  }
})

const cardClasses = computed(() => {
  const baseClasses = 'bg-white rounded-lg overflow-hidden'

  const shadowClasses = {
    none: '',
    sm: 'shadow-sm',
    md: 'shadow-lg',
    lg: 'shadow-xl'
  }

  return `${baseClasses} ${shadowClasses[props.shadow]}`
})
</script>