<template>
  <div id="app" class="container my-3">
    <h3>Innit Vue</h3>

    <div class="card mt-3">
      <div class="card-header">Vue Axios GET</div>
      <div class="card-body">
        <div class="input-group input-group-sm">
          <button class="btn btn-sm btn-primary" @click="getAllData">Get All</button>

          <button class="btn btn-sm btn-warning ml-2" @click="clearGetOutput">Clear</button>
        </div>   
        
        <div v-if="getResult" class="alert alert-secondary mt-2" role="alert"><pre>{{getResult}}</pre></div>
      </div>
    </div>
 
  </div>
</template>

<script>
import http from "./http-common";

export default {
  name: "App",
  data() {
    return {
      getResult: null
    }
  },
  methods: {
    fortmatResponse(res) {
      return JSON.stringify(res, null, 2);
    },

    async getAllData() {
      try {
        const res = await http.get("/knights");

        const result = {
          status: res.status + "-" + res.statusText,
          headers: res.headers,
          data: res.data,
        };

        this.getResult = this.fortmatResponse(result);
      } catch (err) {
        this.getResult = this.fortmatResponse(err.response?.data) || err;
      }
    },

    clearGetOutput() {
      this.getResult = null;
    },
  }
}
</script>

<style>
#app {
  max-width: 600px;
  margin: auto;
}
</style>
