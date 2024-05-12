<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>My Cards</ion-title>
      </ion-toolbar>
    </ion-header>
    
    <ion-content :fullscreen="true">
      <ion-row class="ion-align-items-center" style="height: 100%;">
        <!-- Left navigation bar -->
        <ion-col size="4">
          <ion-list>
            <ion-item
              v-for="item in items"
              :key="item.id"
              :class="{'font-bold': selectedItem === item.id}"
              @click="selectItem(item)"
              button>
              {{ item.text }}
            </ion-item>
          </ion-list>
        </ion-col>

        <!-- Right static content area -->
        <ion-col size="8">
          <ion-card @touchstart="handleTouchStart" @touchmove="handleTouchMove" @touchend="handleTouchEnd">
            <img alt="Silhouette of mountains" src="https://ionicframework.com/docs/img/demos/card-media.png" />
            <ion-card-header>
              <ion-card-title v-text="cardTitle"></ion-card-title>
              <ion-card-subtitle>Card Subtitle</ion-card-subtitle>
            </ion-card-header>

            <ion-card-content v-text="text_test"></ion-card-content>
          </ion-card>
        </ion-col>
      </ion-row>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonCol, IonList, IonRow, IonItem, IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import { ref } from 'vue';

const selectedItem = ref('general'); // 使用 ref 来声明响应式数据，初值为 'general'
const cardTitle = ref('General Cards');

// 选择项目时更新 selectedItem
const selectItem = (item) => {
  selectedItem.value = item.id;
  cardTitle.value = item.cardTitle;
};

// Items for the navigation bar
const items = ref([
  { id: 'general', text: 'General', cardTitle: 'Geneal Cards' },
  { id: 'seminar', text: 'Seminar', cardTitle: 'Seminar Cards' },
  { id: 'limitTime', text: 'Limit Time', cardTitle: 'Limit Time Cards' },
  { id: 'private', text: 'Private', cardTitle: 'Private Cards' },
  { id: 'unsorted', text: 'Unsorted', cardTitle: 'Unsorted Cards' }
]);

const text_test_arr = ref(["You've swipe right", "You've swipe left"]);
const text_test = ref("You've swipe right");

const startX = ref(0);
const startY = ref(0);
const endX = ref(0);
const endY = ref(0);

function handleTouchStart(event: TouchEvent) {
  console.log("Start");
  if (event.changedTouches.length === 1) {
    startX.value = event.changedTouches[0].clientX;
    startY.value = event.changedTouches[0].clientY;
  }
}

function handleTouchMove(event: TouchEvent) {
  if (event.changedTouches.length === 1) {
    endX.value = event.changedTouches[0].clientX;
    endY.value = event.changedTouches[0].clientY;
  }
}

function handleTouchEnd() {
  console.log("End");
  const deltaX = endX.value - startX.value;
  const deltaY = endY.value - startY.value;
  if (Math.abs(deltaX) > Math.abs(deltaY)) { // 确认是水平方向滑动
    if (deltaX > 0) {
      console.log("deltaX:" + deltaX);
      console.log("deltaY:" + deltaY);
      swipeRight();
    } else {
      console.log("deltaX:" + deltaX);
      console.log("deltaY:" + deltaY);
      swipeLeft();
    }
  }
}

function swipeRight() {
  console.log('Swiped right');
  text_test.value = text_test_arr.value[0];
}

function swipeLeft() {
  console.log('Swiped left');
  text_test.value = text_test_arr.value[1];
}


</script>

<style>
/* Ensuring the navigation bar and content area are aligned properly */
ion-col {
  display: flex;
  flex-direction: column;
  padding: 10px;
}

/* Styling for the navigation bar items */
ion-item {
  cursor: pointer;
  margin-bottom: 10px;
}

/* Bold style for the selected item */
.font-bold {
  font-weight: bold;
  color: blue; /* Optional: makes it easier to see which item is selected */
}
</style>


