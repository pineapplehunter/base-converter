<template>
  <v-container>
    <v-responsive class="mx-auto" max-width="1000">
      <v-text-field label="BIN(Binary)" type="text" v-model="bin" @keyup.enter="change('bin')" id="bin" />
      <v-text-field label="OCT(Octal)" type="text" v-model="oct" @keyup.enter="change('oct')" id="oct" />
      <v-text-field label="DEC(Decimal)" type="text" v-model="dec" @keyup.enter="change('dec')" id="dec" />
      <v-text-field label="HEX(Hexadecimal)" type="text" v-model="hex" @keyup.enter="change('hex')" id="hex" />
    </v-responsive>
  </v-container>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

type Base = "bin" | "oct" | "dec" | "hex"

export default defineComponent({
  setup() {
    let num = ref();
    let bin = ref("");
    let oct = ref("");
    let dec = ref("");
    let hex = ref("");

    const change = (base: Base) => {
      if (base == "bin") {
        let text = bin.value;
        if (text == "") {
          num.value = 0;
          return;
        }
        num.value = parseInt(text, 2);
      } else if (base == "oct") {
        let text = oct.value;
        if (text == "") {
          num.value = 0;
          return;
        }
        num.value = parseInt(text, 8);
      } else if (base == "dec") {
        let text = dec.value;
        if (text == "") {
          num.value = 0;
          return;
        }
        num.value = parseInt(text, 10);
      } else if (base == "hex") {
        let text = hex.value;
        if (text == "") {
          num.value = 0;
          return;
        }
        num.value = parseInt(text, 16);
      }

      bin.value = num.value.toString(2);
      oct.value = num.value.toString(8);
      dec.value = num.value.toString(10);
      hex.value = num.value.toString(16);
    };

    return { bin, oct, dec, hex, change };
  },
});
</script>
