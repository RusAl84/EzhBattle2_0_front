<template>
  <q-page class="flex flex-center">
    <q-card class="q-gutter-md" style="width: 500px">
      <q-card class="flex flex-center">
        <img src="~assets/ezh.jpg" style="width: 200px; height: 200px" />
      </q-card>
      <q-card class="q-gutter-md">
        <div class="flex flex-center q-gutter-md">
          <q-btn color="amber" label="turn DOWN" @click="btnTurnDOWN()" />
          <q-btn color="amber" label="turn UP" @click="btnTurnUP()" />
        </div>
      </q-card>
      <q-linear-progress
        style="width: 460px"
        stripe
        rounded
        size="50px"
        :value="progress_bar"
        color="warning"
        class="q-mt-sm"
      />
      <q-card class="flex flex-center q-gutter-md"> {{ progress_bar }}</q-card>
    </q-card>
  </q-page>
</template>

<script>
import { ref } from "vue";
import axios from "axios";

export default {
  setup() {
    return {
      progress_bar: ref(0),
      intervalCtx: null,
      serv_url: "http://192.168.51.29:5000",
    };
  },
  mounted() {
    this.intervalCtx = setInterval(async () => {
      axios
        .get(this.serv_url + "/get_value")
        .then((response) => {
          this.progress_bar = response.data;
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(function () {});
    }, 200);
  },
  unmounted() {
    clearInterval(this.intervalCtx);
  },
  methods: {
    btnTurnDOWN() {
      // this.progress2 +=0.05;
      axios
        .get(this.serv_url + "/down_value")
        .then((response) => {
          this.progress_bar = response.data;
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(function () {});
    },
    btnTurnUP() {
      axios
        .get(this.serv_url + "/up_value")
        .then((response) => {
          this.progress_bar = response.data;
        })
        .catch(function (error) {
          console.log(error);
        })
        .finally(function () {});
    },
  },
};
</script>
