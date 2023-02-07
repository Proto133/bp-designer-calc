<script setup lang="ts">
import { ref, watchEffect, computed } from 'vue';

const basePrice = ref(0);
const MSRP = ref(0);
const installCharge = ref(0);
const par = computed(() => basePrice.value * .77)
const floor = computed(() => basePrice.value * .60)
const title = ref('From MSRP')
const fromBase = ref(true)
const offered = ref()
const cashDep = computed(() => offered.value ? offered.value * .50 : 0)
const freedomDep = computed(() => offered.value ? offered.value * .25 : 0)



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
    case 'base':
      basePrice.value = null as unknown as number;
      break;
  }
}


watchEffect(() => {
  if (!fromBase.value && MSRP.value) {
    basePrice.value = MSRP.value - installCharge.value;
  }
  if (fromBase.value && basePrice.value) {
    MSRP.value = basePrice.value + installCharge.value;
  }
  if (!offered.value && MSRP.value) {
    offered.value = MSRP.value
  }
  if (fromBase.value) {
    title.value = 'From Base';
  } else {
    title.value = 'From MSRP';
  }

})

</script>
<template>
  <main>

    <header>
      <button @click="fromBase = !fromBase"> {{ title }} </button>
    </header>
    <div class="input-wrapper">
      <div class="input">
        <label for="base">Base</label>
        <input :readonly="!fromBase" id="base" type="number" v-model="basePrice" @focus="clearInput('base')" />
      </div>
      <div class="input"> <label for="msrp">MSRP</label>
        <input :readonly="fromBase" id="msrp" type="number" v-model="MSRP" @focus="clearInput('msrp')" />
      </div>
      <div class="input"> <label for="install">Install</label>
        <input id="install" type="number" v-model="installCharge" @focus="clearInput('install')" />
      </div>
    </div>
    <div>
      <button @click="clear">Clear</button>
    </div>
    <div id="currentOffer">
      <h4>
        Current Offer: &nbsp;
        <input id="offered" type="number" v-model="offered" @focus="clearInput('offered')" />
      </h4>
    </div>
    <div class="deal-info">
      <div>
        <h5>
          Par: &nbsp;
          <span>{{ par }}</span>
        </h5>
      </div>
      <div>

        <h5>
          Floor: &nbsp;
          <span>{{ floor }}</span>
        </h5>
      </div>
    </div>
    <div class="wtp-cash">
      <dl>
        Cash
        <dt>Deposit</dt>
        <dd>{{ cashDep }}</dd>
        <dt>Remaining Balance</dt>
        <dd>{{ offered - cashDep }}</dd>

      </dl>
      <div class="wtp-financing">
        Freedom
        <dt>Deposit</dt>
        <dd>{{ freedomDep }}</dd>
        <dt>Amount Financed</dt>
        <dd>{{ offered - freedomDep }}</dd>
      </div>
    </div>
  </main>
</template>
<style lang="scss">
main {
  background-color: aqua;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: row wrap;
  gap: 3rem;

  & * {
    font-size: large;
  }
}

header {
  flex: 0 0 80%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: row wrap;
  gap: 3rem;

  &>* {
    flex: 0 0 45%;
  }
}

#currentOffer {
  border: 1px solid black;
  flex: 0 0 80%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: row wrap;
}

.deal-info {
  display: inline-flex;
  background-color: blueviolet;
  flex: 0 0 60%;
  align-items: center;
  justify-content: space-around;
}

.input-wrapper {
  flex: 0 0 60%;
  display: flex;
  justify-content: center;
  flex-flow: row wrap;
  gap: 1rem;
}

.wtp-cash,
.wtp-financing {
  flex: 0 0 80%;
}

.input {
  display: flex;
  justify-content: space-around;
  flex-flow: row wrap;
  gap: 1rem;
}
</style>