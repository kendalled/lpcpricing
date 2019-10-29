<template>
  <div class="custom-number-inputh-10 w-32 mx-auto mb-12">
    <label for="custom-input-number" class="w-full text-gray-700 text-sm font-semibold">{{ labelText }}
    </label>
    <div class="flex flex-row h-10 shadow-md w-full rounded-lg relative bg-transparent mt-1">
      <button class="functionbtn bg-white text-gray-600 hover:text-gray-700 hover:bg-gray-400 h-full w-20 rounded-l cursor-pointer outline-none" @click="$emit('decrement'), changeVal(0)">
        <span class="m-auto text-2xl font-thin">−</span>
      </button>
      <input
        type="number"
        disabled
        class="outline-none focus:outline-none text-center w-full bg-white font-semibold text-md hover:text-black focus:text-black  md:text-basecursor-default flex items-center text-gray-700  outline-none"
        name="custom-input-number"
        :value="val"
        @change="onChange()"
      >
      <button class="functionbtn bg-white text-gray-600 hover:text-gray-700 hover:bg-gray-400 h-full w-20 rounded-r cursor-pointer" @click="$emit('increment'), changeVal(1)">
        <span class="m-auto text-2xl font-thin">+</span>
      </button>
    </div>
  </div>
</template>

<style>
  .functionbtn:focus {
    outline: none;
  }
  input[type='number']::-webkit-inner-spin-button,
  input[type='number']::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  .custom-number-input input:focus {
    outline: none !important;
  }

  .custom-number-input button:focus {
    outline: none !important;
  }
</style>

<script>
export default {
  props: {
    value: {
      type: Number,
      default: 1
    },
    labelText: {
      type: String,
      default: 'Label Goes Here'
    },
    val: {
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      count: 1
    }
  },
  methods: {
    onChange (e) {
      this.$emit('input', this.count)
    },
    changeVal (way) {
      if (way === 0 && this.count !== 1) {
        this.count -= 1
      }
      if (way === 1) {
        this.count += 1
      }
    }
  }
}
</script>
