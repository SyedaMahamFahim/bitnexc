<template>
  <div>
    <div v-if="0" class="nav-rights">
      <div class="nav-right">
        <div class="bill_flow_box">
          <div class="rightarea-con">
            <div class="form-group">
              <span>
                {{$t('uc.finance.record.start_end')}} ：
              </span>
              <DatePicker v-model="rangeDate" @on-change="changedate" format="yyyy-MM-dd" type="daterange"
                style="width: 200px;margin-right:30px;" @on-clear="clear"></DatePicker>
              <!--<DatePicker v-model="startDate" type="date"></DatePicker>-->
              <!--<span>-->
              <!--{{$t('uc.finance.record.to')}}-->
              <!--</span>-->
              <!--<DatePicker v-model="endDate" type="date"></DatePicker>-->
              <span>{{$t('uc.finance.record.symbol')}} ：</span>
              <Select v-model="coinType" style="width:100px;margin-right:30px;" @on-change="getAddrList" clearable
                :placeholder="$t('common.pleaseselect')">
                <Option v-for="item in coinList" :value="item.unit" :key="item.unit">{{ item.unit }}</Option>
              </Select>
              <span>
                {{$t('uc.finance.record.operatetype')}} ：
              </span>
              <Select v-model="recordValue" clearable style="width:200px" @on-change="getType"
                :placeholder="$t('common.pleaseselect')">
                <Option v-for="item in recordType" :value="item.value" :key="item.value">{{ item.label }}</Option>
              </Select>
              <Button type="warning" @click="queryOrder"
                style="padding: 6px 30px;margin-left:10px;background-color:#f0a70a;border-color:#f0a70a">{{$t('uc.finance.record.search')}}</Button>

            </div>
            <div class="order-table">
              <Table :no-data-text="$t('common.nodata')" :columns="tableColumnsRecord" :data="tableRecord"
                :disabled-hover="true" :loading="loading"></Table>
              <div style="margin: 10px;overflow: hidden">
                <div style="float: right;">
                  <Page :total="total" :pageSize="pageSize" show-total :current="page" @on-change="changePage"
                    id="record_pages"></Page>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="d-flex justify-content-between align-items-center ws-filter-tab "
      style="padding-top: 3px !important; padding-bottom:3px !important;">
      <span @click="$router.go(-1)">
        <b-icon icon="chevron-left" variant="light"></b-icon>
      </span>
      <div class="d-flex col py-2 justify-content-center align-items-center">
        <span>{{$t('appmain.Funding')}}</span>
      </div>
      <div @click="step == 0 ?  step = 1  : step = 0  " class="px-2">
        <b-icon icon="filter-circle" variant="light"></b-icon>
      </div>
    </div>
    <div v-if="step == 0">
      <div v-for="items in tableRecord" class=" p-3" style="border-bottom:1px solid #1A212B;">
        <div class="d-flex justify-content-between align-items-center">
          <span>{{items.symbol}}</span>
          <span>{{items.flag == 0 ? "Finished" :"-"}}</span>
        </div>
        <div class="row mx-0 pt-2">
          <div class="col-4 px-0 d-flex flex-column align-items-start">
            <span class="fs-8 text-center" style="opacity:.5;">{{$t('appmain.Time')}}</span>
            <span class="fs-8 text-center">{{items.createTime.split(':')[0]+":"+items.createTime.split(':')[1]}}</span>
          </div>
          <div class="col-4 px-0 d-flex flex-column align-items-center">
            <span class="fs-8 text-center" style="opacity:.5;">{{$t('appmain.Type')}}</span>
            <span v-if="items.type == 0" class="fs-8 text-center">{{$t('record.charge')}}</span>
            <span v-else-if="items.type == 1" class="fs-8 text-center">{{$t('record.pickup')}}</span>
            <span v-else-if="items.type == 2" class="fs-8 text-center">{{$t('record.transaccount')}}</span>
            <span v-else-if="items.type == 3" class="fs-8 text-center">{{$t('record.exchange')}}</span>
            <span v-else-if="items.type == 4" class="fs-8 text-center">{{$t('record.otcbuy')}}</span>
            <span v-else-if="items.type == 5" class="fs-8 text-center">{{$t('record.otcsell')}}</span>
            <span v-else-if="items.type == 6" class="fs-8 text-center">{{$t('record.activityaward')}}</span>
            <span v-else-if="items.type == 7" class="fs-8 text-center">{{$t('record.promotionaward')}}</span>
            <span v-else-if="items.type == 8" class="fs-8 text-center">{{$t('record.dividend')}}</span>
            <span v-else-if="items.type == 9" class="fs-8 text-center">{{$t('record.vote')}}</span>
            <span v-else-if="items.type == 10" class="fs-8 text-center">{{$t('record.handrecharge')}}</span>
            <span v-else-if="items.type == 12" class="fs-8 text-center">{{$t('record.match')}}</span>
            <span v-else-if="items.type == 13" class="fs-8 text-center">{{$t('record.activitybuy')}}</span>
            <span v-else-if="items.type == 14" class="fs-8 text-center">{{$t('record.ctcbuy')}}</span>
            <span v-else-if="items.type == 15" class="fs-8 text-center">{{$t('record.ctcsell')}}</span>
            <span v-else-if="items.type == 16" class="fs-8 text-center">{{$t('record.redout')}}</span>
            <span v-else-if="items.type == 17" class="fs-8 text-center">{{$t('record.redin')}}</span>
            <span v-else-if="items.type == 18" class="fs-8 text-center">{{$t('record.withdrawcodeout')}}</span>
            <span v-else-if="items.type == 19" class="fs-8 text-center">{{$t('record.withdrawcodein')}}</span>
            <span v-else-if="items.type == 21" class="fs-8 text-center">{{$t('record.contractfee')}}</span>
            <span v-else-if="items.type == 22" class="fs-8 text-center">{{$t('record.contractprofit')}}</span>
            <span v-else-if="items.type == 23" class="fs-8 text-center">{{$t('record.contractloss')}}</span>
            <span v-else-if="items.type == 24" class="fs-8 text-center">{{$t('record.optionfail')}}</span>
            <span v-else-if="items.type == 25" class="fs-8 text-center">{{$t('record.optionfee')}}</span>
            <span v-else-if="items.type == 26" class="fs-8 text-center">{{$t('record.optionreward')}}</span>
            <span v-else class="fs-8 text`-center">-</span>
          </div>
          <div class="col-4 px-0 d-flex flex-column align-items-end">
            <span class="fs-8 text-center" style="opacity:.5;">{{$t('appmain.Amount')}}</span>
            <span class="fs-8 text-center">{{items.amount}}</span>
          </div>
        </div>
        <div class="row mx-0 pt-2">
          <div class="col-4 px-0 d-flex flex-column align-items-start">
            <span class="fs-8 text-center" style="opacity:.5;">{{$t('appmain.ProcessingFees')}}</span>
            <span class="fs-8 text-center">{{items.fee}}</span>
          </div>
          <div class="col-4 px-0 d-flex flex-column align-items-center">
            <span class="fs-8 text-center" style="opacity:.5;">{{$t('appmain.FeesDeduction')}}</span>
            <span class="fs-8 text-center">{{items.discountFee}}</span>
          </div>
          <div class="col-4 px-0 d-flex flex-column align-items-end">
            <span class="fs-8 text-center" style="opacity:.5;">{{$t('appmain.ActualFees')}}</span>
            <span class="fs-8 text-center">{{ parseFloat(items.realFee).toFixed(7) }}</span>
          </div>
        </div>
      </div>

      <div class=" py-4 ws-fund-page d-flex justify-content-center align-items-center">
        <Page :total="total" :pageSize="pageSize" show-total :current="page" @on-change="changePage" id="record_pages">
        </Page>
      </div>

    </div>
    <div v-if="step == 1">
      <div class="ws-filter-form ws-filter-funding-form p-3 d-flex flex-column">
        <span class="fs-7 pb-2 ">
          {{$t('uc.finance.record.start_end')}}
        </span>
        <DatePicker v-model="rangeDate" @on-change="changedate" format="yyyy-MM-dd" type="daterange"
          style="width: 200px;margin-right:30px;" @on-clear="clear"></DatePicker>

        <span class="fs-7 pb-2 pt-3">{{$t('uc.finance.record.symbol')}}</span>
        <Select v-model="coinType" style="width:100px;margin-right:30px;" @on-change="getAddrList" clearable
          :placeholder="$t('common.pleaseselect')">
          <Option v-for="item in coinList" :value="item.unit" :key="item.unit">{{ item.unit }}</Option>
        </Select>
        <span class="fs-7 pb-2 pt-3">
          {{$t('uc.finance.record.operatetype')}}
        </span>
        <Select v-model="recordValue" clearable style="width:200px" @on-change="getType"
          :placeholder="$t('common.pleaseselect')">
          <Option v-for="item in recordType" :value="item.value" :key="item.value">{{ item.label }}</Option>
        </Select>
        <Button type="warning" @click="queryOrder"
          style="padding: 6px 30px;margin-left:10px;background-color:#f0a70a;border-color:#f0a70a">{{$t('uc.finance.record.search')}}</Button>

      </div>
    </div>



  </div>
</template>
<script>
  export default {
    components: {},
    data() {
      return {
        step: 0,
        loading: false,
        ordKeyword: "",
        rangeDate: "",
        startTime: "",
        endTime: "",
        recordValue: "",
        recordType: [
          {
            value: 0,
            label: this.$t("uc.finance.record.charge")
          },
          {
            value: 1,
            label: this.$t("uc.finance.record.pickup")
          },
          {
            value: 2,
            label: this.$t("uc.finance.record.transaccount")
          },
          {
            value: 3,
            label: this.$t("uc.finance.record.exchange")
          },
          {
            value: 4,
            label: this.$t("uc.finance.record.otcbuy")
          },
          {
            value: 5,
            label: this.$t("uc.finance.record.otcsell")
          },
          {
            value: 6,
            label: this.$t("uc.finance.record.activityaward")
          },
          {
            value: 7,
            label: this.$t("uc.finance.record.promotionaward")
          },
          {
            value: 8,
            label: this.$t("uc.finance.record.dividend")
          },
          {
            value: 9,
            label: this.$t("uc.finance.record.vote")
          },
          {
            value: 10,
            label: this.$t("uc.finance.record.handrecharge")
          },
          {
            value: 11,
            label: this.$t("uc.finance.record.match")
          },
          {
            value: 12,
            label: this.$t("uc.finance.record.activitybuy")
          },
          {
            value: 13,
            label: this.$t("uc.finance.record.ctcbuy")
          },
          {
            value: 14,
            label: this.$t("uc.finance.record.ctcsell")
          },
          {
            value: 15,
            label: this.$t("uc.finance.record.redout")
          },
          {
            value: 16,
            label: this.$t("uc.finance.record.redin")
          },
          {
            value: 17,
            label: this.$t("uc.finance.record.withdrawcodeout")
          },
          {
            value: 18,
            label: this.$t("uc.finance.record.withdrawcodein")
          },
          {
            value: 19,
            label: this.$t("uc.finance.record.contractfee")
          },
          {
            value: 20,
            label: this.$t("uc.finance.record.contractprofit")
          },
          {
            value: 21,
            label: this.$t("uc.finance.record.contractloss")
          },
          {
            value: 22,
            label: this.$t("uc.finance.record.optionfail")
          },
          {
            value: 23,
            label: this.$t("uc.finance.record.optionfee")
          },
          {
            value: 24,
            label: this.$t("uc.finance.record.optionreward")
          }
        ],
        coinList: [],
        coinType: "",
        pageSize: 15,
        page: 1,
        total: 0,
        tableRecord: []
      };
    },
    created: function () {
      this.getList(this.page);
      this.getAddrList();
    },
    methods: {
      changedate() {
        if (this.rangeDate[0]) {
          this.startTime = this.dateform(this.rangeDate[0]);
          this.endTime = this.dateform(this.rangeDate[1]);
        }
      },
      changePage(pageindex) {
        this.page = pageindex;
        this.getList(this.page);
      },
      queryOrder() {
        this.step = 0
        if (this.rangeDate.length == 0) {
          this.$Message.error("请选择搜索日期范围");
          return;
        } else {
          try {
            this.page = 1;
            this.getList(this.page);
          } catch (ex) {
            this.$Message.error("请选择搜索日期范围");
            return;
          }
        }
      },
      getAddrList() {
        //获取地址
        this.$http.post(this.host + "/uc/withdraw/support/coin/info").then(response => {
          var resp = response.body;
          if (resp.code == 0 && resp.data.length > 0) {
            this.coinList = resp.data;
            if (this.coinType) {
              this.coinType = this.coinType;
            }
          } else {
            this.$Message.error(resp.message);
          }
        });
      },
      getType() {
        // console.log(this.recordValue);
      },
      dateform(time) {
        var date = new Date(time);
        var y = date.getFullYear();
        var m = date.getMonth() + 1;
        m = m < 10 ? "0" + m : m;
        var d = date.getDate();
        d = d < 10 ? "0" + d : d;
        var h = date.getHours();
        h = h < 10 ? "0" + h : h;
        var minute = date.getMinutes();
        var second = date.getSeconds();
        minute = minute < 10 ? "0" + minute : minute;
        second = second < 10 ? "0" + second : second;
        return y + "-" + m + "-" + d + " " + h + ":" + minute + ":" + second;
      },
      getList(pageNo) {
        //获取tableWithdraw
        let memberId = 0;
        !this.$store.getters.isLogin && this.$router.push("/login");
        this.$store.getters.isLogin && (memberId = this.$store.getters.member.id);
        let startTime = "";
        let endTime = "";
        let symbol = "";
        let type = "";
        this.startTime && (startTime = this.startTime);
        this.endTime && (endTime = this.endTime);
        this.coinType && (symbol = this.coinType);
        if (this.recordValue == 0 || this.recordValue) {
          type = this.recordValue;
        }
        // this.recordValue!="" || this.recordValue!=undefined && (type = this.recordValue);
        this.loading = true;
        let params = {
          pageNo: pageNo,
          pageSize: this.pageSize,
          startTime,
          endTime,
          memberId,
          symbol,
          type
        };
        this.$http.post(this.host + "/uc/asset/transaction/all", params).then(response => {
          var resp = response.body;
          if (resp.code == 0) {
            this.loading = false;
            if (resp.data) {
              let trueData = resp.data;
              this.total = trueData.totalElements;
              this.tableRecord = trueData.content;
            }
          } else {
            this.$Message.error(resp.message);
          }
          console.log(this.tableRecord);
          this.loading = false;
        });
      },
      clear() {
        this.startTime = "";
        this.endTime = "";
      }
    },
    computed: {
      tableColumnsRecord() {
        let columns = [];
        var that = this;
        columns.push({
          title: this.$t("uc.finance.record.chargetime"),
          align: "center",
          width: 160,
          key: "createTime"
        });
        columns.push({
          title: this.$t("uc.finance.record.type"),
          render: function (h, params) {
            let str = "";
            let type = params.row.type;
            if (type == 0) {
              str = that.$t("uc.finance.record.charge");
            } else if (type == 1) {
              str = that.$t("uc.finance.record.pickup");
            } else if (type == 2) {
              str = that.$t("uc.finance.record.transaccount");
            } else if (type == 3) {
              str = that.$t("uc.finance.record.exchange");
            } else if (type == 4) {
              str = that.$t("uc.finance.record.otcbuy");
            } else if (type == 5) {
              str = that.$t("uc.finance.record.otcsell");
            } else if (type == 6) {
              str = that.$t("uc.finance.record.activityaward");
            } else if (type == 7) {
              str = that.$t("uc.finance.record.promotionaward");
            } else if (type == 8) {
              str = that.$t("uc.finance.record.dividend");
            } else if (type == 9) {
              str = that.$t("uc.finance.record.vote");
            } else if (type == 10) {
              str = that.$t("uc.finance.record.handrecharge");
            } else if (type == 11) {
              str = that.$t("uc.finance.record.match");
            } else if (type == 12) {
              str = that.$t("uc.finance.record.activitybuy");
            } else if (type == 13) {
              str = that.$t("uc.finance.record.ctcbuy");
            } else if (type == 14) {
              str = that.$t("uc.finance.record.ctcsell");
            } else if (type == 15) {
              str = that.$t("uc.finance.record.redout");
            } else if (type == 16) {
              str = that.$t("uc.finance.record.redin");
            } else if (type == 17) {
              str = that.$t("uc.finance.record.withdrawcodeout");
            } else if (type == 18) {
              str = that.$t("uc.finance.record.withdrawcodein");
            } else if (type == 19) {
              str = that.$t("uc.finance.record.contractfee");
            } else if (type == 20) {
              str = that.$t("uc.finance.record.contractprofit");
            } else if (type == 21) {
              str = that.$t("uc.finance.record.contractloss");
            } else if (type == 22) {
              str = that.$t("uc.finance.record.optionfail");
            } else if (type == 23) {
              str = that.$t("uc.finance.record.optionfee");
            } else if (type == 24) {
              str = that.$t("uc.finance.record.optionreward");
            } else {
              str = that.$t("uc.finance.record.other");
            }
            return h("div", str, "");
          }
        });
        columns.push({
          title: this.$t("uc.finance.record.symbol"),
          align: "center",
          key: "symbol"
          // render: (h, param) => {
          //   return h("div", {}, param.row._source.symbol);
          // }
        });
        columns.push({
          // title: this.$t("uc.finance.record.num"),
          title: this.$t("uc.finance.record.num"), //到账数量
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.amount
                }
              },
              that.toFloor(params.row.amount || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.record.shouldfee"), //"应付手续费"
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.fee
                }
              },
              that.toFloor(params.row.fee || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.record.discountfee"), //"抵扣手续费"
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.discountFee
                }
              },
              that.toFloor(params.row.discountFee || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.record.realfee"), //"实际手续费"
          align: "center",
          render(h, params) {
            return h(
              "span",
              {
                attrs: {
                  title: params.row.realFee
                }
              },
              that.toFloor(params.row.realFee || "0")
            );
          }
        });
        columns.push({
          title: this.$t("uc.finance.record.status"),
          // key: "status",
          align: "center",
          render: (h, params) => {
            return h("div", that.$t("uc.finance.record.finish"), "");
          }
        });
        return columns;
      }
    }
  };
</script>
<style scoped lang="scss">
  .nav-rights {
    .nav-right {
      height: auto;
      overflow: hidden;
      padding: 0 15px;

      .bill_flow_box .rightarea-con {
        .form-group {
          margin-bottom: 20px;
          text-align: left;
        }
      }
    }
  }
</style>