<template>
    <div id="mapWrapper">

    </div>
</template>

<script>
import { Map as LeafletMap } from "leaflet";
import { FeatureLayer } from "esri-leaflet";
import { vectorBasemapLayer } from "esri-leaflet-vector";

export default {
    name: "EsriMap",
    data() {
        return {
            config: {
                apiKey: "AAPK815e434da023432c85761ff76f64df24XfXgrJUrC9ckvJzf-X_9eMgkGs1R6O7wuAUO-56trc7NsuKZdFF2q4JhM6pBlOPe",
                basemapEnum: "ArcGIS:Streets"
            }
        }
    },
    mounted() {
        this.$nextTick(() => {
            const map = new LeafletMap("mapWrapper").setView([37, 25], 8);

            vectorBasemapLayer(this.config.basemapEnum, {
                apiKey: this.config.apiKey
            }).addTo(map);

            new FeatureLayer({
                url:
                    "https://sampleserver6.arcgisonline.com/arcgis/rest/services/SampleWorldCities/MapServer/0",
            }).addTo(map);
        })
    },

}
</script>

<style lang="scss" scoped>
#mapWrapper {
    $border-color: rgba(
        $color: #2c3e50,
        $alpha: 0.3,
    );
    width: 1130px;
    height: 700px;
    border: 1px solid $border-color;
    box-shadow: 2px 2px 4px 0 $border-color, -2px -2px 4px 0 $border-color;
}
</style>