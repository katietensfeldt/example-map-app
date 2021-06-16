<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div id="map">This is the map page!</div>
  </div>
</template>

<style scoped>
#map {
  width: 100%;
  height: 500px;
}
</style>

<script>
/* global mapboxgl */
export default {
  mounted: function () {
    mapboxgl.accessToken =
      "pk.eyJ1IjoiazEwc2ZlbGR0IiwiYSI6ImNrcHp1bzBkODA2b3AycXRsZmFmMHE4dTEifQ.z0V5fAHJtKa9LiZ3qIiF6w";
    var map = new mapboxgl.Map({
      container: "map", // container id
      style: "mapbox://styles/mapbox/streets-v11", // style URL
      center: [-117.161087, 32.715736], // starting position [lng, lat]
      zoom: 11, // starting zoom
    });

    new mapboxgl.Marker({ color: "green" }).setLngLat([-117.149, 32.7353]).addTo(map);
    new mapboxgl.Marker({ color: "blue" }).setLngLat([-117, 32.7]).addTo(map);
  },
};
</script>
