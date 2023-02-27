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
          <ion-title>
            Pick a Color
          </ion-title>
        </ion-card-header>
        <ion-card-content class="ion-text-center ion-margin">
          <div>
          <input type="color" v-model="selectedColour" @input="setColor" />
        </div>
      </ion-card-content>
      </ion-card>
      <ion-card>
        <ion-card-header>
          <ion-title>
            Preview
          </ion-title>
          </ion-card-header>
          <ion-card-content class="ion-margin">
            <div :style="{ backgroundColor: selectedColour }" class="color-box"></div>
          </ion-card-content>
      </ion-card>
      <ion-card>
        <ion-card-header>
          <ion-title>
            HEX Code
          </ion-title>
        </ion-card-header>
        <ion-card-content class="ion-margin">
          <ion-item lines="none">
            <ion-label>
                {{ selectedColour }}
            </ion-label>
            <ion-button color="light" slot="end" @click="copyHex">
              <ion-icon :icon="copyOutline" slot="start"></ion-icon>
              Copy
            </ion-button>
          </ion-item>
        </ion-card-content>
      </ion-card>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar, toastController  } from '@ionic/vue';
import { IonIcon } from '@ionic/vue';
  import { copyOutline } from 'ionicons/icons';
</script>

<script lang="ts">
export default {
  data() {
    return {
      selectedColour: "#000000",
    };
  },
  methods: {
    setColor() {
      this.$emit("updateColor", this.selectedColour);
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
          message: 'Copied to clipboard!',
          duration: 1500,
          position: 'top'
        });
        await toast.present();
}
  },
};
</script>

<style>
.color-box {
  width: 100%;
  height: 50px;
  margin-top: 10px;
}
</style>