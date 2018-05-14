<template>
    <div>

        <Board id="Board" v-bind:backendServer="BackendServer" v-on:initialized-board="onBoardInit"/>
        <transition enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
            <Sensors v-if="this.BoardInitialized" id="Sensors" v-bind:backendServer="BackendServer" v-on:initialized-sensors="onSensorsInit"/>
        </transition>
        <transition enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
            <Pumps v-if="this.BoardInitialized" id="Sensors" v-bind:backendServer="BackendServer" v-on:initialized-pumps="onPumpsInit"/>
        </transition>
        <transition enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
            <Watering v-if="this.SensorsInitialized && this.PumpsInitialized" id="Watering" v-bind:backendServer="BackendServer"/>
        </transition>




    </div>
</template>

<script>
// @ is an alias to /src
import Board from '@/components/Board/Board.vue';
import Sensors from '@/components/Sensors/Sensors.vue';
import Pumps from '@/components/Pumps/Pumps.vue';
import Watering from '@/components/Watering/Watering.vue';

export default {
    name: 'home',
    components: {
        Board,
        Sensors,
        Pumps,
        Watering
    },
    data: ()=>{
        return {
            BackendServer: 'http://localhost:3000',
            BoardInitialized: false,
            SensorsInitialized: false,
            PumpsInitialized: false
        }
    },
    methods: {
        onBoardInit(){
            this.BoardInitialized = true;
            console.log('Board Initialized!!!!');

            console.log(this.BoardInitialized);
        },
        onSensorsInit(){
            console.log('Home: Sensors initialized!');
            this.SensorsInitialized = true;
        },
        onPumpsInit(){
            this.PumpsInitialized = true;
            console.log('Home: Pumps initialized!');
        }
    }

}
</script>

<style src="./Home.css">
</style>
