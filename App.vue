<template>
  <view class="container">

    <view class="text-container">
        <text>Vue Native</text>
    </view>

    <button
    :on-press="registerClientUser"
    title="Grant token"
    color="#841584"
    accessibility-label="heyy"/>

    <text-input
    :style="{height: 40, borderColor: 'gray', borderWidth: 1}"
    v-model="username"/>

    <text-input
    :style="{height: 40, borderColor: 'gray', borderWidth: 1}"
    v-model="password"/>

    <text class="text-color-primary">{{sandboxText}}</text>

  </view>
</template>

<script>

import Expo from "expo";

import ComponentTest  from "./ComponentTest.vue";

export default {

  components: {
    ComponentTest
  },

  data: function () {
    return {
      sandboxText: "",
      username: "",
      password: "",
      api: ""
    }
  },

  methods: {

    registerClientUser: function () {

      console.log(this.username);
      console.log(this.password);

      fetch(this.api+'/oauth/token', {

        method: 'POST',
        headers: {
        Accept: 'application/json',
          'Content-Type': 'application/json'
        },

        body: JSON.stringify({

            grant_type : 'password',
            client_id : '1',
            client_secret : 'Y1S0sGl775TVx7Mk9hbysyZi3j3KztFCwJJOnFwx',
            username : this.username,
            password : this.password,
            scope : ''

        }),

      }).then(response => {
        console.log(response);
        response.json().then(jsonresponse => {
          this.sandboxText = jsonresponse;
        });
      });

    }

  },

  mounted: function () {

    const { manifest } = Expo.Constants;

    this.api = (typeof manifest.packagerOpts === `object`) && manifest.packagerOpts.dev
    ? manifest.debuggerHost.split(`:`).shift().concat(`:8000`)
    : `api.example.com`;

    this.api = "http://sandboxrest.test";

    fetch(this.api+'/api/test').catch(error => {
      console.log(error);
    }).then(result => {
      //console.log(result);
      result.json().then(jsonres => {
        console.log(jsonres);
        this.sandboxText = jsonres;
      });
    });
  
  }

}
</script>
 
<style>
.container {
  background-color: white;
  margin-top:10%;
}
.text-color-primary {
  color: blue;
}
</style>
