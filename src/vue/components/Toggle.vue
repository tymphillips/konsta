<template>
  <component :is="component" ref="elRef" :class="c.base[state]">
    <input
      type="checkbox"
      :name="name"
      :value="value"
      :disabled="disabled"
      :readonly="readonly"
      :checked="checked"
      :class="c.input"
    />
    <span :class="c.inner[state]" />
    <span ref="rippleTargetElRef" :class="c.thumbWrap[state]">
      <span :class="c.thumb[state]" />
    </span>
    <slot />
  </component>
</template>
<script>
  import { computed, ref } from 'vue';
  import { ToggleClasses } from '../../shared/classes/ToggleClasses.js';
  import { ToggleColors } from '../../shared/colors/ToggleColors.js';
  import { useDarkClasses } from '../shared/use-dark-classes.js';
  import { useThemeClasses } from '../shared/use-theme-classes.js';
  import { useTouchRipple } from '../shared/use-touch-ripple.js';

  export default {
    name: 'k-toggle',
    props: {
      component: {
        type: String,
        default: 'label',
      },
      colors: {
        type: Object,
      },
      ios: {
        type: Boolean,
        default: undefined,
      },
      material: {
        type: Boolean,
        default: undefined,
      },
      checked: { type: Boolean, default: false },
      name: { type: String, default: undefined },
      value: { type: [Number, String], default: undefined },
      disabled: { type: Boolean, default: false },
      readonly: { type: Boolean, default: false },
      touchRipple: { type: Boolean, default: true },
    },
    setup(props, ctx) {
      const elRef = ref(null);
      const rippleTargetElRef = ref(null);

      useTouchRipple(rippleTargetElRef, props, { eventsElRef: elRef });

      const colors = computed(() =>
        ToggleColors(props.colors || {}, useDarkClasses)
      );

      const state = computed(() => (props.checked ? 'checked' : 'notChecked'));

      const c = useThemeClasses(props, () =>
        ToggleClasses(props, colors.value, ctx.attrs.class, useDarkClasses)
      );

      return {
        elRef,
        rippleTargetElRef,
        c,
        state,
      };
    },
  };
</script>
