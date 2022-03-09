<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>{{res}}</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
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
import { defineComponent } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import axios from 'axios';
import qs from "qs";


export default  defineComponent({
  name: 'Tab1Page',
  components: { ExploreContainer, IonHeader, IonToolbar, IonTitle, IonContent, IonPage },
 
  mounted(){
    //axios.post('http://tc-hejulove254-1.kddns.info:33366/home/public/api/testAPI',qs.stringify({key:"00000000"}))
    axios.get('https://reurl.cc/yQGK68')
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
