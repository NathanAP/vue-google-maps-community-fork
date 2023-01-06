# How to access Google Maps object

[Interactive example on Playcode](https://playcode.io/1053679)

To access Google Maps object, you will need to import `GMapObject` and wait for it to have some value, otherwise it will be always `null`.

Here is a simple example on how you can wait for it to have some value:

## Example

```html
<script setup>
  import { ref, watch } from 'vue'
  import { GMapObject } from 'vue-google-maps-community-fork'

  const googleMapsObject = ref(GMapObject)
  watch(googleMapsObject, (newValue) => console.log(newValue))

  const markers = ref([{ position: { lat: 51.093048, lng: 6.84212 } }])

  setTimeout(() => (googleMapsObject.value = GMapObject), 100)
</script>

<template>
  <GMapMap :zoom="5" style="width: 500px; height: 300px">
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
```
