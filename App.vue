<template>
  <view class="container">
    <text class="text-color-primary">{{sandboxText}}</text>
    </view>
</template>

<script>

import Expo from "expo";

export default {
  data: function () {
    return {
      sandboxText: ""
    }
  },
  mounted: function () {

    const { manifest } = Expo.Constants;
    const api = (typeof manifest.packagerOpts === `object`) && manifest.packagerOpts.dev
    ? manifest.debuggerHost.split(`:`).shift().concat(`:8080`)
    : `api.example.com`;

    console.log(api+"/test");
    fetch("http://"+api+"/test").catch(error => {
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
  align-items: center;
  justify-content: center;
  overflow: scroll;
  flex: 1;
}
.text-color-primary {
  color: blue;
}
</style>
