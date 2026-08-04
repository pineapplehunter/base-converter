<template>
  <section :class="sectionClasses">
    <div class="mb-6 flex flex-col gap-2 sm:flex-row sm:items-end sm:justify-between">
      <div>
        <h2 :class="headingClasses">Convert between bases</h2>
        <p :class="subheadingClasses">Type a value and press Enter to sync every field.</p>
      </div>

      <button :class="resetButtonClasses" type="button" @click="reset">Reset</button>
    </div>

    <form class="grid gap-4 md:grid-cols-2" @submit.prevent>
      <label v-for="field in fields" :key="field.key" class="space-y-2">
        <span :class="labelClasses">{{ field.label }}</span>
        <input
          v-model="values[field.key]"
          :name="field.key"
          :placeholder="field.placeholder"
          :class="inputClasses"
          type="text"
          @keyup.enter="convert(field.key)"
        />
      </label>
    </form>

    <p v-if="message" :class="messageClasses">
      {{ message }}
    </p>

    <p :class="footerNoteClasses">Supports arbitrarily large integers via BigInt.</p>
  </section>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from "vue";

type Theme = "light" | "dark";
type BaseKey = "bin" | "oct" | "dec" | "hex";

type Props = {
  theme: Theme;
};

const props = defineProps<Props>();

const values = reactive<Record<BaseKey, string>>({
  bin: "",
  oct: "",
  dec: "",
  hex: "",
});

const message = ref("");

const fields = [
  { key: "bin", label: "BIN (Binary)", placeholder: "101010" },
  { key: "oct", label: "OCT (Octal)", placeholder: "52" },
  { key: "dec", label: "DEC (Decimal)", placeholder: "42" },
  { key: "hex", label: "HEX (Hexadecimal)", placeholder: "2A" },
] as const satisfies ReadonlyArray<{ key: BaseKey; label: string; placeholder: string }>;

const bases: Record<BaseKey, number> = {
  bin: 2,
  oct: 8,
  dec: 10,
  hex: 16,
};

const sectionCommonClasses = "w-full rounded-3xl border p-6 backdrop-blur";
const sectionClasses = computed(
  () =>
    sectionCommonClasses +
    (props.theme === "dark"
      ? " border-white/10 bg-white/5 shadow-2xl shadow-slate-950/40"
      : " border-slate-200 bg-white shadow-2xl shadow-slate-200/40"),
);

const headingCommonClasses = "text-xl font-semibold";
const headingClasses = computed(
  () => headingCommonClasses + (props.theme === "dark" ? " text-white" : " text-slate-900"),
);

const subheadingClasses = computed(
  () => "text-sm" + (props.theme === "dark" ? " text-slate-400" : " text-slate-600"),
);

const resetButtonCommonClasses =
  "inline-flex items-center justify-center rounded-full border px-4 py-2 text-sm font-medium transition";
const resetButtonClasses = computed(
  () =>
    resetButtonCommonClasses +
    (props.theme === "dark"
      ? " border-sky-400/40 text-sky-200 hover:bg-sky-400/10"
      : " border-sky-400/40 text-sky-700 hover:bg-sky-400/10"),
);

const labelCommonClasses = "text-sm font-medium";
const labelClasses = computed(
  () => labelCommonClasses + (props.theme === "dark" ? " text-slate-300" : " text-slate-700"),
);

const inputCommonClasses =
  "w-full rounded-2xl border px-4 py-3 text-lg outline-none transition placeholder:text-slate-500 focus:border-sky-400 focus:ring-2 focus:ring-sky-400/20";
const inputClasses = computed(
  () =>
    inputCommonClasses +
    (props.theme === "dark"
      ? " border-white/10 bg-slate-950/60 text-white"
      : " border-slate-200 bg-slate-50 text-slate-900 placeholder:text-slate-400"),
);

const messageCommonClasses = "mt-4 rounded-2xl border px-4 py-3 text-sm";
const messageClasses = computed(
  () =>
    messageCommonClasses +
    (props.theme === "dark"
      ? " border-amber-400/30 bg-amber-400/10 text-amber-200"
      : " border-amber-500/30 bg-amber-100 text-amber-900"),
);

const footerNoteClasses = computed(
  () => "mt-6 text-sm" + (props.theme === "dark" ? " text-slate-400" : " text-slate-600"),
);

function reset(): void {
  values.bin = "";
  values.oct = "";
  values.dec = "";
  values.hex = "";
  message.value = "";
}

function parseBigInt(input: string, base: number): bigint | null {
  const text = input.trim().toLowerCase();
  if (!text) {
    return null;
  }

  let sign = 1n;
  let start = 0;

  if (text[0] === "+") {
    start = 1;
  } else if (text[0] === "-") {
    sign = -1n;
    start = 1;
  }

  if (start >= text.length) {
    return null;
  }

  let total = 0n;
  const radix = BigInt(base);

  for (const char of text.slice(start)) {
    const digit = Number.parseInt(char, 36);
    if (Number.isNaN(digit) || digit >= base) {
      return null;
    }

    total = total * radix + BigInt(digit);
  }

  return total * sign;
}

function convert(base: BaseKey): void {
  const parsed = parseBigInt(values[base], bases[base]);

  if (parsed === null) {
    message.value = values[base].trim() ? `Please enter a valid ${base.toUpperCase()} value.` : "";
    return;
  }

  values.bin = parsed.toString(2);
  values.oct = parsed.toString(8);
  values.dec = parsed.toString(10);
  values.hex = parsed.toString(16).toUpperCase();
  message.value = "";
}
</script>
