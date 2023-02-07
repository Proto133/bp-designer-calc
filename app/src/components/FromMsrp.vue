<script setup lang="ts">
import { ref, watchEffect, computed } from 'vue';

const basePrice = ref(0);
const MSRP = ref(0);
const installCharge = ref(0);
const par = computed(() => basePrice.value * .77)
const floor = computed(() => basePrice.value * .60)
const title = ref('From MSRP')
const fromBase = ref(true)




function clear() {
  basePrice.value = 0;
  MSRP.value = 0;
  installCharge.value = 0;
}
function clearInput(box: string) {
  switch (box) {
    case 'msrp':
      MSRP.value = null as unknown as number;
      break;
    case 'install':
      installCharge.value = null as unknown as number;
      break;
  }
}


watchEffect(() => {
  basePrice.value = MSRP.value - installCharge.value;
  if (fromBase.value) {
    title.value = 'From Base';
  } else {
    title.value = 'From MSRP';
  }

})

</script>
<template>
  <h3>{{ title }}</h3>
  <button @click="fromBase = !fromBase"> Switch </button>
  <div>
    <div>
      <label for="base">Base</label>
      <input readonly id="base" type="text" v-model="basePrice" />
    </div>
    <div> <label for="msrp">MSRP</label>
      <input id="msrp" type="text" v-model="MSRP" @focus="clearInput('msrp')" />
    </div>
    <div> <label for="install">Install</label>
      <input id="install" type="text" v-model="installCharge" @focus="clearInput('install')" />
    </div>
  </div>
  <div>
    <button @click="clear">Clear</button>
  </div>
  <div>
    <dl>
      <dt>Par</dt>
      <dd>{{ par }}</dd>
      <dt>Floor</dt>
      <dd>{{ floor }}</dd>
    </dl>
  </div>
</template>
