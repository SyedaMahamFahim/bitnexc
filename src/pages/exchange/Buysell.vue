<template>
	<div class="container ws-buysell-container exchange" :class="skin">
		<div class="ws-side-bar" style="background-color: #192330 !important;" v-if="isSidebarOpen">
			<div class="d-flex align-items-center justify-content-between p-3">
				<div>{{$t('appmain.Exchange')}}</div>
				<span @click="isSidebarOpen = false">
					<b-icon icon="menu-button-wide" variant="light"></b-icon>
				</span>

			</div>
			<div class="coin-menu" style="overflow:hidden">
				<div class="sc_filter">
					<span v-show="isLogin" @click="wsBaseCoinTab = 1" :class="{ active: wsBaseCoinTab == 1 }">{{
						$t('service.CUSTOM')
						}}</span>
					<span @click="wsBaseCoinTab = 2" :class="{ active: wsBaseCoinTab == 2 }">USDT</span>
					<span @click="wsBaseCoinTab = 3" :class="{ active: wsBaseCoinTab == 3 }">BTC</span>
					<span @click="wsBaseCoinTab = 4" :class="{ active: wsBaseCoinTab == 4 }">ETH</span>
				</div>
				<div class="ws-ex-table px-2" v-if="wsBaseCoinTab == 1" @click="isSidebarOpen = false">
					<div v-for=" (item, index) in coins.favor">
						<router-link :to="coins.favor[index].href">
							<div class="row ws-ex-tr w-100 mx-0">
								<div class="col-5 px-0 d-flex flex-column">
									<div class="d-flex align-items-center"> <span
											style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
											coins.favor[index].coin
											}}</span><span style="opacity: .5; font-size: 11px;">
											/ {{ coins.favor[index].base }}</span> </div>
									<span style="opacity: .5; font-size: 11px;">24H Vol . {{
										coins.favor[index].volume
										}}</span>
								</div>
								<div class="col-4 px-0 d-flex flex-column">
									<span
										v-bind:class="[coins.favor[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
										style="font-size: 13px;">{{ coins.favor[index].close }}</span>
									<span style="opacity: .5; font-size: 11px;">{{
										coins.favor[index].usdRate.toFixed(2)
										}} USD</span>
								</div>
								<div class="col px-0 d-flex align-items-center justify-content-center">
									<span
										v-bind:class="[coins.favor[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
										coins.favor[index].rose
										}}</span>
								</div>
							</div>
						</router-link>
					</div>
				</div>
				<div class="ws-ex-table px-2" v-if="wsBaseCoinTab == 2" @click="isSidebarOpen = false">
					<div v-for=" (item, index) in coins.USDT">
						<router-link :to="coins.USDT[index].href">
							<div class="row ws-ex-tr w-100 mx-0">
								<div class="col-5 px-0 d-flex flex-column">
									<div class="d-flex align-items-center"> <span
											style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
											coins.USDT[index].coin
											}}</span><span style="opacity: .5; font-size: 11px;">
											/ {{ coins.USDT[index].base }}</span> </div>
									<span style="opacity: .5; font-size: 11px;">24H Vol . {{
										coins.USDT[index].volume
										}}</span>
								</div>
								<div class="col-4 px-0 d-flex flex-column">
									<span
										v-bind:class="[coins.USDT[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
										style="font-size: 13px;">{{ coins.USDT[index].close }}</span>
									<span style="opacity: .5; font-size: 11px;">{{
										coins.USDT[index].usdRate.toFixed(2)
										}} USD</span>
								</div>
								<div class="col px-0 d-flex align-items-center justify-content-center">
									<span
										v-bind:class="[coins.USDT[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
										coins.USDT[index].rose
										}}</span>
								</div>
							</div>
						</router-link>
					</div>
				</div>
				<div class="ws-ex-table px-2" v-if="wsBaseCoinTab == 2" @click="isSidebarOpen = false">
					<div v-for=" (item, index) in coins.BTC">
						<router-link :to="coins.BTC[index].href">
							<div class="row ws-ex-tr w-100 mx-0">
								<div class="col-5 px-0 d-flex flex-column">
									<div class="d-flex align-items-center"> <span
											style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
											coins.BTC[index].coin
											}}</span><span style="opacity: .5; font-size: 11px;">
											/ {{ coins.BTC[index].base }}</span> </div>
									<span style="opacity: .5; font-size: 11px;">24H Vol . {{
										coins.BTC[index].volume
										}}</span>
								</div>
								<div class="col-4 px-0 d-flex flex-column">
									<span
										v-bind:class="[coins.BTC[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
										style="font-size: 13px;">{{ coins.BTC[index].close }}</span>
									<span style="opacity: .5; font-size: 11px;">{{
										coins.BTC[index].usdRate.toFixed(2)
										}} USD</span>
								</div>
								<div class="col px-0 d-flex align-items-center justify-content-center">
									<span
										v-bind:class="[coins.BTC[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
										coins.BTC[index].rose
										}}</span>
								</div>
							</div>
						</router-link>
					</div>
				</div>
				<div class="ws-ex-table px-2" v-if="wsBaseCoinTab == 2" @click="isSidebarOpen = false">
					<div v-for=" (item, index) in coins.ETH">
						<router-link :to="coins.ETH[index].href">
							<div class="row ws-ex-tr w-100 mx-0">
								<div class="col-5 px-0 d-flex flex-column">
									<div class="d-flex align-items-center"> <span
											style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
											coins.ETH[index].coin
											}}</span><span style="opacity: .5; font-size: 11px;">
											/ {{ coins.ETH[index].base }}</span> </div>
									<span style="opacity: .5; font-size: 11px;">24H Vol . {{
										coins.ETH[index].volume
										}}</span>
								</div>
								<div class="col-4 px-0 d-flex flex-column">
									<span
										v-bind:class="[coins.ETH[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
										style="font-size: 13px;">{{ coins.ETH[index].close }}</span>
									<span style="opacity: .5; font-size: 11px;">{{
										coins.ETH[index].usdRate.toFixed(2)
										}} USD</span>
								</div>
								<div class="col px-0 d-flex align-items-center justify-content-center">
									<span
										v-bind:class="[coins.ETH[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
										coins.ETH[index].rose
										}}</span>
								</div>
							</div>
						</router-link>
					</div>
				</div>
			</div>
		</div>
		<div class="ws-exchange-head p-2">

			<span class="w-100 d-flex justify-content-center align-items-center "
				v-if="isListOpen && !isBuySellOpen">Market</span>
			<span @click="isListOpen = true" class="w-100 d-flex justify-content-start align-items-center py-1"
				v-if="!isListOpen && !isBuySellOpen">
				<b-icon icon="chevron-left" variant="light"></b-icon>
				<div class="ps-2">{{ currentCoin.coin }}/{{ currentCoin.base }}</div>
			</span>
			<span class="w-100 d-flex justify-content-between align-items-center py-1"
				v-if="!isListOpen && isBuySellOpen">
				<div class="d-flex align-items-center">
					<span @click="isSidebarOpen = true">
						<b-icon icon="menu-button-wide" variant="light"></b-icon>
					</span>
					<div class="ps-2">{{ currentCoin.coin }}/{{ currentCoin.base }}</div>
				</div>
				<div class="d-flex align-items-center">
					<div class="item mx-1" @click="currentCoinFavorChange">
						<Icon v-if="currentCoinIsFavor" type="ios-star" color="#f0a70a" size="19" />
						<Icon v-else type="ios-star-outline" color="#f0a70a" size="19" />
					</div>
					<router-link
						:to="'/exchange/'+currentCoin.coin.toLowerCase()+'_'+currentCoin.base.toLowerCase()+'?tab=2'"
						class="mx-2" style="font-size: 15px;">
						<b-icon icon="graph-up" variant="light"></b-icon>
					</router-link>
				</div>
			</span>
		</div>
		<div class="main ws-exchange-parent">
			<div class="right" v-if="isListOpen && !isBuySellOpen">
				<div class="coin-menu" style="overflow:hidden">
					<!-- <div style="padding: 8px 10px;height:48px;">
						<Input search :placeholder="$t('common.searchplaceholder')" @on-change="seachInputChange"
							v-model="searchKey" />
					</div> -->
					<div class="sc_filter">
						<span v-show="isLogin" @click="wsBaseCoinTab = 1" :class="{ active: wsBaseCoinTab == 1 }">{{
							$t('service.CUSTOM')
							}}</span>
						<span @click="wsBaseCoinTab = 2" :class="{ active: wsBaseCoinTab == 2 }">USDT</span>
						<span @click="wsBaseCoinTab = 3" :class="{ active: wsBaseCoinTab == 3 }">BTC</span>
						<span @click="wsBaseCoinTab = 4" :class="{ active: wsBaseCoinTab == 4 }">ETH</span>
					</div>
					<!-- <div class="just-container" @click="isListOpen=false">
					<Table  @on-current-change="gohref" highlight-row id="USDT" v-show="basecion==='usdt'"
						:columns="coins.columns" :data="dataIndex"></Table>
					<Table  @on-current-change="gohref" highlight-row id="BTC" v-show="basecion==='btc'"
						:columns="coins.columns" :data="dataIndex"></Table>
					<Table  @on-current-change="gohref" highlight-row id="ETH" v-show="basecion==='eth'"
						:columns="coins.columns" :data="dataIndex"></Table>
					<Table  @on-current-change="gohref" highlight-row v-show="basecion==='favor'"
						:no-data-text="$t('common.nodata')" id="collect" :columns="favorColumns" :data="dataIndex">
					</Table>
					</div> -->
					<div class="ws-ex-table px-2" v-if="wsBaseCoinTab == 1" @click="isListOpen = false">
						<div v-for=" (item, index) in coins.favor">
							<router-link :to="coins.favor[index].href">
								<div class="row ws-ex-tr w-100 mx-0">
									<div class="col-5 px-0 d-flex flex-column">
										<div class="d-flex align-items-center"> <span
												style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
												coins.favor[index].coin
												}}</span><span style="opacity: .5; font-size: 11px;">
												/ {{ coins.favor[index].base }}</span> </div>
										<span style="opacity: .5; font-size: 11px;">24H Vol . {{
											coins.favor[index].volume
											}}</span>
									</div>
									<div class="col-4 px-0 d-flex flex-column">
										<span
											v-bind:class="[coins.favor[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
											style="font-size: 13px;">{{ coins.favor[index].close }}</span>
										<span style="opacity: .5; font-size: 11px;">{{
											coins.favor[index].usdRate.toFixed(2)
											}} USD</span>
									</div>
									<div class="col px-0 d-flex align-items-center justify-content-center">
										<span
											v-bind:class="[coins.favor[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
											coins.favor[index].rose
											}}</span>
									</div>
								</div>
							</router-link>
						</div>
					</div>
					<div class="ws-ex-table px-2" v-if="wsBaseCoinTab == 2" @click="isListOpen = false">
						<div v-for=" (item, index) in coins.USDT">
							<router-link :to="coins.USDT[index].href">
								<div class="row ws-ex-tr w-100 mx-0">
									<div class="col-5 px-0 d-flex flex-column">
										<div class="d-flex align-items-center"> <span
												style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
												coins.USDT[index].coin
												}}</span><span style="opacity: .5; font-size: 11px;">
												/ {{ coins.USDT[index].base }}</span> </div>
										<span style="opacity: .5; font-size: 11px;">24H Vol . {{
											coins.USDT[index].volume
											}}</span>
									</div>
									<div class="col-4 px-0 d-flex flex-column">
										<span
											v-bind:class="[coins.USDT[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
											style="font-size: 13px;">{{ coins.USDT[index].close }}</span>
										<span style="opacity: .5; font-size: 11px;">{{
											coins.USDT[index].usdRate.toFixed(2)
											}} USD</span>
									</div>
									<div class="col px-0 d-flex align-items-center justify-content-center">
										<span
											v-bind:class="[coins.USDT[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
											coins.USDT[index].rose
											}}</span>
									</div>
								</div>
							</router-link>
						</div>
					</div>
					<div class="ws-ex-table px-2" v-if="wsBaseCoinTab == 2" @click="isListOpen = false">
						<div v-for=" (item, index) in coins.BTC">
							<router-link :to="coins.BTC[index].href">
								<div class="row ws-ex-tr w-100 mx-0">
									<div class="col-5 px-0 d-flex flex-column">
										<div class="d-flex align-items-center"> <span
												style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
												coins.BTC[index].coin
												}}</span><span style="opacity: .5; font-size: 11px;">
												/ {{ coins.BTC[index].base }}</span> </div>
										<span style="opacity: .5; font-size: 11px;">24H Vol . {{
											coins.BTC[index].volume
											}}</span>
									</div>
									<div class="col-4 px-0 d-flex flex-column">
										<span
											v-bind:class="[coins.BTC[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
											style="font-size: 13px;">{{ coins.BTC[index].close }}</span>
										<span style="opacity: .5; font-size: 11px;">{{
											coins.BTC[index].usdRate.toFixed(2)
											}} USD</span>
									</div>
									<div class="col px-0 d-flex align-items-center justify-content-center">
										<span
											v-bind:class="[coins.BTC[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
											coins.BTC[index].rose
											}}</span>
									</div>
								</div>
							</router-link>
						</div>
					</div>
					<div class="ws-ex-table px-2" v-if="wsBaseCoinTab == 2" @click="isListOpen = false">
						<div v-for=" (item, index) in coins.ETH">
							<router-link :to="coins.ETH[index].href">
								<div class="row ws-ex-tr w-100 mx-0">
									<div class="col-5 px-0 d-flex flex-column">
										<div class="d-flex align-items-center"> <span
												style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
												coins.ETH[index].coin
												}}</span><span style="opacity: .5; font-size: 11px;">
												/ {{ coins.ETH[index].base }}</span> </div>
										<span style="opacity: .5; font-size: 11px;">24H Vol . {{
											coins.ETH[index].volume
											}}</span>
									</div>
									<div class="col-4 px-0 d-flex flex-column">
										<span
											v-bind:class="[coins.ETH[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
											style="font-size: 13px;">{{ coins.ETH[index].close }}</span>
										<span style="opacity: .5; font-size: 11px;">{{
											coins.ETH[index].usdRate.toFixed(2)
											}} USD</span>
									</div>
									<div class="col px-0 d-flex align-items-center justify-content-center">
										<span
											v-bind:class="[coins.ETH[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
											coins.ETH[index].rose
											}}</span>
									</div>
								</div>
							</router-link>
						</div>
					</div>

					<!-- <table>
						<tr  v-for=" (item, index) in coins.USDT">
							<td>{{ coins.USDT[index].base }}</td>
							<td>{{ coins.USDT[index].volume }}</td>
						</tr>
					</table> -->

					<!--
          <p v-show="basecion!='favor'" style="height:40px;line-height:40px;padding-left:10px;border-bottom:1px solid #26303d;font-size:14px;color:rgb(97, 119, 146);">创新版</p>
          <Table @on-current-change="gohref" highlight-row id="USDT2" v-show="basecion==='usdt'" :columns="coins.columns" :data="dataIndex2"></Table>
          <Table @on-current-change="gohref" highlight-row id="BTC2" v-show="basecion==='btc'" :columns="coins.columns" :data="dataIndex2"></Table>
          <Table @on-current-change="gohref" highlight-row id="ETH2" v-show="basecion==='eth'" :columns="coins.columns" :data="dataIndex2"></Table>
-->
				</div>
			</div>
			<div class="center">
				<div class="symbol flex-column p-3" v-if="!isListOpen && !isBuySellOpen">

					<div class="w-100 d-flex justify-content-between align-items-center">
						<div class="d-flex col-5 flex-column align-items-start">
							<span :class="{ buy: currentCoin.change > 0, sell: currentCoin.change < 0 }"
								style="font-weight:500; font-size:22px;">{{ currentCoin.close |
								toFixed(baseCoinScale)
								}}</span>
							<span class="price-cny">≈ {{ currentCoin.usdRate * CNYRate | toFixed(2) }} USD</span>
						</div>
						<div class="mx-0 row">
							<div class="col-6 ws-ex-symb-text">24h High</div>
							<div class="col-6 ws-ex-symb-text text-end">{{ currentCoin.high | toFixed(baseCoinScale) }}
							</div>
							<div class="col-6 ws-ex-symb-text">24h Low</div>
							<div class="col-6 ws-ex-symb-text text-end">{{ currentCoin.low | toFixed(baseCoinScale) }}
							</div>
							<div class="col-6 ws-ex-symb-text">24h Volume</div>
							<div class="col-6 ws-ex-symb-text text-end">{{ currentCoin.volume }} </div>
						</div>
					</div>

					<!-- <div class="item" @click="currentCoinFavorChange">
						<Icon v-if="currentCoinIsFavor" type="ios-star" color="#f0a70a" size="24" />
						<Icon v-else type="ios-star-outline" color="#f0a70a" size="24" />
					</div> -->
					<!-- <div class="item" style="margin-left: -40px;">
						<Poptip trigger="hover" :title="coinInfo.name" content="content" placement="bottom-start"
							word-wrap width="300">
							<Icon type="md-information-circle" style="color:#546886;margin-left:5px;" />
							<div class="api" slot="content">
								<div class="coin-info">{{coinInfo.information}}</div>
								<p style="text-align:right;margin-top: -10px;"><a :href="coinInfo.infolink"
										target="_blank">{{$t("exchange.moredetail")}}</a></p>
							</div>
						</Poptip>
					</div> -->

					<!-- <div class="item">
						<span class="text">{{$t('service.NewPrice')}}</span>
						<span class="num"
							:class="{buy:currentCoin.change>0,sell:currentCoin.change<0}">{{currentCoin.close | toFixed(baseCoinScale)}}</span>
						<span class="price-cny">≈ {{currentCoin.usdRate*CNYRate | toFixed(2)}} USD</span>
					</div>
					<div class="item">
						<span class="text">{{$t('service.Change')}}</span>
						<span class="num"
							:class="{buy:currentCoin.change>0,sell:currentCoin.change<0}">{{currentCoin.rose}}</span>
					</div>
					<div class="item">
						<span class="text">{{$t('service.high')}}</span>
						<span class="num ">{{currentCoin.high | toFixed(baseCoinScale)}}</span>
					</div>
					<div class="item">
						<span class="text">{{$t('service.low')}}</span>
						<span class="num ">{{currentCoin.low | toFixed(baseCoinScale)}}</span>
					</div>
					<div class="item">
						<span class="text">{{$t('service.ExchangeNum')}}</span>
						<span class="num ">{{currentCoin.volume}} {{currentCoin.coin}}</span>
					</div> -->
				</div>
				<div class="imgtable" v-if="!isListOpen && !isBuySellOpen">
					<div class="handler">
						<span @click="changeImgTable('k')" :class="{ active: currentImgTable === 'k' }">{{
							$t("exchange.kline")
							}}</span>
						<span @click="changeImgTable('s')" :class="{ active: currentImgTable === 's' }">{{
							$t("exchange.depth")
							}}</span>
					</div>
					<div id="kline_container" :class="{ hidden: currentImgTable === 's' }">
					</div>
					<DepthGraph :class="{ hidden: currentImgTable === 'k' }" ref="depthGraph"></DepthGraph>
				</div>
				<div class="trade_wrap ws-trade-wrap " v-if="isBuySellOpen">
					<div class="trade_panel trade_panel_logout">
						<div class="ws-sell-buy-tabs">
							<span v-bind:class="[isBuyTabOpen ? 'active buys' : 'buys']"
								@click="isBuyTabOpen = true">{{$t('appmain.Buy')}}
								<div></div>
							</span>
							<span v-bind:class="[!isBuyTabOpen ? 'active selles' : 'selles']"
								@click="isBuyTabOpen = false">{{$t('appmain.Sell')}} <div></div></span>
						</div>
						<div class="mask" v-show="!isLogin">
							<span>{{ $t("common.please") }}
								<router-link to="/login">
									<span style="color:#f0a70a;">{{ $t("common.login") }}</span>
								</router-link> /
								<router-link to="/register">
									<span style="color:#00dcff;">{{ $t("common.register") }}</span>
								</router-link>
							</span>
						</div>
						<div class="publish-mask" v-show="isLogin && showPublish">
							<div style="width: 100%;margin-top:17%;text-align:center;letter-spacing:3px;">
								<span v-if="publishState == 0">{{ $t("exchange.publishstate0") }}</span>
								<span v-if="publishState == 1">{{ $t("exchange.publishstate1") }}</span>
								<span v-if="publishState == 2">{{ $t("exchange.publishstate2") }}</span>
								<span v-if="publishState == 3">{{ $t("exchange.publishstate3") }}</span>
							</div>
						</div>
						<div class="trade_menu" style="display:none !important;">
							<span @click="limited_price" :class="{ active: !showMarket }">{{
								$t("exchange.limited_price")
								}}</span>
							<span @click="market_price" :class="{ active: showMarket }">{{ $t("exchange.market_price")
								}}</span>
							<div class="fee-wrap" style="display: none;">
								<span>Taker{{ $t("exchange.fees_rate") }}：{{ symbolFee | toPercent }}</span>
								<span>Maker{{ $t("exchange.fees_rate") }}：{{ symbolFee | toPercent }}</span>
								<!-- <a href="/#/helpdetail?cate=1&id=7&cateTitle=常见问题">
                  <Icon type="ios-help-circle-outline" color="#fff" size="16"/>
                </a> -->
							</div>
						</div>
						<div class="trade_bd">
							<div class="panel panel_buy" v-if="isBuyTabOpen">

								<select name="marketss" class="ws-market-select" id=""
									@change="ChangeSelectVal($event)">
									<option value="0" @click="limited_price" selected>{{$t('appmain.Limit')}}</option>
									<option value="1" @click="market_price">{{$t('appmain.Market')}}</option>
								</select>

								<div class="bd bd_limited" v-show="!showMarket">
									<Form ref="formValidate">
										<div class="ws-padding-remove">
											<FormItem>
												<Input @on-keyup="keyEvent" v-model="form.buy.limitPrice"
													:placeholder="$t('exchange.buyprice')"></Input>
												<label class="after">{{ currentCoin.base }}</label>
												<p class="math_price d-flex w-100 justify-content-start ">≈
													{{ currentCoin.usdRate / currentCoin.close * form.buy.limitPrice *
													CNYRate || 0 | toFixed(2)
													}}
													USD</p>
											</FormItem>
										</div>
										<FormItem>
											<label class="before">{{$t('appmain.Amount')}} </label>
											<Input @on-keyup="keyEvent" v-model="form.buy.limitAmount"
												placeholder=""></Input>
											<label class="after">{{ currentCoin.coin }}</label>
										</FormItem>
										<div class="d-flex justify-content-start align-items-center"
											style="opacity:.4 ;">
											<div v-if="isLogin"
												class="hd hd_login d-flex justify-content-end align-items-center">
												<span>{{ $t("exchange.canuse") }} : </span>
												<span class="mx-0">{{ wallet.base | toFloor(baseCoinScale) }}</span>
												<span>{{ currentCoin.base }}</span>
											</div>
										</div>
										<div class="slider-wrap">
											<Slider class="silder-buy" v-model="sliderBuyLimitPercent" :step="25"
												show-tip="always" :tip-format="tipFormat" :disabled="sliderBuyDisabled">
											</Slider>
											<div class="slider-stop" v-for="item in sliderStep"
												:style="'left: ' + item + '%;'"
												@click="silderGo('sliderBuyLimitPercent', item)">
												<div class="slider-block"></div>
											</div>
										</div>
										<div class="total buy_total" style="font-size: 13px !important;">
											{{ $t("exchange.amount") }}
											<span style="opacity: .4;">{{ form.buy.limitTurnover |
												toFloor(baseCoinScale) }}</span>
											{{ currentCoin.base }}
										</div>
										<Button v-if="exchangeable == 1" class="bg-green" @click="buyWithLimitPrice">{{
											$t("exchange.buyin")
											}} {{ currentCoin.coin }}</Button>
										<Button v-else class="bg-gray">{{ $t("exchange.buyin") }} {{ currentCoin.coin
											}}</Button>
									</Form>
								</div>
								<div class="bd bd_market" v-show="showMarket">
									<Form ref="formValidate">
										<FormItem>
											<label class="before">{{ $t('exchange.buyprice') }}</label>
											<Input disabled :placeholder="$t('exchange.buytip')"></Input>
											<label class="after">{{ currentCoin.base }}</label>
										</FormItem>
										<FormItem>
											<label class="before">{{ $t('exchange.amount') }}</label>
											<Input @on-keyup="keyEvent" v-model="form.buy.marketAmount"
												:placeholder="$t('exchange.amount')"></Input>
											<label class="after">{{ currentCoin.base }}</label>
										</FormItem>

										<div class="d-flex justify-content-start align-items-center"
											style="opacity:.4 ;">
											<div v-if="isLogin"
												class="hd hd_login d-flex justify-content-end align-items-center">
												<span>{{ $t("exchange.canuse") }} : </span>
												<span class="mx-0">{{ wallet.base | toFloor(baseCoinScale) }}</span>
												<span>{{ currentCoin.base }}</span>
											</div>
										</div>
										<div class="slider-wrap">
											<Slider class="silder-buy" v-model="sliderBuyMarketPercent" :step="25"
												show-tip="always" :tip-format="tipFormat" :disabled="sliderBuyDisabled">
											</Slider>
											<div class="slider-stop" v-for="item in sliderStep"
												:style="'left: ' + item + '%;'"
												@click="silderGo('sliderBuyMarketPercent', item)">
												<div class="slider-block"></div>
											</div>
										</div>
										<div class="d-flex align-items-center">
											<Button v-if="enableMarketBuy == 1 && exchangeable == 1" class="bg-green"
												@click="buyWithMarketPrice">{{ $t("exchange.buyin") }}({{
												currentCoin.coin
												}})</Button>
											<Button v-else class="bg-gray">{{ $t("exchange.buyin") }}({{
												currentCoin.coin
												}})</Button>
										</div>
									</Form>
								</div>
							</div>
							<div class="panel panel_sell" v-if="!isBuyTabOpen">
								<select name="marketss" class="ws-market-select" id=""
									@change="ChangeSelectVal($event)">
									<option value="0" @click="limited_price" selected>{{$t('appmain.Limit')}}</option>
									<option value="1" @click="market_price">{{$t('appmain.Market')}}</option>
								</select>
								<div class="bd bd_limited" v-show="!showMarket">
									<Form ref="formValidate">
										<div class="ws-padding-remove">

											<FormItem>
												<Input @on-keyup="keyEvent" v-model="form.sell.limitPrice"
													:placeholder="$t('exchange.sellprice')"></Input>
												<label class="after">{{ currentCoin.base }}</label>
												<p class="math_price d-flex w-100 justify-content-start ">≈
													{{ currentCoin.usdRate / currentCoin.close * form.sell.limitPrice *
													CNYRate || 0 | toFixed(2)
													}}
													USD</p>
											</FormItem>
										</div>

										<FormItem>
											<label class="before">{{$t('appmain.Amount')}}</label>
											<Input @on-keyup="keyEvent" v-model="form.sell.limitAmount"
												:placeholder="$t('exchange.sellnum')"></Input>
											<label class="after">{{ currentCoin.coin }}</label>
										</FormItem>
										<div class="d-flex justify-content-start align-items-center"
											style="opacity:.4 ;">
											<div v-if="isLogin"
												class="hd hd_login d-flex justify-content-end align-items-center">
												<span>{{ $t("exchange.canuse") }} : </span>
												<span class="mx-0">{{ wallet.coin.toFixed(6) }}</span>
												<span>{{ currentCoin.coin }}</span>
											</div>
										</div>
										<div class="slider-wrap">
											<Slider class="silder-sell" v-model="sliderSellLimitPercent" :step="25"
												show-tip="always" :tip-format="tipFormat"
												:disabled="sliderSellDisabled"></Slider>
											<div class="slider-stop" v-for="item in sliderStep"
												:style="'left: ' + item + '%;'"
												@click="silderGo('sliderSellLimitPercent', item)">
												<div class="slider-block"></div>
											</div>
										</div>
										<div class="total sell_total" style="font-size: 13px !important;">
											{{ $t("exchange.amount") }}
											<span style="opacity: .4;">{{ form.sell.limitTurnover | toFloor(coinScale)
												}}</span>
											{{ currentCoin.base }}
										</div>
										<Button v-if="exchangeable == 1" class="bg-red" @click="sellLimitPrice">{{
											$t("exchange.sellout")
											}} {{ currentCoin.coin }} </Button>
										<Button v-else class="bg-gray">{{ $t("exchange.sellout") }} {{ currentCoin.coin
											}} </Button>
									</Form>
								</div>
								<div class="bd bd_market" v-show="showMarket">
									<Form ref="formValidate">
										<FormItem>
											<label class="before">{{ $t('exchange.sellprice') }}</label>
											<Input disabled :placeholder="$t('exchange.selltip')"></Input>
											<label class="after">{{ currentCoin.base }}</label>
										</FormItem>
										<FormItem>
											<label class="before">{{ $t('exchange.sellnum') }}</label>
											<Input @on-keyup="keyEvent" v-model="form.sell.marketAmount"
												:placeholder="$t('exchange.sellnum')"></Input>
											<label class="after">{{ currentCoin.coin }}</label>
										</FormItem>
										<div class="d-flex justify-content-start align-items-center"
											style="opacity:.4 ;">
											<div v-if="isLogin"
												class="hd hd_login d-flex justify-content-end align-items-center">
												<span>{{ $t("exchange.canuse") }} : </span>
												<span class="mx-0">{{ wallet.coin.toFixed(6) }}</span>
												<span>{{ currentCoin.coin }}</span>
											</div>
										</div>
										<div class="slider-wrap">
											<Slider class="silder-sell" v-model="sliderSellMarketPercent" :step="25"
												show-tip="always" :tip-format="tipFormat"
												:disabled="sliderSellDisabled"></Slider>
											<div class="slider-stop" v-for="item in sliderStep"
												:style="'left: ' + item + '%;'"
												@click="silderGo('sliderSellMarketPercent', item)">
												<div class="slider-block"></div>
											</div>
										</div>
										<Button v-if="enableMarketSell == 1 && exchangeable == 1" class="bg-red"
											@click="sellMarketPrice">{{ $t("exchange.sellout") }}
											({{ currentCoin.coin }})</Button>
										<Button v-else class="bg-gray">{{ $t("exchange.sellout") }}({{ currentCoin.coin
											}})</Button>
									</Form>
								</div>
							</div>
						</div>
					</div>
					<div class="ws-trade-inner-div-2 col px-1">
						<div>
							<div class="row mx-0 py-2 ">
								<div class="col-6 px-0 text-start " style="font-size: 11px; opacity: .7;">{{$t('appmain.Price')}}(USDT)
								</div>
								<div class="col-6 px-0 text-end " style="font-size: 11px; opacity: .7;">{{$t('appmain.Amount')}}({{
									currentCoin.coin }})</div>
							</div>
							<div class="row ws-ex-tr py-2 w-100 mx-0 border-0" v-for=" (item, index) in plate.askRows"
								v-if="index < 5">
								<div v-bind:class="[plate.askRows[index].direction == 'SELL' ? 'text-danger col-6 px-0 text-start ' : 'text-success col-6 px-0 text-start ']"
									style="font-size: 11px; opacity: 1; text-transform:lowercase !important;">{{
									plate.askRows[index].price }}</div>
								<div class="col-6 px-0 text-end " style="font-size: 11px; opacity: .7;">{{
									plate.askRows[index].amount.toFixed(5) }}</div>
								<span class="ws-plate-progress"
									:style="{ width: ((plate.askRows[index].position /10) * 100).toFixed(2) + '%', backgroundColor: '#4621299e' }"></span>
							</div>
							<div class="plate-nowprice ">
								<span class="price"
									:class="{ buy: currentCoin.change > 0, sell: currentCoin.change < 0 }">{{
									currentCoin.price |
									toFixed(baseCoinScale)
									}}</span>
								<span v-if="currentCoin.change > 0" class="buy">↑</span>
								<span v-else-if="currentCoin.change < 0" class="sell">↓</span>
								<span class="price-cny"
									style="font-size: 11px !important; display: flex !important; padding-top: 2px !important; opacity:.5;"
									≈ {{ currentCoin.usdRate * CNYRate | toFixed(2) }} USD</span>
							</div>
							<div class="row ws-ex-tr py-2 w-100 mx-0 border-0" v-for=" (item, index) in plate.bidRows"
								v-if="index < 5">
								<div v-bind:class="[plate.bidRows[index].direction == 'SELL' ? 'text-danger col-6 px-0 text-start ' : 'text-success col-6 px-0 text-start ']"
									style="font-size: 11px; opacity: 1; text-transform:lowercase !important;">{{
									plate.bidRows[index].price }}</div>
								<div class="col-6 px-0 text-end " style="font-size: 11px; opacity: .7;">{{
									plate.bidRows[index].amount.toFixed(5) }}</div>
								<span class="ws-plate-progress"
									:style="{ width: ((plate.bidRows[index].position /10) * 100).toFixed(2) + '%', backgroundColor: 'rgb(15,54,46)' }"></span>
							</div>
						</div>
					</div>
				</div>
			</div>
			<div class="left plate-wrap pb-5" v-if="!isListOpen && !isBuySellOpen" style="position:relative;">
				<div class="sc_filter">
					<span @click="wsPlateTab = 1" :class="{ active: wsPlateTab == 1 }"
						style="margin:0 !important;">ORDER
						BOOK</span>
					<span @click="wsPlateTab = 2" :class="{ active: wsPlateTab == 2 }"
						style="margin:0 !important;">MARKET
						HISTORY</span>
				</div>
				<div class="lightning-panel" v-if="showCountDown" :style="{ background: countDownBgColor }">
					<img v-if="lang == '简体中文' && publishType == 'FENTAN'"
						src="../../assets/images/lightning-bg.png"></img>
					<img v-if="lang == 'English' && publishType == 'FENTAN'"
						src="../../assets/images/lightning-bg-en.png"></img>
					<img v-if="lang == '简体中文' && publishType == 'QIANGGOU'"
						src="../../assets/images/lightning-bg2.png"></img>
					<img v-if="lang == 'English' && publishType == 'QIANGGOU'"
						src="../../assets/images/lightning-bg2-en.png"></img>
					<div class="l-content">
						<BZCountDown style="width:100%;margin-top:5px;" :countDownBgColor.sync="countDownBgColor"
							:publishState.sync="publishState" :publishType="publishType" :currentTime="currentTime"
							:startTime="startTime" :clearTime="clearTime" :endTime="endTime"
							:showPublishMask="showPublishMask" :hidePublishMask="hidePublishMask"
							:hideCountDown="hideCountDown">
						</BZCountDown>
						<p class="l-info">
							<span class="l-txt">{{ $t("exchange.publishamount") }}：</span><span class="l-count">{{
								publishAmount | toFixedPublishAmount
								}} </span><span class="l-unit">{{ currentCoin.coin
								}}</span> &nbsp;&nbsp;&nbsp;&nbsp;
							<span class="l-txt">{{ $t("exchange.publishprice") }}：</span><span class="l-price">{{
								publishPrice | toFixedPublishPrice
								}} </span><span class="l-unit">{{ currentCoin.base
								}}</span>
						</p>
						<p class="l-detail">
							<router-link target="_blank" to="/announcement/118930">{{ $t("exchange.publishdetail") }}
							</router-link>
						</p>
					</div>
				</div>
				<div class="handlers d-none">
					<span @click="changePlate('all')" class="handler handler-all"
						:class="{ active: selectedPlate == 'all' }"></span>
					<span @click="changePlate('buy')" class="handler handler-green"
						:class="{ active: selectedPlate == 'buy' }"></span>
					<span @click="changePlate('sell')" class="handler handler-red"
						:class="{ active: selectedPlate == 'sell' }"></span>
				</div>
				<div v-if="wsPlateTab == 1">
					<Table v-show="selectedPlate != 'buy'" @on-current-change="buyPlate" highlight-row
						ref="currentRowTable" class="sell_table" :columns="plate.columns" :data="plate.askRows"
						:no-data-text="$t('common.nodata')"></Table>
					<div class="plate-nowprice">
						<span class="price" :class="{ buy: currentCoin.change > 0, sell: currentCoin.change < 0 }">{{
							currentCoin.price |
							toFixed(baseCoinScale)
							}}</span>
						<span v-if="currentCoin.change > 0" class="buy">↑</span>
						<span v-else-if="currentCoin.change < 0" class="sell">↓</span>
						<span class="price-cny"> ≈ {{ currentCoin.usdRate * CNYRate | toFixed(2) }} USD</span>
					</div>
					<Table v-show="selectedPlate != 'sell'" @on-current-change="sellPlate" highlight-row
						class="buy_table" :class="{ hidden: selectedPlate === 'all' }" :columns="plate.columns"
						:data="plate.bidRows" :no-data-text="$t('common.nodata')">
					</Table>
				</div>
				<div v-if="wsPlateTab == 2" class="trade-wrap" v-show="!showCountDown">
					<!-- <Table :columns="trade.columns" :data="trade.rows" :no-data-text="$t('common.nodata')">
					</Table> -->

					<div>
						<div class="row mx-0 py-2">
							<div class="col-3 px-0 text-center " style="font-size: 11px; opacity: .7;">Time</div>
							<div class="col-3 px-0 text-center " style="font-size: 11px; opacity: .7;">Side</div>
							<div class="col-3 px-0 text-center " style="font-size: 11px; opacity: .7;">Price(USDT)</div>
							<div class="col-3 px-0 text-center " style="font-size: 11px; opacity: .7;">Amount(BTC)</div>
						</div>
						<div class="row ws-ex-tr w-100 mx-0 border-0" v-for=" (item, index) in trade.rows">
							<div class="col-3 px-0 text-center " style="font-size: 11px; opacity: .7;">
								{{ new Date(trade.rows[index].time * 1000).toISOString().slice(14, -5) }}</div>
							<div v-bind:class="[trade.rows[index].direction == 'SELL' ? 'text-danger col-3 px-0 text-center ' : 'text-success col-3 px-0 text-center ']"
								style="font-size: 11px; opacity: 1; text-transform:lowercase !important;">{{
								trade.rows[index].direction }}</div>
							<div class="col-3 px-0 text-center " style="font-size: 11px; opacity: .7;">{{
								trade.rows[index].price }}</div>
							<div class="col-3 px-0 text-center " style="font-size: 11px; opacity: .7;">{{
								trade.rows[index].amount.toFixed(4) }}</div>
							<!-- <div class="col-5 px-0 d-flex flex-column">
							<div class="d-flex align-items-center"> <span
									style="font-weight: 500; padding-right: 3px; font-size: 13px;">{{
											coins.ETH[index].coin
									}}</span><span style="opacity: .5; font-size: 11px;">
									/ {{ coins.ETH[index].base }}</span> </div>
							<span style="opacity: .5; font-size: 11px;">24H Vol . {{
									coins.ETH[index].volume
							}}</span>
						</div>
						<div class="col-4 px-0 d-flex flex-column">
							<span
								v-bind:class="[coins.ETH[index].rose.indexOf('-') > -1 ? 'text-danger' : 'text-success']"
								style="font-size: 13px;">{{ coins.ETH[index].close }}</span>
							<span style="opacity: .5; font-size: 11px;">{{
									coins.ETH[index].usdRate.toFixed(2)
							}} USD</span>
						</div>
						<div class="col px-0 d-flex align-items-center justify-content-center">
							<span
								v-bind:class="[coins.ETH[index].rose.indexOf('-') > -1 ? 'ws-ex-talbe-sell' : 'ws-ex-talbe-buy']">{{
										coins.ETH[index].rose
								}}</span>
						</div> -->
						</div>
					</div>
				</div>
			</div>
			<div class="ws-sell-buy w-100" v-if="!isListOpen && !isBuySellOpen">
				<div class="w-100 d-flex align-items-center">
					<div class="d-flex col p-1">
						<button @click="isBuySellOpen = true, isBuyTabOpen = true"
							v-if="enableMarketBuy == 1 && exchangeable == 1" class="btn btn-danger w-100">{{
							$t("exchange.buyin")
							}}({{ currentCoin.coin }})</button>
						<button v-else class="btn btn-secondary w-100">{{ $t("exchange.buyin") }}({{ currentCoin.coin
							}})</button>
					</div>
					<div class="d-flex col p-1">
						<button @click="isBuySellOpen = true, isBuyTabOpen = false"
							v-if="enableMarketSell == 1 && exchangeable == 1" class="btn btn-success w-100">{{
							$t("exchange.sellout")
							}}({{ currentCoin.coin }})</button>
						<button v-else class="btn btn-secondary w-100">{{ $t("exchange.sellout") }}({{ currentCoin.coin
							}})</button>
					</div>
					<div class="d-flex flex-column align-items-center ps-2">
						<div class="item" @click="currentCoinFavorChange">
							<Icon v-if="currentCoinIsFavor" type="ios-star" color="#f0a70a" size="19" />
							<Icon v-else type="ios-star-outline" color="#f0a70a" size="19" />
						</div>
						<span style="font-size:12px; ">Favorites</span>
					</div>
				</div>
			</div>
		</div>
		<div class="order ws-order" v-if="isBuySellOpen" style="margin-top: 5px !important;">
			<div class="order-handler">
				<span @click="changeOrder('current')" :class="{ active: selectedOrder === 'current' }">
					<!-- {{
					$t('exchange.curdelegation')
					}} -->
					{{$t('appmain.Order')}}
				</span>
				<span @click="changeOrder('history')" :class="{ active: selectedOrder === 'history' }">
					<!-- {{
					$t('exchange.hisdelegation')
					}} -->
					{{$t('appmain.OrderHistory')}}
				</span>
				<router-link v-show="selectedOrder === 'current'" class="linkmore" to="/uc/entrust/current">
					{{$t('appmain.All')}}</router-link>
				<router-link v-show="selectedOrder === 'history'" class="linkmore" to="/uc/entrust/history">
					{{$t('appmain.All')}}</router-link>
			</div>
			<div class="table pb-5">
				<div v-if="selectedOrder === 'current'">
					<div class="row ws-ex-tr w-100 py-2 mx-0" v-for=" (item, index) in currentOrder.rows">
						<div class="d-flex align-items-center justify-content-between py-1">
							<div class="d-flex align-items-center">
								<span
									v-bind:class="[currentOrder.rows[index].direction == 'SELL' ? 'text-danger' : 'text-success']"
									style="font-size:12px; text-transform: capitalize;">{{
									currentOrder.rows[index].direction.toLowerCase() }}</span>
								<span class="ps-3" style="font-size:12px;">{{ currentOrder.rows[index].symbol }}</span>
							</div>
							<span style="color: rgb(0, 140, 255); font-size: 12px;"
								@click="openModalRevoke(index)">{{$t('appmain.Revoke')}}</span>
						</div>
						<div class="row mx-0">
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{$t('appmain.Time')}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">{{$t('appmain.Type')}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end">{{$t('appmain.Price')}}({{
								currentOrder.rows[index].baseSymbol }})</div>

							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{currentOrder.rows[index].time }}
							</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">
								<template v-if="currentOrder.rows[index].type == 'LIMIT_PRICE'">{{$t('appmain.Limit')}}</template>
								<template v-if="currentOrder.rows[index].type != 'LIMIT_PRICE'">{{$t('appmain.Market')}}</template>
							</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end">{{ currentOrder.rows[index].price }}
							</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{$t('appmain.Amount')}}({{
								currentOrder.rows[index].coinSymbol }})</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">{{$t('appmain.Deal')}}/{{$t('appmain.Fee')}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end">{{$t('appmain.Amount')}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{
								currentOrder.rows[index].amount.toFixed(2)
								}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">{{
								currentOrder.rows[index].fee.toFixed(4) }}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end">{{
								currentOrder.rows[index].amount.toFixed(2)
								}}</div>
						</div>
					</div>
				</div>
				<!-- <Table height="240"  :columns="currentOrder.columns"
					:data="currentOrder.rows" :no-data-text="$t('common.nodata')"></Table> -->
				<div v-else>
					<div class="row ws-ex-tr w-100 py-2 mx-0" v-for=" (item, index) in historyOrder.rows">
						<div class="d-flex align-items-center justify-content-between">
							<div class="d-flex align-items-center">
								<span
									v-bind:class="[historyOrder.rows[index].direction == 'SELL' ? 'text-danger' : 'text-success']"
									style="font-size:12px; text-transform: capitalize;">
									<template v-if="historyOrder.rows[index].direction == 'SELL'">{{$t('appmain.Sell')}}</template>
									<template v-if="historyOrder.rows[index].direction != 'SELL'">{{$t('appmain.Buy')}}</template>
									{{
									historyOrder.rows[index].direction.toLowerCase() }}</span>
								<span class="ps-3" style="font-size:12px;">{{ historyOrder.rows[index].symbol }}</span>
							</div>
							<span style="font-size:12px;">{{$t('appmain.Deal')}}</span>
						</div>
						<div class="row mx-0">
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{$t('appmain.Time')}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">{{$t('appmain.Type')}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end">{{$t('appmain.Price')}}({{
								historyOrder.rows[index].baseSymbol }})</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{ historyOrder.rows[index].time }}
							</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">
								{{historyOrder.rows[index].type == "LIMIT_PRICE" ? "Limit" : "Market"}}
							</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end">{{ historyOrder.rows[index].price }}
							</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{$t('appmain.Amount')}}({{
								historyOrder.rows[index].coinSymbol }})</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">{{$t('appmain.Deal')}}/{{$t('appmain.Fee')}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end">{{$t('appmain.Amount')}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{ historyOrder.rows[index].amount
								}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">{{
								historyOrder.rows[index].tradedAmount }}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end">{{ historyOrder.rows[index].turnover
								}}</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{$t('appmain.Fee')}}({{
								historyOrder.rows[index].coinSymbol }})</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">{{$t('appmain.AveragePrice')}}({{
								historyOrder.rows[index].baseSymbol }})</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-end"></div>
							<div class="col-4 pb-1 px-0 text-faded-small text-start">{{ historyOrder.rows[index].fee }}
							</div>
							<div class="col-4 pb-1 px-0 text-faded-small text-center">{{ historyOrder.rows[index].price
								}}</div>
						</div>
					</div>
				</div>

			</div>
		</div>


		<!-- Bottom Modal -->
		<div class="bottom-up" v-if="isModalOpen == true" @click=" isModalOpen = false">
			<div class="p-2 d-flex flex-column" style="background-color: #1A212B;">
				<span class="w-100 text-faded-small"></span>
				<span class="fs-8  mb-3 mt-2" style="opacity:.6;">{{$t('appmain.RevokeText')}}</span>
				<button class="btn btn-danger mb-2" @click="cancel(modalIndex)"
					style="border-radius:5px !important;">{{$t('appmain.Revoke')}}</button>
				<button @click=" isModalOpen = false" class="btn btn-secondary"
					style="border:none !important;background-color:#13161D !important; border-radius:5px !important;">{{$t('appmain.Cancel')}}</button>
			</div>
		</div>

	</div>
</template>
<style scoped lang="scss">
	@import url(../../assets/css/exchange.css);
	$night-bg: #0b1520;
	$night-headerbg: #27313e;
	$night-contentbg: #192330;
	$night-color: #fff;

	.exchange .ivu-tooltip-inner {
		padding: 2px 5px !important;
		min-height: 24px !important;
	}

	.exchange {
		color: #fff;
		background-color: #0b1520;

		.main {
			display: flex;
			margin-top: 5px;

			.left {
				flex: 0 0 20%;
				border-radius: 0px;
				margin-right: 10px;

				.handlers {
					font-size: 0;
					padding: 10px 20px;
					background-color: #192330;
					border-top-left-radius: 0px;
					border-top-right-radius: 0px;

					.handler {
						display: inline-block;
						margin-right: 10px;
						width: 20px;
						height: 20px;
						background-size: 100% 100%;
						cursor: pointer;

						&.handler-all {
							background-image: url("../../assets/images/exchange/plate_all.png");

							&.active {
								background-image: url("../../assets/images/exchange/plate_all_active.png");
							}
						}

						&.handler-green {
							background-image: url("../../assets/images/exchange/plate_green.png");

							&.active {
								background-image: url("../../assets/images/exchange/plate_green_active.png");
							}
						}

						&.handler-red {
							background-image: url("../../assets/images/exchange/plate_red.png");

							&.active {
								background-image: url("../../assets/images/exchange/plate_red_active.png");
							}
						}
					}
				}

				.plate-nowprice {
					text-align: center;
					background-color: #27313e;
					line-height: 1;
					display: flex;
					align-items: center;
					height: 40px;
					justify-content: center;
					font-size: 14px;
					font-weight: 500;

					.price {
						font-size: 18px;
						margin-right: 10px;
					}

					.price-cny {
						font-size: 12px;
						margin-left: 10px;
						font-weight: 400;
						color: rgba(219, 222, 246, 0.3);
					}
				}
			}

			.center {
				flex: 0 0 60%;
				margin-right: 5px;

				.imgtable {
					// height: 350px;
					height: 400px;
					position: relative;
					overflow: hidden;
					margin-bottom: 5px;

					.handler {
						position: absolute;
						top: 10px;
						right: 40px;
						z-index: 1000;

						>span {
							background-color: #2c3b59;
							color: #c7cce6;
							padding: 4px 8px;
							cursor: pointer;
							font-size: 13px;
							opacity: 0.5;

							&.active {
								opacity: 1;
							}
						}
					}
				}

				.trade_wrap {
					.trade_menu {
						position: relative;
						background-color: #192330;
						border-top-left-radius: 0px;
						border-top-right-radius: 0px;
						border-bottom: 1px solid #27313e;
						font-size: 0;
						height: 40px;
						line-height: 40px;

						>span {
							font-size: 16px;
							padding: 11px 20px;
							cursor: pointer;

							&.active {
								color: #fff;
								border-bottom: 2px solid #f0a70a;
							}

							&:first-child {
								border-top-left-radius: 0px;
							}
						}

						.fee-wrap {
							position: absolute;
							top: 0;
							right: 20px;
							font-size: 12px;

							>span {
								margin-right: 10px;
								color: #7c7f82;
							}

							>a {
								vertical-align: middle;
							}
						}
					}

					.trade_panel {
						position: relative;

						.mask {
							position: absolute;
							width: 100%;
							height: 100%;
							display: flex;
							background-color: rgba(0, 52, 77, 0.8);
							justify-content: center;
							align-items: center;
							z-index: 100;
							font-size: 24px;
							border-radius: 0px;
							// color: #bcd7e6;
						}

						.publish-mask {
							position: absolute;
							width: 100%;
							height: 100%;
							background-color: rgba(0, 37, 64, 0.93);
							justify-content: center;
							align-items: center;
							z-index: 101;
							font-size: 24px;
							border-radius: 0px;
							// color: #bcd7e6;
						}
					}

					.trade_panel .panel .hd {
						border-bottom: none;

						b {
							color: #fff;
						}

						a {
							color: #f0a70a;
						}
					}
				}
			}

			.right {
				flex: 0 0 19%;
				margin-right: 5px;

				.coin-menu {
					height: 787px;
					// height: 785px;
					background-color: #192330;
					margin-bottom: 10px;
					border-radius: 0px;
				}
			}
		}

		.symbol {
			display: flex;
			justify-content: space-between;
			padding: 15px 30px;
			margin-bottom: 5px;
			align-items: center;
			background-color: #192330;
			line-height: 1;
			border-radius: 0px;

			.item {
				.price-cny {
					font-size: 12px;
					color: #546886;
				}

				.coin {
					font-size: 20px;
				}

				.text {
					width: 100%;
					display: block;
					font-size: 12px;
					color: #999;
					margin-bottom: 5px;
				}

				.num {
					font-size: 12px;
					color: #fff;
				}

				>img {
					display: block;
					width: 18px;
					height: 18px;
					cursor: pointer;
				}
			}
		}

		.order {
			min-height: 227px;
			margin-bottom: 10px;

			.order-handler {
				// background-color: #192330;
				background-color: #192330;
				font-size: 0;
				border-radius: 6px;

				// line-height: 38px;
				>span {
					padding: 0 20px;
					font-size: 14px;
					display: inline-block;
					color: #fff;
					cursor: pointer;
					line-height: 40px;
					background-color: #192330;

					&.active {
						color: #f0a70a;
						background-color: #27313e;
					}

					&:first-child {
						border-top-left-radius: 0px;
					}

					&:last-child {
						border-top-right-radius: 0px;
					}
				}
			}
		}
	}

	.exchange.day {
		color: #333;
		background-color: #fff;

		.main {
			.left {
				background-color: #fff;
				box-shadow: 0 0 2px #ccc;

				.handlers {
					background-color: #fff;
				}

				.plate-nowprice {
					background-color: #fff;
					border-top: 1px solid #f0f0f0;
					border-bottom: 1px solid #f0f0f0;
				}
			}

			.imgtable {
				border-radius: 6px;
				box-shadow: 0 0 2px #ccc;

				.handler {
					>span {
						border: 1px solid #333;
					}
				}
			}

			.trade_wrap {
				box-shadow: 0 0 2px #ccc;

				.trade_menu {
					background-color: #fafafa;

					>span {
						background-color: #fafafa;
						border-right: 1px solid #f0f0f0;

						&.active {
							background-color: #fff;
							color: #f0a70a;
						}

						&:last-child {
							border-top-right-radius: 6px;
						}
					}

					.ivu-icon {
						color: #333 !important;
					}
				}

				.trade_panel {
					box-shadow: 0 0 2px #ccc;

					.mask {
						background-color: rgba(0, 52, 77, 0.8);
						color: #fff;
					}
				}

				.trade_panel .panel .hd {
					border-bottom: none;

					b {
						color: #333;
					}

					a {
						color: #f0a70a;
					}
				}
			}

			.right {
				.coin-menu {
					background-color: #fff;
					box-shadow: 0 2px 2px #ccc;
				}

				.trade-wrap {
					box-shadow: 0 0 2px #ccc;
					border-radius: 6px;
				}

				// .ivu-table-wrapper{
				//         box-shadow:0 0 2px #ccc;
				//       }
			}
		}

		.symbol {
			background-color: #fff;
			box-shadow: 0 0 2px #ccc;

			.item {
				.text {
					color: #999;
				}

				.num {
					color: #333;
				}
			}
		}

		.order {
			box-shadow: 0 2px 2px #ccc;
			min-height: 227px;

			.order-handler {
				margin-right: -2px;
				background-color: #fff;

				>span {
					color: #333;
					background-color: #fafafa;
					box-shadow: 0 0 2px #ccc;

					&.active {
						color: #f0a70a;
						background-color: #fff;
					}
				}
			}
		}
	}

	#kline_container {
		background: #192330;
		height: 100%;
	}

	.coin-info {
		color: #8f9ca5;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 5;
		overflow: hidden;
		padding-top: 15px;
	}

	.trade_bd .panel {
		height: 276px;
	}

	.trade_panel .panel .hd {
		height: 10px;
		line-height: 10px;
	}
</style>
<script>
	import expandRow from "@components/exchange/expand.vue";
	import Datafeeds from "@js/charting_library/datafeed/bitrade.js";
	var Stomp = require("stompjs");
	var SockJS = require("sockjs-client");
	var moment = require("moment");
	import DepthGraph from "@components/exchange/DepthGraph.vue";
	import $ from "@js/jquery.min.js";
	import BZCountDown from "@components/exchange/BZCountDown.vue";
	import { accDiv } from "../../assets/js/decimal";

	export default {
		components: {
			expandRow,
			DepthGraph,
			BZCountDown,
		},
		data() {
			let self = this;
			return {
				SelectVal: 0,
				isModalOpen: false,
				modalIndex: 0,
				isSidebarOpen: false,
				isBuyTabOpen: true,
				isListOpen: false,
				wsBaseCoinTab: 2,
				wsPlateTab: 1,
				isBuySellOpen: true,
				sliderStep: [25, 50, 75, 100],
				sliderBuyLimitPercent: 0,
				sliderSellLimitPercent: 0,
				sliderBuyMarketPercent: 0,
				sliderSellMarketPercent: 0,
				memberRate: 0,
				// userRealVerified: false, //是否实名认证
				collecRequesting: false,
				currentCoinIsFavor: false,
				isUseBHB: false,
				skin: "night", //皮肤样式day&night
				currentImgTable: "k",
				selectedOrder: "current", //当前显示的委托记录
				selectedPlate: "all", //当前显示的买卖盘
				CNYRate: null,
				datafeed: null,
				defaultPath: "btc_usdt",
				basecion: "usdt",
				coinScale: 6,
				baseCoinScale: 6,
				symbolFee: 0.001,
				dataIndex: [],
				dataIndex2: [],
				searchKey: "",
				coinInfo: {},
				currentCoin: {
					base: "",
					coin: "",
					symbol: "",
				},
				enableMarketBuy: 1, // 0:禁用  1:启用
				enableMarketSell: 1,
				exchangeable: 1, // 0:可交易   1:不可交易

				publishType: "NONE",
				currentTime: 0,
				publishAmount: 0,
				publishPrice: 0,
				startTime: "2000-01-01 00:00:00",
				endTime: "2000-01-01 00:00:00",
				clearTime: "2000-01-01 00:00:00",
				showPublish: false,
				showCountDown: false,
				countDownBgColor:
					"linear-gradient(135deg, rgb(0 199 144 / 63%) 10%, rgb(18 33 41) 100%)",
				publishState: 0,
				favorColumns: [
					{
						title: this.$t("exchange.coin"),
						key: "coin",
						sortable: false,
						width: 120,
						className: "coin-menu-symbol",
						render: (h, params) => {
							return h("div", [
								h("Icon", {
									props: {
										// color:"red",
										type: params.row.isFavor ? "ios-star" : "ios-star-outline",
									},
									nativeOn: {
										click: () => {
											event.stopPropagation(); //阻止事件冒泡
											if (this.isLogin) {
												if (
													event.currentTarget.className ==
													"ivu-icon ivu-icon-ios-star"
												) {
													this.cancelCollect(params.index, params.row);
													event.currentTarget.className ==
														"ivu-icon ivu-icon-ios-star-outline";
												} else {
													this.collect(params.index, params.row);
													event.currentTarget.className =
														"ivu-icon ivu-icon-ios-star";
												}
											} else {
												this.$Message.warning("请先登录");
											}
										},
									},
								}),
								h("span", params.row.symbol),
							]);
						},
					},
					{
						title: this.$t("exchange.lastprice"),
						key: "close",
						sortable: true,
						sortMethod: function (a, b, type) {
							let a1 = parseFloat(a);
							let b1 = parseFloat(b);
							if (type == "asc") {
								return a1 - b1;
							} else {
								return b1 - a1;
							}
						},
					},
					{
						title: this.$t("exchange.daychange"),
						key: "rose",
						sortable: true,
						sortMethod: function (a, b, type) {
							let a1 = a.replace(/[^\d|.|-]/g, "") - 0;
							let b1 = b.replace(/[^\d|.|-]/g, "") - 0;
							if (type == "asc") {
								return a1 - b1;
							} else {
								return b1 - a1;
							}
						},
						render: (h, params) => {
							const row = params.row;
							const className = parseFloat(row.rose) < 0 ? "sell" : "buy";
							return h(
								"span",
								{
									attrs: {
										class: className,
									},
								},
								row.rose
							);
						},
					},
				],
				//当前市场上的交易币种，按交易对分
				coins: {
					_map: [],
					USDT: [],
					BTC: [],
					ETH: [],
					USDT2: [],
					BTC2: [],
					ETH2: [],
					favor: [],
					columns: [
						{
							title: this.$t("exchange.coin"),
							key: "coin",
							sortable: false,
							width: 120,
							className: "coin-menu-symbol",
							render: (h, params) => {
								if (params.row.coin == "FLOW") {
									return h("div", [
										h("Icon", {
											props: {
												// color:"red",
												type: params.row.isFavor
													? "ios-star"
													: "ios-star-outline",
											},
											nativeOn: {
												click: () => {
													event.stopPropagation(); //阻止事件冒泡
													if (this.isLogin) {
														if (
															event.currentTarget.className ==
															"ivu-icon ivu-icon-ios-star"
														) {
															this.cancelCollect(params.index, params.row);
															event.currentTarget.className ==
																"ivu-icon ivu-icon-ios-star-outline";
														} else {
															this.collect(params.index, params.row);
															event.currentTarget.className =
																"ivu-icon ivu-icon-ios-star";
														}
													} else {
														this.$Message.warning(this.$t("common.logintip"));
													}
												},
											},
										}),
										h("span", params.row.coin),
										h(
											"Tooltip",
											{
												props: {
													placement: "top-start",
													content: "价格跟随演示，价格为BTC价格的0.92%",
												},
											},
											[
												h(
													"span",
													{
														style: {
															fontSize: "8px",
															padding: "2px 2px 1px 2px",
															color: "#FFF",
															marginLeft: "5px",
															background: "#F30",
															borderRadius: "8px",
															display: "inline-block",
															height: "8px",
															width: "8px",
														},
													},
													""
												),
											]
										),
									]);
								} else if (params.row.coin == "FDC") {
									return h("div", [
										h("Icon", {
											props: {
												// color:"red",
												type: params.row.isFavor
													? "ios-star"
													: "ios-star-outline",
											},
											nativeOn: {
												click: () => {
													event.stopPropagation(); //阻止事件冒泡
													if (this.isLogin) {
														if (
															event.currentTarget.className ==
															"ivu-icon ivu-icon-ios-star"
														) {
															this.cancelCollect(params.index, params.row);
															event.currentTarget.className ==
																"ivu-icon ivu-icon-ios-star-outline";
														} else {
															this.collect(params.index, params.row);
															event.currentTarget.className =
																"ivu-icon ivu-icon-ios-star";
														}
													} else {
														this.$Message.warning(this.$t("common.logintip"));
													}
												},
											},
										}),
										h("span", params.row.coin),
										h(
											"Tooltip",
											{
												props: {
													placement: "right",
													content: "首发抢购活动演示",
												},
											},
											[
												h(
													"span",
													{
														style: {
															fontSize: "8px",
															padding: "2px 2px 1px 2px",
															color: "#FFF",
															marginLeft: "5px",
															background: "#F30",
															borderRadius: "8px",
															display: "inline-block",
															height: "8px",
															width: "8px",
														},
													},
													""
												),
											]
										),
									]);
								} else if (params.row.coin == "FDB") {
									return h("div", [
										h("Icon", {
											props: {
												// color:"red",
												type: params.row.isFavor
													? "ios-star"
													: "ios-star-outline",
											},
											nativeOn: {
												click: () => {
													event.stopPropagation(); //阻止事件冒泡
													if (this.isLogin) {
														if (
															event.currentTarget.className ==
															"ivu-icon ivu-icon-ios-star"
														) {
															this.cancelCollect(params.index, params.row);
															event.currentTarget.className ==
																"ivu-icon ivu-icon-ios-star-outline";
														} else {
															this.collect(params.index, params.row);
															event.currentTarget.className =
																"ivu-icon ivu-icon-ios-star";
														}
													} else {
														this.$Message.warning(this.$t("common.logintip"));
													}
												},
											},
										}),
										h("span", params.row.coin),
										h(
											"Tooltip",
											{
												props: {
													placement: "right",
													content: "首发分摊活动演示",
												},
											},
											[
												h(
													"span",
													{
														style: {
															fontSize: "8px",
															padding: "2px 2px 1px 2px",
															color: "#FFF",
															marginLeft: "5px",
															background: "#F30",
															borderRadius: "8px",
															display: "inline-block",
															height: "8px",
															width: "8px",
														},
													},
													""
												),
											]
										),
									]);
								}
								return h("div", [
									h("Icon", {
										props: {
											// color:"red",
											type: params.row.isFavor ? "ios-star" : "ios-star-outline",
										},
										nativeOn: {
											click: () => {
												event.stopPropagation(); //阻止事件冒泡
												if (this.isLogin) {
													if (
														event.currentTarget.className ==
														"ivu-icon ivu-icon-ios-star"
													) {
														this.cancelCollect(params.index, params.row);
														event.currentTarget.className ==
															"ivu-icon ivu-icon-ios-star-outline";
													} else {
														this.collect(params.index, params.row);
														event.currentTarget.className =
															"ivu-icon ivu-icon-ios-star";
													}
												} else {
													this.$Message.warning(this.$t("common.logintip"));
												}
											},
										},
									}),
									h("span", params.row.coin),
								]);
							},
						},
						{
							title: this.$t("exchange.lastprice"),
							key: "close",
							sortable: true,
							sortMethod: function (a, b, type) {
								let a1 = parseFloat(a);
								let b1 = parseFloat(b);
								if (type == "asc") {
									return a1 - b1;
								} else {
									return b1 - a1;
								}
							},
						},
						{
							title: this.$t("exchange.daychange"),
							key: "rose",
							sortable: true,
							sortMethod: function (a, b, type) {
								let a1 = a.replace(/[^\d|.|-]/g, "") - 0;
								let b1 = b.replace(/[^\d|.|-]/g, "") - 0;
								if (type == "asc") {
									return a1 - b1;
								} else {
									return b1 - a1;
								}
							},
							render: (h, params) => {
								const row = params.row;
								const className = parseFloat(row.rose) < 0 ? "sell" : "buy";
								return h(
									"span",
									{
										attrs: {
											class: className,
										},
									},
									row.rose
								);
							},
						},
					],
				},
				wallet: {
					base: 0.0,
					coin: 0.0,
				},
				showMarket: false,
				fixHistoryHeight: 295,
				// rechargeUrl:'#/finance/recharge',
				// rechargeUSDTUrl:'#/finance/recharge?name=USDT',
				form: {
					buy: {
						limitPrice: 0.0,
						limitAmount: 0.0,
						marketAmount: 0.0,
						limitTurnover: 0.0,
					},
					sell: {
						limitPrice: 0.0,
						limitAmount: 0.0,
						marketAmount: 0.0,
						limitTurnover: 0.0,
					},
				},
				trade: {
					rows: [],
					columns: [
						{
							title: self.$t("exchange.price"),
							key: "price",
							render: (h, params) => {
								const row = params.row;
								const className = row.direction == "BUY" ? "buy" : "sell";

								return h(
									"span",
									{
										attrs: {
											class: className,
										},
									},
									params.row.price.toFixed(this.baseCoinScale)
								);
							},
							renderHeader: (h, params) => {
								const title =
									self.$t("exchange.price") + "(" + self.currentCoin.base + ")";
								return h("span", {}, title);
							},
						},
						{
							title: self.$t("exchange.num"),
							key: "amount",
							render: (h, params) => {
								return h("span", {}, params.row.amount.toFixed(this.coinScale));
							},
							renderHeader: (h, params) => {
								const title =
									self.$t("exchange.num") + "(" + self.currentCoin.coin + ")";
								return h("span", {}, title);
							},
						},
						{
							title: self.$t("exchange.time"),
							key: "time",
							render: (h, params) => {
								return h("span", {}, this.timeFormat(params.row.time));
							},
						},
					],
				},
				//   最新价的 table 数据;
				plate: {
					maxPostion: 10,
					columns: [
						// {
						//   title: self.$t("exchange.stall"),
						//   key: "postion",
						//   render: (h, params) => {
						//     const row = params.row;
						//     const className = row.direction.toLowerCase();
						//     const title =
						//       (row.direction == "BUY"
						//         ? self.$t("exchange.buyin")
						//         : self.$t("exchange.sellout")) +
						//       " " +
						//       row.position;
						//     return h(
						//       "span",
						//       {
						//         attrs: {
						//           class: className
						//         }
						//       },
						//       title
						//     );
						//   }
						// },
						{
							//   价格;
							title: self.$t("exchange.price"),
							key: "price",
							render: (h, params) => {
								let str = "";
								let price = "";
								const className = params.row.direction.toLowerCase();
								params.row.price == 0 && (str = h("span", {}, "--"));
								params.row.price != 0 &&
									(price = params.row.price.toFixed(this.baseCoinScale)) &&
									(str = h(
										"span",
										{
											attrs: {
												class: className,
											},
										},
										price
									));
								return str;
							},
							renderHeader: (h, params) => {
								const title =
									self.$t("exchange.price") + "(" + self.currentCoin.base + ")";
								return h("span", {}, title);
							},
						},
						{
							title: self.$t("exchange.num"),
							key: "amount",
							render: (h, params) => {
								let str = "";
								params.row.amount == 0 && (str = h("span", {}, "--"));
								params.row.amount != 0 &&
									(str = h(
										"span",
										{},
										params.row.amount.toFixed(this.coinScale)
									));
								return str;
							},
							renderHeader: (h, params) => {
								const title =
									self.$t("exchange.num") + "(" + self.currentCoin.coin + ")";
								return h("span", {}, title);
							},
						},
						{
							title: self.$t("exchange.total"),
							key: "totalAmount",
							render: (h, params) => {
								if (params.row.price == 0 || params.row.totalAmount == 0) {
									return h("span", {}, "--");
								} else {
									return h(
										"span",
										{},
										params.row.totalAmount.toFixed(this.coinScale)
									);
								}
							},
							renderHeader: (h, params) => {
								const title =
									self.$t("exchange.total") + "(" + self.currentCoin.coin + ")";
								return h("span", {}, title);
							},
						},
						{
							className: "percenttd",
							width: 1,
							render: (h, params) => {
								let width = "0",
									backgroundColor =
										params.row.direction === "BUY" ? "#00b275" : "#f15057",
									totle =
										params.row.direction === "BUY"
											? this.plate.bidTotle
											: this.plate.askTotle;
								if (params.row.totalAmount) {
									width = (params.row.totalAmount / totle).toFixed(4) * 100 + "%";
								}
								return h(
									"div",
									{
										style: {
											width,
											backgroundColor,
										},
										attrs: {
											class: "percentdiv",
										},
									},
									" "
								);
							},
						},
					],
					askRows: [],
					bidRows: [],
				},
				currentOrder: {
					columns: [
						{
							type: "index",
							width: 40,
							height: 40,
							render: (h, params) => {
								return h(expandRow, {
									props: {
										skin: params.row.skin,
										rows: params.row.detail,
									},
								});
							},
						},
						{
							title: self.$t("exchange.time"),
							key: "time",
							render: (h, params) => {
								return h("span", {}, this.dateFormat(params.row.time));
							},
						},
						{
							title: self.$t("exchange.symbol"),
							key: "symbol",
						},
						{
							title: self.$t("exchange.type"),
							render(h, params) {
								return h(
									"span",
									params.row.type === "LIMIT_PRICE"
										? self.$t("exchange.limited_price")
										: self.$t("exchange.market_price")
								);
							},
						},
						{
							title: self.$t("exchange.direction"),
							key: "direction",
							render: (h, params) => {
								const row = params.row;
								const className = row.direction.toLowerCase();
								return h(
									"span",
									{
										attrs: {
											class: className,
										},
									},
									row.direction == "BUY"
										? self.$t("exchange.buyin")
										: self.$t("exchange.sellout")
								);
							},
						},
						{
							title: self.$t("exchange.price"),
							key: "price",
							render(h, params) {
								return h("span", self.toFloor(params.row.price));
							},
						},
						{
							title: self.$t("exchange.num"),
							key: "amount",
							render(h, params) {
								return h("span", self.toFloor(params.row.amount));
							},
						},
						{
							title: self.$t("exchange.traded"),
							key: "tradedAmount",
							render(h, params) {
								return h("span", self.toFloor(params.row.tradedAmount));
							},
						},
						{
							title: self.$t("exchange.dealamount"),
							key: "turnover",
							render(h, params) {
								return h("span", self.toFloor(params.row.turnover));
							},
						},
						{
							title: self.$t("exchange.fee"),
							key: "turnover",
							render(h, params) {
								return h(
									"span",
									params.row.fee ? self.toFloor(params.row.fee) : "--"
								);
							},
						},
						{
							title: self.$t("exchange.avgPrice"),
							key: "turnover",
							render(h, params) {
								return h(
									"span",
									!params.row.turnover ||
										!params.row.tradedAmount ||
										params.row.tradedAmount <= 0
										? "--"
										: new Number(
											self.toFloor(
												accDiv(
													self.toFloor(params.row.turnover),
													self.toFloor(params.row.tradedAmount)
												)
											)
										).toFixed(6)
								);
							},
						},
						{
							title: self.$t("exchange.action"),
							key: "operate",
							width: 110,
							render: (h, params) => {
								return h(
									"Button",
									{
										props: {
											size: "small",
										},
										style: {
											border: "1px solid #f0ac19",
											color: "#f1ac19",
											"line-height": "1.2",
											"border-radius": "10px",
										},
										on: {
											click: () => {
												// console.log("======开始撤单")
												this.cancel(params.index);
											},
										},
									},
									self.$t("exchange.undo")
								);
							},
						},
					],
					rows: [],
				},
				historyOrder: {
					pageSize: 10,
					total: 10,
					page: 0,
					columns: [
						{
							type: "index",
							width: 40,
							render: (h, params) => {
								return h(expandRow, {
									props: {
										skin: params.row.skin,
										rows: params.row.detail,
									},
								});
							},
						},
						{
							title: self.$t("exchange.time"),
							key: "time",
							render: (h, params) => {
								return h("span", {}, this.dateFormat(params.row.time));
							},
						},
						{
							title: self.$t("exchange.symbol"),
							key: "symbol",
						},
						{
							title: self.$t("exchange.type"),
							render(h, params) {
								return h(
									"span",
									params.row.type === "LIMIT_PRICE"
										? self.$t("exchange.limited_price")
										: self.$t("exchange.market_price")
								);
							},
						},
						{
							title: self.$t("exchange.direction"),
							key: "direction",
							render: (h, params) => {
								const row = params.row;
								const className = row.direction.toLowerCase();
								return h(
									"span",
									{
										attrs: {
											class: className,
										},
									},
									row.direction == "BUY"
										? self.$t("exchange.buyin")
										: self.$t("exchange.sellout")
								);
							},
						},
						{
							title: self.$t("exchange.price"),
							key: "price",
							render(h, params) {
								return h("span", self.toFloor(params.row.price));
							},
						},
						{
							title: self.$t("exchange.num"),
							key: "amount",
							render(h, params) {
								return h("span", self.toFloor(params.row.amount));
							},
						},
						{
							title: self.$t("exchange.done"),
							key: "tradedAmount",
							render(h, params) {
								return h("span", self.toFloor(params.row.tradedAmount));
							},
						},
						{
							title: self.$t("exchange.dealamount"),
							key: "turnover",
							render(h, params) {
								return h("span", self.toFloor(params.row.turnover));
							},
						},
						{
							title: self.$t("exchange.fee"),
							key: "turnover",
							render(h, params) {
								return h(
									"span",
									params.row.fee ? self.toFloor(params.row.fee) : "--"
								);
							},
						},
						{
							title: self.$t("exchange.avgPrice"),
							key: "turnover",
							render(h, params) {
								return h(
									"span",
									!params.row.turnover ||
										!params.row.tradedAmount ||
										params.row.tradedAmount <= 0
										? "--"
										: new Number(
											self.toFloor(
												accDiv(
													self.toFloor(params.row.turnover),
													self.toFloor(params.row.tradedAmount)
												)
											)
										).toFixed(6)
								);
							},
						},
						{
							title: self.$t("exchange.status"),
							key: "status",
							render: (h, params) => {
								const status = params.row.status;
								if (status == "COMPLETED") {
									return h(
										"span",
										{
											style: {
												color: "#f0a70a",
											},
										},
										self.$t("exchange.finished")
									);
								} else if (status == "CANCELED") {
									return h(
										"span",
										{
											style: {
												color: "#7c7f82",
											},
										},
										self.$t("exchange.canceled")
									);
								} else {
									return h("span", {}, "--");
								}
							},
						},
					],
					rows: [],
				},
				fullTrade: {},
			};
		},
		filters: {
			toFixedPublishAmount: function (value) {
				var tem = value.toFixed(7);
				if (value >= 10) {
					tem = value.toFixed(6);
				}
				if (value >= 100) {
					tem = value.toFixed(5);
				}
				if (value >= 1000) {
					tem = value.toFixed(4);
				}
				if (value >= 10000) {
					tem = value.toFixed(3);
				}
				if (value >= 100000) {
					tem = value.toFixed(2);
				}
				if (value >= 1000000) {
					tem = value.toFixed(1);
				}
				if (value >= 10000000) {
					tem = value.toFixed(0);
				}
				return tem;
			},
			toFixedPublishPrice: function (value) {
				var tem = value.toFixed(5).replace(/0+$/g, "");
				if (value >= 1) {
					tem = value.toFixed(4);
				}
				if (value >= 10) {
					tem = value.toFixed(3);
				}
				if (value >= 100) {
					tem = value.toFixed(2);
				}
				if (value >= 1000) {
					tem = value.toFixed(1);
				}
				if (value >= 10000) {
					tem = value.toFixed(1);
				}
				return tem;
			},
		},
		computed: {
			rechargeUSDTUrl: function () {
				return "/uc/recharge?name=" + this.currentCoin.base;
				// return "#/finance/recharge?name=" + this.currentCoin.base;
			},
			rechargeCoinUrl: function () {
				return "/uc/recharge?name=" + this.currentCoin.coin;
				// return "#/finance/recharge?name=" + this.currentCoin.coin;
			},
			isLogin: function () {
				return this.$store.getters.isLogin;
			},
			member: function () {
				return this.$store.getters.member;
			},
			lang: function () {
				return this.$store.state.lang;
			},
			sliderBuyDisabled() {
				let account = this.wallet.base,
					min = this.toFloor(1 / Math.pow(10, this.baseCoinScale));
				return account < min;
			},
			sliderSellDisabled() {
				let account = this.wallet.coin,
					min = this.toFloor(1 / Math.pow(10, this.coinScale));
				return account < min;
			},
		},
		watch: {
			"form.buy.limitPrice": function (val) {
				let price = this.form.buy.limitPrice,
					account = this.wallet.base,
					amount = 0;
				if (val > 0) {
					amount = this.toFloor(
						account.div(price).mul(this.sliderBuyLimitPercent).mul(0.01),
						this.coinScale
					);
				}
				this.form.buy.limitAmount = amount;
				this.form.buy.limitTurnover = this.toFloor(
					val.mul(amount),
					this.baseCoinScale
				);
			},
			"form.buy.limitAmount": function (val) {
				this.form.buy.limitTurnover = this.toFloor(
					val.mul(this.form.buy.limitPrice),
					this.baseCoinScale
				);
			},
			"form.sell.limitPrice": function (val) {
				this.form.sell.limitTurnover = this.toFloor(
					val.mul(this.form.sell.limitAmount),
					this.coinScale
				);
			},
			"form.sell.limitAmount": function (val) {
				this.form.sell.limitTurnover = this.toFloor(
					val.mul(this.form.sell.limitPrice),
					this.coinScale
				);
			},
			lang: function () {
				this.updateLangData();
			},
			$route(to, from) {
				this.init();
			},
			sliderBuyLimitPercent() {
				let price = this.form.buy.limitPrice,
					account = this.wallet.base,
					amount = 0;
				if (price > 0) {
					amount = this.toFloor(
						account.div(price).mul(this.sliderBuyLimitPercent).mul(0.01),
						this.coinScale
					);
				}
				this.form.buy.limitAmount = amount;
			},
			sliderSellLimitPercent() {
				let account = this.wallet.coin;
				this.form.sell.limitAmount = this.toFloor(
					account.mul(this.sliderSellLimitPercent).mul(0.01),
					this.coinScale
				);
			},
			sliderBuyMarketPercent() {
				let price = this.form.buy.limitPrice;
				let account = this.wallet.base;
				this.form.buy.marketAmount = this.toFloor(
					account.mul(this.sliderBuyMarketPercent).mul(0.01),
					this.baseCoinScale
				);
			},
			sliderSellMarketPercent() {
				let account = this.wallet.coin;
				this.form.sell.marketAmount = this.toFloor(
					account.mul(this.sliderSellMarketPercent).mul(0.01),
					this.coinScale
				);
			},
		},
		created: function () {
			this.init();
			// console.log(this.coins);
			// console.log(dataIndex);
			// console.log(this.plate.askRows);
			// console.log(this.plate);
			// console.log(this.trade);
			console.log(this.historyOrder)
		},
		mounted: function () {
			// console.log(this.coins);
		},
		methods: {
			ChangeSelectVal(event) {
				if (event.target.value == 0) {
					this.limited_price()
				} else {
					this.market_price()
				}
			},
			openModalRevoke(index) {
				this.isModalOpen = true;
				this.modalIndex = index;
			},
			seachInputChange() {
				this.searchKey = this.searchKey.toUpperCase();
				if (this.basecion == "favor") {
					this.dataIndex = this.coins.favor.filter(
						(item) => item["coin"].indexOf(this.searchKey) == 0
					);
				} else if (this.basecion == "usdt") {
					this.dataIndex = this.coins.USDT.filter(
						(item) => item["coin"].indexOf(this.searchKey) == 0
					);
				} else if (this.basecion == "btc") {
					this.dataIndex = this.coins.BTC.filter(
						(item) => item["coin"].indexOf(this.searchKey) == 0
					);
				} else if (this.basecion == "eth") {
					this.dataIndex = this.coins.ETH.filter(
						(item) => item["coin"].indexOf(this.searchKey) == 0
					);
				}
			},
			silderGo(silder, val) {
				this[silder] = val;
			},
			init() {
				var params = this.$route.params.pair;
				if (params == undefined) {
					this.$router.push("/exchange/" + this.defaultPath);
					params = this.defaultPath;
				}
				const basecion = params.split("_")[1];
				if (basecion) {
					this.basecion = basecion;
				}
				var coin = params.toUpperCase().split("_")[0];
				var base = params.toUpperCase().split("_")[1];
				this.currentCoin.symbol = coin + "/" + base;
				this.currentCoin.coin = coin;
				this.currentCoin.base = base;
				this.$store.commit("navigate", "nav-exchange");
				this.$store.commit("setSkin", this.skin);
				this.getSymbolScale();
				this.getCoinInfo();
				this.getSymbol(); //包含 K线图、getFavor、startWebsock等
				this.getPlate(); //买卖盘
				this.getPlateFull();
				this.getTrade();
				if (this.isLogin) {
					this.getWallet(); //账户资产信息
					this.getCurrentOrder(); //当前委托
					this.getHistoryOrder(); //历史委托
				}
				this.sliderBuyLimitPercent = 0;
				this.sliderSellLimitPercent = 0;
				this.sliderBuyMarketPercent = 0;
			},
			tipFormat(val) {
				return val + "%";
			},
			changeBaseCion(str) {
				this.basecion = str;
				if (str == "usdt") {
					this.dataIndex = this.coins.USDT;
					this.dataIndex2 = this.coins.USDT2;
				} else if (str == "btc") {
					this.dataIndex = this.coins.BTC;
					this.dataIndex2 = this.coins.BTC2;
				} else if (str == "eth") {
					this.dataIndex = this.coins.ETH;
					this.dataIndex2 = this.coins.ETH2;
				} else if (str == "favor") {
					this.dataIndex = this.coins.favor;
				}
			},
			changePlate(str) {
				if (str != "all") {
					this.plate.maxPostion = 20;
				} else {
					this.plate.maxPostion = 10;
				}
				this.getPlate(str);
				//this.selectedPlate = str;
			},
			changeImgTable(str) {
				this.currentImgTable = str;
			},
			changeOrder(str) {
				this.selectedOrder = str;
			},
			setback() {
				var obk = document.getElementsByClassName("container")[0];
				var height = 0;
				var doc = document;
				window.innerHeight && (height = window.innerHeight);
				!window.innerHeight &&
					doc.body.clientHeight &&
					(height = doc.body.clientHeight);
				!window.innerHeight &&
					!doc.body.clientHeight &&
					doc.documentElement.clientHeight &&
					(height = doc.documentElement.clientHeight);
				obk.style.minHeight = height - 100 + "px";
			},
			updateLangData() {
				this.favorColumns[0].title = this.$t("exchange.coin");
				this.favorColumns[1].title = this.$t("exchange.lastprice");
				this.favorColumns[2].title = this.$t("exchange.daychange");

				this.coins.columns[0].title = this.$t("exchange.coin");
				this.coins.columns[1].title = this.$t("exchange.lastprice");
				this.coins.columns[2].title = this.$t("exchange.daychange");
				// this.coins.columns[3].title = this.$t("exchange.favorite");

				this.trade.columns[0].title = "abc";
				// this.trade.columns[0].title = this.$t("exchange.num");
				// this.trade.columns[1].title = this.$t("exchange.price");
				// this.trade.columns[2].title = this.$t("exchange.time");
				// this.trade.columns[3].title = this.$t("exchange.direction");

				this.plate.columns[0].title = this.$t("exchange.stall");
				this.plate.columns[1].title = this.$t("exchange.price");
				this.plate.columns[2].title = this.$t("exchange.num");
				this.plate.columns[3].title = this.$t("exchange.total");

				this.currentOrder.columns[1].title = this.$t("exchange.time");
				this.currentOrder.columns[2].title = this.$t("exchange.symbol");
				this.currentOrder.columns[3].title = this.$t("exchange.type");
				this.currentOrder.columns[4].title = this.$t("exchange.direction");
				this.currentOrder.columns[5].title = this.$t("exchange.price");
				this.currentOrder.columns[6].title = this.$t("exchange.num");
				this.currentOrder.columns[7].title = this.$t("exchange.traded");
				this.currentOrder.columns[8].title = this.$t("exchange.dealamount");
				this.currentOrder.columns[9].title = this.$t("exchange.action");

				this.historyOrder.columns[1].title = this.$t("exchange.time");
				this.historyOrder.columns[2].title = this.$t("exchange.symbol");
				this.historyOrder.columns[3].title = this.$t("exchange.type");
				this.historyOrder.columns[4].title = this.$t("exchange.direction");
				this.historyOrder.columns[5].title = this.$t("exchange.price");
				this.historyOrder.columns[6].title = this.$t("exchange.num");
				this.historyOrder.columns[7].title = this.$t("exchange.done");
				this.historyOrder.columns[8].title = this.$t("exchange.dealamount");
				this.historyOrder.columns[9].title = this.$t("exchange.status");

				// window.tvWidget.options.time_frames[0].title = this.$t("exchange.realtime");
			},
			getCNYRate() {
				// this.$http
				// 	.post(this.host + "/market/exchange-rate/usd-cny")
				// 	.then(response => {
				// 		var resp = response.body;
				// 		this.CNYRate = resp.data;
				// 	});
				if (this.basecion === "usdt") {
					this.CNYRate = 1;
				} else if (this.basecion === "usdt") {
					for (let item of this.coins.USDT) {
						if (item.coin === "BTC") {
							this.CNYRate = item.price;
						}
					}
				} else {
					for (let item of this.coins.USDT) {
						if (item.coin === "ETH") {
							this.CNYRate = item.price;
						}
					}
				}
			},
			getCoin(symbol) {
				return this.coins._map[symbol];
			},
			getKline() {
				var that = this;
				let config = {
					autosize: true,
					height: 320,
					fullscreen: true,
					symbol: that.symbol,
					interval: "60", // 默认K线周期
					timezone: "Asia/Shanghai",
					toolbar_bg: "#18202a",
					container_id: "kline_container",
					datafeed: that.datafeed,
					library_path:
						process.env.NODE_ENV === "production"
							? "/assets/charting_library/"
							: "/src/assets/js/charting_library/",
					locale: "zh",
					debug: false,
					drawings_access: {
						type: "black",
						tools: [
							{
								name: "Regression Trend",
							},
						],
					},
					disabled_features: [
						"header_resolutions",
						"timeframes_toolbar",
						"header_symbol_search",
						"header_chart_type",
						"header_compare",
						"header_undo_redo",
						"header_screenshot",
						"header_saveload",
						"use_localstorage_for_settings",
						"left_toolbar",
						"volume_force_overlay",
					],
					enabled_features: [
						"hide_last_na_study_output",
						"move_logo_to_main_pane",
					],
					custom_css_url: "bundles/common.css",
					supported_resolutions: ["1", "5", "15", "30", "60", "1D", "1W", "1M"],
					charts_storage_url: "http://saveload.tradingview.com",
					charts_storage_api_version: "1.1",
					client_id: "tradingview.com",
					user_id: "public_user_id",
					overrides: {
						"paneProperties.background": "#1B1E2E",
						"paneProperties.vertGridProperties.color": "rgba(0,0,0,.1)",
						"paneProperties.horzGridProperties.color": "rgba(0,0,0,.1)",
						//"scalesProperties.textColor" : "#AAA",
						"scalesProperties.textColor": "#61688A",
						"mainSeriesProperties.candleStyle.upColor": "#00B564", //蜡烛绿色
						"mainSeriesProperties.candleStyle.downColor": "#f94d55", //蜡烛红色
						"mainSeriesProperties.candleStyle.drawBorder": false,
						"mainSeriesProperties.candleStyle.wickUpColor": "#589065",
						"mainSeriesProperties.candleStyle.wickDownColor": "#AE4E54",
						"paneProperties.legendProperties.showLegend": false,

						"mainSeriesProperties.areaStyle.color1": "rgba(71, 78, 112, 0.5)",
						"mainSeriesProperties.areaStyle.color2": "rgba(71, 78, 112, 0.5)",
						"mainSeriesProperties.areaStyle.linecolor": "#9194a4",
						volumePaneSize: "small",
					},
					studies_overrides: {
						"volume.volume.color.0": "rgba( 249, 77, 85, 0.7)",
						"volume.volume.color.1": "rgba( 0, 181, 100, 0.7)",
						"volume.volume.transparency": 100,
					},
					time_frames: [
						{
							text: "1min",
							resolution: "1",
							description: "realtime",
							title: that.$t("exchange.realtime"),
						},
						{
							text: "1min",
							resolution: "1",
							description: "1min",
						},
						{
							text: "5min",
							resolution: "5",
							description: "5min",
						},
						{
							text: "15min",
							resolution: "15",
							description: "15min",
						},
						{
							text: "30min",
							resolution: "30",
							description: "30min",
						},
						{
							text: "1hour",
							resolution: "60",
							description: "1hour",
							title: "1hour",
						},
						/*{ text: "4hour", resolution: "240", description: "4hour",title: "4hour" },*/
						{
							text: "1day",
							resolution: "1D",
							description: "1day",
							title: "1day",
						},
						{
							text: "1week",
							resolution: "1W",
							description: "1week",
							title: "1week",
						},
						{
							text: "1mon",
							resolution: "1M",
							description: "1mon",
						},
					],
				};
				if (that.skin === "day") {
					config.toolbar_bg = "#fff";
					config.custom_css_url = "bundles/common_day.css";
					config.overrides["paneProperties.background"] = "#fff";
					config.overrides["mainSeriesProperties.candleStyle.upColor"] =
						"#a6d3a5";
					config.overrides["mainSeriesProperties.candleStyle.downColor"] =
						"#ffa5a6";
				}
				require(["@js/charting_library/charting_library.min.js"], function (tv) {
					var widget = (window.tvWidget = new TradingView.widget(config));
					widget.onChartReady(function () {
						widget.chart().executeActionById("drawingToolbarAction");
						widget
							.chart()
							.createStudy("Moving Average", false, false, [5], null, {
								"plot.color": "#EDEDED",
							});
						widget
							.chart()
							.createStudy("Moving Average", false, false, [10], null, {
								"plot.color": "#ffe000",
							});
						widget
							.chart()
							.createStudy("Moving Average", false, false, [30], null, {
								"plot.color": "#ce00ff",
							});
						widget
							.chart()
							.createStudy("Moving Average", false, false, [60], null, {
								"plot.color": "#00adff",
							});
						widget
							.createButton()
							.attr("title", "realtime")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(3);
								widget.setSymbol("", "1");
							})
							.append("<span>分时</span>");

						widget
							.createButton()
							.attr("title", "M1")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(1);
								widget.setSymbol("", "1");
							})
							.append("<span>M1</span>");

						widget
							.createButton()
							.attr("title", "M5")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(1);
								widget.setSymbol("", "5");
							})
							.append("<span>M5</span>");

						widget
							.createButton()
							.attr("title", "M15")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(1);
								widget.setSymbol("", "15");
							})
							.append("<span>M15</span>");

						widget
							.createButton()
							.attr("title", "M30")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(1);
								widget.setSymbol("", "30");
							})
							.append("<span>M30</span>");

						widget
							.createButton()
							.attr("title", "H1")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(1);
								widget.setSymbol("", "60");
							})
							.append("<span>H1</span>")
							.addClass("selected");

						widget
							.createButton()
							.attr("title", "D1")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(1);
								widget.setSymbol("", "1D");
							})
							.append("<span>D1</span>");

						widget
							.createButton()
							.attr("title", "W1")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(1);
								widget.setSymbol("", "1W");
							})
							.append("<span>W1</span>");

						widget
							.createButton()
							.attr("title", "M1")
							.on("click", function () {
								if ($(this).hasClass("selected")) return;
								$(this)
									.addClass("selected")
									.parent(".group")
									.siblings(".group")
									.find(".button.selected")
									.removeClass("selected");
								widget.chart().setChartType(1);
								widget.setSymbol("", "1M");
							})
							.append("<span>M1</span>");
					});
				});
			},
			getFavor() {
				//查询自选(收藏)
				this.$http
					.post(this.host + this.api.exchange.favorFind, {})
					.then((response) => {
						this.coins.favor = [];
						this.currentCoinIsFavor = false;
						var resp = response.body;
						for (var i = 0; i < resp.length; i++) {
							var coin = this.getCoin(resp[i].symbol);
							if (coin != null) {
								coin.isFavor = true;
								this.coins.favor.push(coin);
							}
							if (this.currentCoin.symbol == resp[i].symbol) {
								this.currentCoinIsFavor = true;
							}
						}
					});
			},
			getSymbol() {
				this.$http
					.post(this.host + this.api.market.thumb, {})
					.then((response) => {
						var resp = response.body;
						//先清空已有数据
						for (var i = 0; i < resp.length; i++) {
							var coin = resp[i];
							coin.base = resp[i].symbol.split("/")[1];
							this.coins[coin.base] = [];
							this.coins[coin.base + "2"] = [];
							this.coins._map = [];
							this.coins.favor = [];
						}
						for (var i = 0; i < resp.length; i++) {
							var coin = resp[i];
							coin.price = resp[i].close = resp[i].close.toFixed(
								this.baseCoinScale
							);
							coin.rose =
								resp[i].chg > 0
									? "+" + (resp[i].chg * 100).toFixed(2) + "%"
									: (resp[i].chg * 100).toFixed(2) + "%";
							coin.coin = resp[i].symbol.split("/")[0];
							coin.base = resp[i].symbol.split("/")[1];
							coin.href = (coin.coin + "_" + coin.base).toLowerCase();
							coin.isFavor = false;
							this.coins._map[coin.symbol] = coin;
							if (coin.zone == 0) {
								this.coins[coin.base].push(coin);
							} else {
								this.coins[coin.base + "2"].push(coin);
							}
							if (coin.symbol == this.currentCoin.symbol) {
								this.currentCoin = coin;
								this.form.buy.limitPrice = this.form.sell.limitPrice = coin.price;
							}
						}
						if (this.isLogin) {
							this.getFavor();
						}
						require(["../../assets/js/exchange.js"], function (e) {
							e.clickScTab();
						});
						this.startWebsock();
						this.changeBaseCion(this.basecion);
						this.getCNYRate();
					});
			},
			getCoinInfo() {
				//获取精度
				this.$http
					.post(this.host + this.api.market.coinInfo, {
						unit: this.currentCoin.coin,
					})
					.then((response) => {
						var resp = response.body;
						if (resp != null) {
							this.coinInfo = resp;
						}
					});
			},
			getSymbolScale() {
				//获取精度
				this.$http
					.post(this.host + this.api.market.symbolInfo, {
						symbol: this.currentCoin.symbol,
					})
					.then((response) => {
						var resp = response.body;
						if (resp != null) {
							this.currentCoin.coinScale = resp.coinScale;
							this.currentCoin.baseCoinScale = resp.baseCoinScale;

							this.baseCoinScale = resp.baseCoinScale;
							this.coinScale = resp.coinScale;
							this.symbolFee = resp.fee;

							this.enableMarketBuy = resp.enableMarketBuy;
							this.enableMarketSell = resp.enableMarketSell;

							this.exchangeable = resp.exchangeable;

							this.publishType = resp.publishType;
							this.startTime = resp.startTime;
							this.endTime = resp.endTime;
							this.clearTime = resp.clearTime;
							this.currentTime = parseInt(resp.currentTime / 1000);
							this.publishAmount = resp.publishAmount;
							this.publishPrice = resp.publishPrice;

							var temCurT = moment(resp.currentTime).format(
								"YYYY-MM-DD HH:mm:ss"
							);
							if (temCurT < this.clearTime) {
								if (
									this.publishType == "QIANGGOU" ||
									this.publishType == "FENTAN"
								) {
									this.showPublish = true;
									this.showCountDown = true;
								} else {
									this.showPublish = false;
									this.showCountDown = false;
								}
							} else {
								this.showPublish = false;
								this.showCountDown = false;
							}

							// 获取币种信息
						}
					});
			},
			getPlate(str = "") {
				//买卖盘
				var params = {};
				params["symbol"] = this.currentCoin.symbol;
				this.$http
					.post(this.host + this.api.market.platemini, params)
					.then((response) => {
						this.plate.askRows = [];
						this.plate.bidRows = [];
						let resp = response.body;
						if (resp.ask && resp.ask.items) {
							for (var i = 0; i < resp.ask.items.length; i++) {
								if (i == 0) {
									resp.ask.items[i].totalAmount = resp.ask.items[i].amount;
								} else {
									resp.ask.items[i].totalAmount =
										resp.ask.items[i - 1].totalAmount + resp.ask.items[i].amount;
								}
							}
							if (resp.ask.items.length >= this.plate.maxPostion) {
								for (var i = this.plate.maxPostion; i > 0; i--) {
									var ask = resp.ask.items[i - 1];
									ask.direction = "SELL";
									ask.position = i;
									this.plate.askRows.push(ask);
								}
								const rows = this.plate.askRows,
									len = rows.length,
									totle = rows[0].totalAmount;
								this.plate.askTotle = totle;
							} else {
								for (
									var i = this.plate.maxPostion;
									i > resp.ask.items.length;
									i--
								) {
									var ask = {
										price: 0,
										amount: 0,
									};
									ask.direction = "SELL";
									ask.position = i;
									ask.totalAmount = ask.amount;
									this.plate.askRows.push(ask);
								}
								for (var i = resp.ask.items.length; i > 0; i--) {
									var ask = resp.ask.items[i - 1];
									ask.direction = "SELL";
									ask.position = i;
									this.plate.askRows.push(ask);
								}
								var askItemIndex =
									resp.ask.items.length - 1 > 0 ? resp.ask.items.length - 1 : 0;
								const rows = this.plate.askRows,
									len = rows.length,
									totle = rows[askItemIndex].totalAmount;
								this.plate.askTotle = totle;
							}
						}
						if (resp.bid && resp.bid.items) {
							for (var i = 0; i < resp.bid.items.length; i++) {
								if (i == 0)
									resp.bid.items[i].totalAmount = resp.bid.items[i].amount;
								else
									resp.bid.items[i].totalAmount =
										resp.bid.items[i - 1].totalAmount + resp.bid.items[i].amount;
							}
							for (var i = 0; i < resp.bid.items.length; i++) {
								var bid = resp.bid.items[i];
								bid.direction = "BUY";
								bid.position = i + 1;
								this.plate.bidRows.push(bid);
								if (i == this.plate.maxPostion - 1) break;
							}
							if (resp.bid.items.length < this.plate.maxPostion) {
								for (
									var i = resp.bid.items.length;
									i < this.plate.maxPostion;
									i++
								) {
									var bid = {
										price: 0,
										amount: 0,
									};
									bid.direction = "BUY";
									bid.position = i + 1;
									bid.totalAmount = 0;
									this.plate.bidRows.push(bid);
								}
								var bidItemIndex =
									resp.bid.items.length - 1 > 0 ? resp.bid.items.length - 1 : 0;
								const rows = this.plate.bidRows,
									len = rows.length,
									totle = rows[bidItemIndex].totalAmount;
								this.plate.bidTotle = totle;
							} else {
								const rows = this.plate.bidRows,
									len = rows.length,
									totle = rows[len - 1].totalAmount;
								this.plate.bidTotle = totle;
							}
						}
						if (str != "") {
							this.selectedPlate = str;
						}
					});
			},
			getPlateFull() {
				//深度图
				var params = {};
				params["symbol"] = this.currentCoin.symbol;
				this.$http
					.post(this.host + this.api.market.platefull, params)
					.then((response) => {
						var resp = response.body;
						this.fullTrade = resp;
						resp.skin = this.skin;
						this.$refs.depthGraph.draw(resp);
					});
			},
			updatePlate(type, row) {
				//发现该方法未被使用
				if (type == "sell") {
					for (var i = 0; i < this.plate.askRows.length; i++) {
						if (
							row.price > this.plate.askRows[i].price &&
							i != 0 &&
							this.plate.askRows[i].price > 0
						) {
							this.plate.askRows.splice(i, 0, row);
							this.plate.askRows.shift();
							break;
						} else if (
							i == this.plate.askRows.length - 1 &&
							(row.price < this.plate.askRows[i].price ||
								this.plate.askRows[i].price == 0)
						) {
							this.plate.askRows.push(row);
							this.plate.askRows.shift();
							break;
						}
					}
				} else if (type == "buy") {
					for (var i = 0; i < this.plate.bidRows.length; i++) {
						if (row.price > this.plate.bidRows[i].price) {
							this.plate.bidRows.splice(i, 0, row);
							this.plate.bidRows.pop();
							break;
						}
					}
				}
			},
			getTrade() {
				var params = {};
				params["symbol"] = this.currentCoin.symbol;
				params["size"] = 20;
				this.$http
					.post(this.host + this.api.market.trade, params)
					.then((response) => {
						this.trade.rows = [];
						var resp = response.body;
						for (var i = 0; i < resp.length; i++) {
							this.trade.rows.push(resp[i]);
						}
						for (let ii = 0; ii < this.trade.rows.length; ii++) {

							// function msToTime(s) {
							// 	var ms = s % 1000;
							// 	s = (s - ms) / 1000;
							// 	var secs = s % 60;
							// 	s = (s - secs) / 60;
							// 	var mins = s % 60;
							// 	var hrs = (s - mins) / 60;

							// 	return hrs + ':' + mins + ':' + secs 
							// }
							// this.trade.rows[ii].time = msToTime(this.trade.rows[ii].time)

							// this.trade.rows[ii].time  = new Date(this.trade.rows[ii].time * 1000).toISOString().slice(11, -1);
						}
					});
			},
			startWebsock() {
				if (this.stompClient) {
					this.stompClient.ws.close();
				}
				var stompClient = null;
				var that = this;
				var socket = new SockJS(that.host + that.api.market.ws);
				stompClient = Stomp.over(socket);
				this.stompClient = stompClient;
				stompClient.debug = false;
				// this.datafeed = new Datafeeds.WebsockFeed(that.host+'/market',this.currentCoin,stompClient);
				// this.getKline();
				stompClient.connect({}, function (frame) {
					that.datafeed = new Datafeeds.WebsockFeed(
						that.host + "/market",
						that.currentCoin,
						stompClient,
						that.baseCoinScale
					);
					that.getKline();
					//订阅价格变化消息
					stompClient.subscribe("/topic/market/thumb", function (msg) {
						var resp = JSON.parse(msg.body);
						var coin = that.getCoin(resp.symbol);
						if (coin != null) {
							// coin.price = resp.close.toFixed(2);
							coin.price = resp.close;
							coin.rose =
								resp.chg > 0
									? "+" + (resp.chg * 100).toFixed(2) + "%"
									: (resp.chg * 100).toFixed(2) + "%";
							// coin.close = resp.close.toFixed(2);
							// coin.high = resp.high.toFixed(2);
							// coin.low = resp.low.toFixed(2);
							coin.close = resp.close;
							coin.high = resp.high;
							coin.low = resp.low;
							coin.turnover = parseInt(resp.volume);
							coin.volume = resp.volume;
							coin.usdRate = resp.usdRate;
						}
					});
					//订阅实时成交信息
					stompClient.subscribe(
						"/topic/market/trade/" + that.currentCoin.symbol,
						function (msg) {
							var resp = JSON.parse(msg.body);
							if (resp.length > 0) {
								for (var i = 0; i < resp.length; i++) {
									that.trade.rows.unshift(resp[i]);
								}
							}
							if (that.trade.rows.length > 30) {
								that.trade.rows = that.trade.rows.slice(0, 30);
							}
						}
					);
					if (that.isLogin) {
						//订阅委托取消信息
						stompClient.subscribe(
							"/topic/market/order-canceled/" +
							that.currentCoin.symbol +
							"/" +
							that.member.id,
							function (msg) {
								var resp = JSON.parse(msg.body);
								that.refreshAccount();
							}
						);
						//订阅委托交易完成
						stompClient.subscribe(
							"/topic/market/order-completed/" +
							that.currentCoin.symbol +
							"/" +
							that.member.id,
							function (msg) {
								var resp = JSON.parse(msg.body);
								that.refreshAccount();
							}
						);
						//订阅委托部分交易
						stompClient.subscribe(
							"/topic/market/order-trade/" +
							that.currentCoin.symbol +
							"/" +
							that.member.id,
							function (msg) {
								var resp = JSON.parse(msg.body);
								that.refreshAccount();
							}
						);
					}

					//订阅盘口消息
					stompClient.subscribe(
						"/topic/market/trade-plate/" + that.currentCoin.symbol,
						function (msg) {
							var resp = JSON.parse(msg.body);
							if (resp.direction == "SELL") {
								var asks = resp.items;
								that.plate.askRows = [];
								let totle = 0;
								for (var i = that.plate.maxPostion - 1; i >= 0; i--) {
									var ask = {};
									if (i < asks.length) {
										ask = asks[i];
									} else {
										ask["price"] = 0;
										ask["amount"] = 0;
									}
									ask.direction = "SELL";
									ask.position = i + 1;
									that.plate.askRows.push(ask);
								}
								for (var i = that.plate.askRows.length - 1; i >= 0; i--) {
									if (
										i == that.plate.askRows.length - 1 ||
										that.plate.askRows[i].price == 0
									) {
										that.plate.askRows[i].totalAmount =
											that.plate.askRows[i].amount;
									} else {
										that.plate.askRows[i].totalAmount =
											that.plate.askRows[i + 1].totalAmount +
											that.plate.askRows[i].amount;
									}
									totle += that.plate.askRows[i].amount;
								}
								that.plate.askTotle = totle;
							} else {
								var bids = resp.items;
								that.plate.bidRows = [];
								let totle = 0;
								for (var i = 0; i < that.plate.maxPostion; i++) {
									var bid = {};
									if (i < bids.length) {
										bid = bids[i];
									} else {
										bid["price"] = 0;
										bid["amount"] = 0;
									}
									bid.direction = "BUY";
									bid.position = i + 1;
									that.plate.bidRows.push(bid);
								}
								for (var i = 0; i < that.plate.bidRows.length; i++) {
									if (i == 0 || that.plate.bidRows[i].amount == 0) {
										that.plate.bidRows[i].totalAmount =
											that.plate.bidRows[i].amount;
									} else {
										that.plate.bidRows[i].totalAmount =
											that.plate.bidRows[i - 1].totalAmount +
											that.plate.bidRows[i].amount;
									}
									totle += that.plate.bidRows[i].amount;
								}
								that.plate.bidTotle = totle;
							}
							if (that.currentImgTable == "s") {
								that.getPlateFull();
							}
						}
					);
				});
			},
			limited_price() {
				this.showMarket = false;
			},
			market_price() {
				this.showMarket = true;
			},
			currentCoinFavorChange() {
				if (!this.isLogin) {
					this.$Message.warning(this.$t("common.logintip"));
					return;
				}
				if (this.collecRequesting) {
					return;
				}
				const symbol = this.currentCoin.symbol;
				this.collecRequesting = true;
				if (this.currentCoinIsFavor) {
					//已收藏,去取消收藏
					this.$http
						.post(this.host + this.api.exchange.favorDelete, {
							symbol,
						})
						.then((response) => {
							var resp = response.body;
							if (resp.code == 0) {
								this.$Message.info(this.$t("exchange.cancel_favorite"));
								this.getSymbol(); //刷新状态
								this.currentCoinIsFavor = false;
							}
							this.collecRequesting = false;
						});
				} else {
					//去添加收藏
					this.$http
						.post(this.host + this.api.exchange.favorAdd, {
							symbol,
						})
						.then((response) => {
							var resp = response.body;
							if (resp.code == 0) {
								this.$Message.info(this.$t("exchange.do_favorite"));
								this.getSymbol(); //刷新状态
								this.currentCoinIsFavor = true;
							}
							this.collecRequesting = false;
						});
				}
			},
			collect(index, row) {
				if (!this.isLogin) {
					this.$Message.info(this.$t("common.logintip"));
					return;
				}
				var params = {};
				params["symbol"] = row.symbol;
				this.$http
					.post(this.host + this.api.exchange.favorAdd, params)
					.then((response) => {
						var resp = response.body;
						if (resp.code == 0) {
							this.$Message.info(this.$t("exchange.do_favorite"));
							this.getCoin(row.symbol).isFavor = true;
							row.isFavor = true;
							this.coins.favor.push(row);
							if (this.currentCoin.symbol == row.symbol) {
								this.currentCoinIsFavor = true;
							}
						}
					});
			},
			cancelCollect(index, row) {
				if (!this.isLogin) {
					this.$Message.info(this.$t("common.logintip"));
					return;
				}
				var params = {};
				params["symbol"] = row.symbol;
				this.$http
					.post(this.host + this.api.exchange.favorDelete, params)
					.then((response) => {
						var resp = response.body;
						if (resp.code == 0) {
							this.$Message.info(this.$t("exchange.cancel_favorite"));
							this.getCoin(row.symbol).isFavor = false;
							for (var i = 0; i < this.coins.favor.length; i++) {
								var favorCoin = this.coins.favor[i];
								if (favorCoin.symbol == row.symbol) {
									this.coins.favor.splice(i, 1);
									break;
								}
							}
							if (this.currentCoin.symbol == row.symbol) {
								this.currentCoinIsFavor = false;
							}
						}
					});
			},
			gohref(currentRow, oldCurrentRow) {
				this.$router.push({
					name: "ExchangePair",
					params: {
						pair: currentRow.href,
					},
				});
			},
			buyWithLimitPrice() {
				if (this.form.buy.limitAmount == "") {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						desc: this.$t("exchange.buyamounttip"),
					});
					return;
				}
				var maxAmount = this.wallet.base / this.form.buy.limitPrice;
				if (this.form.buy.limitAmount > maxAmount) {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						desc:
							this.$t("exchange.buyamounttipwarning") + this.toFloor(maxAmount),
					});
					return;
				}
				var that = this;
				var params = {};
				params["symbol"] = this.currentCoin.symbol;
				params["price"] = this.form.buy.limitPrice;
				params["amount"] = this.form.buy.limitAmount;
				params["direction"] = "BUY";
				params["type"] = "LIMIT_PRICE";
				params["useDiscount"] = this.isUseBHB ? "1" : "0"; //是否试用手续费抵扣,0 不使用 1使用
				this.$http
					.post(this.host + this.api.exchange.orderAdd, params)
					.then((response) => {
						var resp = response.body;
						if (resp.code == 0) {
							this.$Notice.success({
								title: that.$t("exchange.tip"),
								desc: that.$t("exchange.success"),
							});
							this.getWallet();
							this.getCurrentOrder();
							this.getHistoryOrder();
							this.form.buy.limitAmount = 0;
							this.sliderBuyLimitPercent = 0;
						} else {
							this.$Notice.error({
								title: that.$t("exchange.tip"),
								desc: resp.message,
							});
						}
					});
			},
			buyWithMarketPrice() {
				if (this.form.buy.marketAmount == "") {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						desc: this.$t("exchange.pricetip"),
					});
					return;
				}
				if (this.form.buy.marketAmount > parseFloat(this.wallet.base)) {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						desc: this.$t("exchange.pricetipwarning") + this.wallet.base,
					});
					return;
				}
				var params = {};
				params["symbol"] = this.currentCoin.symbol;
				params["price"] = 0;
				params["amount"] = this.form.buy.marketAmount;
				params["direction"] = "BUY";
				params["type"] = "MARKET_PRICE";
				params["useDiscount"] = this.isUseBHB ? "1" : "0"; //是否试用手续费抵扣,0 不使用 1使用
				var that = this;
				this.$http
					.post(this.host + this.api.exchange.orderAdd, params)
					.then((response) => {
						var resp = response.body;
						if (resp.code == 0) {
							this.$Notice.success({
								title: that.$t("exchange.tip"),
								desc: that.$t("exchange.success"),
							});
							this.refreshAccount();
							this.sliderBuyMarketPercent = 0;
						} else {
							this.$Notice.error({
								title: that.$t("exchange.tip"),
								desc: resp.message,
							});
						}
					});
			},
			sellLimitPrice() {
				if (this.form.sell.limitAmount == "") {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						desc: this.$t("exchange.sellamounttip"),
					});
					return;
				}
				if (this.form.sell.limitPrice == "") {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						desc: this.$t("exchange.sellpricetip"),
					});
					return;
				}
				if (this.form.sell.limitAmount > parseFloat(this.wallet.coin)) {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						desc: "最多能卖" + this.wallet.coin + "个",
					});
					return;
				}
				var params = {};
				params["symbol"] = this.currentCoin.symbol;
				params["price"] = this.form.sell.limitPrice;
				params["amount"] = this.form.sell.limitAmount;
				params["direction"] = "SELL";
				params["type"] = "LIMIT_PRICE";
				params["useDiscount"] = this.isUseBHB ? "1" : "0"; //是否试用手续费抵扣,0 不使用 1使用
				var that = this;
				this.$http
					.post(this.host + this.api.exchange.orderAdd, params)
					.then((response) => {
						var resp = response.body;

						if (resp.code == 0) {
							this.$Notice.success({
								title: that.$t("exchange.tip"),
								desc: that.$t("exchange.success"),
							});
							this.refreshAccount();
							this.form.sell.limitAmount = 0;
							this.sliderSellLimitPercent = 0;
						} else {
							this.$Notice.error({
								title: that.$t("exchange.tip"),
								desc: resp.message,
							});
						}
					});
			},
			sellMarketPrice() {
				if (this.form.sell.marketAmount == "") {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						desc: this.$t("exchange.sellamounttip"),
					});
					return;
				}
				if (this.form.sell.marketAmount > parseFloat(this.wallet.coin)) {
					this.$Notice.error({
						title: this.$t("exchange.tip"),
						// desc: this.$t("exchange.sellamounttipwarning") + this.wallet.coin
						desc: "最多能卖" + this.wallet.coin + "个",
					});
					return;
				}

				var params = {};
				params["symbol"] = this.currentCoin.symbol;
				params["price"] = this.form.sell.limitPrice;
				params["amount"] = this.form.sell.marketAmount;
				params["direction"] = "SELL";
				params["type"] = "MARKET_PRICE";
				params["useDiscount"] = this.isUseBHB ? "1" : "0"; //是否试用手续费抵扣,0 不使用 1使用
				var that = this;
				this.$http
					.post(this.host + this.api.exchange.orderAdd, params)
					.then((response) => {
						var resp = response.body;
						if (resp.code == 0) {
							this.$Notice.success({
								title: that.$t("exchange.tip"),
								desc: that.$t("exchange.success"),
							});
							this.refreshAccount();
							this.sliderSellMarketPercent = 0;
						} else {
							this.$Notice.error({
								title: that.$t("exchange.tip"),
								desc: resp.message,
							});
						}
					});
			},
			buyPlate(currentRow) {
				this.form.buy.limitPrice = currentRow.price;
				this.form.sell.limitPrice = currentRow.price;
			},
			sellPlate(currentRow) {
				this.form.buy.limitPrice = currentRow.price;
				this.form.sell.limitPrice = currentRow.price;
			},
			/**
			 * 获取钱包信息
			 */
			getWallet() {
				this.$http
					.post(this.host + this.api.uc.wallet + this.currentCoin.base, {})
					.then((response) => {
						var resp = response.body;
						this.wallet.base = resp.data.balance || "";
					});
				this.$http
					.post(this.host + this.api.uc.wallet + this.currentCoin.coin, {})
					.then((response) => {
						var resp = response.body;
						this.wallet.coin = resp.data.balance;
					});
			},
			getCurrentOrder() {
				//查询当前委托
				var params = {};
				params["pageNo"] = 0;
				params["pageSize"] = 100;
				params["symbol"] = this.currentCoin.symbol;
				this.currentOrder.rows = [];
				var that = this;
				this.$http
					.post(this.host + this.api.exchange.current, params)
					.then((response) => {
						var resp = response.body;
						if (resp.content.length > 0) {
							this.currentOrder.rows = resp.content;
							this.currentOrder.rows.forEach((row, index) => {
								
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
								row.skin = that.skin;
								row.price =
									row.type == "MARKET_PRICE"
										? that.$t("exchange.marketprice")
										: row.price;
							});
						}
					});
			},
			getHistoryOrder(pageNo) {
				//查询历史委托
				if (pageNo == undefined) {
					pageNo = this.historyOrder.page;
				} else {
					pageNo = pageNo - 1;
				}
				this.historyOrder.rows = []; //清空数据
				var params = {};
				params["pageNo"] = pageNo;
				params["pageSize"] = this.historyOrder.pageSize;
				params["symbol"] = this.currentCoin.symbol;
				var that = this;
				this.$http
					.post(this.host + this.api.exchange.history, params)
					.then((response) => {
						var resp = response.body;
						let rows = [];
						if (resp.content.length > 0) {
							this.historyOrder.total = resp.totalElements;
							this.historyOrder.page = resp.number;
							for (var i = 0; i < resp.content.length; i++) {
								var row = resp.content[i];
								if (row) {
									row.skin = that.skin;
									row.price =
										row.type == "MARKET_PRICE"
											? that.$t("exchange.marketprice")
											: row.price;
									// this.historyOrder.rows.push(row);
									row.time
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
							}
							this.historyOrder.rows = rows;
						}
					});
			},
			cancel(index) {
				var order = this.currentOrder.rows[index];
				this.$Modal.confirm({
					title: "撤单提示",
					content: this.$t("exchange.undotip"),
					onOk: () => {
						this.$http
							.post(
								this.host + this.api.exchange.orderCancel + "/" + order.orderId,
								{}
							)
							.then((response) => {
								var resp = response.body;
								if (resp.code == 0) {
									this.refreshAccount();
									this.$Notice.success({
										title: this.$t("exchange.tip"),
										desc: this.$t("exchange.cancelsuccess"),
									});
								} else {
									this.$Notice.error({
										title: this.$t("exchange.tip"),
										desc: resp.message,
									});
								}
							});
					},
				});
			},
			refreshAccount: function () {
				this.getCurrentOrder();
				this.getHistoryOrder();
				this.getWallet();
			},
			timeFormat: function (tick) {
				return moment(tick).format("HH:mm:ss");
			},
			dateFormat: function (tick) {
				return moment(tick).format("YYYY-MM-DD HH:mm:ss");
			},
			keyEvent(event) {
				var re1 = new RegExp(
					"([0-9]+.[0-9]{" + this.baseCoinScale + "})[0-9]*",
					""
				);
				this.form.buy.limitPrice = this.form.buy.limitPrice
					.toString()
					.replace(re1, "$1");
				this.form.sell.limitPrice = this.form.sell.limitPrice
					.toString()
					.replace(re1, "$1");
				this.form.buy.marketAmount = this.form.buy.marketAmount
					.toString()
					.replace(re1, "$1");

				var re2 = new RegExp("([0-9]+.[0-9]{" + this.coinScale + "})[0-9]*", "");
				this.form.buy.limitAmount = this.form.buy.limitAmount
					.toString()
					.replace(re2, "$1");
				this.form.sell.limitAmount = this.form.sell.limitAmount
					.toString()
					.replace(re2, "$1");
				this.form.sell.marketAmount = this.form.sell.marketAmount
					.toString()
					.replace(re2, "$1");
			},
			showPublishMask() {
				if (!this.showPublish) {
					this.showPublish = true;
					console.log("show");
				}
			},
			hidePublishMask() {
				if (this.showPublish) {
					this.showPublish = false;
					console.log("hide");
				}
			},
			hideCountDown() {
				if (this.showCountDown) {
					this.showCountDown = false;
				}
			},
			setPublishProgress() { },
		},
	};
</script>