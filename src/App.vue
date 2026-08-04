<template>
  <div :class="shellClasses">
    <header :class="headerClasses">
      <div class="mx-auto flex w-full max-w-5xl items-center justify-between gap-4 px-6 py-4">
        <div>
          <p class="text-sm uppercase tracking-[0.3em] text-sky-400">Base Converter</p>
          <h1 :class="titleClasses">進数変換</h1>
        </div>

        <div class="flex items-center gap-3 text-sm">
          <button :class="themeButtonClasses" type="button" @click="toggleTheme">
            {{ themeLabel }}
          </button>

          <a
            :class="linkButtonClasses"
            href="https://github.com/pineapplehunter/base-converter"
            target="_blank"
            rel="noreferrer"
          >
            GitHub
          </a>
        </div>
      </div>
    </header>

    <main class="mx-auto flex w-full max-w-5xl flex-1 items-center px-6 py-12">
      <BaseConverter :theme="theme" />
    </main>

    <footer :class="footerClasses">
      made by
      <a
        class="font-semibold transition hover:text-sky-400"
        href="https://twitter.com/daniel_program"
        target="_blank"
        rel="noreferrer"
      >
        @daniel_program
      </a>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref, watch } from "vue";

import BaseConverter from "./components/BaseConverter.vue";

type Theme = "light" | "dark";

const theme = ref<Theme>(getInitialTheme());
const isDark = computed(() => theme.value === "dark");

const shellCommonClasses = "min-h-screen transition-colors duration-300";
const shellClasses = computed(
  () =>
    shellCommonClasses +
    (isDark.value ? " bg-slate-950 text-slate-100" : " bg-slate-50 text-slate-900"),
);

const headerCommonClasses = "backdrop-blur";
const headerClasses = computed(
  () =>
    headerCommonClasses +
    (isDark.value
      ? " border-b border-white/10 bg-slate-900/80"
      : " border-b border-slate-200 bg-white/85"),
);

const footerCommonClasses = "px-6 py-6 text-center text-sm";
const footerClasses = computed(
  () =>
    footerCommonClasses +
    (isDark.value
      ? " border-t border-white/10 text-slate-400"
      : " border-t border-slate-200 text-slate-600"),
);

const titleCommonClasses = "text-2xl font-semibold";
const titleClasses = computed(() => titleCommonClasses + (isDark.value ? "" : " text-slate-900"));

const actionButtonCommonClasses = "rounded-full border px-4 py-2 transition";
const themeButtonClasses = computed(
  () =>
    actionButtonCommonClasses +
    (isDark.value
      ? " border-white/10 text-slate-200 hover:border-sky-400 hover:text-white"
      : " border-slate-200 text-slate-700 hover:border-sky-400 hover:text-slate-900"),
);

const linkButtonClasses = computed(
  () =>
    actionButtonCommonClasses +
    (isDark.value
      ? " border-white/10 text-slate-200 hover:border-sky-400 hover:text-white"
      : " border-slate-200 text-slate-700 hover:border-sky-400 hover:text-slate-900"),
);

const themeLabel = computed(() => (isDark.value ? "Light mode" : "Dark mode"));

function getInitialTheme(): Theme {
  if (typeof window === "undefined") {
    return "dark";
  }

  const stored = window.localStorage.getItem("theme");
  if (stored === "light" || stored === "dark") {
    return stored;
  }

  return window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light";
}

function applyTheme(currentTheme: Theme) {
  if (typeof document === "undefined") {
    return;
  }

  document.documentElement.dataset.theme = currentTheme;
  document.documentElement.style.colorScheme = currentTheme;
  window.localStorage.setItem("theme", currentTheme);
}

function toggleTheme() {
  theme.value = theme.value === "dark" ? "light" : "dark";
}

onMounted(() => {
  applyTheme(theme.value);
});

watch(theme, applyTheme);
</script>
