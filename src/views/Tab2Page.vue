<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <br>
        <ion-title> {{year}}年{{month}}月</ion-title>
      </ion-toolbar>
    </ion-header>
    
      <ion-content>
        
        <ion-item v-for="person in persons" :key="person.name">
          <h1>{{person.name}}</h1>
          <ion-item>
          <ion-label position="floating">內科</ion-label>
          <ion-input v-model="person.i" placeholder="內科"></ion-input>
          </ion-item>
          <ion-item>
          <ion-label position="floating">針傷</ion-label>
          <ion-input v-model="person.t" placeholder="針傷"></ion-input>
          </ion-item>
        </ion-item>
        <ion-button type="primary" @click="submit" expand="block">送出</ion-button>
      </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonLabel, IonInput,IonItem, IonButton} from '@ionic/vue';
//import ExploreContainer from '@/components/ExploreContainer.vue';
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import axios from 'axios';
import qs from "qs";
//import { Storage } from '@ionic/storage';

export default defineComponent({
  name: 'Tab2Page',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonLabel, IonInput, IonItem, IonButton },
  setup() {
    let persons=reactive({
      });
    axios.post('http://tc-hejulove254-1.kddns.info:33366/home/public/api/getPerson',qs.stringify({key:"00000000"}))
          .then((response: any) => {
              // handle success
              if (response.data.msg=='correct'){
                //console.log(persons.id=response.data.person[1].id);
              //store.dispatch('EDIT_USER_INFO', form);
              for(let x=0;x<response.data.person.length;x++){
                let singlePerson = reactive({
                      name: '',
                      employer_id:'',
                      employer:'',
                      id:'',
                      i:'',
                      t:'',
                });
                singlePerson.id=response.data.person[x].id;
                singlePerson.employer_id=response.data.person[x].employer_id;
                singlePerson.employer=response.data.person[x].employer;
                singlePerson.name=response.data.person[x].name;
                singlePerson.t;
                singlePerson.i;
                //persons[x]=singlePerson;
                persons[x]=singlePerson;
                console.log(persons);
                //persons.i=0;
                //persons.t=0;
              }
              }else{
                  return ;
              }
          })
          .catch(function (error) {
              // handle error
              console.log(error);
          })
          .then(function () {
              // always executed
          });
          
        //const MyStorage = new Storage();
        //MyStorage.create();
        //MyStorage.set('name', 'Mr. Ionitron');
        const d = new Date();
        const year = d.getFullYear();
        const month = d.getMonth();
        const ym = year+'/'+month;

        const formRef = ref(null);
        const router = useRouter();
        const store = useStore();
        let res = ref();
        const form = reactive({
            userName: '',
            password: '',
            isLogin:false,
        });
        const test = () => {
          console.log(persons);
        };
        const submit = () => {
          axios.post('http://tc-hejulove254-1.kddns.info:33366/home/public/api/send',qs.stringify({key:"00000000",msg:persons,ym:ym}))
          .then((response: any) => {
              //console.log(response);
              //console.log(response.data[1]);
              if(response.data[1]=='success'){
                alert('成功新增！');
                router.push({
                  path: '/tabs/tab1'
                });
              }
          })
          .catch(function (error) {
              console.log(error);
          });
        }
        return {
            form,
            submit,
            res,
            year,
            month,
            persons,
            test
        };   
    },
});
</script>
