<script setup>
import items from '../itemsInfo.json' with { type: 'json' };

import { computed } from 'vue';

const props = defineProps({
  searchValue: String,
});

const filteredItems = computed(() => {
  if (props.searchValue) return items.filter(
    item => item.name.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '').includes(
      props.searchValue.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '')
    )
  )
})
</script>

<template>
  <ul v-if="searchValue">
    <li v-for="item in filteredItems">
      <a :href="`/prices?id=${item.id}&name=${item.name}`">
        <article>
          <h6>{{ item.name }}</h6>
          <img :src="`https://render.albiononline.com/v1/item/${item.id}.png`" />
        </article>
      </a>
    </li>
  </ul>
</template>

<style scoped>
article {
  display: flex;
  flex-direction: column;
  height: 100%;
}

img {
  height: 100px;
  width: 100px;
  align-self: center;
}

ul {
  margin: 0;
  padding: 0;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(170px, 1fr));
  gap: 10px;
}

li {
  list-style: none;
  list-style-type: none;
  margin: 0;
  padding: 0;
}

li:hover {
  transform: scale(1.05, 1.05);
}

a {
  all: unset;
  cursor: pointer;
}
</style>
