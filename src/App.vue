<template>
  <div class="input-wrapper">
    <label>environment map (search on: <a href="https://polyhaven.com/hdris" target="_blank">polyhaven</a>): </label>
    <input v-model="mapInput" placeholder="environment map"/>
    <label>environment map intensity (also decimal allowed): </label>
    <input v-model="intensityInput" placeholder="map intensity" type="number"/>
    <label>configurationIds (divided by commas): </label>
    <textarea v-model="configurationIdsInput"
              placeholder="usm:frame, allnatura:showcase_2, jab:soulmate, visplay:qubo_preset_VAR5"/>
    <button @click="applyValues">Test</button>

  </div>
  <div class="roomle-wrapper">
    <div v-if="showOld" class="roomle-col">
      <roomle-component v-for="configurationId in configurationIds"
                        :configuration-id="configurationId"
      />
    </div>
    <div v-if="showNew" class="roomle-col">
      <roomle-component v-for="configurationId in configurationIds"
                        :configuration-id="configurationId"
                        :environment-map-intensity="intensityInput"
                        :environment-map-url="mapInput"
                        :override-server-url="'https://alpha.roomle.com/t/new-renderer/'"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
  import RoomleComponent from '@/components/RoomleComponent.vue';
  import {computed, nextTick, Ref, ref} from "vue";

  const mapInput = ref(undefined);
  const intensityInput = ref(2);
  const configurationIdsInput: Ref<string | undefined> = ref(undefined);
  let configurationIds = ['usm:frame', 'allnatura:showcase_2', 'jab:soulmate', 'visplay:qubo_preset_VAR5'];
  const showNew = ref(true);
  const showOld = ref(true);


  computed(() => console.log(intensityInput.value));

  const applyValues = () => {
    if (!showNew.value || !showOld.value) {
      return;
    }
    showNew.value = false;
    if (configurationIdsInput.value) {
      const str = configurationIdsInput.value?.replace(/ /g, '');
      configurationIds = str.split(",");
      showOld.value = false;
      nextTick(() => showOld.value = true);
    }
    nextTick(() => showNew.value = true);
  };

  console.log(intensityInput);

</script>

<style scoped>
  .roomle-wrapper {
    display: flex;
    gap: 2rem;
    justify-content: center;
  }

  .roomle-col {
    display: flex;
    width: 45vw;
    gap: 2rem;
    flex-direction: column;
  }

  .input-wrapper {
    display: flex;
    flex-direction: column;
    margin-bottom: 2rem;
    width: 30%;
  }

  input, textarea {
    margin-bottom: 0.5rem;
  }
</style>
