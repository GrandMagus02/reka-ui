<script lang="ts">
import type { Ref } from 'vue'
import type { PrimitiveProps } from '@/Primitive'
import { onBeforeUnmount, ref, toRefs } from 'vue'
import { createContext } from '@/shared'
import { injectNumberFieldRootContext } from './NumberFieldRoot.vue'

export interface NumberFieldScrubAreaProps extends PrimitiveProps {
  /** The axis to scrub along */
  direction?: 'horizontal' | 'vertical'
  /** Pixels of movement required before a value change */
  pixelSensitivity?: number
  /** Distance in pixels before the virtual cursor wraps around */
  teleportDistance?: number
}

export interface NumberFieldScrubAreaContext {
  isScrubbing: Ref<boolean>
  isTouchInput: Ref<boolean>
  isPointerLockDenied: Ref<boolean>
  scrubAreaCursorRef: Ref<HTMLSpanElement | undefined>
  direction: Ref<'horizontal' | 'vertical'>
}

export const [injectNumberFieldScrubAreaContext, provideNumberFieldScrubAreaContext] = createContext<NumberFieldScrubAreaContext>('NumberFieldScrubArea')
</script>

<script setup lang="ts">
import { Primitive, usePrimitiveElement } from '@/Primitive'

const props = withDefaults(defineProps<NumberFieldScrubAreaProps>(), {
  as: 'span',
  direction: 'horizontal',
  pixelSensitivity: 2,
  teleportDistance: undefined,
})

const { direction } = toRefs(props)
const rootContext = injectNumberFieldRootContext()
const { primitiveElement, currentElement } = usePrimitiveElement()

const isScrubbing = ref(false)
const isTouchInput = ref(false)
const isPointerLockDenied = ref(false)
const scrubAreaCursorRef = ref<HTMLSpanElement>()

let accumulatedDelta = 0
let cursorX = 0
let cursorY = 0

const isWebKit = typeof navigator !== 'undefined' && /Safari/.test(navigator.userAgent) && !/Chrome/.test(navigator.userAgent)

function onPointerDown(event: PointerEvent) {
  if (rootContext.disabled.value || rootContext.readonly.value)
    return

  if (event.button !== 0)
    return

  isTouchInput.value = event.pointerType === 'touch'

  if (event.pointerType === 'touch')
    return

  event.preventDefault()
  isScrubbing.value = true
  rootContext.isScrubbing.value = true
  accumulatedDelta = 0

  cursorX = event.clientX
  cursorY = event.clientY

  // Request pointer lock for non-WebKit browsers
  if (!isWebKit && currentElement.value) {
    currentElement.value.requestPointerLock?.()?.catch(() => {
      isPointerLockDenied.value = true
    })
  }

  rootContext.inputEl.value?.focus()

  document.addEventListener('pointermove', onPointerMove)
  document.addEventListener('pointerup', onPointerUp)
}

function onPointerMove(event: PointerEvent) {
  if (!isScrubbing.value)
    return

  const delta = props.direction === 'horizontal' ? event.movementX : -event.movementY
  accumulatedDelta += delta

  // Update virtual cursor position
  if (document.pointerLockElement) {
    cursorX += event.movementX
    cursorY += event.movementY
  }
  else {
    cursorX = event.clientX
    cursorY = event.clientY
  }

  // Handle teleport wrapping
  if (props.teleportDistance != null) {
    const el = currentElement.value
    if (el) {
      const rect = el.getBoundingClientRect()
      const centerX = rect.left + rect.width / 2
      const centerY = rect.top + rect.height / 2

      if (props.direction === 'horizontal') {
        if (cursorX > centerX + props.teleportDistance)
          cursorX = centerX - props.teleportDistance
        else if (cursorX < centerX - props.teleportDistance)
          cursorX = centerX + props.teleportDistance
      }
      else {
        if (cursorY > centerY + props.teleportDistance)
          cursorY = centerY - props.teleportDistance
        else if (cursorY < centerY - props.teleportDistance)
          cursorY = centerY + props.teleportDistance
      }
    }
  }

  // Update cursor element position
  if (scrubAreaCursorRef.value) {
    scrubAreaCursorRef.value.style.transform = `translate3d(${cursorX}px, ${cursorY}px, 0)`
  }

  // Apply value changes based on pixel sensitivity
  if (Math.abs(accumulatedDelta) >= props.pixelSensitivity) {
    const steps = Math.trunc(accumulatedDelta / props.pixelSensitivity)
    accumulatedDelta -= steps * props.pixelSensitivity

    if (steps > 0)
      rootContext.handleIncrease(steps)
    else if (steps < 0)
      rootContext.handleDecrease(-steps)
  }
}

function onPointerUp() {
  isScrubbing.value = false
  rootContext.isScrubbing.value = false

  if (document.pointerLockElement)
    document.exitPointerLock()

  document.removeEventListener('pointermove', onPointerMove)
  document.removeEventListener('pointerup', onPointerUp)
}

function onTouchStart(event: TouchEvent) {
  event.preventDefault()
}

onBeforeUnmount(() => {
  document.removeEventListener('pointermove', onPointerMove)
  document.removeEventListener('pointerup', onPointerUp)
})

provideNumberFieldScrubAreaContext({
  isScrubbing,
  isTouchInput,
  isPointerLockDenied,
  scrubAreaCursorRef,
  direction,
})
</script>

<template>
  <Primitive
    v-bind="props"
    ref="primitiveElement"
    role="presentation"
    :style="{
      touchAction: 'none',
      userSelect: 'none',
      cursor: direction === 'horizontal' ? 'ew-resize' : 'ns-resize',
    }"
    :data-scrubbing="isScrubbing ? '' : undefined"
    @pointerdown="onPointerDown"
    @touchstart.passive="onTouchStart"
  >
    <slot />
  </Primitive>
</template>
