<template>
    <div id="app">
        <transition name="slide">
            <div id="start-div"
                 v-show="stage === 0">
                <div class="dashed"></div>
                <md-button
                        class="md-fab md-primary md-alignment-center"
                        id="start-button"
                        v-on:click="goInfo">
                    <md-icon class="md-size-3x">edit</md-icon>
                </md-button>
            </div>
        </transition>
        <transition name="slide-fade">
            <info-collect-panel
                    id="info-collect-panel"
                    v-show="stage === 1"
                    v-on:listenToGoHomeEvent="goHome">
            </info-collect-panel>
        </transition>
    </div>

</template>

<script>
    import Vue from 'vue'
    import {MdButton, MdIcon} from 'vue-material/dist/components'
    import 'vue-material/dist/vue-material.min.css'
    import 'vue-material/dist/theme/default.css'
    import InfoCollectPanel from "./components/info/InfoCollectPanel";

    Vue.use(MdButton);
    Vue.use(MdIcon);

    export default {
        name: 'app',
        components: {InfoCollectPanel},
        data() {
            return {
                stage: -1,
                isMobile: window.innerWidth <= 700
            }
        },
        methods: {
            goResume() {
                this.stage = 2
            },
            goInfo() {
                this.stage = 1
            },
            goHome() {
                this.stage = 0
            },
        },
        mounted() {
            this.stage = 0;
        }
    }
</script>

<style>
    html {
        font-family: Microsoft YaHei UI, Helvetica Neue, Helvetica, PingFang SC, Hiragino Sans GB, Microsoft YaHei, Arial, sans-serif;
        /*align-content: center;*/
    }

    #start-div {
        position: fixed;
        height: 100%;
        right: 10%;
    }

    .dashed {
        position: relative;
        width: 1px;
        height: 50%;
        left: 50%;
        top: 0;
        border-left: 4px dashed;
        color: ghostwhite;
    }

    #start-button {
        height: 120px;
        width: 120px;
        position: relative;
        left: 50%;
        margin-left: 1px;
        transform: translate(-50%, -50%);

        color: white;
        background: #66ccff;
    }

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif, Monaco;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;

        position: fixed;
        padding: 0;
        margin: 0;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;

        background: gray url("assets/bg-homepage.jpg");
        /*url("https://source.unsplash.com/user/erondu/1600x900");*/
        background-size: cover;
    }

    .slide-fade-enter-active {
        transition: all 0.4s ease;
    }

    .slide-fade-leave-active {
        transition: all 0.4s ease;
    }

    .slide-fade-enter, .slide-fade-leave-to {
        transform: translateY(-100%);
        opacity: 0;
    }

    .slide-enter-active {
        transition: all 1s ease;
    }

    .slide-leave-active {
        transition: all 0.7s ease;
    }

    .slide-enter, .slide-leave-to {
        transform: translateY(-100%);
    }
</style>
