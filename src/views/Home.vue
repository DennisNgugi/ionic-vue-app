<template>
  <div class="ion-page">
    <ion-header>
      <ion-toolbar>
        <ion-title>
          ZipInfo
        </ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <ZipSearch v-on:get-zip="getZipInfo" />
      <ZipInfo v-bind:info="info"/>
      <ClearInfo v-bind:info="info" v-on:clear-info="clearInfo"/>
    </ion-content>
  </div>
</template>

<script>
// @ is an alias to /src
import ZipInfo from '../components/ZipInfo'
import ZipSearch from '../components/ZipSearch'
import ClearInfo from '../components/ClearInfo'

export default {
  name: 'Home',
  components: {
    ZipSearch,
    ZipInfo,
    ClearInfo
  },
  data(){
    return{
      info:null,
    }
  },
  methods:{
    async getZipInfo(zip){
      const resp = await fetch(`http://api.zippopotam.us/us/${zip}`);
        if(resp.status == 404){
          this.showAlert();
        }
        this.info = await resp.json();

    },
    showAlert(){
      return this.$ionic.alertController
      .create({
        header:"Not valid",
        message:"Please enter a valid Us zipcode",
        buttons:["OK"]
      }).then(a=>a.present()
    );
  },
  clearInfo(){
    this.info = ""
  }
  }
}
</script>
