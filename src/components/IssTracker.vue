<template>
  <div id="container">
    <div ref="map" id="map"></div>
    <div id="btn-menu">
      <button class="btn">Refresh</button>
    </div>

  </div>
</template>

<script>
import {fromLonLat} from 'ol/proj';
import Feature from "ol/Feature";
import Point from 'ol/geom/Point';
import VectorLayer from 'ol/layer/Vector';
import VectorSource from 'ol/source/Vector';
import Style from 'ol/style/Style';
import Icon from 'ol/style/Icon';
import View from 'ol/View';
import Map from 'ol/Map';
import Stroke from 'ol/style/Stroke'
import TileLayer from 'ol/layer/Tile';
import OSM from 'ol/source/OSM';
import LineString from 'ol/geom/LineString';
import 'ol/ol.css';

import axios from 'axios';

export default {
  name: "IssTracker",
  components: {},
  props: [],
  data: function() {
    return {
      map: null,
      iss: null,
      issPath: null,
      pointHistory: [],
      latest: {
        "data": {
          "name": "iss",
          "id": 25544,
          "latitude": -46.830020066039,
          "longitude": -118.74964383713,
          "altitude": 437.20108430044,
          "velocity": 27529.679380403,
          "visibility": "daylight",
          "footprint": 4593.959573794,
          "timestamp": 1651020175,
          "daynum": 2459696.5298032,
          "solar_lat": 13.772068339143,
          "solar_lon": 168.70510723224,
          "units": "kilometers"
        },
        "status": 200,
        "statusText": "OK",
        "headers": {
          "cache-control": "max-age=0, no-cache",
          "content-length": "312",
          "content-type": "application/json"
        },
        "config": {
          "transitional": {
            "silentJSONParsing": true,
            "forcedJSONParsing": true,
            "clarifyTimeoutError": false
          },
          "transformRequest": [
            null
          ],
          "transformResponse": [
            null
          ],
          "timeout": 0,
          "xsrfCookieName": "XSRF-TOKEN",
          "xsrfHeaderName": "X-XSRF-TOKEN",
          "maxContentLength": -1,
          "maxBodyLength": -1,
          "env": {
            "FormData": null
          },
          "headers": {
            "Accept": "application/json, text/plain, */*"
          },
          "method": "get",
          "url": "https://api.wheretheiss.at/v1/satellites/25544"
        },
        "request": {}
      },
      iss_icon: require('../assets/img/iss.png'),
      updateIntervalId: null,
      vectorSource: null,
      vlStyle: null,
      lineStyle: new Style({
        stroke: new Stroke({
          color: '#d12710',
          width: 2,
        })
      })
    }
  },
  methods: {
    updatePosition() {
      axios.get('https://api.wheretheiss.at/v1/satellites/25544')
          .then((res) => {
            this.latest = res;
            let lonlat = fromLonLat([this.latest.data.longitude, this.latest.data.latitude]);
            this.pointHistory.push(lonlat);
            this.iss.setGeometry(new Point(lonlat));
            this.addIssPositionPoint();
          })
          .catch((err) => {
            this.$emit("notify", {text: err, color: 'lightcoral'});
            console.error(err);
          });
    },
    addIssPositionPoint() {
      this.issPath.setGeometry(new LineString(this.pointHistory));
      if(this.pointHistory.length > 1) {
        this.issPath.setStyle(this.lineStyle);
      }
    }
  },
  mounted() {
    // Features
    this.iss = new Feature({
      geometry: new Point(fromLonLat([this.latest.data.longitude, this.latest.data.latitude])),
      name: 'ISS',
    });
    this.issPath = new Feature({
      geometry: new Point(fromLonLat([this.latest.data.longitude, this.latest.data.latitude])),
      name: 'ISS-PATH',
    });
    this.issPath.setStyle(new Style({
      stroke: new Stroke({
        color: 'rgba(0, 0, 0, 0)',
        width: 0,
      })
    }));

    // Create Vector Layer and add Iss as a feature
    this.vectorSource = new VectorSource({
      features: [this.iss, this.issPath]
    });
    //
    this.vlStyle = new Style({
      image: new Icon({
        scale: 0.1,
        src: this.iss_icon,
      })
    });

    // Create map
    this.map = new Map({
      target: this.$refs['map'],
      layers: [
          new TileLayer({
            source: new OSM(),
          }),
          new VectorLayer({
            source: this.vectorSource,
            style: this.vlStyle,
          }),
      ],
      view: new View({
        zoom: 0,
        center: [0, 0],
        constrainResolution: true,
      })
    });
    this.updateIntervalId = setInterval(this.updatePosition,2000);
  }
}
</script>

<style scoped>
#container {
  width: 100vw;
  height: 100vh;
}
#map {
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
}
.btn {
  display: block;
}
#btn-menu {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>