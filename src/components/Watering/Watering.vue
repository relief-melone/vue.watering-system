<template>
    <div class="container">
        <h1>Bewässerung aktivieren</h1>

    <form class="form-watering">
        <button class="btn btn-primary" v-if="!WateringInitialized" v-on:click="initWatering">Bewässerung aktivieren</button>
        <button class="btn btn-success" v-if="WateringInitialized" v-on:click="turnOffWatering">Bewässerung aussschalten</button><br>


    </form>
    </div>
</template>

<script>
// Stuff needed for http-requests
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios);
// Socket.io



export default {
    name: 'Watering',

    props: [
        'backendServer'
    ],
    data: function(){
        return{
            WateringParameters:{
                On: true
            },
            WateringInitialized: false
        }
    },
    created: function(){
        var _this = this;
        socket.on('watering-needed', function(Data){
            console.log('Watering: Watering needed!');
        });
    },
    methods: {
        initWatering(TurnOff){
            // If turnOff is true. The Pumps will not run
            console.log(this.sensors);
            if(TurnOff === true){
                this.WateringParameters.On = false;
            } else {
                this.WateringParameters.On = true;
                this.WateringInitialized = true;
            }

            var payload = {
                WateringParameters: this.WateringParameters
            };
            this.$http.post(this.backendServer + '/initialize/watering',payload).then(res=>{

                this.$emit('intialized-watering');
                console.log(res);
            }).catch(res => {
                console.log(res);
            });
        },
        turnOffWatering(){
            this.WateringInitialized = false;
            this.initWatering(true);
        }
    }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Watering.css" scoped>

</style>
