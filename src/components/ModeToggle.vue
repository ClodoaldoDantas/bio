<script lang="ts" setup>
import { SwitchRoot, SwitchThumb } from "radix-vue";
import { onMounted, ref, watch } from "vue";

type Theme = "light" | "dark";

const switchState = ref(false);
const theme = ref<Theme>("light");
const storageKey = "clodoaldobio:theme";

const setTheme = (value: Theme) => {
  theme.value = value;
  document.documentElement.dataset.theme = value;
  localStorage.setItem(storageKey, value);
};

const getThemeByMediaPreference = () => {
  const isDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
  return isDark ? "dark" : "light";
};

const getTheme = (): Theme => {
  const themeStorage = localStorage.getItem(storageKey);

  if (themeStorage) {
    return themeStorage as Theme;
  }

  const theme = getThemeByMediaPreference();
  return theme;
};

watch(switchState, (value) => {
  const newTheme = value ? "dark" : "light";

  if (theme.value === newTheme) {
    return;
  }

  setTheme(newTheme);
});

onMounted(() => {
  const theme = getTheme();

  setTheme(theme);
  switchState.value = theme === "dark";
});
</script>

<template>
  <div class="mode-toggle-wrapper">
    <span>☀️ Light</span>

    <SwitchRoot v-model:checked="switchState" class="switch-root">
      <SwitchThumb class="switch-thumb" />
    </SwitchRoot>

    <span>🌙 Dark</span>
  </div>
</template>

<style lang="scss">
.mode-toggle-wrapper {
  display: flex;
  align-items: center;
  gap: 1rem;

  & > span {
    color: var(--text);
  }
}

.switch-root {
  all: unset;
  width: 42px;
  height: 25px;
  background-color: #9ca3af;
  border-radius: 9999px;
  position: relative;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

.switch-root[data-state="checked"] {
  background-color: var(--action);
}

.switch-thumb {
  display: block;
  width: 21px;
  height: 21px;
  background-color: white;
  border-radius: 9999px;
  transition: transform 100ms;
  transform: translateX(2px);
  will-change: transform;
}

.switch-thumb[data-state="checked"] {
  transform: translateX(19px);
}
</style>
