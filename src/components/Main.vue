<template>
  <div>
    <av-waveform audio-src="/static/example.mp3"></av-waveform>
    <button v-on:click="addMarker" class="btn btn-primary btn-block btn-add-marker"> Add Marker </button>
    <div class="row" v-for="(marker, index) in markers" :key="marker.time">
      <div class="col-md-5">
        <div class="form-group">
          <label>Time</label>
          <input v-model="marker.time" type="time" step="0.001" class="form-control">
        </div>
      </div>
      <div class="col-md-5">
        <div class="form-group">
          <label>Label</label>
          <input v-model="marker.label" type="text" class="form-control">
        </div>
      </div>
      <div class="col-md-2">
        <button class="btn btn-block btn-danger btn-remove" v-on:click="removeMarker(index)"><b-icon icon="trash"></b-icon></button>
      </div>
    </div>
    <button v-b-modal="'bv-json-modal'" class="btn btn-block btn-primary btn-generate-json">Generate JSON</button>
    <b-modal id="bv-json-modal" hide-footer>
      <template #json-title>
        Generated Code
      </template>
      <div class="d-block text-center">
        <pre v-html="this.markers">
          {{markers}}
        </pre>
      </div>
      <b-button class="mt-3" block @click="$bvModal.hide('bv-json-modal')">Close Me</b-button>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'Main',
  data () {
    return {
      markers: []
    }
  },
  methods: {
    addMarker: function() {
      let audio = document.getElementsByTagName('audio');
      console.log('audio[0].currentTime', audio[0].currentTime);
      this.markers.push({
        time: this.msToTime(audio[0].currentTime * 1000),
        msTime: audio[0].currentTime,
        label: ''
      })
    },
    removeMarker: function (index) {
      this.markers.splice(index, 1);
    },
    msToTime: function(s) {
      return new Date(s).toISOString().slice(11, -1);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.btn-add-marker {
  margin: 20px 0px;
}
.btn-remove {
  margin-top: 2rem;
}
pre {
  text-align: left;
}
.btn-generate-json {
  margin-bottom: 20px;
}
</style>
