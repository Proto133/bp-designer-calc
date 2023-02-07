<script setup lang="ts">
import { ref, watchEffect } from 'vue';
const basePrice = ref<number | null>(null);
const MSRP = ref<number | null>(null);
const installCharge = ref<number | null>(null)

function clear() {
  basePrice.value = null;
  MSRP.value = null;
  installCharge.value = null;
}

function clearInput(box: string) {
  switch (box) {
    case 'base':
      basePrice.value = null;
      break;
    case 'install':
      installCharge.value = null;
      break;
  }
}
watchEffect(() => {
  if (basePrice.value && installCharge.value) {
    MSRP.value = Number(basePrice.value) + Number(installCharge.value);
  }
})

</script>
<template>
  <h3>From Base</h3>
  <div>
    <div>
      <label for="base">Base</label>
      <input id="base" type="number" v-model="basePrice" @focus="clearInput('base')" />
    </div>
    <div>
      <label for="msrp">MSRP</label>
      <input readonly id="msrp" type="number" v-model="MSRP" />
    </div>
    <div>
      <label for="install">Install</label>
      <input id="install" type="number" v-model="installCharge" @focus="clearInput('install')" />
    </div>
  </div>
  <div>
    <button @click="clear">Clear</button>
  </div>
</template>
