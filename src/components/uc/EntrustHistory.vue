<template>
  <div>
    <!-- <Form class="form" :model="formItem" :label-width="75" inline>
      <FormItem :label="$t('uc.finance.trade.start_end')">
        <DatePicker type="daterange" v-model="formItem.date" style="width:180px;"></DatePicker>
      </FormItem>
      <FormItem :label="$t('uc.finance.trade.symbol')">
        <Select v-model="formItem.symbol" style="width:100px;" :placeholder="$t('common.pleaseselect')">
          <Option v-for="(item,index) in symbol" :key="index" :value="item.symbol">{{item.symbol}}</Option>
        </Select>
      </FormItem>
      <FormItem :label="$t('uc.finance.trade.type')">
        <Select v-model="formItem.type" style="width:70px;" :placeholder="$t('common.pleaseselect')">
          <Option value="LIMIT_PRICE">{{$t('uc.finance.trade.limit')}}</Option>
          <Option value="MARKET_PRICE">{{$t('uc.finance.trade.market')}}</Option>
        </Select>
      </FormItem>
      <FormItem :label="$t('uc.finance.trade.direction')">
        <Select v-model="formItem.direction" style="width:70px;" :placeholder="$t('common.pleaseselect')">
          <Option value="0">{{$t('uc.finance.trade.buy')}}</Option>
          <Option value="1">{{$t('uc.finance.trade.sell')}}</Option>
        </Select>
      </FormItem>
      <FormItem>
        <Button type="warning" @click="handleSubmit">{{$t('uc.finance.trade.search')}}</Button>
        <Button style="margin-left: 8px" @click="handleClear" class="clear_btn">{{$t('uc.finance.trade.clear')}}</Button>
      </FormItem>
    </Form>
    <div class="table">
      <Table :no-data-text="$t('common.nodata')" :columns="columns" :data="orders" :loading="loading"></Table>
      <div class="page">
        <Page :total="total" :pageSize="pageSize" :current="pageNo" @on-change="loadDataPage"></Page>
      </div>
    </div> -->
    <div class="d-flex justify-content-between align-items-center ws-filter-tab ">
      <span @click="$router.go(-1)">
        <b-icon icon="chevron-left" variant="light"></b-icon>
      </span>
      <div class="d-flex align-items-center">
        <router-link class="ws-filter-links" to="/uc/entrust/current">{{$t('appmain.Order')}}</router-link>
        <router-link class="ws-filter-links" to="/uc/entrust/history">{{$t('appmain.OrderHistory')}}</router-link>
      </div>
      <span @click="isFilterOpen == true ? isFilterOpen = false : isFilterOpen = true ">
        <b-icon icon="filter-circle" variant="light"></b-icon>
      </span>
    </div>
    <div v-if="isFilterOpen == false">
      <div class="row ws-ex-tr w-100 py-2 mx-0" v-for=" (item, index) in orders">
        <div class="d-flex align-items-center justify-content-between py-1">
          <div class="d-flex align-items-center">
            <span v-bind:class="[orders[index].direction == 'SELL' ? 'text-danger' : 'text-success']"
              style="font-size:12px; text-transform: capitalize;">{{
              orders[index].direction.toLowerCase() }}</span>
            <span class="ps-3" style="font-size:12px;">{{ orders[index].symbol }}</span>
          </div>
          <span style=" font-size: 12px;">{{$t('appmain.Deal')}}</span>
        </div>
        <div class="row mx-0">
          <div class="col-4 pb-1 px-0 text-faded-small text-start">{{$t('appmain.Time')}}</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-center">{{$t('appmain.Type')}}</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-end">{{$t('appmain.Price')}}({{
            orders[index].baseSymbol }})</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-start">{{ orders[index].time }}
          </div>
          <div class="col-4 pb-1 px-0 text-faded-small text-center">
            <template v-if="orders[index].type == 'LIMIT_PRICE'">
              {{$t('appmain.Limit')}}
            </template>
            <template v-if="orders[index].type != 'LIMIT_PRICE'">
              {{$t('appmain.Market')}}
            </template>
          </div>
          <div class="col-4 pb-1 px-0 text-faded-small text-end">{{ orders[index].price }}
          </div>
          <div class="col-4 pb-1 px-0 text-faded-small text-start">{{$t('appmain.Amount')}}({{
            orders[index].coinSymbol }})</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-center">{{$t('appmain.Deal')}}/{{$t('appmain.Fee')}}</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-end">{{$t('appmain.Amount')}}</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-start">{{ orders[index].amount.toFixed(2)
            }}</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-center">{{
            orders[index].fee.toFixed(4) }}</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-end">{{ orders[index].amount.toFixed(2)
            }}</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-start">{{$t('appmain.Fee')}}({{ orders[index].direction == 'SELL' ?
            orders[index].baseSymbol : orders[index].coinSymbol }})</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-center">{{$t('appmain.AveragePrice')}}({{
            orders[index].baseSymbol }})</div>
          <div class="col-4 pb-1 px-0 text-faded-small text-end"></div>
          <div class="col-4 pb-1 px-0 text-faded-small text-start">{{ orders[index].fee }}
          </div>
          <div class="col-4 pb-1 px-0 text-faded-small text-center">{{ orders[index].price
            }}</div>
        </div>
      </div>
      <div class="w-100 d-flex justify-content-center align-items-center py-4" v-if="orders.length == 0">
        <span class="w-100 text-center" style="font-size: 12px; opacity:.7;">{{$t('common.nodata')}}</span>
      </div>
    </div>
    <div class="ws-pagee d-flex justify-content-center py-4 ">
      <Page :total="total" :pageSize="pageSize" :current="pageNo" @on-change="loadDataPage"></Page>
    </div>

    <div v-if="isFilterOpen == true" class="d-flex flex-column ws-filter-form py-2 px-2">
      <Form class="d-flex flex-column" :model="formItem" :label-width="75" inline>
        <FormItem :label="'Pair'">
          <Select v-model="formItem.symbol" style="width:100px;" :placeholder="$t('common.pleaseselect')">
            <Option v-for="(item,index) in symbol" :key="index" :value="item.symbol">{{item.symbol}}</Option>
          </Select>
        </FormItem>
        <FormItem :label="'Type'">
          <Select v-model="formItem.type" style="width:70px;" :placeholder="$t('common.pleaseselect')">
            <Option value="LIMIT_PRICE">{{$t('uc.finance.trade.limit')}}</Option>
            <Option value="MARKET_PRICE">{{$t('uc.finance.trade.market')}}</Option>
          </Select>
        </FormItem>
        <FormItem :label="'Side'">
          <Select v-model="formItem.direction" style="width:70px;" :placeholder="$t('common.pleaseselect')">
            <Option value="0">{{$t('uc.finance.trade.buy')}}</Option>
            <Option value="1">{{$t('uc.finance.trade.sell')}}</Option>
          </Select>
        </FormItem>
        <FormItem :label="'Start and end time'">
          <DatePicker type="daterange" v-model="formItem.date" style="width:180px;"></DatePicker>
        </FormItem>
        <div class="w-100" @click="isFilterOpen = false ">
          <Button class="w-100 mt-2 d-flex justify-content-center py-2" type="warning"
            @click="handleSubmit">{{$t('uc.finance.trade.search')}}</Button>
        </div>
      </Form>
    </div>
  </div>

</template>
<script>
  import { accDiv } from "../../assets/js/decimal";

  var moment = require("moment");
  import expandRow from "@components/exchange/expand.vue";

  export default {
    components: { expandRow },
    data() {
      const self = this;
      return {
        isFilterOpen: false,
        loading: false,
        pageSize: 10,
        pageNo: 1,
        total: 10,
        symbol: [],
        formItem: {
          symbol: "",
          type: "",
          direction: "",
          date: ""
        },
        columns: [
          {
            type: "index",
            width: 30,
            render: (h, params) => {
              return h(expandRow, {
                props: {
                  skin: params.row.skin,
                  rows: params.row.detail
                }
              });
            }
          },
          {
            title: self.$t("exchange.time"),
            key: "time",
            minWidth: 55,
            render: (h, params) => {
              return h("span", {}, self.dateFormat(params.row.time));
            }
          },
          {
            title: self.$t("uc.finance.trade.symbol"),
            width: 130,
            key: "symbol"
          },
          {
            title: self.$t("uc.finance.trade.type"),
            width: 70,
            render(h, params) {
              return h(
                "span",
                params.row.type === "LIMIT_PRICE" ? self.$t("exchange.limited_price") : self.$t("exchange.market_price")
              );
            }
          },
          {
            title: self.$t("exchange.direction"),
            key: "direction",
            width: 90,
            render: (h, params) => {
              const row = params.row;
              const className = row.direction.toLowerCase();
              return h(
                "span",
                {
                  attrs: {
                    class: className
                  }
                },
                row.direction == "BUY"
                  ? self.$t("exchange.buyin")
                  : self.$t("exchange.sellout")
              );
            }
          },
          {
            title: self.$t("exchange.price"),
            key: "price",
            render(h, params) {
              return h(
                "span",
                {
                  attrs: {
                    title: params.row.price
                  }
                },
                self.toFloor(params.row.price)
              );
            }
          },
          {
            title: self.$t("exchange.num"),
            key: "amount",
            render(h, params) {
              return h(
                "span",
                {
                  attrs: {
                    title: params.row.amount
                  }
                },
                self.toFloor(params.row.amount)
              );
            }
          },
          {
            title: self.$t("exchange.done"),
            key: "tradedAmount",
            render(h, params) {
              return h(
                "span",
                {
                  attrs: {
                    title: params.row.tradedAmount
                  }
                },
                self.toFloor(params.row.tradedAmount)
              );
            }
          },
          {
            title: self.$t("uc.finance.trade.turnover"),
            key: "turnover",
            render(h, params) {
              return h(
                "span",
                {
                  attrs: {
                    title: params.row.turnover
                  }
                },
                self.toFloor(params.row.turnover)
              );
            }
          },
          {
            title: self.$t("exchange.fee"),
            key: "turnover",
            render(h, params) {
              return h("span", params.row.fee ? self.toFloor(params.row.fee) : '--');
            }
          },
          {
            title: self.$t("exchange.avgPrice"),
            key: "turnover",
            render(h, params) {
              return h("span", !params.row.turnover || !params.row.tradedAmount || params.row.tradedAmount <= 0 ? '--' : new Number(self.toFloor(accDiv(self.toFloor(params.row.turnover), self.toFloor(params.row.tradedAmount)))).toFixed(6));
            }
          },
          {
            title: self.$t("exchange.status"),
            key: "status",
            width: 80,
            render: (h, params) => {
              const status = params.row.status;
              if (status == "COMPLETED") {
                return h(
                  "span",
                  {
                    style: {
                      color: "#f0a70a"
                    }
                  },
                  self.$t("exchange.finished")
                );
              } else if (status == "CANCELED") {
                return h(
                  "span",
                  {
                    style: {
                      color: "#f0a70a"
                    }
                  },
                  self.$t("exchange.canceled")
                );
              } else {
                return h("span", {}, "--");
              }
            }
          }
        ],
        orders: []
      };
    },
    computed: {
      lang: function () {
        return this.$store.state.lang;
      }
    },
    watch: {
      lang: function () {
        this.updateLangData();
      }
    },
    created() {
      this.getHistoryOrder();
      this.getSymbol();
    },
    methods: {
      dateFormat: function (tick) {
        return moment(tick).format("YYYY-MM-DD HH:mm:ss");
      },
      loadDataPage(data) {
        this.pageNo = data;
        this.getHistoryOrder();
      },
      handleSubmit() {
        this.pageNo = 1;
        this.getHistoryOrder();
      },
      handleClear() {
        this.formItem = {
          symbol: "",
          type: "",
          direction: "",
          date: ""
        };
      },
      getHistoryOrder() {
        //查询历史委托
        this.loading = true;
        const { symbol, type, direction, date: rangeDate } = this.formItem,
          startTime = new Date(rangeDate[0]).getTime() || "",
          endTime = new Date(rangeDate[1]).getTime() || "";
        let params = {};
        if (symbol) params.symbol = symbol;
        if (direction) params.direction = direction;
        if (type) params.type = type;
        if (startTime) params.startTime = startTime;
        if (endTime) params.endTime = endTime;
        params.pageNo = this.pageNo;
        params.pageSize = this.pageSize;
        var that = this;
        this.orders = [];
        this.$http
          .post(this.host + "/exchange/order/personal/history", params)
          .then(response => {
            var resp = response.body;
            let rows = [];
            if (resp.content.length > 0) {
              this.total = resp.totalElements;
              for (var i = 0; i < resp.content.length; i++) {
                var row = resp.content[i];
                row.price =
                  row.type == "MARKET_PRICE"
                    ? that.$t("exchange.marketprice")
                    : row.price;

                function msrow(t) {
                  const milliseconds = t

                  const dateObject = new Date(milliseconds)

                  // 30/03/2022, 12:13:49
                  // 03-30 12:13

                  let getrowdate = dateObject.toLocaleString();
                  let getrowdatesplit = getrowdate.split(' ')
                  let getrowdateday = getrowdatesplit[0].split('/')
                  let getrowdatetime = getrowdatesplit[1].split(':')

                  return getrowdateday[1] + "-" + getrowdateday[0] + " " + getrowdatetime[0] + ":" + getrowdatetime[1]
                }

                row.time = msrow(row.time)
                rows.push(row);
              }
              this.orders = rows;
              console.log(this.orders);
            }
            this.loading = false;
          });
      },
      getSymbol() {
        this.$http.post(this.host + this.api.market.thumb, {}).then(response => {
          var resp = response.body;
          if (resp.length > 0) {
            this.symbol = resp;
          }
        });
      },
      updateLangData() {
        this.columns[1].title = this.$t("exchange.time");
        this.columns[2].title = this.$t("uc.finance.trade.symbol");
        this.columns[3].title = this.$t("uc.finance.trade.type");
        this.columns[4].title = this.$t("exchange.direction");
        this.columns[5].title = this.$t("exchange.price");
        this.columns[6].title = this.$t("exchange.num");
        this.columns[7].title = this.$t("exchange.traded");
        this.columns[8].title = this.$t("uc.finance.trade.turnover");
        this.columns[9].title = this.$t("exchange.status");
      }
    }
  };
</script>