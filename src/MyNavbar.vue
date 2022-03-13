<template>
<ion-button @click="logout">logout</ion-button>
  <div class="navWrapper">
    
    <ion-menu side="start" menu-id="first" content-id="main">
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title>Start Menu</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <ion-list>
        <ion-item class="disabled">
          首頁
        </ion-item>
        <ion-item class="disabled">
          使用者:{{userInfo.userName}}
        </ion-item>
        <ion-item class="disabled" @click="logout">
          登出
        </ion-item>
      </ion-list>   
    </ion-content>
  </ion-menu>
  <ion-router-outlet id="main"></ion-router-outlet>
  </div>
</template>

<script lang="ts">
import { useStore } from 'vuex';
import { useRouter } from 'vue-router';
import { defineComponent } from 'vue';
import { IonButton, IonItem,IonList,IonContent,IonMenu,IonToolbar,IonHeader,IonTitle,menuController,IonRouterOutlet } from '@ionic/vue';
export default defineComponent({
  component:{IonItem,IonList,IonContent,IonMenu,IonToolbar,IonHeader,IonTitle,IonRouterOutlet,IonButton},
  setup() {
      const store = useStore();
      const router = useRouter();
      const { userInfo } = store.getters;
      const logout = async () => {
          await store.dispatch('EDIT_USER_INFO', {});
          router.push({
              name: 'LoginView'
          });
      };
      const home = async () => {
          await store.dispatch('EDIT_USER_INFO', {
              userName:userInfo.userName,
              userSection:userInfo.userSection,
              userType:userInfo.userType,
              password:userInfo.password,
          });
          router.push({
              name: 'home'
          });
      };
      return {
          userInfo,
          logout,
      };
      
  },
  data() {
      return {
          activeIndex: '/',
          isActive: false,
      }
  },
  methods: {
    openFirst() {
      menuController.enable(true, 'first');
      menuController.open('first');
    },
    openEnd() {
      menuController.open('end');
    },
    openCustom() {
      menuController.enable(true, 'custom');
      menuController.open('custom');
    }
  }
});
</script>