<template>
  <Object3D :rotation="{ y: Math.PI }">
    <slot></slot>
  </Object3D>
</template>

<script>
/* global requestAnimationFrame, cancelAnimationFrame */
import _OrbitControls from '../threex/controls/OrbitControls'
import Object3D from './Object3D'
import { Clock } from 'three'

export default {
  name: 'OrbitControls',
  mixins: [Object3D],
  components: { Object3D },
  inject: ['global'],

  data () {
    return {
      controls: null,
      timer: null,
      frame: null
    }
  },

  mounted () {
    let domElement = this.global.rendererDom // fixme
    this.controls = new _OrbitControls(this.curObj, domElement)
    this.timer = new Clock()
    this.frame = this.animate()
  },

  beforeDestroy () {
    if (this.frame) {
      cancelAnimationFrame(this.frame)
    }
    if (this.controls) {
      this.controls.dispose()
    }
  },

  methods: {
    animate () {
      this.frame = requestAnimationFrame(this.animate)
      this.controls.update(this.timer.getDelta())
    }
  }
}
</script>
