<template>
  <div>
    <div class="nav-rights d-none">
      <div class="nav-right col-xs-12 col-md-10 padding-right-clear">
        <div class="bill_box rightarea padding-right-clear">
          <div class="shaow">
            <div class="money_table">
              <div style="width: 100%;height: 50px;">
                <div style="float:left;letter-spacing:1px;padding-top: 5px;">
                  <span style="font-size:12px;color:#828ea1;">{{$t('uc.finance.money.totalassets')}}</span>
                  <span style="font-size: 18px;color:#D8E1EB;">${{totalUSDT}}</span>
                  <!--<span style="font-size:10px;color:#828ea1;margin-left: 5px;"> ≈ ¥{{totalCny}}</span>-->
                </div>
                <Input style="float:right;" class="search" search :placeholder="$t('common.searchplaceholder')"
                  @on-change="seachInputChange" v-model="searchKey" />

                <!--<Button @click="toQuickExchange" type="warning" style="padding: 6px 30px;margin-right:30px;background-color:#f0a70a;border-color:#f0a70a;float:right;">{{$t('uc.finance.record.quickExchange')}}</Button>-->
              </div>
              <Table :columns="tableColumnsMoney" :data="tableMoneyShow" :loading="loading" :disabled-hover="true">
              </Table>
            </div>
          </div>
        </div>
      </div>
      <Modal v-model="modal" :title="$t('uc.finance.money.match')" @on-ok="matchGCC">
        <P style="font-weight: bold;padding: 10px 0;">{{$t('uc.finance.money.matchtip1')}}：{{GCCMatchAmount}}</p>
        <p>
          <span>{{$t('uc.finance.money.matchtip2')}}：</span>
          <InputNumber style="width: 150px;" type="text" v-model="matchAmount"
            :placeholder="$t('uc.finance.money.matchtip2')"></InputNumber>
        </p>
      </Modal>
      <Modal v-model="modal_msg" :title="$t('uc.finance.money.match')">
        <p>{{match_msg}}</p>
      </Modal>
      <Modal v-model="transModal" @on-ok="confrimTrans" :title="$t('uc.finance.money.onkeytrans')">
        <p>{{$t('uc.finance.record.chooseTransCoinUnit')}}：
          <Select v-model="toUnit" style="width: 400px;">
            <Option v-for="item in transList" :value="item" :key="item">{{ item }}</Option>
          </Select>
        </p>
        <p style="margin-top: 15px;">{{$t('uc.finance.record.inputTransAmount')}}：
          <InputNumber style="width: 400px;" type="text" v-model="transAmount"
            :placeholder="$t('uc.finance.money.matchtip2')"></InputNumber>
        </p>
      </Modal>


      <Modal v-model="quickExchangeModal" @on-ok="confrimExchange" :title="$t('uc.finance.record.quickExchange')">
        <h2 style="text-align:center;">{{$t('uc.finance.record.currentRate')}}： <span style="color: #45b854">{{priceRate
            | fixed2}} </span></h2>
        <p style="margin-top: 15px;">{{$t('uc.finance.record.from')}}：
          <Select v-model="fromExchangeCoin" style="width: 450px;">
            <Option v-for="item in fromCoinList" :value="item" :key="item">{{ item }}</Option>
          </Select>
        </p>
        <p style="margin-top: 15px;">{{$t('uc.finance.record.to')}}：
          <Select v-model="toExchangeCoin" style="width: 450px;">
            <Option v-for="item in toCoinList" :value="item" :key="item">{{ item }}</Option>
          </Select>
        </p>
        <p style="margin-top: 15px;">{{$t('uc.finance.record.inputexchangeamount')}}:
          <InputNumber style="width: 330px;" type="text" v-model="exchangeAmount"
            :placeholder="$t('uc.finance.record.inputexchangeamount')"></InputNumber>
          <span style="margin-left: 10px;font-weight: bold;">{{fromExchangeCoin}}</span>
        </p>
        <p style="margin-top: 15px;">{{$t('uc.finance.record.predictAmount')}}:
          <InputNumber style="width: 330px;" disabled type="text" v-model="predictAmount"></InputNumber>
          <span style="margin-left: 10px;font-weight: bold;">{{toExchangeCoin}}</span>
        </p>
        <p style="margin-top: 15px;">{{$t('uc.finance.record.inputexchangepasswd')}}:
          <Input style="width: 400px;" type="password" v-model="exchangePassword"
            :placeholder="$t('uc.finance.record.inputexchangepasswd')"></Input>
        </p>
      </Modal>
    </div>

    <div class="d-flex justify-content-between align-items-center ws-filter-tab "
      style="padding-top: 3px !important; padding-bottom:3px !important;">
      <span @click="$router.go(-1)">
        <b-icon icon="chevron-left" variant="light"></b-icon>
      </span>
      <div class="d-flex col py-2 justify-content-center align-items-center">
        <span>{{$t('appmain.Wallet')}}</span>
      </div>
      <div class="ws-drop-1">
        <b-dropdown size="sm" dropleft  text="Detail" class="m-2">
          <b-dropdown-item-button>
            <router-link to="/uc/recharge?name=deposit-record">{{$t('appmain.Depositrecord')}}</router-link>
          </b-dropdown-item-button>
          <b-dropdown-item-button>
            <router-link to="/uc/withdraw?name=withdraw-record">{{$t('appmain.Withdrawrecord')}}</router-link>
          </b-dropdown-item-button>
        </b-dropdown>
      </div>
    </div>

    <div class="p-3">
      <div class="ws-wallet-bg p-3 ">
        <div class="row mx-0">
          <div class="col-lg-6 px-0 d-flex flex-column">
            <span class="fs-7">{{$t('appmain.TotalValue')}}(USDT)</span>
            <span class="fs-5 py-1 fw-500">${{totalUSDT}}</span>
            <span class="fs-7">≈ {{ totalUSDT }} USD</span>
          </div>
        </div>
      </div>

      <div class="py-2 row mx-0">
        <div class="col d-flex  px-0">
          <router-link to='/uc/recharge' class="gray-btn d-flex justify-content-center text-center py-1 w-100 col"
            style="min-width:100% !important; color: white !important;">{{$t('appmain.Deposit')}}</router-link>
        </div>
        <div class="col d-flex  px-2">
          <router-link to='/uc/withdraw' class="gray-btn d-flex justify-content-center text-center py-1 w-100 col"
            style="min-width:100% !important; color: white !important;">{{$t('appmain.Withdraw')}}</router-link>
        </div>
        <div class="col d-flex  px-0">
          <router-link to='/uc/swapAssets?show=transfer' class="gray-btn d-flex justify-content-center text-center py-1 w-100 col"
            style="min-width:100% !important; color: white !important;">{{$t('appmain.Transfer')}}</router-link>
        </div>
      </div>

      <div class="d-flex align-items-center w-100" style="border-bottom:1px solid rgba(0, 0, 0, 0.404) ;">
        <span @click="isFutresOpen = false" :class="!isFutresOpen ? 'active-1 ws-filter-links ' : 'ws-filter-links' "
          style="font-size: 14px !important ;"> {{$t('appmain.FiatandSpot')}}</span>
        <span @click="isFutresOpen = true" :class="isFutresOpen ? 'active-1 ws-filter-links '  : 'ws-filter-links'"
          style="font-size: 14px !important ;">{{$t('appmain.Futures')}}</span>
      </div>
      <div v-if="!isFutresOpen">
        <div class="ws-wallet-search pt-2">
          <Input class="search" search :placeholder="$t('common.searchplaceholder')" @on-change="seachInputChange"
            v-model="searchKey" />
        </div>

        <div v-for="(items,key) in tableMoneyShow" class="d-flex flex-column "
          style=" border-bottom: 1px solid #27313e6e !important;">
          <div class="py-2 d-flex w-100 flex-column"
            @click="openWithdrawModal(items.coinType,items.coin.canRecharge,items.coin.canWithdraw)">
            <span class="fw-500">{{ items.coinType }}</span>
            <div class="row mx-0 pt-2">
              <div class="col-4 px-0 d-flex flex-column">
                <span class="fs-8 text-faded-small">{{$t('appmain.Available')}}</span>
                <span class="fs-9">{{ items.balance.toFixed(7) }}</span>
              </div>
              <div class="col-5 px-0 d-flex flex-column">
                <span class="fs-8 text-faded-small">{{$t('appmain.InOrder')}} </span>
                <span class="fs-9">{{ items.frozenBalance.toFixed(7) }}</span>
              </div>
              <div class="col-3 px-0 d-flex flex-column">
                <span class="fs-8 text-faded-small"> {{$t('appmain.Tobereleased')}}</span>
                <span class="fs-9">{{ items.toReleased.toFixed(7) }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="isFutresOpen">
        <SwapAssets />
      </div>

    </div>

    <div class="bottom-up" v-if="isModalOpen == true" @click=" isModalOpen = false">
      <div class="d-flex text-danger flex-column justify-content-center " style="background-color: #1A212B;">
        <router-link :to="'/uc/recharge?name='+DepsiteWithdrawName" v-if="wscanRecharge == true"
          class="text-success p-2 text-center" style="border-bottom:1px solid rgba(0, 0, 0, 0.404) ;"> {{$t('appmain.Deposit')}}
        </router-link>
        <router-link :to="'/uc/withdraw?name='+DepsiteWithdrawName" v-if="wscanWithdraw == true"
          class="text-danger p-2 text-center"> {{$t('appmain.Withdraw')}}</router-link>
        <span v-if="!wscanRecharge && !wscanWithdraw" class="p-3 text-center text-light fs-8">
          {{$t('appmain.WithdrawText1')}}
        </span>
      </div>
    </div>

  </div>
</template>
<script>

  import SwapAssets from './SwapAssets.vue'

  export default {
    components: {
      SwapAssets
    },
    data() {
      return {
        isFutresOpen: false,
        isModalOpen: false,
        wscanRecharge: true,
        wscanWithdraw: true,
        DepsiteWithdrawName: "",
        GCCMatchAmount: 0,
        matchAmount: 0,
        modal: false,
        loginmsg: this.$t("common.logintip"),
        loading: true,
        ordKeyword: "",
        tableMoney: [],
        tableMoneyShow: [],
        canMatch: true,
        modal_msg: false,
        match_msg: "",
        searchKey: "",
        transModal: false,
        toUnit: "",
        fromUnit: "",
        transAmount: 0,
        transList: ["USDT", "EUSDT", "TUSDT"],
        quickExchangeModal: false,
        fromExchangeCoin: "CCASH",
        toExchangeCoin: "USDT",
        fromCoinList: ["CCASH"],
        toCoinList: ["USDT"],
        exchangeAmount: 0,
        exchangePassword: "",
        predictAmount: 0,
        priceRate: 7.0
      };
    },
    filters: {
      fixed2: function (value) {
        return value.toFixed(2);
      }
    },
    methods: {
      openWithdrawModal(name, canRecharge, canWithdraw) {
        if (canRecharge == 0) {
          this.wscanRecharge = false
        }
        else {
          this.wscanRecharge = true
        }
        if (canWithdraw == 0) {
          this.wscanWithdraw = false
        } else {
          this.wscanWithdraw = true
        }
        this.isModalOpen = true
        this.DepsiteWithdrawName = name
      },
      seachInputChange() {
        this.tableMoneyShow = this.tableMoney.filter(item => item["coinType"].indexOf(this.searchKey) == 0);
      },
      toQuickExchange() {
        this.quickExchangeModal = true;
      },
      confrimExchange() {
        let params = {};
        params["toUnit"] = this.toExchangeCoin;
        params["fromUnit"] = this.fromExchangeCoin;
        params["amount"] = this.transAmount;
        params["jyPassword"] = this.exchangePassword;
        this.$http
          .post(this.host + "/uc/asset/wallet/quick-exchange", params)
          .then(response => {
            var resp = response.body;
            if (resp.code == 0) {
              this.$Message.success(resp.message);
            } else {
              this.$Message.error(resp.message);
            }
            this.quickExchangeModal = false;
          });
        return false;
      },
      getMoney() {
        //获取
        this.$http.post(this.host + "/uc/asset/wallet").then(response => {
          var resp = response.body;
          if (resp.code == 0) {
            this.tableMoney = resp.data;
            for (let i = 0; i < this.tableMoney.length; i++) {
              this.tableMoney[i]["coinType"] = this.tableMoney[i].coin.unit;
            }
            this.loading = false;
            this.tableMoneyShow = this.tableMoney;
          } else {
            this.$Message.error(this.loginmsg);
          }

          console.log(this.tableMoneyShow);
        });
      },
      getGCCMatchAmount() {
        //获取
        this.$http
          .post(this.host + "/uc/asset/wallet/match-check")
          .then(response => {
            var resp = response.body;
            if (resp.code == 0) {
              this.canMatch = true;
              this.GCCMatchAmount = resp.data;
            } else {
              this.canMatch = false;
              this.match_msg = resp.message;
              // this.$Message.error(this.loginmsg);
            }
            this.showMatchDailog();
          });
      },
      getRate() {
        this.$http
          .post(this.host + "/market/ctc-usdt")
          .then(response => {
            var resp = response.body;
            this.priceRate = resp.data.buy;
          });
      },
      showMatchDailog() {
        if (this.canMatch) this.modal = true;
        else this.modal_msg = true;
      },
      matchGCC() {
        if (this.matchAmount <= 0) {
          this.$Message.warning(this.$t("uc.finance.money.matcherr1"));
        } else if (this.matchAmount > this.GCCMatchAmount) {
          this.$Message.warning(this.$t("uc.finance.money.matcherr2"));
        } else {
          //配对
          let params = {};
          params["amount"] = this.matchAmount;
          this.$http
            .post(this.host + "/uc/asset/wallet/match", params)
            .then(response => {
              var resp = response.body;
              if (resp.code == 0) {
                this.$Message.success(this.$t("uc.finance.money.matchsuccess"));
                this.GCCMatchAmount = this.GCCMatchAmount - this.matchAmount;
              } else {
                this.$Message.error(resp.message);
              }
            });
        }
      },
      resetAddress(unit) {
        this.$router.push(
          "/uc/recharge?name=" + unit
        );
      },
      confrimTrans: function () {
        let params = {};
        params["toUnit"] = this.toUnit;
        params["fromUnit"] = this.fromUnit;
        params["amount"] = this.transAmount;
        this.$http
          .post(this.host + "/uc/asset/wallet/trans-usd", params)
          .then(response => {
            var resp = response.body;
            if (resp.code == 0) {
              this.$Message.success(resp.message);
            } else {
              this.$Message.error(resp.message);
            }
            this.transModal = false;
          });
        return false;
      }
    },
    mounted() {
    },
    created() {
      this.getMoney();
    },
    computed: {

      totalUSDT() {
        let usdtTotal = 0;
        for (let i = 0; i < this.tableMoney.length; i++) {
          usdtTotal += (this.tableMoney[i].balance + this.tableMoney[i].frozenBalance) * this.tableMoney[i].coin.usdRate;
        }
        return usdtTotal.toFixed(2);
      },
      totalCny() {
        let cnyTotal = 0;
        for (let i = 0; i < this.tableMoney.length; i++) {
          cnyTotal += (this.tableMoney[i].balance + this.tableMoney[i].frozenBalance) * this.tableMoney[i].coin.cnyRate;
        }
        return cnyTotal.toFixed(2);
      },
      tableColumnsMoney() {
        let self = this;
        let columns = [];
        columns.push({
          title: this.$t("uc.finance.money.cointype"),
          key: "coinType",
          width: 100,
          align: "center"
        });
        columns.push({
          title: this.$t("uc.finance.money.balance"),
          key: "balance",
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.balance
                }
              },
              self.toFloor(params.row.balance || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.money.frozen"),
          key: "frozenBalance",
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.frozenBalance
                }
              },
              self.toFloor(params.row.frozenBalance || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.money.needreleased"),
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.toReleased
                }
              },
              self.toFloor(params.row.toReleased || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.money.operate"),
          key: "price1",
          align: "center",
          width: 250,
          render: function (h, params) {
            var actions = [];
            if (params.row.coin.canRecharge == 1) {
              if ((params.row.address != null && params.row.address != "") || (params.row.coin.accountType == 1)) {
                // 充币
                actions.push(
                  h(
                    "Button",
                    {
                      // 充币;
                      props: {
                        type: "info",
                        size: "small"
                      },
                      on: {
                        click: function () {
                          self.$router.push(
                            "/uc/recharge?name=" + params.row.coin.unit
                          );
                        }
                      },
                      style: {
                        marginRight: "8px"
                      }
                    },
                    self.$t("uc.finance.money.charge")
                  )
                );
              } else {
                //   获取地址按钮;
                actions.push(
                  h(
                    "Button",
                    {
                      props: {
                        type: "info",
                        size: "small"
                      },
                      on: {
                        click: function () {
                          self.resetAddress(params.row.coin.unit);
                        }
                      },
                      style: {
                        marginRight: "8px"
                      }
                    },
                    self.$t("uc.finance.money.charge")
                  )
                );
              }
            } else {
              actions.push(
                h(
                  "Button",
                  {
                    props: {
                      size: "small",
                      disabled: true
                    },
                    on: {
                      click: function () {

                      }
                    },
                    style: {
                      marginRight: "8px"
                    }
                  },
                  self.$t("uc.finance.money.charge")
                )
              );
            }
            if (params.row.coin.canWithdraw == 1) {
              // 提币;
              actions.push(
                h(
                  "Button",
                  {
                    props: {
                      type: "error",
                      size: "small"
                    },
                    on: {
                      click: function () {
                        self.$router.push(
                          "/uc/withdraw?name=" + params.row.coin.unit
                        );
                      }
                    },
                    style: {
                      marginRight: "8px"
                    }
                  },
                  self.$t("uc.finance.money.pickup")
                )
              );
            } else {
              actions.push(
                h(
                  "Button",
                  {
                    props: {
                      size: "small",
                      disabled: true
                    },
                    on: {
                      click: function () {

                      }
                    },
                    style: {
                      marginRight: "8px"
                    }
                  },
                  self.$t("uc.finance.money.pickup")
                )
              );
            }
            if (params.row.coin.unit == "USDT" || params.row.coin.unit == "EUSDT" || params.row.coin.unit == "TUSDT") {
              // actions.push(
              //   h(
              //     "Button",
              //     {
              //       props: {
              //         type: "success",
              //         size: "small",
              //         disabled: false
              //       },
              //       on: {
              //         click: function() {
              //           self.fromUnit = params.row.coin.unit;
              //           self.transModal = true;
              //         }
              //       },
              //       style: {
              //         marginRight: "8px"
              //       }
              //     },
              //     self.$t("uc.finance.money.onkeytrans")
              //   )
              // );
            }
            return h("p", actions);
          }
        });
        return columns;
      }
    }
  };
</script>
<style lang="scss">
  .nav-right {
    .rightarea.bill_box {
      .shaow {
        padding: 5px;
      }

      .money_table {
        .search {
          width: 200px;
          margin-bottom: 10px;
        }

        .ivu-table-wrapper {
          .ivu-table-header {
            background: #27313e;

            th {
              color: #fff;
            }
          }

          .ivu-table-body {
            td {
              color: #fff;

              .ivu-table-cell {
                // padding: 10px 10px;
                padding: 5px 10px;

                p .ivu-btn {
                  background: transparent;
                  height: 25px;
                  padding: 0 0px;
                  border-radius: 0;

                  span {
                    display: inline-block;
                    line-height: 20px;
                    font-size: 12px;
                    padding: 0 15px;
                    letter-spacing: 1px;
                  }
                }

                p .ivu-btn.ivu-btn-info {
                  border: 1px solid #f0ac19;

                  span {
                    color: #f0ac19;
                  }
                }

                p .ivu-btn.ivu-btn-error {
                  border: 1px solid #f15057;

                  span {
                    color: #f15057;

                  }
                }

                p .ivu-btn.ivu-btn-primary {
                  border: 1px solid #00b275;
                  border: 1px solid #00b275;

                  span {
                    color: #00b275;
                  }
                }

                p .ivu-btn.ivu-btn-default {
                  border: 1px solid #2c384f;
                  background: #222c3e;

                  span {
                    color: #54637a;
                  }
                }

                p .ivu-btn.ivu-btn-success {
                  border: 1px solid #32ad00;

                  span {
                    color: #32ad00;
                  }
                }

                p {
                  width: 100%;
                  height: 100%;
                  padding-top: 5px;
                  display: inline-flex;
                  align-items: center;
                  flex-wrap: wrap;

                  button {
                    margin-bottom: 5px;
                  }
                }

              }
            }
          }
        }
      }
    }
  }

  .ivu-input-number-disabled .ivu-input-number-input {
    background: transparent !important;
  }
</style>

<style scoped lang="scss">
  .nav-right {
    height: auto;
    overflow: hidden;
    padding: 0 0 0 15px;

    .rightarea.bill_box {
      padding-left: 15px;
      width: 100%;
      height: auto;
      overflow: hidden;
    }
  }

  .demo-spin-icon-load {
    animation: ani-demo-spin 1s linear infinite;
  }

  @media screen and (max-width:768px) {
    .search {
      display: none;
    }
  }
</style>