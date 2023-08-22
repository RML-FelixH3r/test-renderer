<script lang="ts" setup>
  import RoomleComponent from '/src/components/RoomleComponent.vue';
  import {ref} from "vue";

  const configurationIds = ref(['usm:frame', 'allnatura:showcase_2', 'jab:soulmate', 'visplay:qubo_preset_VAR5']);
</script>

<template>
  <!--  <div class="input-wrapper">-->
  <!--    <input ref="mapInput" placeholder="environment map"/>-->
  <!--    <input ref="intensityInput" placeholder="map intensity"/>-->
  <!--  </div>-->
  <div class="roomle-wrapper">
    <div class="roomle-col">
      <roomle-component v-for="configurationId in configurationIds"
                        :configuration-id="configurationId"
      />
    </div>
    <div class="roomle-col">
      <roomle-component v-for="configurationId in configurationIds"
                        :configuration-id="configurationId"
                        override-server-url="https://alpha.roomle.com/t/new-renderer/"
      />
    </div>
  </div>

</template>

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
    margin-bottom: 2rem;
  }
</style>
