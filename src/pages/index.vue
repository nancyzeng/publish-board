<template>
  <div id="indexPage">
    <HeadNav type="index" ref="head" @publisherChange="publisherChange()"></HeadNav>
    <div class="wd-1220">
      <!-- 数据简报 -->
      <div class="data-paper model-container">
        <div class="model-bg">
          <div class="section-title">
            数据简报
            <span class="time" v-if="show">昨日 {{$beforeDate()}}</span>
            <span class="time" v-else>上周 {{$weekDate().start}} ~ {{$weekDate().end}}</span>
            <span class="click-font" style="margin-left:15px;" @click="change">切换</span>
          </div>
          <div class="content" v-if="dataNewPower">
            <div class="left">
              <div style="margin-bottom:20px;">
                <div class="data-block">
                  <div>
                    <a-icon type="line-chart" />
                    <span class="secondary-font">本社销售点数</span>
                  </div>
                  <div class="data-font" v-if="publishInfos.sale_total">{{publishInfos.sale_total}}</div>
                  <div class="data-font" v-else>--</div>
                </div>
                <div class="data-block">
                  <div>
                    <a-icon type="line-chart" />
                    <span class="secondary-font">本社行业占比</span>
                  </div>
                  <div class="data-font" v-if="publishInfos.sale_ratio">{{publishInfos.sale_ratio}}%</div>
                  <div class="data-font" v-else>--</div>
                </div>
                <div class="data-block">
                  <div>
                    <a-icon type="line-chart" />
                    <span class="secondary-font">本社行业排名</span>
                  </div>
                  <div class="data-font" v-if="publishInfos.sale_rank">{{publishInfos.sale_rank}}</div>
                  <div class="data-font" v-else>--</div>
                </div>
              </div>
              <div id="container"></div>
            </div>
            <div class="right">
              <!-- 主营类目 -->
              <div class="main-category">
                <!-- 主营类目 -->
                <div
                  class="category-content"
                  v-if="publishSummaryInfo&&publishSummaryInfo.main_cate"
                >
                  <div class="category-left">主营类目</div>
                  <div class="category-right">
                    本社主营类目
                    <span
                      class="data-tab data-tab-one"
                    >{{publishSummaryInfo.main_cate.name?publishSummaryInfo.main_cate.name:'--'}}</span>，类目下本社排名第
                    <span
                      class="data-tab data-tab-one"
                    >{{publishSummaryInfo.main_cate.rank?publishSummaryInfo.main_cate.rank:'--'}}</span>名
                  </div>
                </div>
                <!-- 本社最佳 -->
                <div
                  class="category-content"
                  v-if="publishSummaryInfo&&publishSummaryInfo.best_rank"
                >
                  <div class="category-left">本社最佳</div>
                  <div class="category-right">
                    <div class="category-content">
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.best_rank.goods_name?publishSummaryInfo.best_rank.goods_name:'--'}}</span>总计销售点数
                      <!-- </div> -->
                      <!-- <div> -->
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.best_rank.sale_total?publishSummaryInfo.best_rank.sale_total:'--'}}</span>点进入总榜第
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.best_rank.all_rank?publishSummaryInfo.best_rank.all_rank:'--'}}</span>名，
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.best_rank.cate_name?publishSummaryInfo.best_rank.cate_name:'--'}}</span>类目第
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.best_rank.cate_rank?publishSummaryInfo.best_rank.cate_rank:'--'}}</span>名
                    </div>
                  </div>
                </div>
                <!-- 涨幅较高 -->
                <div
                  class="category-content"
                  v-if="publishSummaryInfo&&publishSummaryInfo.best_incr"
                >
                  <div class="category-left">涨幅较高</div>
                  <div class="category-right">
                    <div class="category-content">
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.best_incr.goods_name?publishSummaryInfo.best_incr.goods_name:'--'}}</span>销售点数较上个周期
                      <span
                        class="data-tab data-tab-two"
                        v-if="publishSummaryInfo.best_incr.incr_rate > 0"
                      >+{{publishSummaryInfo.best_incr.incr_rate}}%</span>
                      <span
                        v-else
                        class="data-tab data-tab-three"
                      >{{publishSummaryInfo.best_incr.incr_rate?publishSummaryInfo.best_incr.incr_rate:'--'}}</span>
                      <!-- </div> -->
                      <!-- <div> -->
                      ，增长幅度较大
                    </div>
                  </div>
                </div>
                <!-- 大盘销售 -->
                <div
                  class="category-content"
                  v-if="publishSummaryInfo&&publishSummaryInfo.sale_all"
                >
                  <div class="category-left">大盘销售</div>
                  <div class="category-right">
                    <div class="category-content">
                      全行业整体销售点数
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.sale_all.points?publishSummaryInfo.sale_all.points:'--'}}</span>较上个周期
                      <!-- </div> -->
                      <!-- <div> -->
                      <span
                        class="data-tab data-tab-two"
                        v-if="publishSummaryInfo.sale_all.incr_rate > 0"
                      >{{publishSummaryInfo.sale_all.incr_rate}}%</span>
                      <span class="data-tab data-tab-three" v-else>0%</span>
                    </div>
                  </div>
                </div>
                <!-- 大盘新品 -->
                <div
                  class="category-content"
                  v-if="publishSummaryInfo&&publishSummaryInfo.goods_all"
                >
                  <div class="category-left">大盘新品</div>
                  <div class="category-right">
                    <div class="category-content">
                      全行业总计动销新品
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.goods_all.new_total?publishSummaryInfo.goods_all.new_total:'--'}}</span>件，销售点数
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.goods_all.sale_new_total?publishSummaryInfo.goods_all.sale_new_total:'--'}}</span>点，占全行业的
                      <span
                        class="data-tab data-tab-one"
                      >{{publishSummaryInfo.goods_all.rate?publishSummaryInfo.goods_all.rate:'--'}}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="content" style="height:357px;position:relative;" v-else>
            <ModelNoPower type="noPower"></ModelNoPower>
          </div>
        </div>
      </div>
      <!-- 快捷入口，临期事件 -->
      <div class="model-container clearfix">
        <!-- 快捷入口 -->
        <div class="quick-enter float-left model-bg">
          <div class="section-title">快捷入口</div>
          <div class="enter-content">
            <span class="enter-click" @click="$refs.head.noOpen()">选题参考</span>
            <span class="enter-click" @click="$refs.head.noOpen()">人群分析</span>
            <span class="enter-click" @click="$refs.head.noOpen()">竞社分析</span>
            <span class="enter-click" @click="$refs.head.noOpen()">作者库</span>
            <span class="enter-click" @click="$refs.head.noOpen()">大事件</span>
          </div>
        </div>
        <!-- 临期事件 -->
        <div class="impend-event float-left model-bg">
          <div class="section-title clearfix">
            <span class="float-left">临期事件</span>
            <span class="click-font float-right" @click="$refs.head.noOpen()">更多</span>
          </div>
          <!-- 暂未开放 -->
          <div class="no-open" style="position:relative;">
            <ModelNoPower type="noOpen"></ModelNoPower>
          </div>
        </div>
      </div>
      <div class="model-container">
        <div class="model-bg last-week">上周排行数据 {{$weekDate().start}} ~ {{$weekDate().end}}</div>
      </div>
      <!-- 品种排行 -->
      <div class="model-container clearfix">
        <!-- 全品种销售排行 -->
        <div class="float-left sale-rank model-bg">
          <div class="section-title clearfix">
            <span class="float-left">全品种销售排行</span>
            <span class="click-font float-right" @click="$refs.head.noOpen()">更多</span>
          </div>
          <div class="table-data" v-if="dataRankPower">
            <div class="table small" v-if="allGoodsRank.length > 0">
              <table style="table-layout:fixed;">
                <colgroup>
                  <col width="45" />
                  <col width="175" />
                  <col width="74" />
                  <col width="74" />
                </colgroup>
                <thead>
                  <tr>
                    <td>排行</td>
                    <td>品种</td>
                    <td style="text-align:right;">销售点数</td>
                    <td style="text-align:right;">较上周期</td>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item,index) in allGoodsRank" :key="index">
                    <td style="text-align:center;">{{item.sale_rank}}</td>
                    <td>
                      <div class="goods-desc">
                        <img
                          :src="item.cover_pic"
                          alt
                          width="40px"
                          height="40px"
                          v-if="item.cover_pic"
                        />
                        <span v-else class="no-pic" style="min-width:40px;min-height:40px;"></span>
                        <span
                          class="goods-name click-font"
                          :title="item.goods_name"
                          @click="toDetail(item,index)"
                        >{{item.goods_name}}</span>
                      </div>
                    </td>
                    <td style="text-align:right;">{{item.sale_total}}</td>
                    <td style="text-align:right;">
                      <span
                        class="percent"
                        v-if="item.compare_rate > 0"
                      >+{{item.compare_rate.toFixed(2)}}%</span>
                      <span class="green" v-else>{{item.compare_rate.toFixed(2)}}%</span>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-else class="wrong-status">
              <a-empty />
            </div>
          </div>
          <div class="table-data" v-else>
            <ModelNoPower type="noPower"></ModelNoPower>
          </div>
        </div>
        <!-- 全品种销售涨幅排行 -->
        <div class="float-left up-rank model-bg">
          <div class="section-title clearfix">
            <span class="float-left">全品种销售涨幅排行</span>
            <span class="click-font float-right" @click="$refs.head.noOpen()">更多</span>
          </div>
          <div class="table-data" v-if="dataRankPower">
            <div class="table small" v-if="allUpRank.length > 0">
              <table style="table-layout:fixed;">
                <colgroup>
                  <col width="45" />
                  <col width="175" />
                  <col width="74" />
                  <col width="74" />
                </colgroup>
                <thead>
                  <tr>
                    <td>排行</td>
                    <td>品种</td>
                    <td style="text-align:right;">销售点数</td>
                    <td style="text-align:right;">较上周期</td>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item,index) in allUpRank" :key="index">
                    <td style="text-align:center;">{{item.sale_rank}}</td>
                    <td>
                      <div class="goods-desc">
                        <img
                          :src="item.cover_pic"
                          alt
                          width="40px"
                          height="40px"
                          v-if="item.cover_pic"
                        />
                        <span v-else class="no-pic" style="min-width:40px;min-height:40px;"></span>
                        <span
                          class="click-font goods-name"
                          :title="item.goods_name"
                          @click="toDetail(item,index)"
                        >{{item.goods_name}}</span>
                      </div>
                    </td>
                    <td style="text-align:right;">{{item.sale_total}}</td>
                    <td style="text-align:right;">
                      <span
                        class="percent"
                        v-if="item.compare_rate > 0"
                      >+{{item.compare_rate.toFixed(2)}}%</span>
                      <span class="green" v-else>{{item.compare_rate.toFixed(2)}}%</span>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-else class="wrong-status">
              <a-empty />
            </div>
          </div>
          <div class="table-data" v-else>
            <ModelNoPower type="noPower"></ModelNoPower>
          </div>
        </div>
        <!-- 新品销售排行 -->
        <div class="float-left new-rank model-bg">
          <div class="section-title clearfix">
            <span class="float-left">新品销售排行</span>
            <span class="click-font float-right" @click="$refs.head.noOpen()">更多</span>
          </div>
          <div class="table-data" v-if="dataRankPower">
            <div class="table small" v-if="newGoodsRank.length > 0">
              <table style="table-layout:fixed;">
                <colgroup>
                  <col width="45" />
                  <col width="175" />
                  <col width="74" />
                  <col width="74" />
                </colgroup>
                <thead>
                  <tr>
                    <td>排行</td>
                    <td>品种</td>
                    <td style="text-align:right;">销售点数</td>
                    <td style="text-align:right;">上市天数</td>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(item,index) in newGoodsRank" :key="index">
                    <td style="text-align:center;">{{item.sale_rank}}</td>
                    <td>
                      <div class="goods-desc">
                        <img
                          :src="item.cover_pic"
                          alt
                          width="40px"
                          height="40px"
                          v-if="item.cover_pic"
                        />
                        <span v-else class="no-pic" style="min-width:40px;min-height:40px;"></span>
                        <span
                          class="goods-name click-font"
                          :title="item.goods_name"
                          @click="toDetail(item,index)"
                        >{{item.goods_name}}</span>
                      </div>
                    </td>
                    <td style="text-align:right;">{{item.sale_total}}</td>
                    <td style="text-align:right;">{{item.onsale_days}}</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-else class="wrong-status">
              <a-empty />
            </div>
          </div>
          <div class="table-data" v-else>
            <ModelNoPower type="noPower"></ModelNoPower>
          </div>
        </div>
      </div>
      <!-- 出版社top -->
      <div class="model-container clearfix">
        <!--出版社TOP 5-->
        <div class="publish-top float-left model-bg">
          <div class="section-title clearfix">
            <span class="float-left">出版社TOP 5</span>
            <span class="click-font float-right" @click="$refs.head.noOpen()">更多</span>
          </div>
          <!-- 暂未开放 -->
          <div class="no-open" style="position:relative;">
            <ModelNoPower type="noOpen"></ModelNoPower>
          </div>
        </div>
        <!-- 竞社 -->
        <div class="compete-publish float-left model-bg">
          <div class="section-title clearfix">
            <span class="float-left">竞社</span>
            <span class="click-font float-right" @click="$refs.head.noOpen()">更多</span>
          </div>
          <!-- 暂未开放 -->
          <div class="no-open" style="position:relative;">
            <ModelNoPower type="noOpen"></ModelNoPower>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped lang="scss" src="@/style/scss/pages/index.scss"></style>
<script>
import { INDEX_PUBLISHERINFO, INDEX_RANKS } from "../apis/index.js";
import { Chart } from "@antv/g2";
export default {
  data() {
    return {
      show: true,
      changeChart: null,
      data: [
        // { date: "06-01", value: 300 },
        // { date: "06-02", value: 300 },
        // { date: "06-03", value: 300 },
        // { date: "06-04", value: 300 },
        // { date: "06-05", value: 300 },
        // { date: "06-06", value: 300 },
        // { date: "06-07", value: 900 },
        // { date: "06-08", value: 500 },
        // { date: "06-09", value: 200 },
        // { date: "06-10", value: 1200 },
        // { date: "06-11", value: 200 },
        // { date: "06-12", value: 400 },
        // { date: "06-13", value: 800 },
        // { date: "06-14", value: 400 },
        // { date: "06-15", value: 100 },
        // { date: "06-16", value: 200 },
        // { date: "06-17", value: 600 },
        // { date: "06-18", value: 700 },
        // { date: "06-19", value: 800 },
        // { date: "06-20", value: 1000 },
        // { date: "06-21", value: 500 },
        // { date: "06-22", value: 200 },
        // { date: "06-23", value: 700 },
        // { date: "06-24", value: 900 },
        // { date: "06-25", value: 300 },
        // { date: "06-26", value: 900 },
        // { date: "06-27", value: 800 },
        // { date: "06-28", value: 300 },
        // { date: "06-29", value: 300 },
        // { date: "06-30", value: 300 },
        // { date: "06-31", value: 300 }
      ],
      beforeData: [
        // { date: "01:00", value: 300 },
        // { date: "02:00", value: 600 },
        // { date: "03:00", value: 800 },
        // { date: "04:00", value: 1300 },
        // { date: "05:00", value: 1500 },
        // { date: "06:00", value: 500 }
      ],
      publishInfos: {
        sale_rank: 0,
        sale_ratio: 0,
        sale_total: 0
      },
      publishSummaryInfo: {
        main_cate: {
          // 主营类目
          name: "",
          rank: 0
        },
        best_rank: {
          // 本社最佳
          goods_name: "",
          sale_total: 0,
          all_rank: 0,
          cate_name: "",
          cate_rank: 0
        },
        best_incr: {
          // 涨幅较高
          goods_name: "",
          incr_rate: ""
        },
        sale_all: {
          // 大盘销售
          points: 0,
          incr_rate: ""
        },
        goods_all: {
          // 大盘新品
          new_total: 0,
          sale_new_total: 0,
          rate: ""
        }
      },
      // 全品种销售排行
      allGoodsRank: [],
      // 涨幅排行
      allUpRank: [],
      // 新品排行
      newGoodsRank: [],
      isFirst: true,
      // 接口权限
      dataNewPower: true,
      dataRankPower: true
    };
  },
  mounted() {
    setTimeout(() => {
      this.getData();
      this.getRankData();
    }, 500);
  },
  methods: {
    async getData() {
      let data = {
        organization_id: this.$refs.head.publishInfo.organization_id,
        publisher_id: this.$refs.head.publishInfo.publisher_id,
        period: this.show
          ? this.$beforeDate().replace(/-/g, "")
          : this.$weekDate().weekth,
        date_type: this.show ? 1 : 2,
        start_date: this.$beforeDate().replace(/-/g, "")
      };
      let res = await INDEX_PUBLISHERINFO(data);
      if (res.code == 0) {
        this.dataNewPower = true;
        let _this = this;
        this.data = res.data.sale_total_list.reverse();
        if (this.isFirst) {
          this.initData();
        } else {
          this.changeChart.changeData(this.data);
        }
        this.isFirst = false;
        if (res.data.publisher_info.sale_rank) {
          this.publishInfos.sale_rank = res.data.publisher_info.sale_rank;
        }
        if (res.data.publisher_info.sale_ratio) {
          this.publishInfos.sale_ratio = res.data.publisher_info.sale_ratio;
        }
        if (res.data.publisher_info.sale_total) {
          this.publishInfos.sale_total = res.data.publisher_info.sale_total;
          this.publishSummaryInfo = res.data.publisher_summary_info;
        }
        if (
          Object.keys(this.publishSummaryInfo).length > 0 &&
          this.publishSummaryInfo.best_incr.incr_rate
        ) {
          this.publishSummaryInfo.best_incr.incr_rate = Number(
            this.publishSummaryInfo.best_incr.incr_rate.replace(/%/, "")
          );
        }
        if (
          Object.keys(this.publishSummaryInfo).length > 0 &&
          this.publishSummaryInfo.sale_all.incr_rate
        ) {
          this.publishSummaryInfo.sale_all.incr_rate = Number(
            this.publishSummaryInfo.sale_all.incr_rate.replace(/%/, "")
          );
        }

      } else {
        if (res.code == 1008) {
          this.$router.push({ name: "loginindex" });
        }else if (res.code == 1009) {
          this.dataNewPower = false;
        }else{
          this.$refs.head.globalTip(1, res.message);
        }
      }
    },
    async getRankData() {
      let data = {
        organization_id: this.$refs.head.publishInfo.organization_id,
        publisher_id: this.$refs.head.publishInfo.publisher_id,
        period: this.$weekDate().weekth,
        start_date: this.$weekDate().start.replace(/-/g, "")
      };
      let res = await INDEX_RANKS(data);
      if (res.code == 0) {
        this.allGoodsRank = [];
        this.allUpRank = [];
        this.newGoodsRank = [];
        this.allGoodsRank = res.data.rank_all;
        this.allUpRank = res.data.rank_incr;
        this.newGoodsRank = res.data.rank_new;
        this.dataRankPower = true;
      } else {
        if (res.code == 1008) {
          this.$router.push({ name: "loginindex" });
        }else if (res.code == 1009) {
          this.dataRankPower = false;
        }else{
          this.$refs.head.globalTip(1, res.message);
        }
      }
    },
    change() {
      this.show ? (this.show = false) : (this.show = true);
      this.getData();
    },
    initData() {
      let _this = this;
      this.changeChart = new Chart({
        container: "container",
        autoFit: true,
        autoHide: false,
        height: 220
      });
      this.changeChart.data(this.data);
      this.changeChart.scale("value", {
        alias: "本社销售：",
        nice: true
      });
      this.changeChart.axis("date", {
        tickLine: null,
        label: {
          formatter: function(value) {
            // console.log(value)
            if (
              value == _this.data[0].date ||
              value == _this.data[2].date ||
              value == _this.data[4].date ||
              value == _this.data[6].date ||
              value == _this.data[8].date ||
              value == _this.data[10].date ||
              value == _this.data[12].date ||
              value == _this.data[14].date ||
              value == _this.data[16].date ||
              value == _this.data[18].date ||
              value == _this.data[20].date
            ) {
              return value;
            } else {
              return "";
            }
          },
          autoHide: false | true,
          autoRotate: false
        }
      });
      this.changeChart.tooltip({
        showMarkers: false
      });
      this.changeChart.interaction("active-region");
      this.changeChart
        .interval()
        .position("date*value")
        .style("date", val => {
          let _date1, _date2, _date3, _date4, _date5, _date6, _date7;
          if (_this.show) {
            _date1 = _this.$beforeDate().substring(5);
          } else {
            _date1 = _this.$weekDate().start.substring(5);
            _date2 = _this
              .$moment(_this.$weekDate().start)
              .day(2)
              .format("YYYY-MM-DD")
              .substring(5);
            _date3 = _this
              .$moment(_this.$weekDate().start)
              .day(3)
              .format("YYYY-MM-DD")
              .substring(5);
            _date4 = _this
              .$moment(_this.$weekDate().start)
              .day(4)
              .format("YYYY-MM-DD")
              .substring(5);
            _date5 = _this
              .$moment(_this.$weekDate().start)
              .day(5)
              .format("YYYY-MM-DD")
              .substring(5);
            _date6 = _this
              .$moment(_this.$weekDate().start)
              .day(6)
              .format("YYYY-MM-DD")
              .substring(5);
            _date7 = _this
              .$moment(_this.$weekDate().start)
              .day(7)
              .format("YYYY-MM-DD")
              .substring(5);
          }
          // console.log(val)
          if (
            val == _date1 ||
            val == _date2 ||
            val == _date3 ||
            val == _date4 ||
            val == _date5 ||
            val == _date6 ||
            val == _date7
          ) {
            return {
              fillOpacity: 0.4,
              lineWidth: 0,
              stroke: "#636363"
              // lineDash: [3, 2]
            };
          }
          return {
            fillOpacity: 1,
            lineWidth: 0,
            stroke: "#437AE7",
            lineDash: [3, 2]
          };
        });

      this.changeChart.render();
    },
    toDetail(item, index) {
      this.$router.push({
        name: "detail",
        query: {
          goods_id: item.goods_id
        }
      });
    },
    publisherChange() {
      // console.log('hahah')
      this.getData();
      this.getRankData();
    }
  }
};
</script>
