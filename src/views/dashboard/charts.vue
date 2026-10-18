<template>
  <div class="app-container home">
    <header class="overview-heading"><div><h1>仓库概览</h1><p>入库、出库与库存趋势</p></div><span class="demo-label">演示数据</span></header>
    <div class="station-top">
      <el-row :gutter="12" class="mt5">
        <el-col :xs="24" :sm="12" :lg="6">
          <div class="top-item-box item-box-one" style="display: flex;">
            <div style="flex:2;height:100%;">
              <div>入库</div>
              <div style="text-align:center;margin-top:30px;"><span style="font-size:26px;font-weight:bold;">33</span>
              </div>
            </div>
            <div style="flex:3;display: flex;flex-direction:column;justify-content:space-evenly">
              <div>待入库：15</div>
              <div>待质检：15</div>
              <div>待上架：2</div>
              <div>待完成：1 </div>
            </div>
          </div>
        </el-col>
        <el-col :xs="24" :sm="12" :lg="6">
          <div class="top-item-box item-box-two" style="display: flex;">
            <div style="flex:2;height:100%;">
              <div>出库</div>
              <div style="text-align:center;margin-top:30px;"><span style="font-size:26px;font-weight:bold;"
              >20</span></div>
            </div>
            <div style="flex:3;display: flex;flex-direction:column;justify-content:space-evenly">
              <div>待配货：5</div>
              <div>待波次：5</div>
              <div>待拣货：5</div>
              <div>待出库：5</div>
            </div>
          </div>
        </el-col>
        <el-col :xs="24" :sm="12" :lg="6">
          <div class="top-item-box item-box-three" style="display: flex;">
            <div style="flex:2;height:100%;">
              <div>其他</div>
              <div style="text-align:center;margin-top:30px;"><span style="font-size:26px;font-weight:bold;"
              >15</span></div>
            </div>
            <div style="flex:3;display: flex;flex-direction:column;justify-content:space-evenly">
              <div>待截单：5</div>
              <div>异常单：5</div>
              <div>今日到货：5</div>
            </div>
          </div>
        </el-col>
        <el-col :xs="24" :sm="12" :lg="6">
          <div class="top-item-box item-box-four" style="display: flex;">
            <div style="flex:2;height:100%;">
              <div>库存预警</div>
              <div style="text-align:center;margin-top:30px;"><span style="font-size:26px;font-weight:bold;">5</span>
              </div>
            </div>
            <div style="flex:3;display: flex;flex-direction:column;justify-content:space-evenly">
              <div>松陵仓：1</div>
              <div>盛泽仓：2</div>
              <div>园区仓：2</div>
            </div>
          </div>
        </el-col>
      </el-row>
    </div>
    <div class="station-middle">
      <el-row :gutter="12">
        <el-col :xs="24" :sm="12" :lg="6">
          <el-card class="box-card" shadow="never">
            <div class="card-title">仓库货物占比</div>
            <div style="height: calc(100% - 30px);">
              <StationPie height="100%"></StationPie>
              <div></div>
            </div>
          </el-card>
        </el-col>
        <el-col :xs="24" :sm="24" :lg="18">
          <el-card class="box-card" shadow="never">
            <div style="display:flex;justify-content: space-between;align-items: center;">
              <div class="card-title">生产入库趋势</div>
              <el-radio-group v-model="tabPosition" @change="dateChange">
                <!-- <el-radio-button label="day">当日</el-radio-button> -->
                <el-radio-button label="month">本月</el-radio-button>
                <el-radio-button label="year">今年</el-radio-button>
              </el-radio-group>
            </div>
            <div style="height: calc(100% - 30px);">
              <StationBar height="100%" :chartData="barChartData" :xName="barXName"/>
            </div>
          </el-card>
        </el-col>
      </el-row>
    </div>
    <div class="station-bottom">
      <el-row :gutter="12">
        <el-col :xs="24" :sm="12" :lg="6">
          <el-card class="box-card" shadow="never">
            <div class="card-title">近7日销售出库</div>
            <div style="height: calc(100% - 30px);">
              <StationLine height="100%" itemColor="#ee4368" yName="件" :chartData="lineDataOne"/>
            </div>
          </el-card>
        </el-col>
        <el-col :xs="24" :sm="12" :lg="6">
          <el-card class="box-card" shadow="never">
            <div class="card-title">近7日领料出库</div>
            <div style="height: calc(100% - 30px);">
              <StationLine height="100%" :chartData="lineDataTwo" yName="件" itemColor="#5470c6"/>
            </div>
          </el-card>
        </el-col>
        <el-col :xs="24" :sm="12" :lg="6">
          <el-card class="box-card" shadow="never">
            <div class="card-title">近7日移库</div>
            <div style="height: calc(100% - 30px);">
              <StationLine height="100%"  :chartData="lineDataThree" yName="件"
                           itemColor="#c58bea"
              />
            </div>
          </el-card>
        </el-col>
        <el-col :xs="24" :sm="12" :lg="6">
          <el-card class="box-card" shadow="never">
            <div class="card-title">近7日退货入库</div>
            <div style="height: calc(100% - 30px);">
              <StationLine height="100%" yName="件" :chartData="lineDataFour"
                           itemColor="#c7a428"
              />
            </div>
          </el-card>
        </el-col>
      </el-row>
    </div>
  </div>
</template>
<script setup>
import StationPie from './components/StationPie.vue'
import StationLine from './components/StationLine.vue'
import StationBar from './components/StationBar.vue'
import { onMounted, ref } from 'vue'
import moment from 'moment';

const tabPosition = ref('month')
const barChartData = ref({
  yData: [79, 68, 56, 72, 51, 63, 67, 71, 58, 81, 64, 77, 56, 69]
})
const barXName = ref('日')
const lineDataOne = ref({
  yData: [79, 65, 21, 67, 21, 89, 56],
})
const lineDataTwo = ref({
  yData: [45, 72, 16, 37, 64, 28, 46],
})
const lineDataThree = ref(
  {
    yData: [16, 27, 37, 16, 27, 21, 11],
  }
)
const lineDataFour = ref({
  yData: [134, 107, 94, 173, 37, 143, 86],
})

onMounted((()=>{
  initTime();
  dateChange('month');
}))

// 时间类型选择
function dateChange(value) {
  let date = new Date()
  let month = date.getMonth() + 1
  let day = date.getDate()
  let barXData = []
  let barYData = []
  if(value === 'year') {
    for(let i = 0; i < month; i++) {
      barXData.push(moment().subtract(i, 'months').format('YYYY-MM'))
      barYData.push(Math.floor(Math.random()*(180-120+1))+120)
    }
    barXName.value = '月'
  } else {
    for(let i = 0; i < day; i++) {
      barXData.push(moment().subtract(i, 'days').format('MM-DD'))
      barYData.push(Math.floor(Math.random()*(30-15+1))+15)
    }
    barXName.value = '日'
  }
  barChartData.value = {
    xData: barXData.reverse(),
    yData: barYData
  }
}
// 初始化时间模拟数据
function initTime() {
  let lineXData = []
  for(let i = 0; i < 7; i++) {
    lineXData.push(moment().subtract(i, 'days').format('MM-DD'))
  }
  lineXData = lineXData.reverse()
  lineDataOne.value.xData = lineXData
  lineDataTwo.value.xData = lineXData
  lineDataThree.value.xData = lineXData
  lineDataFour.value.xData = lineXData
}


</script>


<style scoped>
.app-container {
  min-height: calc(100vh - 84px);
  padding: 24px;
}

.top-item-box {
  height: 160px;
  background: #fff;
  margin-bottom: 12px;
  border-radius: 6px;
  border: 1px solid #d1d9e0;
  color: #1f2328;
  padding: 16px;
}

.item-box-one, .item-box-two, .item-box-three, .item-box-four {
  background: #fff;
}

.box-card {
  height: 400px;
  margin-bottom: 12px;
  background-color: #fff;
  border-color: #d1d9e0;
}

.box-card :deep(.el-card__body) {
  height: 100%;
}

.card-title {
  font-weight: bold;
  height: 30px;
  display: flex;
  align-items: center;
}

.overview-heading { display: flex; justify-content: space-between; align-items: center; gap: 16px; margin-bottom: 24px; }
.overview-heading h1 { margin: 0 0 8px; font-size: 24px; font-weight: 600; color: #1f2328; }
.overview-heading p { margin: 0; font-size: 14px; color: #59636e; }
.demo-label { border: 1px solid #d1d9e0; border-radius: 20px; padding: 4px 10px; font-size: 12px; color: #59636e; white-space: nowrap; }
@media (max-width: 767px) { .app-container { padding: 16px; } }

</style>
