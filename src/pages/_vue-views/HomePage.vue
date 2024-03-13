<script setup>
import ItemList from '../../components/ItemList.vue';
import SearchBar from '../../components/SearchBar.vue';

import { ref } from 'vue';

const searchValue = ref('');
</script>

<template>
  <div>
    <div class="header">
      <div class="container">
        <SearchBar v-model="searchValue" />
      </div>
    </div>

    <div class="container">
      <ItemList :search-value />
    </div>
  </div>
</template>

<style scoped>
.header {
  position: sticky;
  top: 56px;
  /* Navbar's height */
  z-index: 999;
  background-color: #13171f;
}
</style>
