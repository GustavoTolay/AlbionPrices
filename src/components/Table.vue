<script setup>
import { computed, reactive } from 'vue';

const props = defineProps({
  items: Array,
  quality: String,
})

const order = reactive({ price: 'asc' });

const changePriceOrder = () => {
  if (order.price == 'desc') order.price = 'asc';
  else order.price = 'desc';
}

const arrowImg = computed(() => {
  if (order.price == 'desc') return '/arrowup.svg';
  return '/arrowdown.svg';
})

const filteredItems = computed(() => {
  const filtered = props.items.filter(
    item => item.quality == props.quality
  );

  if (order.price == 'asc') return filtered.sort((a, b) => a.price - b.price);
  else if (order.price == 'desc') return filtered.sort((a, b) => b.price - a.price);
})
</script>

<template>
  <table class='striped'>
    <thead>
      <tr>
        <th scope='col'>Ciudad</th>
        <th scope='col' @click="changePriceOrder" class='price-button'>
          Precio <img :src='arrowImg' alt='' />
        </th>
        <th scope='col'>Fecha</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in filteredItems">
        <th scope="row">
          <img class="city-icon"
            :src="`https://render.albiononline.com/v1/item/T8_CAPEITEM_FW_${item.city == 'Fort Sterling' ? 'FORTSTERLING' : item.city.toUpperCase()}`"
            alt="" />
          {{ item.city }}
        </th>
        <td>{{ item.price || '-' }}</td>
        <td>{{ item.date == '0001-01-01' ? '-' : item.date + ', ' + item.hour.slice(0, 5) }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
.price-button:hover {
  cursor: pointer;
  filter: brightness(1.5);
}

.city-icon {
  height: 40px;
  margin-right: 10px;
}
</style>