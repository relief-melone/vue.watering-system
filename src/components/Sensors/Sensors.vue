<template>
    <div class="container">
        <h1>Sensoren</h1>

    <form @submit.prevent="initSensors" class="form-sensors">
        <button class="btn" type="submit" v-bind:class="{'btn-primary': !SensorsInitialized, 'btn-success': SensorsInitialized}">Aktivieren</button>
        <div class="form-group sensor" v-for="(sensor, index) in sensors">
            <label>{{sensor.Name}}</label>
            <input v-model="sensor.On" type="checkbox" class="form-control" name="on" value="0">
            <transition enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
                <div v-if="sensor.On">
                    <label>Angeschlossene Pumpe</label>
                    <select class="form-control" v-model="sensor.AttachedPump">
                        <option value="0">1</option>
                        <option value="1">2</option>
                        <option value="2">3</option>
                        <option value="3">4</option>
                    </select>
                    <label>Minimale Feuchtigkeit</label>
                    <input class="form-control" v-model="sensor.MinimumMoisture" type="text">
                    <label>Aktuelle Feuchtigkeit: </label> <span>{{sensor.CurrentMoisture}}</span>

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
    name: 'Sensors',

    props: [
        'backendServer'
    ],
    data: function(){
        return{
            sensors: [
                {
                    "Name": "Sensor 1",
                    "AttachedPump": 0,
                    "MinimumMoisture" : 20,
                    "On": true,
                    "CurrentMoisture" : 0
                },
                {
                    "Name": "Sensor 2",
                    "AttachedPump": 0,
                    "MinimumMoisture" : 20,
                    "On": false,
                    "CurrentMoisture" : 0
                },
                {
                    "Name": "Sensor 3",
                    "AttachedPump": 0,
                    "MinimumMoisture" : 20,
                    "On": false
                },
                {
                    "Name": "Sensor 4",
                    "AttachedPump": 0,
                    "MinimumMoisture" : 20,
                    "On": false
                }
            ],
            SensorsInitialized: false
        }
    },
    created: function(){
        var _this = this;
        socket.on('sensor-data', function(Data){
            _this.sensors[Data.PowerPinIndex].CurrentMoisture = Math.round(Data.CurrentMoisture*100)/100;
        });
    },
    methods: {
        initSensors(){
            var payload = {
                Sensors : this.sensors
            }
            this.$http.post(this.backendServer + '/initialize/Sensors',payload).then(res=>{
                console.log('Sensors: Initialized');
                this.SensorsInitialized = true;
                this.$emit('initialized-sensors');
                console.log(res);
            }).catch(res => {
                console.log('Error initializing Sensors');
                console.log(res);
            });
        }
    }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Sensors.css" scoped>

</style>
