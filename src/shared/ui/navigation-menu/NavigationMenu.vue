<script setup lang="ts">
  import { type HTMLAttributes, computed } from 'vue';
  import {
    NavigationMenuRoot,
    type NavigationMenuRootEmits,
    type NavigationMenuRootProps,
    useForwardPropsEmits,
  } from 'radix-vue';
  import NavigationMenuViewport from './NavigationMenuViewport.vue';
  import { cn } from '@/shared/lib/cn';
  import { provideNavigationMenuCustomContext } from './navigation-menu.lib';

  const props = defineProps<
    NavigationMenuRootProps & { class?: HTMLAttributes['class'] }
  >();

  const emits = defineEmits<NavigationMenuRootEmits>();

  const delegatedProps = computed(() => {
    const { class: _, ...delegated } = props;

    return delegated;
  });

  const forwarded = useForwardPropsEmits(delegatedProps, emits);

  provideNavigationMenuCustomContext({
    orientation: props.orientation || 'horizontal',
  });
</script>

<template>
  <NavigationMenuRoot
    v-bind="forwarded"
    :class="
      cn(
        'relative z-10 flex max-w-max flex-1 items-center justify-center',
        props.class,
      )
    "
  >
    <slot />
    <NavigationMenuViewport />
  </NavigationMenuRoot>
</template>
