<script setup lang="ts">
// Figma の NumberButton / FunctionButton / OperatorButton に対応
type Kind = 'number' | 'function' | 'operator'

const props = withDefaults(defineProps<{
  kind?: Kind
  label: string
}>(), {
  kind: 'number'
})

// UButton の hover / focus で色が変わらないよう、同じ色で上書きする
const kindClass: Record<Kind, string> = {
  number: 'bg-button-number-bg hover:bg-button-number-bg focus-visible:bg-button-number-bg border-button-number-border',
  function: 'bg-button-function-bg hover:bg-button-function-bg focus-visible:bg-button-function-bg border-button-function-border',
  operator: 'bg-button-operator-bg hover:bg-button-operator-bg focus-visible:bg-button-operator-bg border-button-operator-border'
}
</script>

<template>
  <UButton
    color="neutral"
    variant="ghost"
    :class="[
      'aspect-square w-full justify-center rounded-full border p-0',
      'font-normal text-[38px] leading-none text-text-primary',
      'active:brightness-125',
      kindClass[props.kind]
    ]"
  >
    {{ label }}
  </UButton>
</template>
