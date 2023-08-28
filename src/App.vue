<template>
  <div class="input-wrapper">
    <div class="input-col">
      <div>
        <label>moc: </label>
        <input v-model="isMoc" type="checkbox">
      </div>
      <label>configuration or (if moc) plan ids (divided by commas): </label>
      <textarea v-model="configurationIdsInput"
                placeholder="usm:frame, allnatura:showcase_2, jab:soulmate, visplay:qubo_preset_VAR5"/>
      <button @click="applyValuesAllConfigs">submit and reload all configurators</button>
    </div>
    <div class="input-col">
      <label>environment map (<a
          href="https://roomle.atlassian.net/wiki/spaces/IK/pages/2353594369/How+to+setup+environment+maps+in+new+renderer"
          target="_blank">how to get a map</a>): </label>
      <input v-model="mapInput"
             placeholder="https://dl.polyhaven.org/file/ph-assets/HDRIs/exr/1k/studio_small_09_1k.exr"/>
      <label>environment map intensity (also decimal allowed): </label>
      <input v-model="intensityInput" placeholder="map intensity" step="0.1" type="number"/>
      <button @click="applyValuesNewRenderer">submit and reload new renderer configurators</button>
    </div>
  </div>

  <div class="roomle-wrapper">
    <div v-if="showOld" class="roomle-col">
      <roomle-component v-for="configurationId in configurationIds"
                        :configuration-id="configurationId"
                        :isMoc="isMoc"
      />
    </div>
    <div v-if="showNew" class="roomle-col">
      <roomle-component v-for="configurationId in configurationIds"
                        :configuration-id="configurationId"
                        :environment-map-intensity="intensityInput"
                        :environment-map-url="mapInput"
                        :isMoc="isMoc"
                        :override-server-url="'https://alpha.roomle.com/t/new-renderer/'"
      />
    </div>
  </div>
</template>

<script lang="ts" setup>
  import RoomleComponent from '@/components/RoomleComponent.vue';
  import {computed, nextTick, ref} from "vue";

  const mapInput = ref(undefined);
  const intensityInput = ref(2);
  const configurationIdsInput = ref<string | undefined>(undefined);
  const isMoc = ref(false);
  let configurationIds = ['usm:frame', 'allnatura:showcase_2', 'jab:soulmate', 'visplay:qubo_preset_VAR5'];
  const showNew = ref(true);
  const showOld = ref(true);


  computed(() => console.log(intensityInput.value));

  const applyValuesAllConfigs = () => {
    if (!showOld.value) {
      return;
    }
    if (configurationIdsInput.value) {
      const str = configurationIdsInput.value?.replace(/ /g, '');
      configurationIds = str.split(",");
    }
    showOld.value = false;
    nextTick(() => showOld.value = true);
    applyValuesNewRenderer();
  };

  const applyValuesNewRenderer = () => {
    if (!showNew.value) {
      return;
    }
    showNew.value = false;
    nextTick(() => showNew.value = true);
  };

  const applyValues = () => {
    if (!showNew.value || !showOld.value) {
      return;
    }
    showNew.value = false;
    if (configurationIdsInput.value) {
      const str = configurationIdsInput.value.replace(/ /g, '');
      const newConfigurationIds = str.split(",");
      if (JSON.stringify(configurationIds) !== JSON.stringify(newConfigurationIds)) {
        configurationIds = newConfigurationIds;
        showOld.value = false;
        nextTick(() => showOld.value = true);
      }
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
    margin-bottom: 2rem;
    gap: 2rem;
    width: 100%;
    justify-content: center;
  }

  .input-col {
    display: flex;
    flex-direction: column;
    width: 45vw;
  }

  input, textarea {
    margin-bottom: 0.5rem;
  }
</style>
