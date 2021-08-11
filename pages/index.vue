<template>
  <div
    class="RPG"
    :class="classes"
    @drop="handleDrop"
    @dragover="handleDragOver"
    @dragleave="handleDragOver"
  >
    <h1>Recoil Plot Generator</h1>
    <Plot v-if="src" :src="src" />
    <div v-else>
      Drag an image to get started
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      imageSrc: 'http://placehold.it/300x400',
      // src: 'https://1.bp.blogspot.com/-o2Fe_Rc-zjg/XjL95RWH7II/AAAAAAAAAos/8WBTg07ZQZwmZDdRbJUuuvK0qvKEVLxwwCEwYBhgL/s640/M4+Recoil+Control_Detailed.PNG'
      src: '',
      classes: {
        isHover: false
      }
    }
  },
  created () {
    // this.loadImage(this.imageSrc)
  },
  methods: {
    handleDrop (event) {
      this.handleDragOver(event)
      const files = event.target.files || event.dataTransfer.files
      files.forEach(this.parseFile)
      return false
    },
    handleDragOver (event) {
      event.stopPropagation()
      event.preventDefault()
      this.classes.isHover = event.type === 'dragover'
    },
    parseFile (file) {
      const reader = new FileReader()
      reader.onload = (event) => {
        this.loadImage(event.target.result)
      }
      reader.readAsDataURL(file)
    },
    loadImage (src) {
      this.src = src
    }
  }
}
</script>

<style>
.RPG {
  /* display: grid; */
  min-height: 100vh;
  /* place-items: center; */
}
.Form {
  display: flex;
}
</style>
