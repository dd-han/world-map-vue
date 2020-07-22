<template>
  <div class="world-map-legend-wrapper">
    <div class="legend-box">
      <div class="legend" v-for="legend in legendLabels">
        <div class="legend-color" :style="{ 'background-color': legend.color }"></div>
        <span>{{ legend.label }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import { getMaxAndMinCountryDataValues } from './dynamic-map-css'

const formatLegentLabel = (value) => {
  if (value === 0) {
    return value.toLocaleString() + '+'
  }

  const digits = Math.floor(Math.log10(Math.abs(value)))
  const accuracyMagnification = Math.pow(10, digits)
  const approximation = Math.floor(value / accuracyMagnification) * accuracyMagnification
  return approximation.toLocaleString() + '+'
}


export default {
  name: 'Legend',
  props: {
    countryData: {
      type: Object,
      required: true
    },
    chroma: {
      type: Function,
      required: true
    }
  },
  computed: {
    legendLabels() {
      const result = []
      const { max } = getMaxAndMinCountryDataValues(this.countryData)

      const steps = 5
      for (let step = 1; step <= steps; step += 1) {
        const progress = 1 / steps * step
        result.push({
          label: formatLegentLabel(max * progress),
          color: this.chroma(progress).hex()
        })
      }
      result.reverse()
      return result
    },
  }
}
</script>

<style>

  .world-map-legend-wrapper .legend {
    display: inline-block;
    width: 20%;
    line-height: 20px;
    font-size: 14px;
    height: 20px;
  }

  .world-map-legend-wrapper .legend-color {
    display: inline-block;
    float: left;
    margin-right: 0.5rem;
    width: 20px;
    height: 20px;
  }

</style>