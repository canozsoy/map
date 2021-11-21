<template>
    <div id="app">
        <esri-map ref="map" />
        <marker-form @add-marker="addMarker" />
    </div>
</template>

<script>
import EsriMap from "@/components/EsriMap.vue";
import MarkerForm from "@/components/MarkerForm.vue";

export default {
    name: 'App',
    components: {
        EsriMap,
        MarkerForm
    },
    methods: {
      addMarker(val) {
        this.$refs.map.addMarker(val);
      }
    }
}
</script>

<style lang="scss" scoped>
#app {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
  padding: 40px;
}
</style>
