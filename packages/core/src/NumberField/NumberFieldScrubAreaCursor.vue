<script lang="ts">
import type { PrimitiveProps } from '@/Primitive'

export interface NumberFieldScrubAreaCursorProps extends PrimitiveProps {}
</script>

<script setup lang="ts">
import { computed } from 'vue'
import { Primitive } from '@/Primitive'
import { injectNumberFieldScrubAreaContext } from './NumberFieldScrubArea.vue'

const props = withDefaults(defineProps<NumberFieldScrubAreaCursorProps>(), {
  as: 'span',
})

const scrubAreaContext = injectNumberFieldScrubAreaContext()

const isVisible = computed(() =>
  scrubAreaContext.isScrubbing.value
  && !scrubAreaContext.isTouchInput.value
  && !scrubAreaContext.isPointerLockDenied.value,
)
</script>

<template>
  <Teleport to="body">
    <Primitive
      v-if="isVisible"
      :ref="(el: any) => { scrubAreaContext.scrubAreaCursorRef.value = el?.$el ?? el }"
      v-bind="props"
      role="presentation"
      :style="{
        position: 'fixed',
        top: 0,
        left: 0,
        pointerEvents: 'none',
        zIndex: 2147483647,
      }"
    >
      <slot />
    </Primitive>
  </Teleport>
</template>
