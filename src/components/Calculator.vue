<template>
  <div>
    <h1>When do you turn 1 billion seconds old?</h1>
      <input
        type="text"
        name="birthdate"
        v-model="birthdate"
        placeholder="Birth Date"/>
      <select v-model="format">
        <option v-for="format in formats" :key="format">{{format}}</option>
      </select>
    <h2
      v-if="billion_seconds"
      id="birthdate">
      You'll be 1 billion seconds old on {{billion_seconds}}
    </h2>
  </div>
</template>

<script>
import moment from 'moment'
export default {
  data () {
    return {
      birthdate: '',
      format: 'YYYY-MM-DD',
      formats: [
        'YYYY-MM-DD',
        'MM/DD/YYYY',
        'DD/MM/YYYY'
      ]
    }
  },
  methods: {
  },
  computed: {
    billion_seconds () {
      let m = moment(this.birthdate, this.format)
      if (m.isValid()) {
        return m.add(1000000000, 'seconds').format(this.format)
      } else {
        return ''
      }
    }
  }
}
</script>

<style scoped>
</style>
