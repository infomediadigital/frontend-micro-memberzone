<script setup lang="ts">
import { type HTMLAttributes } from 'vue'
import {
  DialogClose,
  DialogContent,
  type DialogContentEmits,
  type DialogContentProps,
  DialogOverlay,
  DialogPortal,
  useForwardPropsEmits,
} from 'radix-vue'
import { type MemberzoneVariants, memberzoneVariants } from '@/lib/cva'
import { X } from 'lucide-vue-next'
import { cn } from '@/lib/utils'

interface MemberzoneContentProps extends DialogContentProps {
  class?: HTMLAttributes['class']
  side?: MemberzoneVariants['side']
}

defineOptions({
  inheritAttrs: false,
})

const props = defineProps<MemberzoneContentProps>()

const emits = defineEmits<DialogContentEmits>()

const delegatedProps = computed(() => {
  // eslint-disable-next-line @typescript-eslint/no-unused-vars
  const { class: _, side, ...delegated } = props

  return delegated
})

const forwarded = useForwardPropsEmits(delegatedProps, emits)
</script>

<template>
  <DialogPortal>
    <DialogOverlay
      class="fixed inset-0 z-50 bg-black/80  data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0"
    />
    <DialogContent
      :class="cn(memberzoneVariants({ side }), props.class)"
      v-bind="{ ...forwarded, ...$attrs }"
    >
      <slot />

      <DialogClose
        class="absolute right-4 top-4 rounded-sm opacity-70 ring-offset-white transition-opacity hover:opacity-100 focus:outline-none focus:ring-2 focus:ring-neutral-950 focus:ring-offset-2 disabled:pointer-events-none data-[state=open]:bg-neutral-100"
      >
         <X class="cursor-pointer" />
      </DialogClose>
    </DialogContent>
  </DialogPortal>
</template>
