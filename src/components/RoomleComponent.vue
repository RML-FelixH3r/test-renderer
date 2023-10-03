<template>
  <div ref="roomleContainer" class="roomle-container"></div>
</template>

<script lang="ts" setup>
  import type {PropType} from "vue";
  import {onMounted, reactive, ref} from "vue";
  // @ts-ignore
  import RoomleConfiguratorApi from '@roomle/embedding-lib/roomle-configurator-api';

  const props = defineProps({
        overrideServerUrl: String as PropType<string | null>,
        configurationId: String as PropType<string | null>,
        environmentMapUrl: String as PropType<string | null>,
        environmentMapIntensity: Number as PropType<number | null>,
        isMoc: Boolean,
        legacyLight: Boolean
      }
  );
  const roomleContainer = ref(null);
  const options = reactive({
    overrideServerUrl: props.overrideServerUrl,
    id: props.configurationId,
    envMapUrl: props.environmentMapUrl,
    envMapIntensity: props.environmentMapIntensity,
    moc: props.isMoc,
    legacyLight: props.legacyLight
  });
  onMounted(async () => {
    await loadConfigurator();
    // @ts-ignore
    console.log(roomleContainer.value);
  });

  const loadConfigurator = async () => {
    await RoomleConfiguratorApi.createConfigurator(
        "demoConfigurator",
        roomleContainer.value,
        options
    );
  };

</script>

<style scoped>
  .roomle-container {
    width: 100%;
    height: 50vh;
  }
</style>
