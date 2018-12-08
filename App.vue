<template>
  <view class="container">

    <view class="text-container">
        <text>Vue Native</text>
    </view>

    <text-input
    :style="{height: 40, borderColor: 'gray', borderWidth: 1}"
    v-model="username"/>

    <text-input
    :style="{height: 40, borderColor: 'gray', borderWidth: 1}"
    v-model="password"/>

    <text class="text-color-primary">{{sandboxText}}</text>

    <button
    :on-press="registerClientUser"
    title=""
    color="#841584"
    accessibility-label="heyy"/>

  </view>
</template>

<script>

import Expo from "expo";

export default {

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

      fetch('http://'+this.api+'/oauth/token', {

        method: 'POST',
        headers: {
        Accept: 'application/json',
          'Content-Type': 'application/json'
        },

        body: JSON.stringify({

            grant_type : 'password',
            client_id : '4',
            client_secret : 'secret here',
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

    fetch('http://'+this.api+'/api/test').catch(error => {
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
