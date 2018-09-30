<template>
    <md-card class="info-collect-panel">
        <div class="info-wrapper">
            <md-steppers md-vertical>
                <md-step id="personal" md-label="个人信息">
                    <md-field>
                        <label>姓</label>
                        <md-input v-model="personal.lastName"></md-input>
                    </md-field>
                    <md-field>
                        <label>名</label>
                        <md-input v-model="personal.firstName"></md-input>
                    </md-field>
                </md-step>
                <md-step id="education" md-label="教育背景">
                    <div v-for="item in education" :key="item.college">
                        <md-field>
                            <label>学校</label>
                            <md-input v-model="item.college"></md-input>
                        </md-field>
                        <md-field>
                            <label>专业</label>
                            <md-input v-model="item.major"></md-input>
                        </md-field>
                        <md-field>
                            <label>GPA/上限</label>
                            <md-input v-model="item.gpa"></md-input>
                            <span>/</span>
                            <md-input v-model="item.gpa_limit"></md-input>
                        </md-field>
                        <md-button class="md-accent" v-on:click="delEducation(key)">
                            <md-icon>delete</md-icon>
                        </md-button>
                        <md-divider/>
                    </div>
                    <md-button class="md-primary md-raised" v-on:click="addEducation">
                        <md-icon>add</md-icon>
                    </md-button>
                </md-step>
                <md-step id="working" md-label="工作经历">

                </md-step>
            </md-steppers>
        </div>
        <md-divider/>
        <div class="bottom-bar">
            <md-button class="md-primary" v-on:click="goHome">
                <md-icon>home</md-icon>
            </md-button>
        </div>
    </md-card>
</template>

<script>
    import Vue from 'vue'
    import 'vue-material/dist/vue-material.min.css'
    import VueMaterial from 'vue-material'

    Vue.use(VueMaterial);

    export default {
        name: "InfoCollectPanel",
        // props: [isMobile],
        data() {
            return {
                personal: {
                    firstName: null,
                    lastName: null,
                },
                education: []
            }
        },
        methods: {
            goHome: function () {
                this.$emit('listenToGoHomeEvent')
            },
            addEducation: function () {
                this.education.push({
                    college: null,
                    major: null,
                    gpa: 0,
                    gpa_limit: 4.00
                })
            },
            delEducation: function (key) {
                // new VueMaterial.MdSnackbar().$emit(key);
                // console.log(key);
                this.education.splice(key, 1)
            }
        }
    }
</script>

<style scoped>
    .info-collect-panel {
        position: fixed;
        display: flex;
        flex-flow: column;
        height: 90%;
        width: 90%;
        top: 5%;
        left: 5%;
        box-shadow: 2px 4px 8px rgba(0, 0, 0, 0.3);
        overflow-y: scroll;
        z-index: -1;
        background: white;
    }

    .buttom-bar {
        flex: 0 1 auto;
    }

    .info-wrapper {
        flex: 1 1 auto;
        width: 100%;
        overflow-y: scroll;
    }

    .bottom-left {
        left: 0;
        bottom: 2px;
    }

    .bottom-center {
        position: absolute;
        left: 50%;
        bottom: 2px;
    }

    #info-wrapper {
        overflow-y: scroll;
    }
</style>