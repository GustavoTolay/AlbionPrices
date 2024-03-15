<script setup>
import { ref, defineModel } from 'vue';

const searchValue = defineModel();
const itemName = ref('');

const onClick = () => {
  // Check that name isn't just spaces
  if (itemName.value.replace(/\s/g, '')) searchValue.value = itemName.value;
};
</script>

<template>
  <form action="#">
    <fieldset role="group">
      <input
        v-model="itemName"
        type="text"
        name="name"
        placeholder="Nombre del producto"
      />
      <input type="submit" value="Buscar" @click.prevent="onClick" />
    </fieldset>
  </form>
</template>

<style scoped>
form {
  position: sticky;
  top: 0;
  z-index: 999;
}
</style>
