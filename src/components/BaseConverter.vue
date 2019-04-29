<template>
  <div class="container">
    <v-text-field label="BIN(Binary)" outline type="text" :value="bin" @keyup.enter="change('bin')" id="bin" /><br>
    <v-text-field label="OCT(Octal)" outline type="text" :value="oct" @keyup.enter="change('oct')" id="oct" /><br>
    <v-text-field label="DEC(Decimal)" outline type="text" :value="dec" @keyup.enter="change('dec')" id="dec" /><br>
    <v-text-field label="HEX(Hexadecimal)" outline type="text" :value="hex" @keyup.enter="change('hex')" id="hex" />
  </div>
</template>

<script>
export default {
  name: "BaseConverter",
  data () {
    return {
      num: 200,
      empty: true,
      check: 1
    }
  },
  computed: {
    bin () {
      if (this.empty) return ""
      return this.num.toString(2);
    },
    oct () {
      if (this.empty) return ""
      return this.num.toString(8);
    },
    dec () {
      if (this.empty) return ""
      return this.num.toString(10);
    },
    hex () {
      if (this.empty) return ""
      return this.num.toString(16).toUpperCase();
    }
  },
  methods: {
    change (base) {
      if (base == "bin") {
          let text = document.querySelector("#bin").value
          if (text == "") {
            this.num = 0
            this.empty = true
            return
          }
          this.empty = false
          this.num = parseInt(text, 2)
      } else if (base == "oct"){
          let text = document.querySelector("#oct").value
          if (text == "") {
            this.num = 0
            this.empty = true
            return
          }
          this.empty = false
          this.num = parseInt(text, 8)
      } else if(base == "dec"){
          let text = document.querySelector("#dec").value
          if (text == "") {
            this.num = 0
            this.empty = true
            return
          }
          this.empty = false
          this.num = parseInt(text, 10)
      } else if (base == "hex") {
          let text = document.querySelector("#hex").value
          if (text == "") {
            this.num = 0
            this.empty = true
            return
          }
          this.empty = false
          this.num = parseInt(text, 16)
      }
    }
  }
}
</script>