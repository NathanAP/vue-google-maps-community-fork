# Using custom renderer function and clustering algorithm

In order to use custom renderer function and clustering algorithm, you will need to do the following step before:

## Install `@googlemaps/makerclusterer` package into your own project

Use `npm install @googlemaps/markerclusterer` to install the library into your project.

Note: this is required because it was the best option for us to make Clustering viable. The original version of this library wasn't supporting the new MarkerClustering package version, so we had to make adjustments. You can read more about it in [this PR](https://github.com/NathanAP/vue-google-maps-community-fork/pull/19).

## Example[^1]

Following this example will help you create your own render function and clustering algorithm.

```html
<template>
  <GMapMap
    ref="google"
    :center="center"
    :zoom="7"
    map-type-id="terrain"
    style="width: 500px; height: 300px"
  >
    <GMapCluster :algorithm="algorithm" :renderer="{ render: render }">
      <GMapMarker
        v-for="(m, index) in markers"
        :key="index"
        :clickable="true"
        :draggable="true"
        :position="m.position"
        @click="center = m.position"
      />
    </GMapCluster>
  </GMapMap>
</template>
<script>
  import { GridAlgorithm } from '@googlemaps/markerclusterer'

  export default {
    name: 'App',
    data() {
      return {
        algorithm: new GridAlgorithm({}),
        center: { lat: 51.093048, lng: 6.84212 },
        google: null,
        markers: [
          {
            position: {
              lat: 51.093048,
              lng: 6.84212,
            },
          }, // A long list of clusters
        ],
      }
    },
    methods: {
      render: ({ count, position }) =>
        new this.google.maps.Marker({
          label: {
            text: String(count),
            color: 'white',
            fontSize: '10px',
          },
          position,
          // adjust zIndex to be above other markers
          zIndex: Number(this.google.maps.Marker.MAX_ZINDEX) + count,
        }),
    },
  }
</script>
```

You can also have more control over the cluster marker by replacing the above `render` function with the following
example:

```js
render: ({count, position}, stats) => {
  // change color if this cluster has more markers than the mean cluster
  const color = count > Math.max(10, stats.clusters.markers.mean) ? "#ff0000" : "#0000ff";

  // create svg url with fill color
  const svg = window.btoa(`
    <svg fill="${color}" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 240 240">
      <circle cx="120" cy="120" opacity=".6" r="70" />
      <circle cx="120" cy="120" opacity=".3" r="90" />
      <circle cx="120" cy="120" opacity=".2" r="110" />
    </svg>`);

  // create marker using svg icon
  return new this.google.maps.Marker({
    position,
    icon: {
      url: `data:image/svg+xml;base64,${svg}`,
      scaledSize: new this.google.maps.Size(45, 45),
    },
    label: {
      text: String(count),
      color: "rgba(255,255,255,0.9)",
      fontSize: "12px",
    },
    title: `Cluster of ${count} markers`,
    // adjust zIndex to be above other markers
    zIndex: Number(this.google.maps.Marker.MAX_ZINDEX) + count,
  });
}
```

The above code changes the cluster marker color based on the mean of markers contained in each of them, and how to use
an SVG icon for the cluster marker. Note the use of the `stats` parameter. For more information on it take a look at
the `@googlemaps/markerclusterer`
[ClusterStats doc](https://googlemaps.github.io/js-markerclusterer/classes/ClusterStats.html).

[^1]: The render functions examples are from
the `@googlemaps/markerclusterer` [docs](https://googlemaps.github.io/js-markerclusterer/).
