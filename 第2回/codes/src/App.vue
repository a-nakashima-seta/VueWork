<script setup lang="ts">
import PropsEmit from '@/components/PropsEmit/index.vue'
import ProvideInject from '@/components/ProvideInject/index.vue'
import Pinia from '@/components/Pinia/index.vue'

</script>

<template>
  <div>
    <h1>Props / Emit</h1>
    <PropsEmit />

    <div class="spacer"></div>

    <h1>Provide / Inject</h1>
    <ProvideInject />

    <h1>Pinia</h1>
    <Pinia />
  </div>
</template>

<style scoped>
.spacer {
  margin-bottom: 100px;
}
</style>