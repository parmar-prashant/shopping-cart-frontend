<template>
  <div class="container grid grid-cols-1 sm:grid-cols-2 m-auto gap-2">
    <div class="col-span-1 text-center sm:col-span-2">
      <AppHeader />
    </div>
    <div class="col-span-1 sm:col-span-2 bg-neutral-200 mt-3 p-1">
      <Product />
    </div>
  </div>
</template>

<script>

import AppHeader from '../src/components/skeleton/AppHeader.vue';
import Product from './components/Product.vue';

/**
 * This component is the root component.
 * Based on the test requirements, I have divided the whole app into AppHeader and the body section i.e. Product information.
 */
export default {
  name: 'App',
  components: {
    AppHeader,
    Product
  }
}
</script>

<style>
</style>
