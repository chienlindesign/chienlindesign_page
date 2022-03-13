<template>
  <ion-page>
    <ion-header translucent>
      <ion-toolbar>
        <ion-button @click="logout">logout</ion-button>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <swiper>
        <swiper-slide>
          <ion-card>
          <img src="https://ionicframework.com/docs/demos/api/card/madison.jpg" />
          <ion-card-header>
            <ion-card-subtitle>Destination</ion-card-subtitle>
            <ion-card-title>Madison, WI</ion-card-title>
          </ion-card-header>
          <ion-card-content>
            Founded in 1829 on an isthmus between Lake Monona and Lake Mendota, Madison was named the capital of the
            Wisconsin Territory in 1836.
          </ion-card-content>
        </ion-card>
        </swiper-slide>
        <swiper-slide>
          <ion-card>
          <img src="https://ionicframework.com/docs/demos/api/card/madison.jpg" />
          <ion-card-header>
            <ion-card-subtitle>Destination</ion-card-subtitle>
            <ion-card-title>Madison, WI</ion-card-title>
          </ion-card-header>
          <ion-card-content>
            Founded in 1829 on an isthmus between Lake Monona and Lake Mendota, Madison was named the capital of the
            Wisconsin Territory in 1836.
          </ion-card-content>
        </ion-card>
        </swiper-slide>
        <swiper-slide>
          <ion-card>
          <img src="https://ionicframework.com/docs/demos/api/card/madison.jpg" />
          <ion-card-header>
            <ion-card-subtitle>Destination</ion-card-subtitle>
            <ion-card-title>Madison, WI</ion-card-title>
          </ion-card-header>
          <ion-card-content>
            Founded in 1829 on an isthmus between Lake Monona and Lake Mendota, Madison was named the capital of the
            Wisconsin Territory in 1836.
          </ion-card-content>
        </ion-card>
        </swiper-slide>
      </swiper>
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">{{res}}</ion-title>
        </ion-toolbar>
      </ion-header>
    
      <ExploreContainer name="tab1" />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
/* eslint-disable */
import { defineComponent } from 'vue';
import { useStore } from 'vuex';
import { useRouter } from 'vue-router';
import MyNavbar from '../MyNavbar.vue'
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton ,IonCard ,IonCardHeader ,IonCardSubtitle, IonCardTitle, IonCardContent } from '@ionic/vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import axios from 'axios';
import qs from "qs";
import 'swiper/css';
import '@ionic/vue/css/ionic-swiper.css';

export default  defineComponent({
  name: 'Tab1Page',
  components: { ExploreContainer, IonHeader, IonToolbar, IonTitle, IonContent, IonPage, Swiper, SwiperSlide, MyNavbar, IonButton,IonCard ,IonCardHeader ,IonCardSubtitle, IonCardTitle, IonCardContent },
  setup() {
      const store = useStore();
      const router = useRouter();
      const { userInfo } = store.getters;
      const logout = async () => {
          await store.dispatch('EDIT_USER_INFO', {});
          router.push({
              name: 'LoginView'
          });
          window.location.reload();
      };
      /*const home = async () => {
          await store.dispatch('EDIT_USER_INFO', {
              userName:userInfo.userName,
              userSection:userInfo.userSection,
              userType:userInfo.userType,
              password:userInfo.password,
          });
          router.push({
              name: 'home'
          });
      };*/
      return {
          userInfo,
          logout,
      };
  },
  mounted(){
    axios.post('http://tc-hejulove254-1.kddns.info:33366/home/public/api/testAPI',qs.stringify({key:"00000000"}))
      .then((response: any) => {
        // handle success
        this.res=response.data;
        console.log(response.data);
      })
      .catch(function (error) {
        // handle error
        console.log(error);
      })
      .then(function () {
        // always executed
      });
      
  },
  data(){
    return{
      res:''
    }
  },
});
</script>
