<template>
  <div>

    <!-- <div class="nav-rights d-none">
    <div class="nav-right col-xs-12 col-md-10 padding-right-clear">
      <div class="bill_box rightarea padding-right-clear">
        <div class="shaow">
          <div class="money_table">
            <div style="width: 100%;height: 50px;">
            <div style="float:left;letter-spacing:1px;padding-top: 5px;">
                <span style="font-size:12px;color:#828ea1;">{{$t('uc.finance.swap.totalassets')}}</span>
                <span style="font-size: 18px;color:#D8E1EB;">${{totalUSDT}}</span>
                <span style="font-size:10px;color:#828ea1;margin-left: 5px;"> ≈ ¥{{totalCny}}</span>
            </div>
            <Input style="float:right;" class="search" search :placeholder="$t('common.searchplaceholder')" @on-change="seachInputChange" v-model="searchKey"/>

            <Button type="primary" @click="onTransferClick" style="padding: 6px 15px;margin-right:30px;letter-spacing:2px;color:#f0ac19;background-color:transparent;border:1px solid #f0ac19;float:right;">{{$t('uc.finance.swap.transfor')}}</Button>
            </div>
            <Table :columns="tableColumnsMoney" :data="tableMoneyShow" :loading="loading" :disabled-hover="true"></Table>
          </div>
        </div>
      </div>
    </div> -->


    <div v-if="step == 1" :title="$t('uc.finance.swap.transfor') + ' - USDT'">
      <div class="d-flex justify-content-between align-items-center ws-filter-tab "
        style="padding-top: 3px !important; padding-bottom:3px !important;">
        <span @click="$router.go(-1)">
          <b-icon icon="chevron-left" variant="light"></b-icon>
        </span>
        <div class="d-flex col py-2 justify-content-center align-items-center">
          <span> {{$t('appmain.Assettransfer')}}</span>
        </div>
      </div>
      <div class="px-2">

        <div class=" mt-3 d-flex" style="border:1px solid #1A212B; border-radius: 5px;">
          <div class="d-flex p-2 px-4 flex-column align-items-center justify-content-between">
            <span class="fs-8">{{$t('appmain.Form')}}</span>
            <img src="../../assets/images/wsdot.png" style="height:33px;" alt="">
            <span class="fs-8">{{$t('appmain.To')}}</span>
          </div>
          <div class="col d-flex flex-column pe-2">
            <template v-if="transferDirection == 1">
              <div class="d-flex flex-column p-2 py-1" style="border-bottom:1px solid #1A212B; ">
                <span class="fs-8">{{$t('appmain.FiatandSpot')}}</span>
                <span class="fs-9" style="opacity: .6;">{{$t('appmain.Balance')}} {{swapWallet.avaBalance | fixed2}} USDT</span>
              </div>
              <div @click="step = 2" class="d-flex align-items-center">
                <div class="col d-flex flex-column p-2 py-1">
                  <span class="fs-8">{{$t('appmain.Futures')}}</span>
                  <span class="fs-9" style="opacity: .6;">{{$t('appmain.Balance')}} {{assetsWallet.balance | fixed2}} USDT</span>
                </div>
                <span>
                  <b-icon icon="chevron-right" variant="light"></b-icon>
                </span>
              </div>
            </template>
            <template v-if="transferDirection == 2">
              <div @click="step = 2" class="d-flex align-items-center" style="border-bottom:1px solid #1A212B; ">
                <div class="col d-flex flex-column p-2 py-1">
                  <span class="fs-8">{{$t('appmain.Futures')}}</span>
                  <span class="fs-9" style="opacity: .6;">{{$t('appmain.Balance')}} {{assetsWallet.balance | fixed2}} USDT</span>
                </div>
                <span>
                  <b-icon icon="chevron-right" variant="light"></b-icon>
                </span>
              </div>
              <div class="d-flex flex-column p-2 py-1">
                <span class="fs-8">{{$t('appmain.FiatandSpot')}}</span>
                <span class="fs-9" style="opacity: .6;">{{$t('appmain.Balance')}} {{swapWallet.avaBalance | fixed2}} USDT</span>
              </div>
            </template>
          </div>
          <div @click="changeTo(transferDirection == 1 ? 2 : 1 )"
            class="px-4 d-flex justify-content-between align-items-center" style="background-color:#1A212B;">
            <b-icon icon="arrow-down-up" variant="light"></b-icon>
          </div>
        </div>

        <div class="d-flex flex-column pt-3">
          <span class="fs-8">{{$t('appmain.Coin')}}</span>
          <div @click="step = 3" style="border-bottom:1px solid #1A212B"
            class="d-flex justify-content-between align-items-center ">
            <span class="fw-500 py-1">{{wallet1symbol == "" ? "Choose currency" : wallet1symbol }}</span>
            <span>
              <b-icon icon="chevron-right" variant="light"></b-icon>
            </span>
          </div>
        </div>

        <div class="d-flex flex-column pt-3">
          <span class="fs-8">{{$t('appmain.Amount')}}</span>
          <div class="ws-transfer-inp d-flex align-items-center">
            <InputNumber style="width: 330px;margin-left:15px;" type="text" v-model="transferAmount"
              :placeholder="$t('uc.finance.swap.inputtransferamount')"></InputNumber>
            <span class="fs-7 " @click="onTransAll">{{$t('uc.finance.swap.all')}}</span>
          </div>
        </div>

        <Button type="warning" style="border-radius: 5px !important; overflow: hidden;" class="w-100 mt-4" size="large"
          @click="confirmOk">{{$t("uc.finance.swap.oktransfer")}}</Button>

        <!-- <div style="width: 100%; min-height: 32px;margin-top: 15px;">
          <div style="width: 45%;display: inline-flex;height:32px;line-height: 32px;float:left;">
            <Select v-model="walletOne" style="width: 450px;"
              :placeholder="$t('uc.finance.swap.currencyaccount') + '(USDT)'">
              <Option v-for="item in walletOneList" :value="item" :key="item">{{ item }}</Option>
            </Select>
          </div>

          <div v-if="transferDirection == 1" @click="changeTo(2)"
            style="width: 10%;float: left;text-align: center;font-size:18px;height: 30px; line-height:30px;font-weight: bold;color:">
            <Tooltip :content="$t('uc.finance.swap.clickchange')">
              <Icon style="font-weight:bold;color:#19be6b;" type="md-arrow-forward" />
            </Tooltip>
          </div>

          <div v-if="transferDirection == 2" @click="changeTo(1)"
            style="width: 10%;float: left;text-align: center;font-size:18px;height: 30px; line-height:30px;font-weight: bold;color:">
            <Tooltip :content="$t('uc.finance.swap.clickchange')">
              <Icon style="font-weight:bold;color:#19be6b;" type="md-arrow-back" />
            </Tooltip>
          </div>

          <div style="width: 45%;display: inline-flex;height:32px;line-height: 32px;float: right;">
            <Select v-model="walletTwo" style="width: 450px;" :placeholder="$t('uc.finance.swap.swapaccount')"
              @on-change="onChangeTwo">
              <Option v-for="item in tableMoney" :value="item.id" :key="item.id">{{ item.symbol
                }}{{$t("uc.finance.swap.swap")}}</Option>
            </Select>
          </div>
        </div>
        <div style="width: 100%; min-height: 20px;margin-top: 0px;font-size: 10px;color:rgb(97, 104, 138);">
          <div style="width: 50%;height:20px;line-height: 20px;float:left;text-align: left;">
            {{$t('uc.finance.swap.avaamount')}}：{{assetsWallet.balance | fixed2}}
          </div>
          <div style="width: 50%;height:20px;line-height: 20px;float: right;text-align: right;">
            {{$t('uc.finance.swap.avaamount')}}：{{swapWallet.avaBalance | fixed2}}
          </div>
        </div> -->


      </div>
    </div>

    <div v-if="step == 3">
      <div class="d-flex justify-content-between align-items-center ws-filter-tab "
        style="padding-top: 3px !important; padding-bottom:3px !important;">
        <span @click="step = 1">
          <b-icon icon="chevron-left" variant="light"></b-icon>
        </span>
        <div class="d-flex col py-2 justify-content-center align-items-center">
          <span>{{$t('appmain.Choosecurrency ')}}</span>
        </div>
      </div>
      <div v-for="item in tableMoney" @click="wswalletTwo(item.id,item.symbol)" style="border-bottom: 1px solid #1A212B;" class="px-3 fw-500 py-2">
        {{ item.symbol.split('/')[0] }}
      </div>
    </div>
    <div v-if="step == 2">
      <div class="d-flex justify-content-between align-items-center ws-filter-tab mb-2 "
        style="padding-top: 3px !important; padding-bottom:3px !important;">
        <span @click="step = 1">
          <b-icon icon="chevron-left" variant="light"></b-icon>
        </span>
        <div class="d-flex col py-2 justify-content-center align-items-center">
          <span> {{$t('appmain.ChooseAccount ')}}</span>
        </div>
      </div>
      <div v-for="item in walletOneList" @click="wswalletOne(item)" style="background-color: #1A212B;" class="px-3 py-2">
        {{ item }}
      </div>
    </div>

    <div v-if="step == 0">
      <div v-for="(items,key) in tableMoneyShow" class="d-flex flex-column "
        style=" border-bottom: 1px solid #27313e6e !important;">
        <div class="py-2 d-flex w-100 flex-column">
          <span class="fw-500">{{items.symbol}}  {{$t('appmain.Futures ')}}</span>
          <div class="row mx-0 pt-2 pb-2">
            <div class="col-3 px-0 d-flex flex-column align-items-center">
              <span class="fs-8 text-center text-faded-small"> {{$t('appmain.Accountequity ')}}</span>
              <span class="fs-9 text-center">{{ items.usdtBalance.toFixed(5) }}</span>
            </div>
            <div class="col-3 px-0 d-flex flex-column align-items-center">
              <span class="fs-8 text-center text-faded-small"> {{$t('appmain.Availablemargin')}}</span>
              <span class="fs-9 text-center">{{ items.avaBalance.toFixed(5) }}</span>
            </div>
            <div class="col-3 px-0 d-flex flex-column align-items-center">
              <span class="fs-8 text-center text-faded-small">{{$t('appmain.Usedmargin ')}}</span>
              <span class="fs-9 text-center">{{ items.coinBalance.toFixed(5) }}</span>
            </div>
            <div class="col-3 px-0 d-flex flex-column align-items-center">
              <span class="fs-8 text-center text-faded-small"> {{$t('appmain.Freezemargin ')}}</span>
              <span class="fs-9 text-center">{{ items.coinFrozenBalance.toFixed(5) }}</span>
            </div>
          </div>
        </div>
      </div>
      <div v-if="tableMoneyShow.length < 0"
        class="py-4 w-100 d-flex justify-content-center align-items fs-8 text-faded-small">
        {{$t('common.nodata ')}}
      </div>
    </div>

  </div>

</template>
<script>
  export default {
    components: {},
    data() {
      return {
        wallet1symbol:"",
        step: 0,
        loginmsg: this.$t("common.logintip"),
        loading: true,
        ordKeyword: "",
        tableMoney: [],
        tableMoneyShow: [],
        searchKey: "",
        transferModal: false,
        transferDirection: 1,
        walletOne: null,
        walletTwo: null,
        transferAmount: 0,
        predictAmount: 0,
        walletTwoList: [],
        walletOneList: [this.$t('uc.finance.swap.currencyaccount') + '(USDT)'],
        assetsWallet: {
          id: 0,
          balance: 0
        },
        swapWallet: {
          id: 0,
          avaBalance: 0
        },
      };
    },
    methods: {
      wswalletOne(wsid){
        this.walletOne = wsid
        this.step = 1
      },
      wswalletTwo(wsid,wssymbol){
        this.walletTwo = wsid
        this.wallet1symbol = wssymbol.split('/')[0]
        this.onChangeTwo()
        this.step = 1
      },
      seachInputChange() {
        this.tableMoneyShow = this.tableMoney.filter(item => item["symbol"].indexOf(this.searchKey) == 0);
      },
      getMoney() {
        //获取
        this.$http.post(this.host + "/swap/wallet/list").then(response => {
          var resp = response.body;
          if (resp.code == 0) {
            this.tableMoney = resp.data;
            for (let i = 0; i < this.tableMoney.length; i++) {
              this.tableMoney[i]["symbol"] = this.tableMoney[i].contractCoin.symbol;
              // 如果是全仓并且收益小于0
              if (this.tableMoney[i].usdtTotalProfitAndLoss < 0 && this.tableMoney[i].usdtPattern == "CROSSED") {
                this.tableMoney[i]["avaBalance"] = this.tableMoney[i].usdtBalance + this.tableMoney[i].usdtTotalProfitAndLoss;
              } else {
                this.tableMoney[i]["avaBalance"] = this.tableMoney[i].usdtBalance;
              }

              if (this.swapWallet.id == this.tableMoney[i].id) {
                this.swapWallet = this.tableMoney[i];
              }
            }
            this.loading = false;
            this.tableMoneyShow = this.tableMoney;
            console.log(this.tableMoneyShow);
          } else {
            this.$Message.error(this.loginmsg);
          }
        });
      },
      getAssets() {
        //获取
        this.$http.post(this.host + "/uc/asset/wallet/usdt").then(response => {
          var resp = response.body;
          if (resp.code == 0) {
            this.assetsWallet = resp.data;
          } else {
            this.$Message.error(this.loginmsg);
          }
        });
      },
      onTransferClick() {
        this.transferModal = true;
      },
      changeTo(val) {
        this.transferDirection = val;
        this.transferAmount = 0.00;
      },
      confirmOk() {
        if (this.assetsWallet.id == 0 || this.swapWallet.id == 0) {
          this.$Message.error(this.$t('uc.finance.swap.pleaseselectwallet'));
          return;
        }
        if (this.transferAmount <= 0 || this.transferAmount == "" || this.transferAmount == undefined) {
          this.$Message.error(this.$t('uc.finance.swap.pleaseinputamount'));
          return;
        }
        let params = {
          unit: "USDT",
          from: this.transferDirection == 1 ? 0 : 1,
          to: this.transferDirection == 1 ? 1 : 0,
          fromWalletId: this.transferDirection == 1 ? this.assetsWallet.id : this.swapWallet.id,
          toWalletId: this.transferDirection == 1 ? this.swapWallet.id : this.assetsWallet.id,
          amount: this.transferAmount
        };

        this.$http.post(this.host + "/swap/wallet/trans", params).then(response => {
          var resp = response.body;
          if (resp.code == 0) {
            console.log(resp.data);
            this.getMoney();
            this.getAssets();
            this.onChangeTwo();
          } else {
            this.$Message.error(this.loginmsg);
          }
        });

        this.transferModal = false;
      },
      onChangeTwo() {
        console.log(this.walletTwo);
        for (let i = 0; i < this.tableMoney.length; i++) {
          if (this.tableMoney[i].id == this.walletTwo) {
            this.swapWallet = this.tableMoney[i];
          }
        }
      },
      onTransAll() {
        if (this.transferDirection == 1) {
          this.transferAmount = this.assetsWallet.balance;
        } else {
          this.transferAmount = this.swapWallet.avaBalance;
        }
      }
    },
    created() {
      this.getMoney();
      this.getAssets();

      let wspath = this.$route.query.show || "";

      if (wspath == "transfer") {
        this.step = 1
      }
      else {
        this.step = 0
      }

    },
    filters: {
      fixed2: function (value) {
        return value.toFixed(2);
      }
    },
    computed: {
      totalUSDT() {
        let usdtTotal = 0;
        for (let i = 0; i < this.tableMoney.length; i++) {
          usdtTotal += this.tableMoney[i].usdtBalance + this.tableMoney[i].usdtFrozenBalance + this.tableMoney[i].usdtBuyPrincipalAmount + this.tableMoney[i].usdtSellPrincipalAmount + this.tableMoney[i].usdtTotalProfitAndLoss;
        }
        return usdtTotal.toFixed(2);
      },
      totalCny() {
        let cnyTotal = 0;
        for (let i = 0; i < this.tableMoney.length; i++) {
          var sumV = this.tableMoney[i].usdtBalance + this.tableMoney[i].usdtFrozenBalance + this.tableMoney[i].usdtBuyPrincipalAmount + this.tableMoney[i].usdtSellPrincipalAmount + this.tableMoney[i].usdtTotalProfitAndLoss;

          cnyTotal += sumV * this.tableMoney[i].contractCoin.usdtRate;
        }

        return cnyTotal.toFixed(2);
      },
      tableColumnsMoney() {
        let self = this;
        let columns = [];
        columns.push({
          title: this.$t("uc.finance.swap.swaptype"),
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.symbol
                }
              },
              params.row.symbol.replace("/", "") + " " + self.$t("uc.finance.swap.swap"),
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.swap.swapassets"),
          key: "balance",
          align: "center",
          render(h, params) {
            return h(
              "span", {},
              (params.row.usdtBalance + params.row.usdtFrozenBalance + params.row.usdtBuyPrincipalAmount + params.row.usdtSellPrincipalAmount + params.row.usdtTotalProfitAndLoss).toFixed(4)
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.swap.profitandloss"),
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.usdtTotalProfitAndLoss
                }
              },
              self.toFloor(params.row.usdtTotalProfitAndLoss.toFixed(4) || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.swap.avabalance"),
          key: "usdtBalance",
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.usdtBalance
                }
              },
              self.toFloor(params.row.usdtBalance.toFixed(4) || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.swap.usebalance"),
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.toReleased
                }
              },
              (params.row.usdtBuyPrincipalAmount + params.row.usdtSellPrincipalAmount).toFixed(4)
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.swap.frozenbalance"),
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.usdtFrozenBalance
                }
              },
              self.toFloor(params.row.usdtFrozenBalance.toFixed(4) || "0")
            );
          }
        });
        return columns;
      }
    }
  };
</script>