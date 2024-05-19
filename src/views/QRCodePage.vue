<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>QR Code</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true" class="ion-padding">
      <div class="container">
        <ion-item lines="none">
          <qrcode-vue :value="value" :level="level" :render-as="renderAs" />
        </ion-item>

        <ion-item lines="none">
          <ion-input v-model="inputValue" placeholder="QR Code Content"></ion-input>
        </ion-item>

        <ion-item lines="none" class="buttons">
          <ion-button @click="generateQRCode">Generate</ion-button>
          <ion-button @click="reset" :disabled="!inputValue">Reset</ion-button>
        </ion-item>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonItem,
  IonInput,
  IonButton,
} from '@ionic/vue';
import { ref } from 'vue';
import QrcodeVue, { Level, RenderAs } from 'qrcode.vue';

// 定義組件狀態
const value = ref('qrcode');
const level = ref<Level>('M');
const renderAs = ref<RenderAs>('svg');

const inputValue = ref('Nothing');

// 定義方法
const generateQRCode = () => {
  value.value = inputValue.value;
};

const reset = () => {
  inputValue.value = '';
  value.value = '';
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  text-align: center;
}

.buttons {
  display: flex;
  justify-content: center;
  gap: 10px;
  width: 100%;
  margin-top: 10px;
}

ion-item {
  width: 100%;
  max-width: 400px;
}
</style>
