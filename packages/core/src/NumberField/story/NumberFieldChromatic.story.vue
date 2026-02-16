<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { ref } from 'vue'
import { NumberFieldDecrement, NumberFieldIncrement, NumberFieldInput, NumberFieldRoot, NumberFieldScrubArea, NumberFieldScrubAreaCursor } from '..'

const value = ref(5)
</script>

<template>
  <Story
    title="NumberField/Chromatic"
    :layout="{ type: 'grid', width: '50%' }"
  >
    <Variant title="Uncontrolled">
      <NumberFieldRoot
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
        :default-value="5"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Controlled">
      <NumberFieldRoot
        v-model="value"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Decimal">
      <NumberFieldRoot
        :default-value="0"
        :format-options="{
          signDisplay: 'exceptZero',
          minimumFractionDigits: 1,
          maximumFractionDigits: 2,
        }"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Percentage">
      <NumberFieldRoot
        :default-value="0.05"
        :step="0.01"
        :format-options="{
          style: 'percent',
        }"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Currency values">
      <NumberFieldRoot
        :default-value="5"
        :format-options="{
          style: 'currency',
          currency: 'EUR',
          currencyDisplay: 'code',
          currencySign: 'accounting',
        }"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Units">
      <NumberFieldRoot
        :default-value="5"
        :format-options="{
          style: 'unit',
          unit: 'inch',
          unitDisplay: 'long',
        }"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Minimum">
      <NumberFieldRoot
        :default-value="5"
        :min="0"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Maximum">
      <NumberFieldRoot
        :default-value="5"
        :max="20"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Step (3)">
      <NumberFieldRoot
        :step="3"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Step (3) + Minimum (2)">
      <NumberFieldRoot
        :min="2"
        :step="3"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Step (3) + Minimum (2) + Maximum (21)">
      <NumberFieldRoot
        :min="2"
        :max="21"
        :step="3"
        class="text-sm flex items-center border bg-blackA7 border-blackA9 rounded-md text-white"
      >
        <NumberFieldDecrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:minus" />
        </NumberFieldDecrement>
        <NumberFieldInput class="bg-transparent w-20 tabular-nums focus:outline-0 p-1" />
        <NumberFieldIncrement class="p-2 disabled:opacity-20">
          <Icon icon="radix-icons:plus" />
        </NumberFieldIncrement>
      </NumberFieldRoot>
    </Variant>

    <Variant title="Scrub Area (Horizontal)">
      <NumberFieldRoot
        :default-value="50"
        class="text-sm flex items-center text-white"
      >
        <NumberFieldScrubArea class="p-2 cursor-ew-resize select-none">
          <label>Value</label>
          <NumberFieldScrubAreaCursor>
            <Icon
              icon="radix-icons:width"
              class="size-6 text-white"
            />
          </NumberFieldScrubAreaCursor>
        </NumberFieldScrubArea>
        <NumberFieldInput class="bg-blackA7 border border-blackA9 rounded-md w-20 tabular-nums focus:outline-0 p-1" />
      </NumberFieldRoot>
    </Variant>

    <Variant title="Scrub Area (Vertical)">
      <NumberFieldRoot
        :default-value="50"
        class="text-sm flex items-center text-white"
      >
        <NumberFieldScrubArea
          direction="vertical"
          class="p-2 cursor-ns-resize select-none"
        >
          <label>Value</label>
          <NumberFieldScrubAreaCursor>
            <Icon icon="radix-icons:cursor-arrow" />
          </NumberFieldScrubAreaCursor>
        </NumberFieldScrubArea>
        <NumberFieldInput class="bg-blackA7 border border-blackA9 rounded-md w-20 tabular-nums focus:outline-0 p-1" />
      </NumberFieldRoot>
    </Variant>

    <Variant title="Scrub Area (Disabled)">
      <NumberFieldRoot
        :default-value="50"
        disabled
        class="text-sm flex items-center text-white"
      >
        <NumberFieldScrubArea class="p-2 cursor-ew-resize select-none disabled:opacity-20">
          <label>Value</label>
          <NumberFieldScrubAreaCursor>
            <Icon icon="radix-icons:cursor-arrow" />
          </NumberFieldScrubAreaCursor>
        </NumberFieldScrubArea>
        <NumberFieldInput class="bg-blackA7 border border-blackA9 rounded-md w-20 tabular-nums focus:outline-0 p-1" />
      </NumberFieldRoot>
    </Variant>

    <Variant title="Scrub Area with Min/Max">
      <NumberFieldRoot
        :default-value="50"
        :min="0"
        :max="100"
        class="text-sm flex items-center text-white"
      >
        <NumberFieldScrubArea class="p-2 cursor-ew-resize select-none">
          <label>Opacity</label>
          <NumberFieldScrubAreaCursor>
            <Icon icon="radix-icons:cursor-arrow" />
          </NumberFieldScrubAreaCursor>
        </NumberFieldScrubArea>
        <NumberFieldInput class="bg-blackA7 border border-blackA9 rounded-md w-20 tabular-nums focus:outline-0 p-1" />
      </NumberFieldRoot>
    </Variant>
  </Story>
</template>
