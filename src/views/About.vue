<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div id="map"></div>
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
  data: function () {
    return {
      places: [
        { lat: 32.7341, lng: -117.1446, description: "Balboa Park" },
        { lat: 33.115659, lng: -117.119979, description: "Stone brewing company" },
        { lat: 32.7353, lng: -117.149, description: "San Diego Zoo" },
        { lat: 32.681, lng: -117.1783, description: "Coronado Island" },
        { lat: 32.7076, lng: -117.157, description: "Petco Park" },
        { lat: 33.0974, lng: -116.9957, description: "San Diego Zoo Safari Park" },
        { lat: 33.13271, lng: -117.22243, description: "My house" },
      ],
    };
  },
  mounted: function () {
    mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_ACCESS_TOKEN;
    var map = new mapboxgl.Map({
      style: "mapbox://styles/mapbox/light-v10",
      center: [-117.1611, 32.7157],
      zoom: 12,
      pitch: 45,
      bearing: -17.6,
      container: "map",
      antialias: true,
    });

    map.on("load", function () {
      // Insert the layer beneath any symbol layer.
      var layers = map.getStyle().layers;
      var labelLayerId;
      for (var i = 0; i < layers.length; i++) {
        if (layers[i].type === "symbol" && layers[i].layout["text-field"]) {
          labelLayerId = layers[i].id;
          break;
        }
      }

      // The 'building' layer in the Mapbox Streets
      // vector tileset contains building height data
      // from OpenStreetMap.
      map.addLayer(
        {
          id: "add-3d-buildings",
          source: "composite",
          "source-layer": "building",
          filter: ["==", "extrude", "true"],
          type: "fill-extrusion",
          minzoom: 15,
          paint: {
            "fill-extrusion-color": "#aaa",

            // Use an 'interpolate' expression to
            // add a smooth transition effect to
            // the buildings as the user zooms in.
            "fill-extrusion-height": ["interpolate", ["linear"], ["zoom"], 15, 0, 15.05, ["get", "height"]],
            "fill-extrusion-base": ["interpolate", ["linear"], ["zoom"], 15, 0, 15.05, ["get", "min_height"]],
            "fill-extrusion-opacity": 0.6,
          },
        },

        labelLayerId
      );
    });
    this.places.forEach((place) => {
      var popup = new mapboxgl.Popup({ offset: 25 }).setText(place.description);
      new mapboxgl.Marker().setLngLat([place.lng, place.lat]).setPopup(popup).addTo(map);
    });
  },
};
</script>
// // this.places.forEach((place) => { // var popup = new mapboxgl.Popup({ offset: 25 }).setText(place.description); //
new // mapboxgl.Marker({ draggable: true }).setLngLat([place.lng, place.lat]).setPopup(popup).addTo(map); // }); // var
popup = // new mapboxgl.Popup({ offset: 25 }).setText("San Diego Zoo is a great place to see animals."); // // create
Marker // var // marker1 = new mapboxgl.Marker({ draggable: true }).setLngLat([-117.149,
32.7353]).setPopup(popup).addTo(map); // // console.log(marker1); // // Create Marker with non, default color // new
mapboxgl.Marker({ color: "green" // }).setLngLat([-117, 32.7]).addTo(map);
