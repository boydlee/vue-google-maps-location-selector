<template>
  <div class="map-container"></div>
</template>

<script>
  export default {
    props: {
      latitude: {
        type: Number,
        default: 55.01657628017477
      },
      longitude: {
        type: Number,
        default: -7.309233337402361
      }
    },

    data(){
      return {
        lat: this.latitude,
        lng: this.longitude,
        map: null,
        marker: null
      }
    },

    methods: {
        updatePosition: function(newVal) {
            this.lat = newVal.split(',')[0];
            this.lng = newVal.split(',')[1];
            let myLatlng = new google.maps.LatLng(this.lat, this.lng);
            this.marker.setPosition(myLatlng);
            this.map.setCenter(myLatlng);
        }
    },

    mounted(){
      // Set coordinates
      let myLatlng = new google.maps.LatLng(this.lat, this.lng);

      // Options
      let mapOptions = {
        zoom: 15,
        center: myLatlng
      };

      // Apply options
      this.map = new google.maps.Map(this.$el, mapOptions);

      // Add marker
      this.marker = new google.maps.Marker({
        position: myLatlng,
        map: this.map
      });

      this.marker.setMap(this.map);

      let self = this;

      google.maps.event.addListener(self.map, "center_changed", function() {
        let lat = self.map.getCenter().lat();
        let lon = self.map.getCenter().lng();
        let newLatLng = {lat: lat, lng: lon};
        self.marker.setPosition(newLatLng);

        self.$emit('locationUpdated', newLatLng)
      });
    }
  }
</script>
