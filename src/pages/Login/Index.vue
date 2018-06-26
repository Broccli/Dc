<template>
  <v-layout>
    <v-card contextual-style="dark">
      <span slot="header">
        Calculator
      </span>
      <div slot="body">
        <!-- @submit.prevent="login(user)" -->
        <form  id="distance_form">
          <div class="form-group">
            <div class="input-group">
              <div class="input-group-addon">
                <i class="fa fa-map-marker fa-fw"></i>
              </div>
              <input
               id="from_places"
                placeholder="Entar a loction"
                class="form-control"
              >
               <input id="origin" name="origin" required="" type="hidden" />
            </div>
          </div>
          <div class="form-group">
            <div class="input-group">
              <div class="input-group-addon">
                <i class="fa fa-map fa-fw"></i>
              </div>
              <input
                id="to_places"
                placeholder="Enter a location"
                class="form-control"
              >
              <input id="destination" name="destination" required="" type="hidden" />
            </div>
          </div>
          <div class="form-group">
            <button class="btn btn-outline-primary" style="">
              Calculate
            </button>
            <input type="submit" class="btn btn-outline-primary" value="Calculate">
          </div>
        </form>
        <div id="result">
          <ul class="list-group">
            <li class="list-group-item">Distance In Mile:</li>
            <li class="list-group-item">Distance In Kilo:</li>
            <li class="list-group-item">In Min:</li>
            <li class="list-group-item">From:</li>
            <li class="list-group-item">To:</li>
          </ul>
        </div>

      </div>
    </v-card>
  </v-layout>
</template>

<script>
  /* ============
   * Login Index Page
   * ============
   *
   * Page where the user can login.
   */


  import VLayout from '@/layouts/Minimal';
  import VCard from '@/components/Card';

  export default {
    mounted() {
       function calc() {
        // add input listeners
        google.maps.event.addDomListener(window, 'load', function Load () {
            this.$parent from_places = new google.maps.places.Autocomplete(document.getElementById('from_places'));
            var to_places = new google.maps.places.Autocomplete(document.getElementById('to_places'));

            google.maps.event.addListener(from_places, 'place_changed', function fromPlaces () {
                var from_place = from_places.getPlace();
                var from_address = from_place.formatted_address;
                document.getElementById('#origin').val(from_address);
            });

            google.maps.event.addListener(to_places, 'place_changed', function toPlaces () {
                var to_place = to_places.getPlace();
                var to_address = to_place.formatted_address;
                document.getElementById('#destination').val(to_address);
            });

        });
        // calculate distance
        function calculateDistance() {
            var origin = document.getElementById('#origin').val();
            var destination = getElementById('#destination').val();
            var service = new google.maps.DistanceMatrixService();
            service.getDistanceMatrix(
                {
                    origins: [origin],
                    destinations: [destination],
                    travelMode: google.maps.TravelMode.DRIVING,
                    unitSystem: google.maps.UnitSystem.IMPERIAL, // miles and feet.
                    // unitSystem: google.maps.UnitSystem.metric, // kilometers and meters.
                    avoidHighways: false,
                    avoidTolls: false
                }, callback);
        }
        // get distance results
        function callback(response, status) {
            if (status != google.maps.DistanceMatrixStatus.OK) {
                document.getElementById('#result').html(err);
            } else {
                var origin = response.originAddresses[0];
                var destination = response.destinationAddresses[0];
                if (response.rows[0].elements[0].status === "ZERO_RESULTS") {
                    document.getElementById('#result').html("Better get on a plane. There are no roads between "  + origin + " and " + destination);
                } else {
                    var distance = response.rows[0].elements[0].distance;
                    var duration = response.rows[0].elements[0].duration;
                    // console.log(response.rows[0].elements[0].distance);
                    var distance_in_kilo = distance.value / 1000; // the kilom
                    var distance_in_mile = distance.value / 1609.34; // the mile
                    var duration_text = duration.text;
                    var duration_value = duration.value;
                    document.getElementById('#in_mile').text(distance_in_mile.toFixed(2));
                    document.getElementById('#in_kilo').text(distance_in_kilo.toFixed(2));
                    document.getElementById('#duration_text').text(duration_text);
                    document.getElementById('#duration_value').text(duration_value);
                    document.getElementById('#from').text(origin);
                    document.getElementById('#to').text(destination);
                }
            }
        }
        // print results on submit the form
        document.getElementById('#distance_form').submit(function final(e){
            e.preventDefault();
            calculateDistance();
        });

    };
    },
    /**
     * The name of the page.
     */
    name: 'login-index',

    /**
     * The data that can be used by the page.
     *
     * @returns {Object} The view-model data.
     */
    data() {
      return {
        user: {
          email: null,
          password: null,
        },
      };
    },

    /**
     * The methods the page can use.
     */
    methods: {
      /**
       * Will log the user in.
       *
       * @param {Object} user The user to be logged in.
       */
      login(user) {
        this.$store.dispatch('auth/login', user);
      },
    },

    /**
     * The components the page can use.
     */
    components: {
      VLayout,
      VCard,
    },
  };
</script>
