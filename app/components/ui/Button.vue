<template>
  <button
    :class="buttonClasses"
    :disabled="disabled"
    @click="$emit('click', $event)"
  >
    <slot />
  </button>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  variant: {
    type: String,
    default: 'solid',
    validator: (value) => ['solid', 'ghost', 'outline', 'subtle'].includes(value)
  },
  color: {
    type: String,
    default: 'primary',
    validator: (value) => ['primary', 'gray', 'red', 'blue', 'green'].includes(value)
  },
  size: {
    type: String,
    default: 'md',
    validator: (value) => ['xs', 'sm', 'md', 'lg'].includes(value)
  },
  disabled: {
    type: Boolean,
    default: false
  }
})

defineEmits(['click'])

const buttonClasses = computed(() => {
  const baseClasses = 'inline-flex items-center justify-center font-medium rounded-md transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed'

  const sizeClasses = {
    xs: 'px-2 py-1 text-xs',
    sm: 'px-2.5 py-1.5 text-sm',
    md: 'px-4 py-2 text-sm',
    lg: 'px-4 py-2 text-base'
  }

  const variantClasses = {
    solid: {
      primary: 'bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500',
      gray: 'bg-gray-600 text-white hover:bg-gray-700 focus:ring-gray-500',
      red: 'bg-red-600 text-white hover:bg-red-700 focus:ring-red-500',
      blue: 'bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500',
      green: 'bg-green-600 text-white hover:bg-green-700 focus:ring-green-500'
    },
    ghost: {
      primary: 'text-blue-600 hover:bg-blue-50 focus:ring-blue-500',
      gray: 'text-gray-600 hover:bg-gray-50 focus:ring-gray-500',
      red: 'text-red-600 hover:bg-red-50 focus:ring-red-500',
      blue: 'text-blue-600 hover:bg-blue-50 focus:ring-blue-500',
      green: 'text-green-600 hover:bg-green-50 focus:ring-green-500'
    },
    outline: {
      primary: 'border border-blue-600 text-blue-600 hover:bg-blue-50 focus:ring-blue-500',
      gray: 'border border-gray-600 text-gray-600 hover:bg-gray-50 focus:ring-gray-500',
      red: 'border border-red-600 text-red-600 hover:bg-red-50 focus:ring-red-500',
      blue: 'border border-blue-600 text-blue-600 hover:bg-blue-50 focus:ring-blue-500',
      green: 'border border-green-600 text-green-600 hover:bg-green-50 focus:ring-green-500'
    },
    subtle: {
      primary: 'bg-blue-100 text-blue-700 hover:bg-blue-200 focus:ring-blue-500',
      gray: 'bg-gray-100 text-gray-700 hover:bg-gray-200 focus:ring-gray-500',
      red: 'bg-red-100 text-red-700 hover:bg-red-200 focus:ring-red-500',
      blue: 'bg-blue-100 text-blue-700 hover:bg-blue-200 focus:ring-blue-500',
      green: 'bg-green-100 text-green-700 hover:bg-green-200 focus:ring-green-500'
    }
  }

  return `${baseClasses} ${sizeClasses[props.size]} ${variantClasses[props.variant][props.color]}`
})
</script>