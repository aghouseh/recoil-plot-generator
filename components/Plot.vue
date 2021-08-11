<template>
  <div class="Plot">
    <div class="Plot-options">
      <label>
        <input v-model="showPointIndex" type="checkbox">
        Show Point Index
      </label>
      <label>
        <input v-model="showDeltas" type="checkbox">
        Show Deltas
      </label>
    </div>
    <div class="Plot-grid">
      <div class="Plot-container">
        <img
          ref="image"
          class="Plot-image"
          draggable="false"
          :src="src"
          @click="handleImageClick"
        >
        <div class="Plot-points">
          <PlotPoint
            v-for="point in sortedPoints"
            :key="point.id"
            :x="point.x"
            :y="point.y"
            :title="`x: ${point.rx}, y: ${point.ry}, hyp: ${point.dh}`"
            @contextmenu.native.prevent="removePoint(point)"
          />
        </div>
      </div>
      <div class="Plot-table">
        <table>
          <thead>
            <tr>
              <th v-if="showPointIndex" class="Plot-data">
                #
              </th>
              <th class="Plot-data">
                X
              </th>
              <th class="Plot-data">
                Y
              </th>
              <th class="Plot-data">
                Hyp
              </th>
              <th v-if="showDeltas" class="Plot-data">
                DX
              </th>
              <th v-if="showDeltas" class="Plot-data">
                DY
              </th>
              <th v-if="showDeltas" class="Plot-data">
                DHyp
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(point, index) in sortedPoints" :key="point.id">
              <td v-if="showPointIndex" class="Plot-data">
                {{ index }}
              </td>
              <td class="Plot-data">
                {{ point.rx }}
              </td>
              <td class="Plot-data">
                {{ point.ry }}
              </td>
              <td class="Plot-data">
                {{ point.rh }}
              </td>
              <td v-if="showDeltas" class="Plot-data">
                {{ point.dx }}
              </td>
              <td v-if="showDeltas" class="Plot-data">
                {{ point.dy }}
              </td>
              <td v-if="showDeltas" class="Plot-data">
                {{ point.dh }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div>
      <button @click="() => (points = [])">
        Clear points
      </button>
    </div>

    <div class="Plot-metrics">
      <div>Total shots: {{ points.length }}</div>
      <div>Average distance: {{ averageDistance }}</div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Plot',
  props: {
    src: {
      type: String,
      default: ''
    }
  },
  data () {
    return {
      id: 1,
      points: [],
      selected: null,
      showPointIndex: false,
      showDeltas: false
    }
  },
  computed: {
    rect () {
      return this.$refs.image.getBoundingClientRect()
    },
    sortedPoints () {
      const sorted = [...this.points].sort((a, b) => a.hyp - b.hyp)
      const [firstPoint] = sorted
      let lastPoint = firstPoint
      return sorted.map((point) => {
        const rx = point.x - firstPoint.x
        const ry = point.y - firstPoint.y
        const dx = point.x - lastPoint.x
        const dy = point.y - lastPoint.y
        lastPoint = point
        return {
          ...point,
          dx,
          dy,
          rx,
          ry,
          dh: this.getHypotenuse({ x: dx, y: dy }),
          rh: this.getHypotenuse({ x: rx, y: ry })
        }
      })
    },
    averageDistance () {
      if (!this.sortedPoints.length) {
        return 'N/A'
      }
      const lastPoint = this.sortedPoints.slice(-1).pop()
      const [firstPoint] = this.sortedPoints
      const distance = (lastPoint.hyp - firstPoint.hyp) / this.points.length
      return distance.toFixed(2)
    }
  },
  methods: {
    getHypotenuse ({ x, y }) {
      return Math.hypot(x, y).toFixed(2)
    },
    addPoint ({ x, y }) {
      const id = this.id++
      const hyp = this.getHypotenuse({ x, y })
      this.points.push({ id, hyp, x, y })
    },
    removePoint ({ id }) {
      this.points = this.points.filter(point => point.id !== id)
    },
    handleImageClick (event) {
      const { bottom, left } = this.rect
      const { x, y } = event
      this.addPoint({
        x: x - left,
        y: bottom - y
      })
    }
  }
}
</script>

<style>
.Plot-grid {
  display: flex;
}
.Plot-data {
  text-align: left;
  min-width: 3em;
}
.Plot-container {
  position: relative;
}
</style>
