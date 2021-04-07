<template>
  <div>
    <div class="waveform-container">
    <div class="waveform-wrapper" :style="{width: canvasWidth+'px'}">
      <av-waveform :canv-width="canvasWidth" :key="canvasWidth" v-bind:canv-height="canvasHeight" audio-src="/static/example.mp3">
      </av-waveform>
      <div v-for="marker in markers" :key="marker.time" class="marker" :style="{left: marker.relativePosition + '%'}">
        <div class="marker-label">{{marker.label}}</div>
      </div>
    </div>
    </div>
    <div class="row">
      <div class="col-md-1">
        <button class="btn btn-default btn-info" v-on:click="moreZoom">+</button>
      </div>
      <div class="col-md-1">
        <button class="btn btn-default  btn-info" v-on:click="minusZoom">-</button>
      </div>
    </div>
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
      markers: [],
      canvasWidth: 500,
      canvasHeight: 120,
      windowfftPower: 7,
      audioFullLength: 0
    }
  },
  mounted: function() {
    console.log('sdjhfsjg');
    let audio = document.getElementsByTagName('audio');
    audio[0].onloadedmetadata = () => {
      console.log('entering');
      this.audioFullLength = audio[0].duration;
      console.log('this.audioFullLength', this.audioFullLength);
    };
  },
  methods: {
    addMarker: function() {
      let audio = document.getElementsByTagName('audio');
      let time = this.msToTime(audio[0].currentTime * 1000);

      console.log('audio[0].currentTime', audio[0].currentTime);
      console.log('this.audioFullLength', this.audioFullLength);

      this.markers.push({
        time: time,
        msTime: audio[0].currentTime,
        label: '',
        relativePosition: (audio[0].currentTime / this.audioFullLength) * 100
      })
    },
    removeMarker: function (index) {
      this.markers.splice(index, 1);
    },
    msToTime: function(s) {
      return new Date(s).toISOString().slice(11, -1);
    },
    moreZoom: function() {
      console.log('entering', this.canvasWidth);
      this.canvasWidth += 100;
      this.windowfftPower ++;
    },
    minusZoom: function() {
      if(this.canvasWidth > 300) {
        this.canvasWidth -= 100;
      }
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
.waveform-container {
  overflow-x: scroll;
  width: 100%;
}
.waveform-wrapper {
  width: auto;
  position: relative;
  display: block;
  margin: auto;
}
.marker {
  width: 1px;
  height: 120px;
  background-color: red;
  position: absolute;
  bottom: 10px;
  left: 0px;
}

.marker-label {
  position: absolute;
  bottom: 0px;
}
</style>
