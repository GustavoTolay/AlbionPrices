<script setup>
import TableVue from '../../components/Table.vue';
import { ref, shallowReactive } from 'vue';

// Get URL params
const params = new URLSearchParams(window.location.search);
const id = params.get("id");
const name = params.get("name");

// Controls if to show all the qualities or only "normal" one on select
const showQualities = ref(false);

// Get items info
fetch(`https://west.albion-online-data.com/api/v2/stats/prices/${id}?locations=Caerleon,Bridgewatch,Fort Sterling,Martlock,Thetford,Lymhurst&qualities=${6}`)
  .then(res => res.json())
  .then(res => {
    separateTableItems(res);
    if (res.length > 6) showQualities.value = true;
  })
  .catch(err => {
    console.log(err);
  });

// Separates each item content between the tables.
function separateTableItems(items) {
  return Object.keys(tablesContent)
    .forEach((tableName) => tablesContent[tableName] =
      items.map((item) => {
        const [date, hour] = item[tableName + '_date'].split('T');
        return {
          city: item.city,
          quality: item.quality,
          price: item[tableName],
          date,
          hour
        }
      })
    );
}

// Default table values
const defaultValues = [
  { city: "Bridgewatch", quality: '1', price: 0, date: '0001-01-01', hour: '00:00:00' },
  { city: "Caerleon", quality: '1', price: 0, date: '0001-01-01', hour: '00:00:00' },
  { city: "Fort Sterling", quality: '1', price: 0, date: '0001-01-01', hour: '00:00:00' },
  { city: "Lymhurst", quality: '1', price: 0, date: '0001-01-01', hour: '00:00:00' },
  { city: "Martlock", quality: '1', price: 0, date: '0001-01-01', hour: '00:00:00' },
  { city: "Thetford", quality: '1', price: 0, date: '0001-01-01', hour: '00:00:00' }
]
const tablesContent = shallowReactive({
  'sell_price_min': defaultValues,
  'sell_price_max': defaultValues,
  'buy_price_min': defaultValues,
  'buy_price_max': defaultValues,
});

// Select quality value
const quality = ref('1');
</script>

<template>
  <main class='container'>
    <section class='header'>
      <article class='item'>
        <header>{{ name }}</header>
        <img :src='`https://render.albiononline.com/v1/item/${id}.png`' alt='' />
      </article>
      <article>
        <header>Opciones</header>
        <label for='quality'>Calidad:</label>
        <select v-model="quality" name='quality' id='quality'>
          <option value='1'>Normal</option>
          <!-- Check any table -->
          <template v-if="showQualities">
            <option value='2'>Bueno</option>
            <option value='3'>Sobresaliente</option>
            <option value='4'>Excelente</option>
            <option value='5'>Obra Maestra</option>
          </template>

        </select>
      </article>
    </section>

    <h2>Órdenes de venta</h2>
    <div>
      <article>
        <h3>Precio Min.</h3>
        <TableVue :quality :items="tablesContent['sell_price_min']" />
      </article>
      <article>
        <h3>Precio Max.</h3>
        <TableVue :quality :items="tablesContent['sell_price_max']" />
      </article>
    </div>

    <h2>Órdenes de compra</h2>
    <div>
      <article>
        <h3>Precio Min.</h3>
        <TableVue :quality :items="tablesContent['buy_price_min']" />
      </article>
      <article>
        <h3>Precio Max.</h3>
        <TableVue :quality :items="tablesContent['buy_price_max']" />
      </article>
    </div>
    <!-- Footnote -->
    <span><strong>*</strong> Datos de las últimas 24 horas.</span>
  </main>
</template>

<style scoped>
div {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

main {
  padding-top: 0;
}

section {
  display: flex;
  justify-content: space-between;
  gap: 30px;
}

img {
  height: 130px;
}
</style>