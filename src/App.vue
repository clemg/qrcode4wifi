<template>
  <v-app align="center">
    <div class="pa-8">
      <h1>
        <v-icon>mdi-wifi-star</v-icon>
        QR Code 4 wifi
        <v-icon>mdi-wifi-star</v-icon>
      </h1>

      <div class="mt-6">
        <p>Enter your wifi point information and get a QR code to scan for your guests.
          <br>You can print it to stick the code on the fridge, on the door or just keep it on your desk.
          <br>Source available at: <a href="https://github.com/clemg/qrcode4wifi" target="_blank">github.com/clemg/qrcode4wifi</a>
        </p>
      </div>

      <v-main>
        <Card/>
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
  }
};
</script>