<template>
  <div id="card">
    <div @input="updateNetworkEncryptionType($event.target.value)">
      <input type="radio" id="WEP" name="networkEncryptionType" value="WEP">
      <label for="WEP">WEP</label>

      <input type="radio" id="WPA/WPA2" name="networkEncryptionType" value="WPA" checked>
      <label for="WPA/WPA2">WPA/WPA2</label>

      <input type="radio" id="None" name="networkEncryptionType" value="nopass">
      <label for="None">None</label>
    </div>

    <div id="printable" @input="renderQRCode()">
      <vue-qrcode :value="value" errorCorrectionLevel="H" />

      <label for="wifi-ssid">Wifi name</label>
      <input name="wifi-ssid" id="wifi-ssid" @input="updateQRCodeSSID($event.target.value)" required>

      <label for="wifi-password">Wifi password</label>
      <input name="wifi-password" id="wifi-password" min="8" @input="updateQRCodePassword($event.target.value)">
    </div>

    <button type="button" v-on:click="print()">Print</button>
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
    },
    print() {
      if (! (this.ssid.length > 0)) return alert("Wifi name cannot be empty!");
      if (this.network_encryption === "WPA" && ! (this.password.length >= 8))
        return alert("Password must be at least 8 characters long!");
      if (this.network_encryption === "WEP" && ! (this.password.length >= 6))
        return alert("Password must be at least 6 characters long!");

      let page = window.open("", "PRINT");

      page.document.write("<!DOCTYPE html><html lang='en'><head><title>" + document.title + "</title>");
      page.document.write("</head><body>");
      page.document.write(document.getElementById("printable").innerHTML);
      page.document.write("</body></html>");

      page.document.close();
      page.focus();
      page.print();
      page.close();
    }
  }
}
</script>

<style>
#card {
  border: 2px dashed;
}
</style>