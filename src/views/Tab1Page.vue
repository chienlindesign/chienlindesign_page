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
           <MonthlyChart 
            v-if="state.isLoaded" 
            v-bind:chartData="state.chartData" 
            v-bind:chartOptions="state.chartOptions" 
          />
        </ion-card>
        </swiper-slide>
        <swiper-slide>
          <ion-card>
          <LineChart 
            v-if="state.isLoaded" 
            v-bind:chartData="state.chartData" 
            v-bind:chartOptions="state.chartOptions" 
          />
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
          <!--<ion-title size="large">{{res}}</ion-title>-->
        </ion-toolbar>
      </ion-header>
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
import MonthlyChart from '@/components/ExploreContainer.vue';
import LineChart from '@/components/LineChart.vue';
import axios from 'axios';
import qs from "qs";
import 'swiper/css';
import '@ionic/vue/css/ionic-swiper.css';
//import { Chart } from 'chart.js';


export default  defineComponent({
  name: 'Tab1Page',
  components: { LineChart, MonthlyChart, IonHeader, IonToolbar, IonTitle, IonContent, IonPage, Swiper, SwiperSlide, MyNavbar, IonButton,IonCard ,IonCardHeader ,IonCardSubtitle, IonCardTitle, IonCardContent },

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
    const d = new Date();
    const year = d.getFullYear();
    const month = d.getMonth();
    const ym = year+'/'+month;
    console.log(ym);

    this.state.isLoaded = false;
    axios.post('http://tc-hejulove254-1.kddns.info:33366/home/public/api/getPerf',qs.stringify({key:"00000000",ym:ym}))
      .then((response: any) => {
        // handle success
        this.res=response.data;
        this.state.chartData = response.data;
        this.state.isLoaded = true;
        console.log(response.data);
        console.log(this.state.chartData);
        console.log(this.chartData1);
      })
      .catch(function (error) {
        // handle error
        console.log(error);
      })
      .then(function () {
        // always executed
      });
      
  },
  data () {
    return {
      res:'',
      state: {
        isLoaded: false,
        chartData: null,
        chartOptions: {
          responsive: false
        }
      },
      chartData1: {
          datasets: [
            {
              data: [1, 2, 3, 4],
              backgroundColor: ['Red', 'Yellow', 'Blue', 'Green']
            }
          ],
          // These labels appear in the legend and in the tooltips when hovering different arcs
          labels: ['Red', 'Yellow', 'Blue', 'Green']
        }
      
    }
  }
});
</script>
