<template>
  <div id="card">
    <vue-qrcode :value="value" errorCorrectionLevel="H" />

    <div @input="renderQRCode()">
      <div @input="updateNetworkEncryptionType($event.target.value)">
        <input type="radio" id="WEP" name="networkEncryptionType" value="WEP">
        <label for="WEP">WEP</label>

        <input type="radio" id="WPA/WPA2" name="networkEncryptionType" value="WPA" checked>
        <label for="WPA/WPA2">WPA/WPA2</label>

        <input type="radio" id="None" name="networkEncryptionType" value="nopass">
        <label for="None">None</label>
      </div>

      <label for="wifi-ssid">Wifi name</label>
      <input name="wifi-ssid" id="wifi-ssid" @input="updateQRCodeSSID($event.target.value)" required>

      <label for="wifi-password">Wifi password</label>
      <input name="wifi-password" id="wifi-password" min="8" @input="updateQRCodePassword($event.target.value)">
    </div>
  </div>
</template>

<script>
import VueQrcode from 'vue-qrcode'

export default {
  name: 'Card',
  components: {
    VueQrcode,
  },
  data() {
    return {
      ssid: "",
      password: "",
      network_encryption: "WAP",
      value: " ",
    }
  },
  methods: {
    updateQRCodeSSID(ssid) {
      this.ssid = escape(ssid);
    },
    updateQRCodePassword(password) {
      this.wifipassword = password;
    },
    updateNetworkEncryptionType(mode) {
      this.network_encryption = mode;
    },
    renderQRCode() {
      this.value =
          "WIFI:T:" + this.network_encryption +
          ";S:" + this.ssid +
          ";P:" + this.password +
          ";;";
    }
  }
}
</script>

<style>
#card {
  border: 2px dashed;
}
</style>