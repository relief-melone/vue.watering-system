<template>

    <form @submit.prevent="initBoard">
        <div class="form-group form-inline">
            <select name="host_ip" class="form-control" v-model="hostOrIpSelect" v-on:change="hostIpPlaceholder(e)">
                <option value="host">Host</option>
                <option value="ip">IP</option>
            </select>

            <input class="form-control" v-model="hostOrIp" type="text" placeholder="Hostname of your board" name="host" value="ESP_AC6CB1" />
            <label>:</label>
            <input class="form-control" v-model="port" type="text" name="port" value="8181" placeholder="Enter Port">
            <button type="submit" class="btn" v-bind:class="{'btn-primary': !boardInitialized, 'btn-success': boardInitialized}">Verbinden</button><br>

        </div>
    </form>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios);

export default {
    name: 'Board',
    props: [
        'backendServer'
    ],
    data: function(){
        return{
            hostOrIpSelect:'',
            hostOrIp: '',
            port: '8080',
            boardResponse: '',
            boardInitialized: false
        }
    },
    created: function(){

    },
    methods: {
        hostIpPlaceholder: function(){
            if(this.hostOrIpSelect === 'ip'){
                this.hostOrIp = '192.168.2.101';
            } else {
                this.hostOrIp = 'ESP_AC6CB1';
            }
        },
        initBoard: function(){
            var postBody = this.hostOrIpSelect === 'ip' ? {
                Ip : this.hostOrIp,
                Port: parseInt(this.port)
            } : {
                Hostname : this.hostOrIp,
                Port: parseInt(this.port)
            }
            console.log(postBody);
            this.$http.post(this.backendServer + '/Initialize/Board', postBody).then(res => {
                this.boardResponse = res.data.message;
                this.boardInitialized = true;
                this.$emit('initialized-board');
            }, res =>{
                this.boardResponse = res.data.message;
            });
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Board.css" scoped>

</style>
