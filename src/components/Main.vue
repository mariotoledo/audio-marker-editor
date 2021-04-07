<template>
  <div>
    <av-waveform audio-src="/static/example.mp3"></av-waveform>
    <button v-on:click="addMarker" class="btn btn-primary btn-block btn-add-marker"> Add Marker </button>
    <div class="row" v-for="marker in markers" :key="marker.time">
      <div class="col-md-5">
        <div class="form-group">
          <label for="formGroupExampleInput">Time</label>
          <input v-model="marker.time" type="time" step="0.001" class="form-control">
        </div>
      </div>
    </div>
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
      console.log('jdksfklsf');
      let audio = document.getElementsByTagName('audio');
      console.log('audio[0].currentTime', audio[0].currentTime);
      this.markers.push({
        time: this.msToTime(audio[0].currentTime * 1000),
        type: 'normal'
      })
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
</style>
