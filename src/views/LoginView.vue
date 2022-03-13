<template>
    <ion-content fullscreen>
        <ion-card id="card">
            
            <ion-card-content>
                    <ion-item>
                    <ion-label position="floating">帳號</ion-label>
                    <ion-input v-model="form.userName"></ion-input>
                    </ion-item>
                    <ion-item>
                    <ion-label position="floating">密碼</ion-label>
                    <ion-input type="password" v-model="form.password"></ion-input>
                    </ion-item>
                    <br>
                    <ion-button type="primary" @click="submit"> 登入</ion-button>
            </ion-card-content>
        </ion-card>
    </ion-content>
</template>

<script lang="ts">
import { IonCard,IonLabel,IonInput,IonButton,IonCardContent,IonItem,IonContent } from '@ionic/vue';
import { reactive, ref, watch } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import { defineComponent } from 'vue';
import axios from 'axios';
import qs from "qs";

export default defineComponent({
    name: 'LoginView',
    components: {IonCard,IonLabel,IonInput,IonButton,IonCardContent,IonItem,IonContent},
    setup() {
        const formRef = ref(null);
        const router = useRouter();
        const store = useStore();
        let res = ref();
        const form = reactive({
            userName: '',
            password: '',
            isLogin:false,
        });
        const rules = reactive({
            userName: [{ required: true, message: '請填寫使用者名稱', trigger: ['blur', 'change'] }],
            password: [{ required: true, message: '請填寫密碼', trigger: ['blur', 'change'] }]
        });
        const submit = () => {
            //if(formRef.value){
                axios.post('http://tc-hejulove254-1.kddns.info:33366/home/public/api/testAPI',qs.stringify({key:form.password}))
                .then((response: any) => {
                    // handle success
                    if (response.data=='correct'){
                    form.isLogin = true;
                    store.dispatch('EDIT_USER_INFO', form);
                    router.push({
                        path: '/tabs/tab1'
                    });
                    window.location.reload();
                    }else{
                        alert('帳號或密碼錯誤!');
                        res = response.data;
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
        //}
        }
        watch(res, (nV, oV) => {
            console.log(nV, oV)
            console.log(res.value)
            }, {
            immediate: true
            }) 
        return {
            form,
            //rules,
            //formRef,
            submit,
            res,
        };   
    },

});
</script>
<style scoped>
#card {
  align-items: center;
  justify-content: center;
  display: flex;
  border-radius: 8px;
  box-shadow: 1px 2px 8px rgba(180, 180, 180, 0.65);
  height: 400px;
  margin: 6rem auto 4rem auto;
  width: 329px;
}
</style>