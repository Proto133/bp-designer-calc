<script setup>
import { ref, watchEffect } from 'vue';
const basePrice = ref(0);
const MSRP = ref(0);
const installCharge = ref(0);
const calculateFromBase = ref(false);

// const calculatedBasePrice = {
//   get() {
//     return MSRP.value - installCharge.value;
//   },
//   set(value) {
//     MSRP.value = value + installCharge.value;
//   }
// }

// const calculatedMSRP = {
//   get() {
//     return basePrice.value + installCharge.value;
//   },
//   set(value) {
//     basePrice.value = value - installCharge.value;
//   }
// };

function handleCalculateMsrp() {
  return MSRP.value = basePrice.value + installCharge.value;
}
function handleCalculateBasePrice() {
  return basePrice.value = MSRP.value - installCharge.value;
}

function clear() {
  basePrice.value = 0;
  MSRP.value = 0;
  installCharge.value = 0;
}

watchEffect(() => {
  if (MSRP.value > 0) {
    handleCalculateBasePrice()
  }
  if (basePrice.value > 0) {
    handleCalculateMsrp()
  }
})

</script>
<template>
  <h3>DealCalculations</h3>
  <div>
    <label for="checkbox">

      Calculate from Base Price: {{ calculateFromBase }}
    </label>
    <input id="checkbox" type="checkbox" v-model="calculateFromBase" />

    <label for="base">Base</label>
    <input id="base" type="text" v-model="basePrice" :on-blur="handleCalculateMsrp()" />
    <label for="msrp">MSRP</label>
    <input id="msrp" type="text" v-model="MSRP" :on-blur="handleCalculateBasePrice()" />
    <label for="install">Install</label>
    <input id="install" type="text" v-model="installCharge" />
  </div>
  <div>
    <button @click="clear">Clear</button>
  </div>
</template>
