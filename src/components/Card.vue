<template>
  <div id="card" class="rounded-xl py-4 px-8">
    <div id="networkEncryptionOptions"
         v-show="!printing">
      <v-radio-group
          @change="renderQRCode()"
          v-model="network_encryption"
          mandatory
      >
        <h3>Choose your network's encryption type:</h3>
        <small class="pb-2">If you don't know what to chose, most common is WPA/WPA2.</small>
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
    </div>

    <div
        @input="renderQRCode()"
    >
      <div class="text-center d-flex flex-column align-center">
        <vue-qrcode
            id="qr-code"
            :value="value"
            errorCorrectionLevel="H"
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

      <div id="passwordField">
        <v-text-field
            label="Wifi password"
            v-model="password"
            id="password"
            v-show="!hidePassword || !printing"
        ></v-text-field>
      </div>
    </div>

    <div id="printingOptions">
      <div
          class="text-center d-flex flex-column align-center"
      >
        <v-switch
            label="Hide password on printing ?"
            v-model="hidePassword"
            hide-details
            class="py-4"
            v-show="!printing">
        ></v-switch>

        <v-btn
            elevation="2"
            v-on:click="print"
            v-show="!printing"
        >
          Print
          <v-icon right>
            mdi-printer
          </v-icon>
        </v-btn>
      </div>
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
      printing: false,
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
      if (!(this.ssid.length > 0)) return alert("Wifi name cannot be empty!");
      if (this.network_encryption === "WPA" && !(this.password.length >= 8))
        return alert("Password must be at least 8 characters long!");
      if (this.network_encryption === "WEP" && !(this.password.length >= 6))
        return alert("Password must be at least 6 characters long!");

      this.printing = true;
      this.$nextTick(function() {
        window.print();
        this.printing = false;
      });
    }
  }
}
</script>

<style>
#card {
  border: 2px dashed;
}
</style>