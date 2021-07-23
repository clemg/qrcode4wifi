<template>
  <div id="card" class="rounded-xl py-4 px-8">
      <v-radio-group
          @change="renderQRCode()"
          v-model="network_encryption"
          mandatory>
        <v-radio
            label="WEP"
            value="WEP"
        ></v-radio>
        <v-radio
            label="WPA/WPA2"
            value="WPA"
        ></v-radio>
        <v-radio
            label="No password"
            value="nopass"
        ></v-radio>
      </v-radio-group>

      <div id="printable-all" @input="renderQRCode()">
        <div id="printable-nopass">
          <div class="text-center d-flex flex-column align-center">
            <vue-qrcode
                id="qr-code"
                :value="value"
                errorCorrectionLevel="H"
                width="170"
            />
          </div>


          <v-text-field
              label="Wifi name"
              maxlength="32"
              v-model="ssid"
              counter="32"
              required
              id="ssid"
          ></v-text-field>
        </div>

        <v-text-field
            label="Wifi password"
            v-model="password"
            id="password"
        ></v-text-field>
      </div>

      <div class="text-center d-flex flex-column align-center">
        <v-switch
            label="Hide password on printing ?"
            v-model="hidePassword"
            hide-details
            class="py-4"
        ></v-switch>

        <v-btn
            elevation="2"
            v-on:click="print()"
        >
          Print
          <v-icon right>
            mdi-printer
          </v-icon>
        </v-btn>
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
      network_encryption: "WPA",
      hidePassword: false,
      value: "WIFI:T:WPA;S:;P:;;",
    }
  },
  methods: {
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
      if (this.hidePassword) {
        page.document.write(document.getElementById("printable-nopass").innerHTML);
      } else {
        page.document.write(document.getElementById("printable-all").innerHTML);
      }
      // Filling data not copied by getElementById()
      page.document.getElementById("ssid").value = this.ssid;
      page.document.getElementById("password").value = this.password;
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