<template>
    <md-card class="info-collect-panel">
        <div class="info-wrapper">
            <md-steppers md-vertical>
                <md-step id="personal" md-label="个人信息">
                    <img id="avatar" class="avatar" v-show="personal.avatar" src=""
                         v-on:click="personal.avatar = null" alt="Avatar"/>
                    <md-button class="md-primary md-raised" @click="openFilePicker">
                        上传头像
                    </md-button>
                    <input id="filePicker" @change="pickedAvatar" type="file" hidden/>
                    <md-field>
                        <label>姓</label>
                        <md-input v-model="personal.lastName"></md-input>
                    </md-field>
                    <md-field>
                        <label>名</label>
                        <md-input v-model="personal.firstName"></md-input>
                    </md-field>
                    <md-field>
                        <label>Email</label>
                        <md-input v-model="personal.email"></md-input>
                    </md-field>
                    <md-field>
                        <label>手机</label>
                        <md-input v-model="personal.phone"></md-input>
                    </md-field>
                </md-step>
                <md-step id="education" md-label="教育背景">
                    <div v-for="(item, idx) in education" v-bind:key="idx">
                        <div class="subhead inline_wrapper">
                            <li class="">教育背景 {{idx+1}}</li>
                        </div>
                        <md-field>
                            <label>学校</label>
                            <md-input v-model="item.college"></md-input>
                        </md-field>
                        <md-field>
                            <label>专业</label>
                            <md-input v-model="item.major"></md-input>
                        </md-field>
                        <md-datepicker v-model="item.start_date">
                            <label>入学年月</label>
                        </md-datepicker>
                        <md-datepicker v-model="item.end_date">
                            <label>毕业年月</label>
                        </md-datepicker>
                        <md-field>
                            <label>GPA/上限</label>
                            <md-input v-model="item.gpa"></md-input>
                            <span>/</span>
                            <md-input v-model="item.gpa_limit">}</md-input>
                        </md-field>
                        <md-button class="md-accent" v-on:click="delEducation(idx)">
                            <md-icon>remove</md-icon>
                        </md-button>
                        <md-divider class="margin-bottom-10"/>
                    </div>
                    <md-button class="md-primary md-raised fill-width" v-on:click="addEducation">
                        <md-icon>add</md-icon>
                    </md-button>
                </md-step>
                <md-step id="working" md-label="工作经历">
                    <div v-for="(item, idx) in working" v-bind:key="idx">
                        <div class="subhead inline_wrapper">
                            <li class="">教育背景 {{idx+1}}</li>
                        </div>
                        <md-field>
                            <label>单位</label>
                            <md-input v-model="item.unit"></md-input>
                        </md-field>
                        <md-field>
                            <label>任职</label>
                            <md-input v-model="item.position"></md-input>
                        </md-field>
                        <md-datepicker v-model="item.start_date">
                            <label>开始年月</label>
                        </md-datepicker>
                        <md-datepicker v-model="item.end_date">
                            <label>结束年月</label>
                        </md-datepicker>
                        <md-field>
                            <label>主要工作</label>
                            <md-textarea v-model="item.major_work"></md-textarea>
                        </md-field>
                        <md-button class="md-accent" v-on:click="delWorking(idx)">
                            <md-icon>remove</md-icon>
                        </md-button>
                        <md-divider class="margin-bottom-10"/>
                    </div>
                    <md-button class="md-primary md-raised fill-width" v-on:click="addWorking">
                        <md-icon>add</md-icon>
                    </md-button>
                </md-step>
                <md-step id="skill" md-label="技能点">
                    <div v-for="(item, idx) in skills" v-bind:key="idx">
                        <md-field>
                            <label>技能</label>
                            <md-input v-model="item.name"></md-input>
                            <button style="background: none; border: none" v-on:click="delSkill(idx)">
                                <md-icon style="color: #ff5252">delete</md-icon>
                            </button>
                        </md-field>
                        <star-rating v-model="item.star" :star-size="30"></star-rating>
                        <md-divider class="margin-bottom-10"/>
                    </div>
                    <md-button class="md-primary md-raised fill-width" v-on:click="addSkill">
                        <md-icon>add</md-icon>
                    </md-button>
                </md-step>
            </md-steppers>
        </div>
        <md-divider/>
        <div class="bottom-bar">
            <md-button class="md-primary" v-on:click="clean">清空</md-button>
            <md-button class="md-accent" v-on:click="goHome">回到主页</md-button>
            <md-button class="md-primary" v-on:click="goResume">下一步</md-button>
        </div>
        <md-dialog-confirm
                :md-active.sync="cleaning"
                md-title="清空已录入的信息？"
                md-content="这将不可恢复。"
                md-cancel-text="不了不了"
                md-confirm-text="仍要清空"
                @md-cancel="onCleanCancel"
                @md-confirm="onCleanConfirm"/>
    </md-card>
</template>

<script>
    import Vue from 'vue'
    import 'vue-material/dist/vue-material.min.css'
    import VueMaterial from 'vue-material'
    import StarRating from "vue-star-rating"

    Vue.use(StarRating);
    Vue.use(VueMaterial);

    export default {
        name: "InfoCollectPanel",
        components: {StarRating},
        // props: [isMobile],
        data() {
            return {
                cleaning: false,
                personal: {
                    avatar: null,
                    phone: null,
                    firstName: null,
                    lastName: null,
                    email: null,
                },
                education: [],
                working: [],
                skills: [],
            }
        },
        methods: {
            goHome: function () {
                this.$emit('listenToGoHomeEvent')
            },
            goResume: function () {
                this.$emit('listenToGoResumeEvent')
            },
            clean: function () {
                this.cleaning = true;
            },
            onCleanCancel: function () {
                this.cleaning = false;
            },
            onCleanConfirm: function () {
                this.personal = {
                    avatar: null,
                    firstName: null,
                    lastName: null,
                    email: null,
                    phone: null,
                };
                this.education = [];
                this.working = [];
                this.skills = [];
                this.cleaning = false;
            },
            openFilePicker: function () {
                document.getElementById('filePicker').click();
            },
            pickedAvatar: function () {
                let avatar = document.getElementById('avatar');
                let reader = new FileReader();
                let avatar_file = document.getElementById('filePicker').files[0];
                reader.onload = function (e) {
                    avatar.setAttribute('src', e.target.result);
                };
                reader.readAsDataURL(avatar_file);
                this.personal.avatar = avatar_file;
            },
            addEducation: function () {
                this.education.push({
                    college: null,
                    major: null,
                    start_date: null,
                    end_date: null,
                    gpa: 0,
                    gpa_limit: 4.00,
                });
            },
            delEducation: function (key) {
                this.education.splice(key, 1);
            },
            addWorking: function () {
                this.working.push({
                    unit: null,
                    position: null,
                    start_date: null,
                    end_date: null,
                    major_work: null,
                });
            },
            delWorking: function (key) {
                this.working.splice(key, 1);
            },
            addSkill: function () {
                this.skills.push({
                    name: null,
                    star: 0,
                });
            },
            delSkill: function (key) {
                this.skills.splice(key, 1);
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
        overflow-y: hidden;
        z-index: -1;
        background: white;
    }

    .inline_wrapper {
        display: flex;
        align-items: baseline;
    }

    .subhead {
        text-align: center;
    }

    .fill-width {
        width: 100%;
        max-width: 300px;
    }

    .inline_1 {
        flex: 1;
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

    .margin-bottom-10 {
        margin-bottom: 10px;
    }

    .avatar {
        max-height: 200px;
        max-width: 200px;
        border-radius: 25px;
    }
</style>