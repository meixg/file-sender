<template>
    <div id="app">
        <div>WebRTC file sender</div>
        <info :peerId="peerId"/>
    </div>
</template>

<script lang="ts">
import Vue from "vue";
import Info from "./components/Info.vue";
import Peer, {DataConnection} from "peerjs";

interface Data {
    peerId: string;
    targetId: string;
    peer: Peer;
    conn?: DataConnection
}

export default Vue.extend({
    name: "app",
    data() {
        return {
            peerId: '',
            peer: new Peer({key: 'lwjd5qra8257b9'}),
            conn: undefined
        } as Data;
    },
    components: {
        Info
    },
    mounted() {
        this.peer.on('open', id => {
            this.peerId = id;
        });
        this.peer.on('connection', conn => {
            this.conn = conn;
            this.setOpen();
        })
    },
    methods: {
        connect(id: string) {
            this.conn = this.peer.connect(id);
            this.setOpen();
        },
        setOpen() {
            if (!this.conn) {
                return;
            }

            this.conn.on('open', () => {
                this.conn && this.conn.on('data', data => {
                    this.handleData(data);
                });
            });
        },
        handleData(data: any) {
            
        }
    }
});
</script>

<style lang="less">
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
