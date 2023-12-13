<template>
<q-page class="flex flex-center">
    <q-card  class="q-gutter-md"  style="width: 500px">
      <q-card class="flex flex-center">
        <img
          src="~assets/ezh.jpg"
          style="width: 200px; height: 200px"
        >
      </q-card>
      <q-card class="q-gutter-md">
        <div class="flex flex-center q-gutter-md">
          <q-btn color="amber"  label="turn DOWN" @click="btnClick2()" />
          <q-btn color="amber"  label="turn UP" @click="btnClick3()" />
        </div>
      </q-card>
    <q-linear-progress style="width: 420px" stripe rounded size="50px" :value="progress_bar" color="warning" class="q-mt-sm" />
    <q-card class="flex flex-center q-gutter-md"> {{ progress_bar }}</q-card>
    </q-card>
  </q-page>
</template>

<script>
import { ref } from 'vue'
import axios from 'axios';

export default {
  setup () {
    return {
      progress_bar: ref(0),
      intervalCtx: null,
    }
  },
  mounted() {
    this.intervalCtx = setInterval(async () => {
      axios.get('http://192.168.52.9:5000/get_value')
        .then( (response) => {
          this.progress_bar = response.data;
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(function () {
        });
    }, 200);
  },
  unmounted() {
    clearInterval(this.intervalCtx);
  },
  methods:{
    btnClick1(){
      axios.post('http://192.168.52.9:5000/sum', {
        text: this.text.trim(),
      })
      .then((response) =>{
        this.res = response.data.trim();
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
        },
      btnClick2(){
        // this.progress2 +=0.05;
        axios.get('http://192.168.52.9:5000/down_value')
        .then( (response) => {
          this.progress_bar = response.data;
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(function () {
        });
        },
      btnClick3(){
        axios.get('http://192.168.52.9:5000/up_value')
        .then( (response) => {
          this.progress_bar = response.data;
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(function () {
        });
        }
  }
}
</script>
