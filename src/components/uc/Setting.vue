<template>
    <div>
        <div v-if="step==0" class="d-flex flex-column" style="min-height:100vh ;">
            <div class="col">
                <div class="d-flex justify-content-between align-items-center ws-filter-tab "
                    style="padding-top: 3px !important; padding-bottom:3px !important;">
                    <router-link to="/uc">
                        <b-icon icon="chevron-left" variant="light"></b-icon>
                    </router-link>
                    <div class="d-flex col py-2 justify-content-center align-items-center text-capitalize">
                        <span>{{$t('appmain.settings')}}</span>
                    </div>
                </div>
                <div @click="step=1" class="py-2 d-flex justify-content-between align-items-center ws-filter-tab "
                    style="border-bottom: 7px solid #0E151F; border-top: 7px solid #0E151F;">
                    <span class="fs-7 pb-1">{{$t('appmain.Language')}}</span>
                    <div class="d-flex align-items-center">
                        <span class="px-1 fs-7">
                            <b-icon icon="chevron-right" variant="light"></b-icon>
                        </span>
                    </div>
                </div>
                <router-link to="/announcement/0" class="py-2 d-flex justify-content-between align-items-center ws-filter-tab "
                    style="border-bottom: 2px solid #0E151F;">
                    <span class="fs-7 pb-1">{{$t('appmain.Announcement')}}</span>
                    <div class="d-flex align-items-center">
                        <span class="px-1 fs-7">
                            <b-icon icon="chevron-right" variant="light"></b-icon>
                        </span>
                    </div>
                </router-link>
                <router-link to="/help" class="py-2 d-flex justify-content-between align-items-center ws-filter-tab "
                    style="border-bottom: 2px solid #0E151F;">
                    <span class="fs-7 pb-1">{{$t('appmain.Helpcenter')}}</span>
                    <div class="d-flex align-items-center">
                        <span class="px-1 fs-7">
                            <b-icon icon="chevron-right" variant="light"></b-icon>
                        </span>
                    </div>
                </router-link>
            </div>
            <div class="py-5 px-3">
                <button class="gray-btn w-100 " @click="logout"> {{$t('appmain.Signout')}}</button>
            </div>
        </div>

        <!-- Language -->
        <div v-if="step==1" class="d-flex flex-column" style="min-height:100vh ;">
            <div class="col">
                <div class="d-flex justify-content-between align-items-center ws-filter-tab "
                    style="padding-top: 3px !important; padding-bottom:3px !important;">
                    <span @click="step=0">
                        <b-icon icon="chevron-left" variant="light"></b-icon>
                    </span>
                    <div class="d-flex col py-2 justify-content-center align-items-center">
                        <span>{{$t('appmain.Language')}}</span>
                    </div>
                </div>
                <div @click="changelanguage('en')" class="py-2 d-flex  align-items-center ws-filter-tab "
                    style="border-bottom: 3px solid #0E151F; border-top: 10px solid #0E151F;">
                    <span class="fs-7 pb-1">English</span>
                </div>
                <div @click="changelanguage('zh')" class="py-2 d-flex  align-items-center ws-filter-tab "
                    style="border-bottom: 3px solid #0E151F; border-top: 3px solid #0E151F;">
                    <span class="fs-7 pb-1">简体中文</span>
                </div>
                <div @click="changelanguage('jp')" class="py-2 d-flex  align-items-center ws-filter-tab "
                    style="border-bottom: 3px solid #0E151F; border-top: 3px solid #0E151F;">
                    <span class="fs-7 pb-1">日本語</span>
                </div>
                <div @click="changelanguage('tw')" class="py-2 d-flex  align-items-center ws-filter-tab "
                    style="border-bottom: 3px solid #0E151F; border-top: 3px solid #0E151F;">
                    <span class="fs-7 pb-1">繁體中文</span>
                </div>
            </div>
        </div>
        <!-- Language -->
    </div>
    </div>
</template>
<script>
    export default {
        components: {},
        data() {
            const validatePass = (rule, value, callback) => {
                if (value === "") {
                    callback(new Error(this.$t("uc.safe.newpwdmsg1")));
                } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    callback(new Error(this.$t("uc.safe.newpwdmsg1")));
                } else {
                    callback();
                }
            };
            const validatePassCheck = (rule, value, callback) => {
                if (value === "") {
                    callback(new Error(this.$t("uc.safe.newpwdmsg2")));
                } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    callback(new Error(this.$t("uc.safe.newpwdmsg2")));
                } else if (value !== this.formValidate4.newPw) {
                    callback(new Error(this.$t("uc.safe.newpwdmsg2")));
                } else {
                    callback();
                }
            };
            const validateMPass = (rule, value, callback) => {
                if (value === "") {
                    callback(new Error(this.$t("uc.safe.pwdmsg1")));
                } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    callback(new Error(this.$t("uc.safe.pwdmsg1")));
                } else {
                    callback();
                }
            };
            const validateMPassCheck = (rule, value, callback) => {
                if (value === "") {
                    callback(new Error(this.$t("uc.safe.pwdmsg2")));
                } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    callback(new Error(this.$t("uc.safe.pwdmsg2")));
                } else if (value !== this.formValidate5.newMPw) {
                    callback(new Error(this.$t("uc.safe.pwdmsg2")));
                } else {
                    callback();
                }
            };
            const validatepw7 = (rule, value, callback) => {
                if (value === "") {
                    callback(new Error(this.$t("uc.safe.pwdmsg1")));
                } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    callback(new Error(this.$t("uc.safe.pwdmsg1")));
                } else {
                    callback();
                }
            };
            const validatepw7check = (rule, value, callback) => {
                if (value === "") {
                    callback(new Error(this.$t("uc.safe.pwdmsg1")));
                } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    callback(new Error(this.$t("uc.safe.pwdmsg2")));
                } else if (value !== this.formValidate7.pw7) {
                    callback(new Error(this.$t("uc.safe.pwdmsg2")));
                } else {
                    callback();
                }
            };
            const validateMPass8 = (rule, value, callback) => {
                if (value === "") {
                    callback(new Error(this.$t("uc.safe.pwdmsg1")));
                } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    callback(new Error(this.$t("uc.safe.pwdmsg1")));
                } else {
                    callback();
                }
            };
            const validateMPassCheck8 = (rule, value, callback) => {
                if (value === "") {
                    callback(new Error(this.$t("uc.safe.pwdmsg2")));
                } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    callback(new Error(this.$t("uc.safe.pwdmsg2")));
                } else if (value !== this.formValidate8.newMPw8) {
                    callback(new Error(this.$t("uc.safe.pwdmsg2")));
                } else {
                    callback();
                }
            };
            return {
                isRouterAlive: true,
                step: 0,
                country: "中国",
                areas: [],
                fGetBackFundpwd: false,
                imgPreview: "",
                imgNext: "",
                imgLast: "",
                loginmsg: this.$t("common.logintip"),
                memberlevel: "",
                frontCardImg: require("../../assets/images/frontCardImg.png"),
                backCardImg: require("../../assets/images/backCardImg.png"),
                handCardImg: require("../../assets/images/HandCardImg.png"),

                uploadHeaders: { "x-auth-token": localStorage.getItem("TOKEN") },
                uploadUrl: this.host + "/uc/upload/oss/image",

                usernameS: "",
                user: {},
                choseItem: 0,
                accountValue: "1",
                formValidate2: {
                    mail: "",
                    vailCode1: "",
                    password: ""
                },
                formValidate3: {
                    mobile: "",
                    vailCode2: "",
                    password: ""
                },
                formValidate4: {
                    oldPw: "",
                    newPw: "",
                    newPwConfirm: "",
                    vailCode3: ""
                },
                formValidate5: {
                    oldPw: "",
                    newMPw: "",
                    newMPwConfirm: ""
                    // vailCode5: '',
                },
                formValidate6: {
                    realName: "",
                    idCard: ""
                },
                formValidate7: {
                    pw7: "",
                    pw7Confirm: ""
                },
                formValidate8: {
                    newMPw8: "",
                    newMPwConfirm8: "",
                    vailCode5: ""
                },
                ruleValidate: {
                    mail: [
                        {
                            required: true,
                            type: "email",
                            message: this.$t("uc.safe.emailtip"),
                            trigger: "blur"
                        }
                    ],
                    vailCode1: [
                        {
                            required: true,
                            message: this.$t("uc.safe.codetip"),
                            trigger: "blur"
                        }
                    ],
                    mobile: [
                        {
                            required: true,
                            message: this.$t("uc.safe.telnotip"),
                            trigger: "blur"
                        }
                    ],
                    vailCode2: [
                        {
                            required: true,
                            message: this.$t("uc.safe.codetip"),
                            trigger: "blur"
                        }
                    ],
                    vailCode3: [
                        {
                            required: true,
                            message: this.$t("uc.safe.codetip"),
                            trigger: "blur"
                        }
                    ],
                    password: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.pwdmsg1"),
                            trigger: "blur"
                        }
                    ],
                    oldPw: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.oldpwdtip"),
                            trigger: "blur"
                        }
                    ],
                    newPw: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.newpwdmsg1"),
                            trigger: "blur"
                        },
                        { validator: validatePass, trigger: "blur" }
                    ],
                    newPwConfirm: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.newpwdmsg2"),
                            trigger: "blur"
                        },
                        { validator: validatePassCheck, trigger: "blur" }
                    ],
                    newMPw: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.pwdmsg1"),
                            trigger: "blur"
                        },
                        { validator: validateMPass, trigger: "blur" }
                    ],
                    newMPwConfirm: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.pwdmsg2"),
                            trigger: "blur"
                        },
                        { validator: validateMPassCheck, trigger: "blur" }
                    ],
                    pw7: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.pwdmsg1"),
                            trigger: "blur"
                        },
                        { validator: validatepw7, trigger: "blur" }
                    ],
                    pw7Confirm: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.pwdmsg2"),
                            trigger: "blur"
                        },
                        { validator: validatepw7check, trigger: "blur" }
                    ],
                    vailCode5: [
                        {
                            required: true,
                            message: this.$t("uc.safe.codetip"),
                            trigger: "blur"
                        }
                    ],
                    realName: [
                        {
                            required: true,
                            message: this.$t("uc.safe.realnametip"),
                            trigger: "blur"
                        }
                    ],
                    idCard: [
                        {
                            required: true,
                            message: this.$t("uc.safe.idcardtip"),
                            trigger: "blur"
                        }
                    ],
                    newMPw8: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.pwdmsg1"),
                            trigger: "blur"
                        },
                        { validator: validateMPass8, trigger: "blur" }
                    ],
                    newMPwConfirm8: [
                        {
                            required: true,
                            type: "string",
                            min: 6,
                            message: this.$t("uc.safe.pwdmsg2"),
                            trigger: "blur"
                        },
                        { validator: validateMPassCheck8, trigger: "blur" }
                    ]
                },
                time1: 60, // 发送验证码倒计时
                time2: 60, // 发送验证码倒计时
                time3: 60, // 发送验证码倒计时
                time5: 60, // 发送验证码倒计时
                sendMsgDisabled1: false,
                sendMsgDisabled2: false,
                sendMsgDisabled3: false,
                sendMsgDisabled5: false
            };
        },
        methods: {
            reload() {
                this.isRouterAlive = false;
                this.$nextTick(function () {
                    this.isRouterAlive = true;
                })
            },
            changelanguage: function (name) {
                console.log("change language: " + name);
                if (name == "en") {
                    this.$store.commit("setlang", "English");
                    this.$i18n.locale = "en";
                    this.reload()
                }
                if (name == "zh") {
                    this.$store.commit("setlang", "简体中文");
                    this.$i18n.locale = "zh";
                    this.reload()
                }
                if (name == "jp") {
                    this.$store.commit("setlang", "日本語");
                    this.$i18n.locale = "jp";
                    this.reload()
                }
                if (name == "tw") {
                    this.$store.commit("setlang", "繁體中文");
                    this.$i18n.locale = "tw";
                    this.reload()
                }
            },
            logout() {
                this.$http.post(this.host + "/uc/loginout", {}).then(response => {
                    var resp = response.body;
                    if (resp.code == 0) {
                        this.$Message.success(resp.message);
                        this.$store.commit("setMember", null);
                        setTimeout(() => {
                            location.href = "/";
                        }, 1500);
                    } else {
                        this.$Message.error(resp.message);
                    }
                });
            },
            beforeUpload(data) {
                if (data && data.size >= 1024000 * 2) {
                    this.$Message.error("上传图片大小不能超过2M");
                    return false;
                }
            },
            frontHandleSuccess(res, file, fileList) {
                this.$refs.upload1.fileList = [fileList[fileList.length - 1]];
                if (res.code == 0) {
                    this.frontCardImg = this.imgPreview = res.data;
                } else {
                    this.$Message.error(res.message);
                }
            },
            backHandleSuccess(res, file, fileList) {
                this.$refs.upload2.fileList = [fileList[fileList.length - 1]];
                if (res.code == 0) {
                    this.backCardImg = this.imgNext = res.data;
                } else {
                    this.$Message.error(res.message);
                }
            },
            handHandleSuccess(res, file, fileList) {
                this.$refs.upload3.fileList = [fileList[fileList.length - 1]];
                if (res.code == 0) {
                    this.handCardImg = this.imgLast = res.data;
                } else {
                    this.$Message.error(res.message);
                }
            },
            noPhone() {
                this.$Message.info(this.$t("uc.safe.bindphonetip"));
                this.showItem(3);
            },
            showItemFundpwd() {
                this.fGetBackFundpwd = false;
                this.handleReset("formValidate5");
                this.showItem(5);
            },
            renderPw() {
                this.$Modal.confirm({
                    title: this.$t("uc.safe.resetfundpwd"),
                    onOk: () => {
                        this.$Message.info("Clicked ok");
                    },
                    render: h => {
                        return h("Input", {
                            props: {
                                value: this.value,
                                autofocus: true
                            },
                            on: {
                                input: val => {
                                    this.value = val;
                                }
                            }
                        });
                    }
                });
            },
            submit(name) {
                //实名认证
                if (name == "formValidate6") {
                    if (this.imgPreview == "") {
                        this.$Message.error(this.$t("uc.safe.upload_positivetip"));
                        return false;
                    }
                    if (this.imgNext == "") {
                        this.$Message.error(this.$t("uc.safe.upload_negativetip"));
                        return false;
                    }
                    if (this.imgLast == "") {
                        this.$Message.error(this.$t("uc.safe.upload_handtip"));
                        return false;
                    }
                    let param = {};
                    param["realName"] = this.formValidate6.realName;
                    param["idCard"] = this.formValidate6.idCard;
                    param["idCardFront"] = this.imgPreview;
                    param["idCardBack"] = this.imgNext;
                    param["handHeldIdCard"] = this.imgLast;
                    this.$http
                        .post(this.host + "/uc/approve/real/name", param)
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.member.realName = this.formValidate6.realName;
                                this.$store.commit("setMember", this.member);
                                this.$Message.success(this.$t("uc.safe.save_success"));
                                this.getMember();
                                this.choseItem = 0;
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                }
                //邮箱认证
                if (name == "formValidate2") {
                    let param = {};
                    param["email"] = this.formValidate2.mail;
                    param["code"] = this.formValidate2.vailCode1;
                    param["password"] = this.formValidate2.password;
                    this.$http
                        .post(this.host + "/uc/approve/bind/email", param)
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t("uc.safe.save_success"));
                                this.getMember();
                                this.choseItem = 0;
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                }
                //手机认证
                if (name == "formValidate3") {
                    let param = {};
                    param["phone"] = this.formValidate3.mobile;
                    param["code"] = this.formValidate3.vailCode2;
                    param["password"] = this.formValidate3.password;
                    param["country"] = this.country;
                    this.$http
                        .post(this.host + "/uc/approve/bind/phone", param)
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t("uc.safe.save_success"));
                                this.getMember();
                                this.choseItem = 0;
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                }
                //登录密码
                if (name == "formValidate4") {
                    let param = {};
                    param["oldPassword"] = this.formValidate4.oldPw;
                    param["newPassword"] = this.formValidate4.newPw;
                    param["code"] = this.formValidate4.vailCode3;
                    param["verify"] = 0;
                    this.$http
                        .post(this.host + "/uc/approve/update/password", param)
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t("uc.safe.save_success"));
                                this.getMember();
                                this.choseItem = 0;
                                localStorage.removeItem("MEMBER");
                                localStorage.removeItem("TOKEN");
                                this.$store.state.showLogout = true;
                                this.$store.state.showLogin = false;
                                this.$store.commit("setMember", null);
                                let self = this;
                                setTimeout(() => {
                                    self.$router.push("/login");
                                }, 1000);
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                }
                //修改资金密码
                if (name == "formValidate5") {
                    let param = {};
                    param["oldPassword"] = this.formValidate5.oldPw;
                    param["newPassword"] = this.formValidate5.newMPw;
                    // param['code'] = this.formValidate5.vailCode5
                    this.$http
                        .post(this.host + "/uc/approve/update/transaction/password", param)
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t("uc.safe.save_success"));
                                this.handleReset("formValidate5");
                                this.getMember();
                                this.choseItem = 0;
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                }
                //设置资金密码
                if (name == "formValidate7") {
                    let param = {};
                    param["jyPassword"] = this.formValidate7.pw7;
                    this.$http
                        .post(this.host + "/uc/approve/transaction/password", param)
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t("uc.safe.save_success"));
                                this.getMember();
                                this.choseItem = 0;
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                }
                //找回资金密码
                if (name == "formValidate8") {
                    let param = {};
                    param["newPassword"] = this.formValidate8.newMPw8;
                    param["code"] = this.formValidate8.vailCode5;
                    this.$http
                        .post(this.host + "/uc/approve/reset/transaction/password", param)
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t("uc.safe.save_success"));
                                this.fGetBackFundpwd = false;
                                this.handleReset("formValidate5");
                                this.getMember();
                                this.choseItem = 0;
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                }
            },
            handleSubmit(name) {
                this.$refs[name].validate(valid => {
                    if (valid) {
                        this.submit(name);
                    } else {
                        this.$Message.error(this.$t("uc.safe.save_failure"));
                    }
                });
            },
            handleReset(name) {
                this.$refs[name].resetFields();
            },
            showItem(index) {
                this.choseItem = index;
            },
            send(index) {
                let me = this;
                if (index == 1) {
                    if (this.formValidate2.mail) {
                        //获取邮箱code
                        this.$http
                            .post(this.host + "/uc/bind/email/code", {
                                email: this.formValidate2.mail
                            })
                            .then(response => {
                                var resp = response.body;
                                if (resp.code == 0) {
                                    me.sendMsgDisabled1 = true;
                                    let interval = window.setInterval(function () {
                                        if (me.time1-- <= 0) {
                                            me.time1 = 60;
                                            me.sendMsgDisabled1 = false;
                                            window.clearInterval(interval);
                                        }
                                    }, 1000);
                                } else {
                                    this.$Message.error(resp.message);
                                }
                            });
                    } else {
                        this.$refs.formValidate2.validateField("mail");
                    }
                } else if (index == 2) {
                    if (this.formValidate3.mobile) {
                        //获取手机code
                        this.$http
                            .post(this.host + "/uc/mobile/bind/code", {
                                phone: this.formValidate3.mobile,
                                country: this.country
                            })
                            .then(response => {
                                var resp = response.body;
                                if (resp.code == 0) {
                                    me.sendMsgDisabled2 = true;
                                    let interval = window.setInterval(function () {
                                        if (me.time2-- <= 0) {
                                            me.time2 = 60;
                                            me.sendMsgDisabled2 = false;
                                            window.clearInterval(interval);
                                        }
                                    }, 1000);
                                } else {
                                    this.$Message.error(resp.message);
                                }
                            });
                    } else {
                        this.$refs.formValidate3.validateField("mobile");
                    }
                } else if (index == 3) {
                    //登录密码获取手机code
                    this.$http
                        .post(this.host + "/uc/email/update/password/code")
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                me.sendMsgDisabled3 = true;
                                let interval = window.setInterval(function () {
                                    if (me.time3-- <= 0) {
                                        me.time3 = 60;
                                        me.sendMsgDisabled3 = false;
                                        window.clearInterval(interval);
                                    }
                                }, 1000);
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                } else if (index == 5) {
                    //资金密码获取手机code
                    this.$http
                        .post(this.host + "/uc/email/transaction/code")
                        .then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                me.sendMsgDisabled5 = true;
                                let interval = window.setInterval(function () {
                                    if (me.time5-- <= 0) {
                                        me.time5 = 60;
                                        me.sendMsgDisabled5 = false;
                                        window.clearInterval(interval);
                                    }
                                }, 1000);
                            } else {
                                this.$Message.error(resp.message);
                            }
                        });
                }
            },
            getMember() {
                //获取个人安全信息
                var self = this;
                this.$http
                    .post(this.host + "/uc/approve/security/setting")
                    .then(response => {
                        var resp = response.body;
                        if (resp.code == 0) {
                            this.user = resp.data;
                            this.usernameS = this.user.username.slice(0, 1);
                        } else {
                            this.$Message.error(this.loginmsg);
                            // this.$Message.error(this.$t('common.logintip'));
                        }
                    });
            },
            getAreas() {
                var params = {
                    isReg: 1
                };
                this.$http.post(this.host + this.api.common.area, params).then(response => {
                    var resp = response.body;
                    this.areas = resp.data;
                    if (resp.data.length > 0) {
                        this.country = this.areas[0].zhName;
                    }
                });
            }
        },
        computed: {
            member: function () {
                return this.$store.getters.member;
            },
            lang() {
                return this.$store.state.lang;
            }
        },
        created() {
            this.getMember();
            let level = this.$store.getters.member.memberRate;
            level == 0 && (this.memberlevel = "普通会员");
            level == 1 && (this.memberlevel = "超级群主");
            level == 2 && (this.memberlevel = "超级合伙人");
            this.getAreas();
        }
    };
</script>
<style scoped lang="scss">
    button.ivu-btn {
        &:focus {
            box-shadow: 0 0 0 2px rgba(45, 140, 240, 0);
        }
    }

    button.ivu-btn.ivu-btn-primary {
        box-shadow: 0 0 0 2px rgba(45, 140, 240, 0);
    }

    .nav-right {
        padding-left: 15px;

        .user .user-top-icon {
            height: 80px;
            border-bottom: 1px dashed #27313e;
            position: relative;
            display: flex;
            justify-content: flex-start;
            align-items: center;
            padding: 0 50px;
        }
    }

    .uploadimgtip {
        position: relative;
        top: -20px;
        color: #f0a70a;
    }

    .account-box .account-in .account-item .account-detail {
        padding: 30px 0;
        // background: white;
        margin: 6px 0;
    }

    .account-box .account-in .account-item .account-detail .detail-list {
        width: 40%;
        margin: 0 auto;
    }

    .account-box .account-in .account-item .account-detail .detail-list .input-control {
        margin-bottom: 10px;
        height: 45px;
    }

    .detail-list .input-control .ivu-input-group-prepend {
        width: 63px;
    }

    .detail-list .input-control .ivu-input {
        height: 45px;
    }

    .account-box .account-in .account-item {
        margin-bottom: 10px;
    }

    .account-box .account-in .account-item .account-item-in {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        padding: 15px 30px 15px 50px;
        // background-color: white;
        -webkit-box-shadow: 0 1px 0 0 rgba(69, 112, 128, 0.06);
        box-shadow: 0 1px 0 0 rgba(69, 112, 128, 0.06);
        font-size: 14px;
        color: #fff;
    }

    .account-box .account-in .account-item .account-item-in .icons {
        height: 17px;
        width: 17px;
        display: inline-block;
        margin-top: -1px;
        background-size: 100% 100%;
    }

    .account-box .account-in .account-item .account-item-in .yesImg {
        background-image: url(../../assets/img/overicon.png);
    }

    .icons.noImg {
        background-image: url(../../assets/img/noicon.png);
    }

    .account-box .account-in .account-item .account-item-in .card-number {
        width: 142px;
        height: 40px;
        margin-right: 15px;
        border-right: 1px dashed #27313e;
        padding: 0 15px;
        line-height: 40px;
        text-align: left;
        display: inline-block;
    }

    .account-box .account-in .account-item .account-item-in .bankInfo {
        width: 70%;
        text-align: left;
    }

    .account-box .account-in .account-item .account-item-in .btn {
        padding: 8px 10px;
        cursor: pointer;
    }

    .tips-g {
        color: #8994a3;
        font-size: 12px;
    }

    .gr {
        color: #b4b4b4;
    }

    .m1 {
        display: inline-block;
        width: 25px;
        height: 25px;
        background: url(../../assets/img/m1.png);
        background-size: 100% 100%;
        vertical-align: middle;
        margin-right: 5px;
    }

    .m2 {
        display: inline-block;
        width: 25px;
        height: 25px;
        background: url(../../assets/img/m2.png);
        background-size: 100% 100%;
        vertical-align: middle;
        margin-right: 5px;
    }

    .m3 {
        display: inline-block;
        width: 25px;
        height: 25px;
        background: url(../../assets/img/m3.png);
        background-size: 100% 100%;
        vertical-align: middle;
        margin-right: 5px;
    }

    .itp {
        display: inline-block;
    }

    .user-right {
        width: 80%;
    }

    .rightarea {
        padding-left: 15px !important;
        padding-right: 15px !important;
        margin-bottom: 60px !important;
    }

    .rightarea .rightarea-top {
        line-height: 75px;
        border-bottom: #f1f1f1 solid 1px;
    }

    .rightarea .trade-process {
        line-height: 30px;
        padding: 0 15px;
        background: #f1f1f1;
        display: inline-block;
        position: relative;
        margin-right: 20px;
    }

    .rightarea .trade-process.active {
        color: #eb6f6c;
        background: #f9f5eb;
    }

    .rightarea .trade-process .icon {
        background: #fff;
        border-radius: 20px;
        height: 20px;
        width: 20px;
        display: inline-block;
        line-height: 20px;
        text-align: center;
        margin-right: 10px;
    }

    .rightarea .trade-process .arrow {
        position: absolute;
        top: 10px;
        right: -5px;
        width: 0;
        height: 0;
        border-top: 5px solid transparent;
        border-bottom: 5px solid transparent;
        border-left: 5px solid #f1f1f1;
    }

    .rightarea .trade-process.active .arrow {
        border-left: 5px solid #f9f5eb;
    }

    .rightarea .rightarea-tabs {
        border: none;
    }

    .rightarea .rightarea-tabs li>a {
        width: 100%;
        height: 100%;
        padding: 0;
        margin-right: 0;
        font-size: 14px;
        color: #646464;
        border-radius: 0;
        border: none;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .rightarea .rightarea-tabs li>a:hover {
        background-color: #fcfbfb;
    }

    .rightarea .rightarea-tabs li {
        width: 125px;
        height: 40px;
        position: relative;
        margin: -1px 0 0 -1px;
        border: 1px solid #f1f1f1;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
    }

    .rightarea .rightarea-tabs li.active {
        background-color: #fcfbfb;
    }

    .rightarea .rightarea-tabs li:last-child {
        border-right: 1px solid #f1f1f1;
    }

    .rightarea .rightarea-tabs li.active>a,
    .rightarea .rightarea-tabs li:hover>a {
        color: #da2e22;
        border: none;
    }

    .rightarea .panel-tips {
        border: 3px solid #fdfaf3;
        color: #9e9e9e;
        font-size: 12px;
    }

    .rightarea .panel-tips .panel-header {
        background: #fdfaf3;
        line-height: 40px;
        margin-bottom: 15px;
    }

    .rightarea .panel-tips .panel-title {
        font-size: 16px;
    }

    .rightarea .recordtitle {
        cursor: pointer;
    }

    .user .top-icon {
        /* background: url("../../images/user/userplist.png") no-repeat 0 0; */
        width: 75px;
        height: 75px;
        display: inline-block;
    }

    .user .top-icon.intro {
        background-position: 0 -670px;
    }

    .user .user-info {
        padding: 0px 10px;
        background-color: #fff;
        color: #fff;
    }

    .user .user-info .user-info-top {
        border-bottom: 1px dashed #27313e;
        padding-bottom: 20px;
    }

    .user .user-info h5 {
        font-size: 18px;
    }

    .user .user-info h5 .iconfont {
        font-size: 20px;
        color: #e24a64;
        margin-left: 10px;
    }

    .user-avatar {
        display: flex;
        justify-content: space-between;
    }

    .user-icons {
        display: flex;
        align-self: center;
        width: 300px;
    }

    .user-icons .icons-in {
        height: 45px;
        width: 45px;
        border-radius: 50%;
        background-color: #00b5f6;
        display: flex;
        justify-content: center;
        align-items: center;
        align-self: center;
    }

    .user-icons .icons-in em {
        color: white;
        font-size: 20px;
        font-style: normal;
    }

    .user-icons .user-name {
        margin-left: 10px;
        display: flex;
        justify-content: flex-start;
        /* align-items: center; */
        flex-direction: column;
    }

    .user-icons .user-name span {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        width: 225px;
        height: 52px;
        overflow: hidden;
        font-size: 14px;
        white-space: nowrap;
        text-overflow: ellipsis;
        -o-text-overflow: ellipsis;
    }

    .user-top-icon .trade-info {
        width: 420px;
        padding-left: 30px;
        display: flex;
    }

    .user-top-icon .trade-info .item {
        display: flex;
        flex-direction: column;
        width: 100%;
    }

    .user-top-icon .trade-info .item.capital {
        width: 60%;
    }

    .user-icons .user-name span.uid {
        color: #8994a3;
        font-size: 12px;
    }

    .circle-info {
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        height: 80px;
        width: 80px;
        border-radius: 50%;
        border: 2px solid #ebeff5;
        margin-left: 14px;
    }

    .circle-info span {
        font-weight: bolder;
    }

    .circle-info p {
        color: #8994a3;
        margin: 0;
        padding: 0;
    }

    .circle-info p.count {
        color: #fff;
        font-size: 14px;
        font-weight: 600;
    }

    .user-avatar-public {
        background: #df9a00;
        border-radius: 50%;
        height: 52px;
        width: 52px;
        display: flex;
        justify-content: center;
        align-items: center;
        box-shadow: 0 1px 5px 0 rgba(71, 78, 114, 0.24);
        position: relative;
    }

    .user-avatar-public>.user-avatar-in {
        background: #f0a70a;
        border-radius: 50%;
        height: 42px;
        width: 42px;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
    }

    /*新加样式*/
    .router-link-active {
        color: red;
    }

    /* router-link-exact-active router-link-active */
    .account-item-in i {
        color: #f0a70a !important;
    }

    .btn {
        color: #f0a70a;
    }

    .ivu-btn-primary {
        background-color: #f0a70a;
        border-color: #f0a70a;
    }
</style>
<style lang="scss">
    li.ivu-upload-list-file.ivu-upload-list-file-finish {
        &:hover {
            span {
                color: #f0a70a;
            }
        }
    }

    .idcard-title {
        font-size: 13px;
        margin-bottom: 10px;
    }

    .acc_sc {
        margin-top: 10px;
    }

    .idcard-desc {
        padding: 10px 150px 50px 150px;

        >p {
            font-size: 13px;
            margin-bottom: 10px;
            text-align: left;
            color: #828ea1;
        }
    }

    @media screen and (max-width:768px) {
        .safe .nav-right .user .user-top-icon {
            padding: 0 0 !important;
        }

        .safe .account-box .account-in .account-item .account-item-in {
            padding: 15px 0px 15px 0px;
        }

        .safe .account-box .account-in .account-item .account-item-in .bankInfo {
            width: 50% !important;
        }

        .safe .account-box .account-in .account-item .account-item-in .card-number {
            width: 100px !important;
        }

        .safe .user-icons .user-name span {
            width: 100px !important;
        }
    }
</style>