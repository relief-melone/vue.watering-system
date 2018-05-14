<template>
    <div class="container">
        <h1>Pumpen</h1>

    <form @submit.prevent="initPumps" class="form-pumps">
        <button class="btn" type="submit" v-bind:class="{'btn-primary': !pumpsInitialized, 'btn-success': pumpsInitialized}">Aktivieren</button>
        <div class="form-group pump" v-for="(pump, index) in pumps">
            <label>{{pump.Name}}</label>
            <input v-model="pump.On" type="checkbox" class="form-control" name="on" value="0">
            <transition enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
                <div v-if="pump.On">
                    <label>Förderleistung: [l/h]</label>
                    <input class="form-control" v-model="pump.LitersPerHour" type="text">
                    <label>Wassermenge [ml]</label>
                    <input class="form-control" v-model="pump.MlPerWatering" type="text">
                    <label>Minimales Intervall [min]</label>
                    <input class="form-control" v-model="pump.MinimumIntervalInMinutes" type="text">
                </div>
            </transition>
        </div>
        <br>

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
    name: 'Pumps',

    props: [
        'backendServer'
    ],
    data: function(){
        return{
            pumps: [
                {
                    "Name": "Pumpe 1",
                    "LitersPerHour":110,
                    "MinimumIntervalInMinutes" : 60,
                    "MlPerWatering" : 200,
                    "On": true
                },
                {
                    "Name": "Pumpe 2",
                    "LitersPerHour":110,
                    "MinimumIntervalInMinutes" : 60,
                    "MlPerWatering" : 200,
                    "On": false
                },
                {
                    "Name": "Pumpe 3",
                    "LitersPerHour":110,
                    "MinimumIntervalInMinutes" : 60,
                    "MlPerWatering" : 200,
                    "On": false
                },
                {
                    "Name": "Pumpe 4",
                    "LitersPerHour":110,
                    "MinimumIntervalInMinutes" : 60,
                    "MlPerWatering" : 200,
                    "On": false
                }
            ],
            pumpsInitialized: false
        }
    },
    created: function(){

    },
    methods: {
        initPumps(){
            console.log(this.pumps);
            var payload = {
                Pumps : this.pumps
            }
            this.$http.post(this.backendServer + '/initialize/Pumps',payload).then(res=>{
                console.log('Pumps: Initialized!')
                this.$emit('initialized-pumps');
                this.pumpsInitialized = true;
                console.log(res);
            }).catch(res => {
                console.log(res);
            });
        }
    }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Pumps.css" scoped>

</style>
