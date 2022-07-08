<template>
  <div>

    <div class="help" v-if="0">
      <img class="bannerimg" src="../../assets/images/help_banner.jpg">
      <div class="help_container">
        <h1>{{$t("header.helpcenter")}}</h1>
        <div class="main">
          <div class="section" v-for="section in helpData">
            <h3 v-if="langPram == 'CN'">{{section.titleCN}}</h3>
            <h3 v-if="langPram == 'EN'">{{section.titleEN}}</h3>
            <div class="list-wrap">
              <router-link v-if="langPram == 'CN'" class="item" :title="item.title"
                v-for="(item, index) in section.content" :to="{path:'helpdetail',query:{cate:section.cate,id:item.id}}"
                :key="index">
                <span class="text">{{item.title}}</span>
              </router-link>
              <router-link v-if="langPram == 'EN'" class="item" :title="item.title"
                v-for="(item, index) in section.content" :to="{path:'helpdetail',query:{cate:section.cate,id:item.id}}"
                :key="index">
                <span class="text">{{item.title}}</span>
              </router-link>
            </div>
            <div class="route-wrap">
              <router-link v-if="langPram == 'CN'" :to="{path:'helplist',query:{cate:section.cate}}">
                {{$t("common.more")}}>></router-link>
              <router-link v-if="langPram == 'EN'" :to="{path:'helplist',query:{cate:section.cate}}">
                {{$t("common.more")}}>></router-link>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="step == 0" class="d-flex justify-content-between align-items-center ws-filter-tab "
      style="padding-top: 3px !important; padding-bottom:3px !important;">
      <span @click="$router.go(-1)">
        <b-icon icon="chevron-left" variant="light"></b-icon>
      </span>
      <div class="d-flex col py-2 justify-content-center align-items-center">
        <span>{{$t('appmain.Helpcenter')}}</span>
      </div>
    </div>
    <div class="p-2" v-for="section in helpData">
      <div class="d-flex justify-content-between align-items-center pb-2">
        <span class="fw-500" v-if="langPram == 'CN'">{{section.titleCN}}</span>
        <span class="fw-500" v-if="langPram == 'EN'">{{section.titleEN}}</span>
        <router-link class="fs-7 text-warning" :to="{path:'helplist',query:{cate:section.cate}}">{{$t('common.more')}}</router-link>
      </div>
      <router-link v-if="langPram == 'CN'" :title="item.title" v-for="(item, index) in section.content" :to="{path:'helpdetail',query:{cate:section.cate,id:item.id}}" :key="index"
        class="d-flex p-2 px-3 justify-content-between align-items-center mb-2"
        style="background-color:#1A212B; border-radius: 5px;">
        <span class="fs-7 col">
          {{item.title}}
        </span>
        <span class="ps-3">
          <b-icon icon="chevron-right" variant="light"></b-icon>
        </span>
      </router-link>
      <router-link v-if="langPram == 'EN'" :title="item.title"
      v-for="(item, index) in section.content" :to="{path:'helpdetail',query:{cate:section.cate,id:item.id}}" :key="index"
        class="d-flex p-2 px-3 justify-content-between align-items-center mb-2"
        style="background-color:#1A212B; border-radius: 5px;">
        <span class="fs-7 col">
          {{item.title}}
        </span>
        <span class="ps-3">
          <b-icon icon="chevron-right" variant="light"></b-icon>
        </span>
      </router-link>
    </div>


  </div>
</template>


<script>
  export default {
    data() {
      return {
        step: 0,
        helpData: []
      };
    },
    created: function () {
      this.init();
      this.getData();
    },
    computed: {
      lang() {
        return this.$store.state.lang;
      },
      langPram() {
        if (this.$store.state.lang == "简体中文") {
          return "CN";
        }
        if (this.$store.state.lang == "English") {
          return "EN";
        }
        return "CN";
      }
    },
    methods: {
      init() {
        this.$store.commit("navigate", "nav-uc");
      },
      getData() {
        let param = {};
        param["lang"] = this.langPram;
        this.$http.post(this.host + "/uc/ancillary/more/help", param).then(res => {
          if (res.status == 200 && res.body.code == 0) {
            this.helpData = res.body.data;
          } else {
            this.$Message.error(res.body.message);
          }
        });
      }
    }
  };
</script>