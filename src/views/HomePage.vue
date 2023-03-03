<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Color Picker Demo</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-card class="">
        <ion-card-header>
          <ion-title>Pick a Color</ion-title>
        </ion-card-header>
        <ion-card-content class="ion-text-center ion-margin">
          <color-picker @input="onInput"></color-picker>
        </ion-card-content>
      </ion-card>
      <ion-card>
        <ion-card-header>
          <ion-title>Preview</ion-title>
        </ion-card-header>
        <ion-card-content class="ion-margin">
          <div
            :style="{ backgroundColor: selectedColour }"
            class="color-box"></div>
        </ion-card-content>
      </ion-card>
      <ion-card>
        <ion-card-header>
          <ion-title>HEX Code</ion-title>
        </ion-card-header>
        <ion-card-content class="ion-margin">
          <ion-item lines="none">
            <ion-label>
              {{ selectedColour }}
            </ion-label>
            <ion-button color="light" slot="end" @click="copyHex">
              <ion-icon :icon="copyOutlineIcon" slot="start"></ion-icon>
              Copy
            </ion-button>
          </ion-item>
        </ion-card-content>
      </ion-card>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
  import {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    toastController,
  } from "@ionic/vue";
  import { IonIcon } from "@ionic/vue";
  import { copyOutline } from "ionicons/icons";
  import { reactive } from "vue";
  import ColorPicker from "@radial-color-picker/vue-color-picker";

  export default {
    components: { ColorPicker },
    setup() {
      const color = reactive({
        hue: 50,
        saturation: 100,
        luminosity: 50,
        alpha: 1,
      });
    },
    data() {
      return {
        selectedColour: "#000000",
        copyOutlineIcon: copyOutline, // Import the copyOutline icon and use it in the template
      };
    },
    mounted() {
      console.log("hello");
    },
    methods: {
      onInput(hue) {
        this.selectedColour = this.hueToHex(hue);
      },
      hueToHex(hue) {
        const huePrime = hue / 60;
        const c = 1;
        const x = 1 - Math.abs((huePrime % 2) - 1);
        let rgb;

        if (huePrime >= 0 && huePrime <= 1) {
          rgb = [c, x, 0];
        } else if (huePrime > 1 && huePrime <= 2) {
          rgb = [x, c, 0];
        } else if (huePrime > 2 && huePrime <= 3) {
          rgb = [0, c, x];
        } else if (huePrime > 3 && huePrime <= 4) {
          rgb = [0, x, c];
        } else if (huePrime > 4 && huePrime <= 5) {
          rgb = [x, 0, c];
        } else if (huePrime > 5 && huePrime <= 6) {
          rgb = [c, 0, x];
        }

        const [r, g, b] = rgb.map((value) => {
          const v = Math.round(value * 255);
          return v.toString(16).padStart(2, "0");
        });

        return `#${r}${g}${b}`;
      },
      async copyHex() {
        const text = this.selectedColour;
        const textarea = document.createElement("textarea");
        textarea.value = text;
        document.body.appendChild(textarea);
        textarea.select();
        document.execCommand("copy");
        document.body.removeChild(textarea);
        const toast = await toastController.create({
          message: "Copied to clipboard!",
          duration: 1500,
          position: "top",
        });
        await toast.present();
      },
    },
  };
</script>

<style>
  @import "@radial-color-picker/vue-color-picker/dist/vue-color-picker.min.css";
  .color-box {
    width: 100%;
    height: 50px;
    margin-top: 10px;
  }
</style>
