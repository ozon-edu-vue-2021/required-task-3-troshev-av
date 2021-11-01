<template>
  <component
    :is="tag"
    class="ui-button"
    :class="[`ui-button_theme_${theme}`, `ui-button_style_${styleType}`]"
    :href="href"
  >
    <span class="ui-button__inner">
      <slot></slot>
    </span>

    <span class="ui-button__append" v-if="$slots.append">
      <slot name="append"></slot>
    </span>
  </component>
</template>

<script>
export default {
  name: "UiButton",
  props: {
    href: {
      type: String,
      default: null,
    },
    theme: {
      type: String,
      default: "primary",
    },
    styleType: {
      type: String,
      default: "plain",
    },
  },
  computed: {
    tag() {
      return this.href ? "a" : "button";
    },
  },
};
</script>

<style>
.ui-button {
  display: inline-flex;
  gap: 20px;
  justify-content: center;
  align-items: center;
  border: none;
  border-radius: 12px;
  padding: 0 8px;
  height: 40px;
  min-width: 40px;
  box-sizing: border-box;
  font-size: 20px;
  font-weight: 700;
  text-decoration: none;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.ui-button_theme_primary {
  --button-theme-hsl: var(--primary-hsl);
}

.ui-button_style_plain {
  --button-background-opacity: 0.1;

  background-color: hsla(
    var(--button-theme-hsl),
    var(--button-background-opacity)
  );
  color: hsl(var(--button-theme-hsl));
}

.ui-button_style_plain:hover {
  --button-background-opacity: 0.2;
}

.ui-button_style_plain:active {
  --button-background-opacity: 0.26;
}

.ui-button__inner {
  display: inline-flex;
}

.ui-button__append {
  display: inline-flex;
}
</style>
