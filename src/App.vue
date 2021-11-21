<template>
    <div id="app">
        <esri-map />
    </div>
</template>

<script>
import EsriMap from "@/components/EsriMap.vue";

export default {
    name: 'App',
    components: {
        EsriMap
    }
}
</script>

<style lang="scss" scoped>
#app {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 40px;
}
</style>
