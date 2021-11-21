<template>
    <div id="mapWrapper">
        <l-map
            :options="mapOptions"
            ref="map"
            @ready="placeVectorMap"
        >
            <l-marker
                v-for="(item, index) in markers"
                :key="index"
                :lat-lng="[item.lat, item.lon]"
            >
                <l-popup :options="popupOptions">
                    {{item.lat}}, {{item.lon}}
                </l-popup>
            </l-marker>
        </l-map>
    </div>
</template>

<script>
import L from "leaflet";
import { LMap, LMarker, LPopup } from "vue2-leaflet";
import { vectorBasemapLayer } from "esri-leaflet-vector";
import { geosearch, arcgisOnlineProvider } from "esri-leaflet-geocoder";

export default {
    name: "EsriMap",
    components: {
        LMap,
        LMarker,
        LPopup
    },
    data() {
        return {
            config: {
                apiKey: "AAPK815e434da023432c85761ff76f64df24XfXgrJUrC9ckvJzf-X_9eMgkGs1R6O7wuAUO-56trc7NsuKZdFF2q4JhM6pBlOPe",
                basemapEnum: "ArcGIS:DarkGray"
            },
            mapOptions: {
                minZoom: 2,
                maxBounds: [[-Infinity, -180], [Infinity, 180]],
                attribution: false
            },
            markers: [],
            popupOptions: {
                offset: [0, -15]
            }
        }
    },
    methods: {
        placeVectorMap() {
            const map = this.$refs.map.mapObject;

            vectorBasemapLayer(this.config.basemapEnum, {
                apiKey: this.config.apiKey
            }).addTo(map);

            this.placeGeoCoding(map);

        },
        placeGeoCoding(map) {

            const searchControl = geosearch({
                position: "topright",
                placeholder: "Enter an address",
                useMapBounds: false,
                providers: [arcgisOnlineProvider({
                    apikey: this.config.apiKey,
                    nearby: {
                        lat: 0,
                        lng: 0
                    }
                })]
            }).addTo(map);

            const results = L.layerGroup().addTo(map);

            searchControl.on("results", (data) => {
                results.clearLayers();
                for (let i = data.results.length - 1; i >= 0; i--) {
                    const lngLatString = `${Math.round(data.results[i].latlng.lng * 100000) / 100000}, ${Math.round(data.results[i].latlng.lat * 100000) / 100000}`;
                    const marker = L.marker(data.results[i].latlng);
                    marker.bindPopup(`<b>${lngLatString}</b><p>${data.results[i].properties.LongLabel}</p>`)
                    results.addLayer(marker);
                    marker.openPopup();
                }
            })
        },
        addMarker(val) {
            this.markers.push(val);
        }
    }

}
</script>

<style lang="scss" scoped>
#mapWrapper {
    position: relative;
    top: 0;
    left: 0;
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