<template>
  <div ref="roomleContainer" class="roomle-container"></div>
</template>

<script lang="ts" setup>
  import {onMounted, ref} from "vue";
  // @ts-ignore
  import RoomleConfiguratorApi from '@roomle/embedding-lib/roomle-configurator-api';

  const props = defineProps({
        overrideServerUrl: String,
        configurationId: String,
        environmentMapUrl: String,
        environmentMapIntensity: Number
      }
  );
  const roomleContainer = ref(null);
  onMounted(async () => {
    const options = {
      overrideServerUrl: props.overrideServerUrl,
      id: props.configurationId,
      envMapUrl: props.environmentMapUrl,
      envMapIntensity: props.environmentMapIntensity
    };
    if (!roomleContainer.value) {
      console.log("null");
    }
    const configurator = await RoomleConfiguratorApi.createConfigurator(
        "demoConfigurator",
        roomleContainer.value,
        options
    );
    if (configurator.ui) {
      console.log("init");
    }
  });
</script>

<style scoped>
  .roomle-container {
    width: 100%;
    height: 50vh;
  }
</style>
