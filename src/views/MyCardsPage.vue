<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>My Cards</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">My Cards</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content>
        <ion-row class="ion-align-items-center" style="height: 100%;">
          <ion-col size="4">
            <ion-list>
              <ion-col size="3" class="custom-slider">
                <!-- Custom slider for items -->
                <div class="custom-slider" @click="handleScroll">
                  <div
                    v-for="item in items" 
                    :key="item.id" 
                    :data-id="item.id" 
                    :class="{'font-bold': selectedItem.value === item.id}" 
                    @click="selectItem(item.id)" 
                    :ref="itemRefs">
                    <ion-item button>{{ item.text }}</ion-item>
                  </div>
                </div>
              </ion-col>
            </ion-list>
          </ion-col>
          <ion-col size="8">
            <!-- Dynamic Component -->
            <!-- <component :is="currentComponent"></component> -->
          </ion-col>
        </ion-row>
      </ion-content>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonApp, IonCol, IonList, IonRow, IonItem, IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonBIonLabel, IonSegment, IonSegmentButtontton } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import { ref, onMounted } from 'vue';

const selectedItem = ref({ id: "" }); // 使用ref来声明响应式数据

// 选择项目时更新 selectedItem
const selectItem = (id) => {
  selectedItem.value = id;
};

const handleScroll = (event) => {
    // 获取容器和项目的 DOM 元素
    console.log(event);
    const container = event.target;
    const items = container.children;

    // 容器中点的位置
    const centerPosition = container.scrollTop + container.offsetHeight / 2;

    // 用于存储最接近中心的项目和其偏移量
    let closestItem = ref<HTMLElement | null>(null);
    let closestDistance = Infinity;

    console.log(items);
    // 遍历项目，找到中心点最接近的项目
    Array.from<HTMLElement>(items).forEach((item: HTMLElement) => {
      // 项目中心点的位置
      const itemCenter = item.offsetTop + item.offsetHeight / 2;
      // 计算中心点的距离
      const distance = Math.abs(centerPosition - itemCenter);
      console.log(distance);
      // 检查是否是最接近的
      if (distance < closestDistance) {
        console.log(444);
        closestDistance = distance;
        closestItem = item; // 正确地改变 ref 的值
      }
      console.log(item);
    });

    // 如果找到了最接近的项目，则更新选中状态
    if (closestItem) {
      console.log(555);
      // selectedItem.value = closestItem;
    }
}

// 示例数据，确保每个元素都有一个唯一的 id
const items = ref([
  { id: 'general', text: 'General' },
  { id: 'seminar', text: 'Seminar' },
  { id: 'limitTime', text: 'Limit Time' },
  { id: 'private', text: 'Private' },
  { id: 'unsorted', text: 'Unsorted' }
]);

const itemRefs = ref([])

onMounted(() => {
  alert(itemRefs.value.map(i => i.textContent))
})
</script>

<style>
.custom-slider .slider-container {
  display: flex;
  flex-direction: column;
  overflow-y: scroll; /* Allow vertical scrolling */
  height: 200px; /* Set a fixed height */
}

.custom-slider ion-item {
  flex: none; /* Prevent items from growing or shrinking */
  margin-bottom: 50px; /* Adjust the margin as needed */
}

.font-bold {
  font-weight: bold;
}
</style>
