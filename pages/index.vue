<template>
  <div class="RPG">
    <h1>Recoil Plot Generator</h1>
    <div class="Form">
      <input v-model="imageSrc" type="text">
      <button @click="loadImage">
        Load Image
      </button>
    </div>
    <Plot v-if="src" :src="src" />
  </div>
</template>

<script>
export default {
  data () {
    return {
      imageSrc: 'http://placehold.it/300x400',
      // src: 'https://1.bp.blogspot.com/-o2Fe_Rc-zjg/XjL95RWH7II/AAAAAAAAAos/8WBTg07ZQZwmZDdRbJUuuvK0qvKEVLxwwCEwYBhgL/s640/M4+Recoil+Control_Detailed.PNG'
      src: ''
    }
  },
  created () {
    this.loadImage()
  },
  methods: {
    loadImage () {
      this.src = this.imageSrc
    }
  }
}
</script>

<style>
.RPG {
  display: grid;
  /* place-items: center; */
}
.Form {
  display: flex;
}
</style>
