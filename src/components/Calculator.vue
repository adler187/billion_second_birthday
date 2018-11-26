<template>
  <v-layout column>
    <v-flex xs6>
      <div class="white elevation-2">
        <h2>{{message}}</h2>
        <v-menu
          :close-on-content-click="false"
          v-model="date_picker_open"
          :nudge-right="40"
          lazy
          transition="scale-transition"
          offset-y
          min-width="290px">
          <v-text-field
            slot="activator"
            label="Birth Date"
            v-model="birthdate"
            prepend-icon="event"
            readonly>
          </v-text-field>
          <v-date-picker
            v-model="birthdate"
            @input="date_picker_open = false">
          </v-date-picker>
        </v-menu>
        <v-menu
          ref="menu"
          :close-on-content-click="false"
          v-model="time_picker_open"
          :nudge-right="40"
          :return-value.sync="birthtime"
          lazy
          transition="scale-transition"
          offset-y
          max-width="290px"
          min-width="290px">
          <v-text-field
            slot="activator"
            v-model="birthtime"
            label="Birth Time"
            prepend-icon="access_time"
            readonly>
          </v-text-field>
          <v-time-picker
            v-if="time_picker_open"
            v-model="birthtime"
            @change="$refs.menu.save(birthtime)">
          </v-time-picker>
        </v-menu>
      </div>
    </v-flex>
    <v-flex xs6>
      <div class="white elevation-2">
        <v-expansion-panel
          v-model="panel"
          expand>
          <v-expansion-panel-content>
            <div slot="header">Advanced Options</div>
            <v-card>
              <v-card-text class="grey lighten-4">
                <v-flex >
                  <v-select
                    :items="formats"
                    v-model="format"
                    label="Date Format">
                  </v-select>
                </v-flex>
              </v-card-text>
            </v-card>
          </v-expansion-panel-content>
        </v-expansion-panel>
      </div>
    </v-flex>
  </v-layout>
</template>

<script>
import moment from 'moment'
export default {
  data: () => ({
    interval_id: null,
    now: moment(),
    birthdate: new Date().toISOString().substr(0, 10),
    birthtime: '00:00',
    format: 'YYYY-MM-DD',
    formats: [
      'YYYY-MM-DD',
      'MM/DD/YYYY',
      'DD/MM/YYYY'
    ],
    date_picker_open: false,
    time_picker_open: false
  }),
  methods: {
  },
  computed: {
    billion_seconds () {
      return moment(this.birthdate + ' ' + this.birthtime).add(1000000000, 's')
    },
    seconds_until_billion () {
      return this.billion_seconds.diff(this.now, 's')
    },
    message () {
      if (!this.billion_seconds.isValid()) {
        return ''
      }

      let formatted = this.billion_seconds.format(this.format)
      if (this.seconds_until_billion > 0) {
        return `You will turn 1 billion seconds old on ${formatted} in ${this.seconds_until_billion} seconds.`
      } else {
        return `You turned 1 billion seconds old on ${formatted}`
      }
    }
  },
  mounted: function () {
    if (this.interval_id) {
      clearInterval(this.interval_id)
    }

    this.interval_id = setInterval(() => {
      this.now = moment()
    }, 1000)
  }
}
</script>

<style scoped>
</style>
