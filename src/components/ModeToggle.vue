<script lang="ts" setup>
import { onMounted, ref } from "vue";

type Theme = "light" | "dark";

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

onMounted(() => {
  const theme = getTheme();
  setTheme(theme);
});
</script>

<template>
  <button v-if="theme === 'dark'" type="button" @click="setTheme('light')">
    🌞 Tema Claro
  </button>

  <button v-else type="button" @click="setTheme('dark')">🌚 Tema Escuro</button>
</template>

<style lang="scss" scoped>
button {
  background-color: transparent;
  border: 0;

  font-size: 1.125rem;
  color: var(--text);
}
</style>
