<template>
  <div style="max-width: 75%; margin: 0 auto">
    <div class="slider-group-title">
      <p>{{data ? data.title: ''}}</p>
    </div>
    <div v-if="!data.data" class="empty-list">No {{data ? data.title: ''}} at the moment.</div>
    <v-slide-group v-if="data.data" :show-arrows="false" center-active>
      <v-slide-item
        v-for="(item, index) in data.data"
        :key="index"
        v-slot:default="{ active, toggle }"
      >
        <Item :item="item" class="ma-4" />
      </v-slide-item>
    </v-slide-group>
  </div>
</template>
<script>
import Item from './item'
export default {
  components: {
    Item
  },
  props: {
    data: Object
  }
}
</script>

<style scoped>
.slider-group-title {
  font-size: 1.5rem;
  text-decoration: underline;
  font-weight: bold;
  font-family: "Advent Pro", sans-serif;
  letter-spacing: 1.3px;
}

.empty-list {
  text-align: center;
  padding: 50px 0;
  background-color: #eee;
}
</style>
