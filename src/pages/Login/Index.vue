<template>
  <v-layout>
    <v-card contextual-style="dark">
      <span slot="header">
        Calculator
      </span>
      <div slot="body">
        <form @submit.prevent="login(user)">
          <div class="form-group">
            <div class="input-group">
              <div class="input-group-addon">
                <i class="fa fa-map-marker fa-fw"></i>
              </div>
              <input
                v-model="user.email"
                type="email"
                placeholder="Entar a loction"
                class="form-control"
              >
            </div>
          </div>
          <div class="form-group">
            <div class="input-group">
              <div class="input-group-addon">
                <i class="fa fa-map fa-fw"></i>
              </div>
              <input
                v-model="user.password"
                type="password"
                placeholder="Enter a location"
                class="form-control"
              >
            </div>
          </div>
          <div class="form-group">
            <button class="btn btn-outline-primary" style="">
              Calculate
            </button>
          </div>
        </form>
        <div>
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
