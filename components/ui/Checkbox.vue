<template>
  <div
    class="checkbox-outer"
    :class="{ disabled }"
    role="presentation"
    @click="toggle"
  >
    <button
      class="checkbox"
      role="checkbox"
      :disabled="disabled"
      :class="{ checked: value }"
      :aria-label="description"
      :aria-checked="value"
    >
      <CheckIcon v-if="value" aria-hidden="true" />
    </button>
    <!-- aria-hidden is set so screenreaders only use the <button>'s aria-label -->
    <p v-if="label" aria-hidden="true">{{ label }}</p>
    <slot v-else />
  </div>
</template>

<script>
import CheckIcon from '~/assets/images/utils/check.svg?inline'

export default {
  name: 'Checkbox',
  components: {
    CheckIcon,
  },
  props: {
    label: {
      type: String,
      default: '',
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    description: {
      type: String,
      default: '',
    },
    value: Boolean,
    clickEvent: {
      type: Function,
      default: () => {},
    },
  },
  methods: {
    toggle() {
      if (!this.disabled) {
        this.$emit('input', !this.value)
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.checkbox-outer {
  display: flex;
  align-items: center;
  cursor: pointer;

  &.disabled {
    opacity: 0.6;
    cursor: not-allowed;

    button {
      cursor: not-allowed;

      &:active,
      &:hover,
      &:focus {
        background-color: var(--color-button-bg);
      }
    }
  }

  p {
    user-select: none;
    padding: 0.2rem 0rem;
    margin: 0;
  }

  &:focus-visible,
  &:hover {
    color: var(--color-heading);

    button {
      background-color: var(--color-button-bg-hover);

      &.checked {
        background-color: var(--color-brand-hover);
      }
    }
  }
  &:active {
    color: var(--color-text-dark);

    button {
      background-color: var(--color-button-bg-active);

      &.checked {
        background-color: var(--color-brand-active);
      }
    }
  }
}

.checkbox {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;

  width: 1rem;
  height: 1rem;

  padding: 0;
  margin: 0 0.5rem 0 0;

  &.checked {
    background-color: var(--color-brand);
  }

  svg {
    color: var(--color-text-inverted);
    stroke-width: 0.2rem;
    height: 0.8rem;
    width: 0.8rem;
    flex-shrink: 0;
  }
}
</style>
