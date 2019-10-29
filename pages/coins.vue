<template>
  <div id="pageroot" class="bg-gray-100 text-center mx-auto pt-24">
    <!-- Full width column -->
    <div class="flex mb-4">
      <h2 class="flex-1 py-2 text-gray-800 text-3xl">
        Quote Calculator for Challenge Coins
      </h2>
    </div>

    <!-- Five columns -->
    <div class="flex mb-4 px-12">
      <div v-for="i in 2" :key="i" class="flex-1 mx-6">
        <label class="typo__label">Select with search</label>
        <multiselect
          v-model="value"
          :options="options"
          :custom-label="nameWithLang"
          placeholder="Select one"
          label="name"
          track-by="name"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Multiselect from 'vue-multiselect'
export default {
  head: {
    link: [
      { rel: 'stylesheet', href: 'https://firebasestorage.googleapis.com/v0/b/lpcpricing.appspot.com/o/vue-multiselect.min.css?alt=media&token=3d643c8a-78ba-4d00-ac47-9938f1e679a5' }
    ]
  },
  components: {
    Multiselect
  },
  data () {
    return {
      value: { name: 'Plating', language: 'Cost' },
      options: [
        { name: 'Vue.js', language: 'JavaScript' },
        { name: 'Rails', language: 'Ruby' },
        { name: 'Sinatra', language: 'Ruby' },
        { name: 'Laravel', language: 'PHP' },
        { name: 'Phoenix', language: 'Elixir' }
      ]
    }
  },
  methods: {
    nameWithLang ({ name, language }) {
      return `${name} — [${language}]`
    }
  }
}
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
