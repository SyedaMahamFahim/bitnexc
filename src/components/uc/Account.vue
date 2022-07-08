<template>
    <div>
        <div class="nav-rights uc_account" v-if="0">
            <div class="nav-right col-xs-12 col-md-10 padding-right-clear">
                <div class="bill_box rightarea padding-right-clear record account-box">
                    <section class="trade-group merchant-top">
                        <i class="merchant-icon tips"></i>
                        <span class="tips-word">{{$t('uc.account.pagetitle')}}</span>
                        <span class="tips-g">{{$t('uc.account.pagetip')}}</span>
                    </section>
                    <section class="accountContent">
                        <div class="account-in">
                            <div class="account-item">
                                <div class="account-item-in">
                                    <i class="icons bankfor"></i>
                                    <span class="card-number">{{$t('uc.account.backcardno')}}</span>
                                    <p v-if="user.bankVerified==1" class="bankInfo" style="color: #fff;">
                                        {{user.bankInfo.cardNo}}
                                    </p>
                                    <p v-else class="bankInfo">
                                        {{$t('uc.account.backcardtip')}}
                                    </p>
                                    <a class="btn" v-if="user.bankVerified==1"
                                        @click="showItem(1)">{{$t('uc.account.modify')}}</a>
                                    <a class="btn" v-else @click="showItem(1)">{{$t('uc.account.bind')}}</a>
                                </div>
                                <div class="account-detail" v-show="choseItem==1">
                                    <div class="detail-list">
                                        <Form ref="formValidate1" :model="formValidate1" :rules="ruleValidate"
                                            :label-width="85">
                                            <!-- name -->
                                            <FormItem :label="$t('uc.account.name')" prop="name">
                                                <Input disabled size="large" v-model="formValidate1.name"></Input>
                                            </FormItem>
                                            <!-- bankName -->
                                            <FormItem :label="$t('uc.account.bankaccount')" prop="bankName">
                                                <!--                                            <Select v-model="formValidate1.bankName" size="large">-->
                                                <!--                                                <Option v-for="item in bankNameList" :value="item.value" :key="item.value">{{ item.label }}</Option>-->
                                                <!--                                            </Select>-->
                                                <Input size="large" v-model="formValidate1.bankName"></Input>
                                            </FormItem>
                                            <!-- bankBranch -->
                                            <FormItem :label="$t('uc.account.bankbranch')" prop="bankBranch">
                                                <Input v-model="formValidate1.bankBranch" size="large"></Input>
                                            </FormItem>
                                            <!-- bankNo -->
                                            <FormItem :label="$t('uc.account.bankno')" prop="bankNo">
                                                <Input v-model="formValidate1.bankNo" size="large" type="text"></Input>
                                            </FormItem>
                                            <!-- bankNoConfirm -->
                                            <FormItem :label="$t('uc.account.confirmbankno')" prop="bankNoConfirm">
                                                <Input v-model="formValidate1.bankNoConfirm" size="large"
                                                    type="text"></Input>
                                            </FormItem>
                                            <!-- passwd -->
                                            <FormItem :label="$t('uc.account.fundpwd')" prop="password">
                                                <Input v-model="formValidate1.password" type="password"
                                                    size="large"></Input>
                                            </FormItem>
                                            <!-- Button -->
                                            <FormItem>
                                                <Button type="primary"
                                                    @click="handleSubmit('formValidate1')">{{$t('uc.account.save')}}</Button>
                                                <!-- <Button type="ghost" @click="handleReset('formValidate1')" style="margin-left: 8px">Reset</Button> -->
                                            </FormItem>
                                        </Form>
                                    </div>
                                </div>
                            </div>
                            <div class="account-item">
                                <div class="account-item-in">
                                    <i class="icons alipay"></i>
                                    <span class="card-number">{{$t('uc.account.zfbaccount')}}</span>
                                    <p v-if="user.aliVerified==1" class="bankInfo" style="color: #fff;">
                                        {{user.alipay.aliNo}}
                                    </p>
                                    <p v-else class="bankInfo">
                                        {{$t('uc.account.zfbaccounttip')}}
                                    </p>
                                    <a class="btn" v-if="user.aliVerified==1"
                                        @click="showItem(2)">{{$t('uc.account.modify')}}</a>
                                    <a class="btn" v-else @click="showItem(2)">{{$t('uc.account.bind')}}</a>
                                </div>
                                <div class="account-detail" v-show="choseItem==2">
                                    <div class="detail-list">
                                        <Form ref="formValidate2" :model="formValidate2" :rules="ruleValidate"
                                            :label-width="95">
                                            <Row>
                                                <Col span="8">
                                                <input type="hidden" name="aliPreview" :value="aliPreview" />
                                                <img v-if="aliImg" :alt="$t('uc.account.imgtip')"
                                                    style="width: 200px;height: 200px;" :src="aliImg">
                                                <img v-else :alt="$t('uc.account.imgtip')"
                                                    style="width: 200px;height: 200px;"
                                                    src="../../assets/images/upload_placeholder.png">
                                                <div class="acc_sc">
                                                    <Upload ref="upload1" :on-success="aliHandleSuccess"
                                                        :headers="uploadHeaders" :action="uploadUrl">
                                                        <Button
                                                            icon="ios-cloud-upload-outline">{{$t('uc.safe.upload')}}</Button>
                                                    </Upload>
                                                </div>
                                                </Col>

                                                <Col span="16">
                                                <!-- name -->
                                                <FormItem :label="$t('uc.account.name')" prop="name">
                                                    <Input disabled size="large" v-model="formValidate2.name"></Input>
                                                </FormItem>
                                                <!-- alipay -->
                                                <FormItem :label="$t('uc.account.zfbaccount')" prop="alipay">
                                                    <Input v-model="formValidate2.alipay" size="large"></Input>
                                                </FormItem>
                                                <!-- passwd -->
                                                <FormItem :label="$t('uc.account.fundpwd')" prop="password">
                                                    <Input v-model="formValidate2.password" type="password"
                                                        size="large"></Input>
                                                </FormItem>
                                                <!-- Button -->
                                                <FormItem>
                                                    <Button type="primary"
                                                        @click="handleSubmit('formValidate2')">{{$t('uc.account.save')}}</Button>
                                                    <!-- <Button type="ghost" @click="handleReset('formValidate2')" style="margin-left: 8px">Reset</Button> -->
                                                </FormItem>
                                                </Col>

                                            </Row>

                                        </Form>
                                    </div>
                                </div>
                            </div>
                            <div class="account-item">
                                <div class="account-item-in">
                                    <i class="icons wechat"></i>
                                    <span class="card-number">{{$t('uc.account.wxaccount')}}</span>
                                    <p v-if="user.wechatVerified==1" class="bankInfo" style="color: #fff;">
                                        {{user.wechatPay.wechat}}
                                    </p>
                                    <p v-else class="bankInfo">
                                        {{$t('uc.account.wxaccounttip')}}
                                    </p>
                                    <a class="btn" v-if="user.wechatVerified==1"
                                        @click="showItem(3)">{{$t('uc.account.modify')}}</a>
                                    <a class="btn" v-else @click="showItem(3)">{{$t('uc.account.bind')}}</a>
                                </div>
                                <div class="account-detail" v-show="choseItem==3">
                                    <div class="detail-list">
                                        <Form ref="formValidate3" :model="formValidate3" :rules="ruleValidate"
                                            :label-width="85">
                                            <Row>
                                                <Col span="8">
                                                <input type="hidden" name="wePreview" :value="wePreview" />
                                                <img v-if="weImg" :alt="$t('uc.account.imgtip')"
                                                    style="width: 200px;height: 200px;" :src="weImg">
                                                <img v-else :alt="$t('uc.account.imgtip')"
                                                    style="width: 200px;height: 200px;"
                                                    src="../../assets/images/upload_placeholder.png">
                                                <div class="acc_sc">
                                                    <Upload ref="upload2" :on-success="weHandleSuccess"
                                                        :headers="uploadHeaders" :action="uploadUrl">
                                                        <Button
                                                            icon="ios-cloud-upload-outline">{{$t('uc.safe.upload')}}</Button>
                                                    </Upload>
                                                </div>
                                                </Col>
                                                <Col span="16">
                                                <!-- name -->
                                                <FormItem :label="$t('uc.account.name')" prop="name">
                                                    <Input disabled size="large" v-model="formValidate3.name"></Input>
                                                </FormItem>
                                                <!-- wechat -->
                                                <FormItem :label="$t('uc.account.wxaccount')" prop="wechat">
                                                    <Input v-model="formValidate3.wechat" size="large"></Input>
                                                </FormItem>
                                                <!-- passwd -->
                                                <FormItem :label="$t('uc.account.fundpwd')" prop="password">
                                                    <Input v-model="formValidate3.password" type="password"
                                                        size="large"></Input>
                                                </FormItem>
                                                <!-- Button -->
                                                <FormItem>
                                                    <Button type="primary"
                                                        @click="handleSubmit('formValidate3')">{{$t('uc.account.save')}}</Button>
                                                    <!-- <Button type="ghost" @click="handleReset('formValidate3')" style="margin-left: 8px">Reset</Button> -->
                                                </FormItem>
                                                </Col>
                                            </Row>
                                        </Form>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </section>
                </div>
            </div>
        </div>

        <div v-if="step == 0">
            <div class="d-flex justify-content-between align-items-center ws-filter-tab "
                style="padding-top: 3px !important; padding-bottom:3px !important;">
                <span @click="$router.go(-1)">
                    <b-icon icon="chevron-left" variant="light"></b-icon>
                </span>
                <div class="d-flex col py-2 justify-content-center align-items-center">
                    <span> {{$t('appmain.settings')}}</span>
                </div>
            </div>
            <div class="p-2">
                <span class="text-faded-small">
                    {{$t('uc.account.pagetip')}}
                </span>
            </div>
            <div @click="step = 1" class=" px-3 d-flex align-items-center justify-content-between "
                style="background-color: #1A212B; padding-top: 10px; padding-bottom: 10px; border-bottom: 1px solid #0f141a;">
                <span>{{$t('uc.account.backcardno')}}</span>
                <div>
                    <span v-if="user.bankVerified==1"
                        class="text-faded-1 fs-7 page-view2">{{$t('uc.account.modify')}}</span>
                    <span v-else class="text-faded-1 fs-7 page-view2">{{$t('uc.account.bind')}}</span>
                    <b-icon icon="chevron-right" variant="light"></b-icon>
                </div>
            </div>
            <div @click="step = 2" class=" px-3 d-flex align-items-center justify-content-between "
                style="background-color: #1A212B; padding-top: 10px; padding-bottom: 10px; border-bottom: 1px solid #0f141a;">
                <span>{{$t('uc.account.zfbaccount')}}</span>
                <div>
                    <span v-if="user.aliVerified==1"
                        class="text-faded-1 fs-7 page-view2">{{$t('uc.account.modify')}}</span>
                    <span v-else class="text-faded-1 fs-7 page-view2">{{$t('uc.account.bind')}}</span>
                    <b-icon icon="chevron-right" variant="light"></b-icon>
                </div>
            </div>
            <div @click="step = 3" class=" px-3 d-flex align-items-center justify-content-between "
                style="background-color: #1A212B; padding-top: 10px; padding-bottom: 10px; border-bottom: 1px solid #0f141a;">
                <span>{{$t('uc.account.wxaccount')}}</span>
                <div>
                    <span class="text-faded-1 fs-7 page-view2"
                        v-if="user.wechatVerified==1">{{$t('uc.account.modify')}}</span>
                    <span class="text-faded-1 fs-7 page-view2" v-else>{{$t('uc.account.bind')}}</span>
                    <b-icon icon="chevron-right" variant="light"></b-icon>
                </div>
            </div>
        </div>
        <div v-if="step == 1">
            <div class="d-flex justify-content-between align-items-center ws-filter-tab "
                style="padding-top: 3px !important; padding-bottom:3px !important;">
                <span @click="step = 0">
                    <b-icon icon="chevron-left" variant="light"></b-icon>
                </span>
                <div class="d-flex col py-2 justify-content-center align-items-center">
                    <span> BindBank {{$t('appmain.Account')}}</span>
                </div>
                <span class="fs-7 text-warning" @click="handleSubmit('formValidate1')">{{$t('safe.save')}}</span>
            </div>
            <div class="account-detail ws-phone-bind ws-login-password">
                <div class="detail-list mt-2">
                    <Form ref="formValidate1" :model="formValidate1" :rules="ruleValidate" :label-width="85">
                        <!-- name -->
                        <FormItem :label="$t('uc.account.name')" prop="name">
                            <Input disabled size="large" v-model="formValidate1.name"></Input>
                        </FormItem>
                        <!-- bankName -->
                        <FormItem :label="$t('uc.account.bankaccount')" prop="bankName">
                            <!--                                            <Select v-model="formValidate1.bankName" size="large">-->
                            <!--                                                <Option v-for="item in bankNameList" :value="item.value" :key="item.value">{{ item.label }}</Option>-->
                            <!--                                            </Select>-->
                            <Input size="large" v-model="formValidate1.bankName"></Input>
                        </FormItem>
                        <!-- bankBranch -->
                        <FormItem :label="$t('uc.account.bankbranch')" prop="bankBranch">
                            <Input v-model="formValidate1.bankBranch" size="large"></Input>
                        </FormItem>
                        <!-- bankNo -->
                        <FormItem :label="$t('uc.account.bankno')" prop="bankNo">
                            <Input v-model="formValidate1.bankNo" size="large" type="text"></Input>
                        </FormItem>
                        <!-- bankNoConfirm -->
                        <FormItem :label="$t('uc.account.confirmbankno')" prop="bankNoConfirm">
                            <Input v-model="formValidate1.bankNoConfirm" size="large" type="text"></Input>
                        </FormItem>
                        <!-- passwd -->
                        <FormItem :label="$t('uc.account.fundpwd')" prop="password">
                            <Input v-model="formValidate1.password" type="password" size="large"></Input>
                        </FormItem>
                    </Form>
                </div>
            </div>
        </div>
        <div v-if="step == 2">
            <div class="d-flex justify-content-between align-items-center ws-filter-tab "
                style="padding-top: 3px !important; padding-bottom:3px !important;">
                <span @click="step = 0">
                    <b-icon icon="chevron-left" variant="light"></b-icon>
                </span>
                <div class="d-flex col py-2 justify-content-center align-items-center">
                    <span> BindAlipay {{$t('appmain.Account')}}</span>
                </div>
                <span class="fs-7 text-warning" @click="handleSubmit('formValidate2')">{{$t('safe.save')}}</span>
            </div>
            <div class="account-detail ws-phone-bind ws-login-password">
                <div class="detail-list ws-file-inp-1 mt-2">
                    <Form ref="formValidate2" :model="formValidate2" :rules="ruleValidate" :label-width="95">
                        <input type="hidden" name="aliPreview" :value="aliPreview" />

                        <div @click="wsOpenFile()" class="px-2 ps-3 d-flex align-items-center justify-content-between"
                            style=" border-bottom: 2px solid #0E151F; background-color: #1A212B; padding-top: 5px; padding-bottom: 5px; ">
                            <span class="fs-8">{{$t('appmain.QrCode')}}</span>
                            <img src="../../assets/images/wsup.png" style="width:45px" alt="">
                        </div>

                        <div class="acc_sc ws-file-1 d-none">
                            <Upload ref="upload1" :on-success="aliHandleSuccess" :headers="uploadHeaders"
                                :action="uploadUrl">
                                <Button icon="ios-cloud-upload-outline">{{$t('uc.safe.upload')}}</Button>
                            </Upload>
                        </div>
                        <!-- name -->
                        <FormItem :label="$t('uc.account.name')" prop="name">
                            <Input disabled size="large" v-model="formValidate2.name"></Input>
                        </FormItem>
                        <!-- alipay -->
                        <FormItem :label="$t('uc.account.zfbaccount')" prop="alipay">
                            <Input v-model="formValidate2.alipay" size="large"></Input>
                        </FormItem>
                        <!-- passwd -->
                        <FormItem :label="$t('uc.account.fundpwd')" prop="password">
                            <Input v-model="formValidate2.password" type="password" size="large"></Input>
                        </FormItem>
                        <!-- Button -->

                    </Form>
                </div>
            </div>
        </div>
        <div v-if="step == 3">
            <div class="d-flex justify-content-between align-items-center ws-filter-tab "
                style="padding-top: 3px !important; padding-bottom:3px !important;">
                <span @click="step = 0">
                    <b-icon icon="chevron-left" variant="light"></b-icon>
                </span>
                <div class="d-flex col py-2 justify-content-center align-items-center">
                    <span> BindWeChat {{$t('appmain.Account')}}</span>
                </div>
                <span class="fs-7 text-warning" @click="handleSubmit('formValidate3')">{{$t('safe.save')}}</span>
            </div>
            <div class="account-detail ws-phone-bind ws-login-password">
                <div class="detail-list mt-2">
                    <Form ref="formValidate3" :model="formValidate3" :rules="ruleValidate" :label-width="85">
                        <div class="ws-file-inp-2">
                            <div @click="wsOpenFile2()"
                                class="px-2 ps-3 d-flex align-items-center justify-content-between"
                                style=" border-bottom: 2px solid #0E151F; background-color: #1A212B; padding-top: 5px; padding-bottom: 5px; ">
                                <span class="fs-8">{{$t('appmain.QrCode')}}</span>
                                <img src="../../assets/images/wsup.png" style="width:45px" alt="">
                            </div>
                            <input type="hidden" name="wePreview" :value="wePreview" />
                            <div class="acc_sc d-none">
                                <Upload ref="upload2" :on-success="weHandleSuccess" :headers="uploadHeaders"
                                    :action="uploadUrl">
                                    <Button icon="ios-cloud-upload-outline">{{$t('uc.safe.upload')}}</Button>
                                </Upload>
                            </div>
                        </div>

                        <!-- name -->
                        <FormItem :label="$t('uc.account.name')" prop="name">
                            <Input disabled size="large" v-model="formValidate3.name"></Input>
                        </FormItem>
                        <!-- wechat -->
                        <FormItem :label="$t('uc.account.wxaccount')" prop="wechat">
                            <Input v-model="formValidate3.wechat" size="large"></Input>
                        </FormItem>
                        <!-- passwd -->
                        <FormItem :label="$t('uc.account.fundpwd')" prop="password">
                            <Input v-model="formValidate3.password" type="password" size="large"></Input>
                        </FormItem>
                        <!-- Button -->

                    </Form>
                </div>
            </div>
        </div>
    </div>

</template>
<script>

    export default {
        components: {
        },
        data() {
            const validatePass = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(this.$t('uc.account.banknomsg1')));
                    // } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    // } else if (!/([0-9]){6,18}/.test(value)) {
                    //     callback(new Error(this.$t('uc.account.banknomsg1')));
                } else {
                    callback();
                }
            };
            const validatePassCheck = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error(this.$t('uc.account.banknomsg1')));
                    // } else if (!/([a-zA-Z0-9]){6,18}/.test(value)) {
                    // } else if (!/([0-9]){6,18}/.test(value)) {
                    //     callback(new Error(this.$t('uc.account.banknomsg1')));
                } else if (value !== this.formValidate1.bankNo) {
                    callback(new Error(this.$t('uc.account.banknomsg2')));
                } else {
                    callback();
                }
            };
            return {
                step: 0,
                uploadHeaders: { 'x-auth-token': localStorage.getItem('TOKEN') },
                uploadUrl: this.host + '/uc/upload/oss/image',
                aliImg: '',
                aliPreview: '',
                weImg: '',
                wePreview: '',
                isNoName: true,
                msg: '',
                choseItem: 0,
                user: {},
                formValidate1: {
                    name: '',
                    password: '',
                    bankName: '',
                    bankBranch: '',
                    bankNo: '',
                    bankNoConfirm: '',

                },
                formValidate2: {
                    name: '',
                    alipay: '',
                    password: '',
                },
                formValidate3: {
                    name: '',
                    wechat: '',
                    password: '',
                },
                bankNameList: [
                    {
                        value: '中国工商银行',
                        label: '中国工商银行'
                    },
                    {
                        value: '中国农业银行',
                        label: '中国农业银行'
                    },
                    {
                        value: '中国建设银行',
                        label: '中国建设银行'
                    },
                    {
                        value: '中国邮政储蓄银行',
                        label: '中国邮政储蓄银行'
                    },
                    {
                        value: '招商银行',
                        label: '招商银行'
                    },
                    {
                        value: '中国银行',
                        label: '中国银行'
                    },
                    {
                        value: '交通银行',
                        label: '交通银行'
                    },
                    {
                        value: '中信银行',
                        label: '中信银行'
                    },
                    {
                        value: '华夏银行',
                        label: '华夏银行'
                    },
                    {
                        value: '中国民生银行',
                        label: '中国民生银行'
                    },
                    {
                        value: '广发银行',
                        label: '广发银行'
                    },
                    {
                        value: '平安银行',
                        label: '平安银行'
                    },
                    {
                        value: '兴业银行',
                        label: '兴业银行'
                    },
                    {
                        value: '上海浦东发展银行',
                        label: '上海浦东发展银行'
                    },
                    {
                        value: '浙商银行',
                        label: '浙商银行'
                    },
                    {
                        value: '渤海银行',
                        label: '渤海银行'
                    },
                    {
                        value: '恒丰银行',
                        label: '恒丰银行'
                    },
                    {
                        value: '花旗银行',
                        label: '花旗银行'
                    },
                    {
                        value: '渣打银行',
                        label: '渣打银行'
                    },
                    {
                        value: '汇丰银行',
                        label: '汇丰银行'
                    },
                    {
                        value: '中国光大银行',
                        label: '中国光大银行'
                    },
                    {
                        value: '上海银行',
                        label: '上海银行'
                    },
                    {
                        value: '江苏银行',
                        label: '江苏银行'
                    },
                    {
                        value: '重庆银行',
                        label: '重庆银行'
                    },
                    {
                        value: '天津银行',
                        label: '天津银行'
                    },
                    {
                        value: '厦门银行',
                        label: '厦门银行'
                    },
                    {
                        value: '城市商业银行',
                        label: '城市商业银行'
                    },
                    {
                        value: '农村商业银行',
                        label: '农村商业银行'
                    },
                    {
                        value: '徽商银行',
                        label: '徽商银行'
                    },



                ],
                ruleValidate: {
                    name: [
                        { required: true, message: this.$t('uc.account.verifiedmsg'), trigger: 'blur' }
                    ],
                    bankName: [
                        { required: true, message: this.$t('uc.account.bankaccountmsg'), trigger: 'change' }
                    ],
                    bankBranch: [
                        { required: true, message: this.$t('uc.account.bankbranchmsg'), trigger: 'blur' }
                    ],
                    bankNo: [
                        { required: true, type: 'string', min: 1, message: this.$t('uc.account.banknomsg1'), trigger: 'blur' },
                        { validator: validatePass, trigger: 'blur' },
                    ],
                    bankNoConfirm: [
                        { required: true, type: 'string', min: 1, message: this.$t('uc.account.banknomsg2'), trigger: 'blur' },
                        { validator: validatePassCheck, trigger: 'blur' },
                    ],
                    password: [
                        { required: true, message: this.$t('uc.account.fundpwdmsg1'), trigger: 'blur' },
                        { min: 6, message: this.$t('uc.account.fundpwdmsg2'), trigger: 'blur' }
                    ],
                    alipay: [
                        { required: true, message: this.$t('uc.account.zfbaccountmsg'), trigger: 'blur' }
                    ],
                    wechat: [
                        { required: true, message: this.$t('uc.account.wxaccountmsg'), trigger: 'blur' }
                    ],
                },

            }
        },
        methods: {
            wsOpenFile() {
                document.querySelector('.ws-file-inp-1 .ivu-upload button').click()
            },
            wsOpenFile2() {
                document.querySelector('.ws-file-inp-2 .ivu-upload button').click()
            },
            aliHandleSuccess(res, file, fileList) {
                // console.log(res);
                this.$refs.upload1.fileList = [fileList[fileList.length - 1]];
                this.aliImg = this.aliPreview = res.data;
            },
            weHandleSuccess(res, file, fileList) {
                this.$refs.upload2.fileList = [fileList[fileList.length - 1]];
                this.weImg = this.wePreview = res.data;
            },
            handleSubmit(name) {
                this.$refs[name].validate((valid) => {
                    if (valid) {
                        this.submit(name)
                    } else {
                        this.$Message.error(this.$t('uc.account.save_failure'));
                    }
                })
            },
            handleReset(name) {
                this.$refs[name].resetFields();
            },
            submit(name) {
                //银行卡
                if (name == 'formValidate1') {
                    let param = {}
                    param['bank'] = this.formValidate1.bankName
                    param['branch'] = this.formValidate1.bankBranch
                    param['jyPassword'] = this.formValidate1.password
                    param['realName'] = this.formValidate1.name
                    param['cardNo'] = this.formValidate1.bankNo
                    if (this.user.bankVerified == 1) { //修改
                        this.$http.post(this.host + '/uc/approve/update/bank', param).then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t('uc.account.save_success'));
                                this.getAccount()
                                this.choseItem = 0
                            } else {
                                this.$Message.error(resp.message);
                            }
                        })
                    } else { //设置
                        this.$http.post(this.host + '/uc/approve/bind/bank', param).then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t('uc.account.save_success'));
                                this.getAccount()
                                this.choseItem = 0
                            } else {
                                this.$Message.error(resp.message);
                            }
                        })
                    }
                }
                //支付宝
                if (name == 'formValidate2') {
                    let param = {}
                    param['ali'] = this.formValidate2.alipay
                    param['jyPassword'] = this.formValidate2.password
                    param['realName'] = this.formValidate2.name
                    param['qrCodeUrl'] = this.aliPreview;

                    if (this.user.aliVerified == 1) {
                        this.$http.post(this.host + '/uc/approve/update/ali', param).then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t('uc.account.save_success'));
                                this.getAccount()
                                this.choseItem = 0
                            } else {
                                this.$Message.error(resp.message);
                            }
                        })
                    } else {
                        this.$http.post(this.host + '/uc/approve/bind/ali', param).then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t('uc.account.save_success'));
                                this.getAccount()
                                this.choseItem = 0
                            } else {
                                this.$Message.error(resp.message);
                            }
                        })
                    }
                }
                //微信
                if (name == 'formValidate3') {
                    let param = {}
                    param['wechat'] = this.formValidate3.wechat
                    param['jyPassword'] = this.formValidate3.password
                    param['realName'] = this.formValidate3.name
                    param['qrCodeUrl'] = this.wePreview;

                    if (this.user.wechatVerified == 1) {
                        this.$http.post(this.host + '/uc/approve/update/wechat', param).then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t('uc.account.save_success'));
                                this.getAccount()
                                this.choseItem = 0
                            } else {
                                this.$Message.error(resp.message);
                            }
                        })
                    } else {
                        this.$http.post(this.host + '/uc/approve/bind/wechat', param).then(response => {
                            var resp = response.body;
                            if (resp.code == 0) {
                                this.$Message.success(this.$t('uc.account.save_success'));
                                this.getAccount()
                                this.choseItem = 0
                            } else {
                                this.$Message.error(resp.message);
                            }
                        })
                    }
                }
            },
            showItem(index) {
                this.choseItem = index;
            },
            noName() {
                this.$Message.error(this.msg);
            },
            getAccount() {
                //获取个人账户信息
                this.$http.post(this.host + '/uc/approve/account/setting').then(response => {
                    var resp = response.body;
                    // console.log(resp);
                    if (resp.code == 0) {
                        this.user = resp.data;
                        this.formValidate1.name = this.formValidate2.name = this.formValidate3.name = this.user.realName
                        // this.usernameS = (this.user.username + '').slice(0, 1)
                        // this.dataCount = resp.data.length
                        this.isNoName = false
                        //设置
                        this.formValidate1.bankName = this.user.bankInfo == null ? '' : this.user.bankInfo.bank
                        this.formValidate1.bankBranch = this.user.bankInfo == null ? '' : this.user.bankInfo.branch
                        this.formValidate1.bankNo = this.user.bankInfo == null ? '' : this.user.bankInfo.cardNo
                        this.formValidate2.alipay = this.user.alipay == null ? '' : this.user.alipay.aliNo
                        this.formValidate3.wechat = this.user.wechatPay == null ? '' : this.user.wechatPay.wechat
                        this.aliImg = this.aliPreview = this.user.alipay == null ? '' : this.user.alipay.qrCodeUrl;
                        this.weImg = this.wePreview = this.user.wechatPay == null ? '' : this.user.wechatPay.qrWeCodeUrl;

                    } else {
                        this.msg = resp.message;
                        //this.$Message.error(resp.message)
                        this.$Notice.error({
                            title: this.$t("common.tip"),
                            desc: resp.message
                        });
                        this.$router.push("/uc/safe");
                    }
                })
            }

        },
        created() {
            this.getAccount()
        },
        computed: {
        }
    }
</script>