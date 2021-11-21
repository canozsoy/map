<template>
    <form @submit.prevent="addMarker">
        <div>Add markers</div>
        <label for="addLat">Latitude:</label>
        <input
            type="text"
            id="addLat"
            v-model="lat"
        >
        <label for="addLon">Longitude:</label>
        <input
            type="text"
            id="addLon"
            v-model="lon"
        >
        <button type="submit">Add</button>
    </form>
</template>

<script>
export default {
    name: "MarkerForm",
    data() {
        return {
            lat: "",
            lon: ""
        }
    },
    methods: {
        addMarker() {
            this.$emit("add-marker", {
                lat: +this.lat,
                lon: +this.lon
            });
            this.lat = this.lon = "";
        }
    }
}
</script>

<style lang="scss" scoped>
form {
    padding: 20px;
}
</style>