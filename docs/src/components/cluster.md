# Cluster

Here you will find some uses for Google Maps Clusterers.

[[toc]]

## Before everything

Clustering your markers are only possible because of `@googlemaps/markerclusterer`! The community is thankful for your effort!

Of course, you can have a look at [their docs](https://googlemaps.github.io/js-markerclusterer/) for more information on how you can use the props described below.

## Cluster your markers

You can add clusters to your `GMapMarker` using `GMapCluster` component inside of `GMapMap` component:

```html
<template>
  <GMapMap :center="center" :zoom="7" map-type-id="terrain" style="width: 500px; height: 300px">
    <GMapCluster>
      <GMapMarker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        :clickable="true"
        :draggable="true"
        @click="center = m.position"
      />
    </GMapCluster>
  </GMapMap>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        center: { lat: 51.093048, lng: 6.84212 },
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
  }
</script>
```

## Props

You can pass the following props to control behavior of clusters:

### algorithm

Defines the algorithm to use to cluster the markers (default `SuperClusterAlgorithm`):

```js
:algorithm="algorithm"
```

### renderer

Defines the function to use for render the cluster markers (default `DefaultRenderer`).

```js
:renderer="{ render: render }"
```
