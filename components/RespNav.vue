<template>
  <nav class="bg-white shadow p-3 mt-0 w-full fixed z-20 top-0">
    <div class="container mx-auto flex flex-wrap items-center">
      <div class="flex pt-2 w-full md:w-1/2 justify-center md:justify-start text-gray-700">
        <nuxt-link class="text-gray-700 no-underline hover:text-gray-700 hover:no-underline lg:ml-4" to="/">
          <img src="https://lapelpinsandcoins.com/images/lapel.png" class="w-56" alt="Lapel Pins & Coins Logo">
        </nuxt-link>
      </div>
      <div class="flex w-full content-center justify-between md:w-1/2 md:justify-end">
        <ul class="list-reset flex justify-between flex-1 md:flex-none items-center">
          <li class="mr-3">
            <nuxt-link class="inline-block text-gray-700 tracking-wide no-underline hover:text-orange-500 hover:text-underline py-2 px-4" to="/">
              Lapel Pins
            </nuxt-link>
          </li>
          <li class="mr-3">
            <nuxt-link class="inline-block text-gray-700 tracking-wide no-underline hover:text-orange-500 hover:text-underline py-2 px-4" to="coins">
              Challenge Coins
            </nuxt-link>
          </li>
          <li class="mr-3">
            <nuxt-link class="inline-block text-gray-700 tracking-wide no-underline hover:text-orange-500 hover:text-underline py-2 px-4" to="keychains">
              Keychains
            </nuxt-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'RespNav'
}
</script>

<style scoped>

</style>
