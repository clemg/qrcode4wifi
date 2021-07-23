<template>
  <v-app>
    <div class="pa-8">
      <div align="center">
        <h1>QR Code 4 wifi</h1>

        <div class="mt-6">
          <p>Enter your wifi point information and get a QR code to scan for your guests.
            <br>You can print it to stick the code on the fridge, the door or just keep it on your desk.
            <br>Source available at: <a href="https://github.com/clemg/qrcode4wifi" target="_blank">github.com/clemg/qrcode4wifi</a></p>
        </div>
      </div>


      <v-main>
        <Card />
      </v-main>

    </div>

  </v-app>
</template>

<script>
import Card from './components/Card';

export default {
  name: 'App',

  components: {
    Card,
  },

  data: () => ({
    //
  }),
};
</script>


<style>
/*html, body {
  margin: 2vw;
}*/
</style>