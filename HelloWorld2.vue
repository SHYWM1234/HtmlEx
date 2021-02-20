<template>
  <div class="hello" id="app">
  <h2>云服务器ECS最新资源价格查询</h2>
  <hr style="color:gray" size="2" width="60%">
  <el-card class="values-form-layout">
  <el-row>
  <el-select v-model="regionId" @change="onClick_RegionId" placeholder="请选择 区域">
    <el-option
    v-for="item in options_qy"
    :key="item.regionId"
    :label="item.localName"
    :value="item.regionId">
    </el-option>
  </el-select>
  </el-row>
  <br>
  <el-row>
    <el-select v-model="instanceTypeFamily" @change="onClick_InstanceTypeFamily" placeholder="请选择 实例规格族">
      <el-option
      v-for="item in options_slggz"
      :key="item.instanceTypeFamilyId"
      :value="item.instanceTypeFamilyId">
      </el-option>
    </el-select>
  </el-row>
  <br>
  <el-row>
  <el-select v-model="instanceType" placeholder="请选择 实例规格">
    <el-option
    v-for="item in options_slgg"
    :key="item.instanceTypeId"
    :label="item.instanceTypeId"
    :value="item.instanceTypeId">
    </el-option>
  </el-select>
  </el-row>
  <br>
  <el-row>
  <el-select v-model="systemDiskCategory" placeholder="请选择 云盘类型">
    <el-option
    v-for="item in options_yplx"
    :key="item.value_yplx"
    :label="item.label_yplx"
    :value="item.value_yplx">
    </el-option>
  </el-select>
  </el-row>
  <br>
  <el-row>云盘容量(GiB)</el-row>
  <el-row>
  <el-slider v-model="systemDiskSize" :min="20" :max="500" show-input></el-slider>
  </el-row>
  <br>
  <el-row>
  <el-select v-model="priceUnit" @change="onclick_priceUnit" placeholder="请选择 付费模式">
    <el-option
    v-for="item in options_ffms"
    :key="item.value_ffms"
    :label="item.label_ffms"
    :value="item.value_ffms">
    </el-option>
  </el-select>
  </el-row>
  <br>
  <el-row>购买时长</el-row>
  <el-input-number id='timeLength' v-model="period" :min="1" :max=periodMax label="文字描述"></el-input-number>
  <br><br>
  <br>

  <el-button type="primary" @click = "onSubmit ()" icon="el-icon-search" plain>查询</el-button>
  <el-button id="getSelectsBtn" type="danger" @click = "onInit ()" icon="el-icon-setting" plain>初始化下拉列表</el-button>
  <br><br>
  <el-row><a href="https://help.aliyun.com/document_detail/25370.html?spm=a2c4g.11186623.6.566.70147609wZnUr5">计费详解</a></el-row>
  <br>
  </el-card>
  <el-card class="values-form-layout11">
    <h4>查询结果：</h4>
    <span id = 'resultarea'>无</span>
  </el-card>
  <el-card class="values-form-layout22">
    <span>
      <h6>待提交参数：</h6>
      区域：{{regionId}}<br>
      实例规格：{{instanceType}}<br>
      云盘类型：{{systemDiskCategory}}<br>
      云盘容量：{{systemDiskSize}}<br>
      付费模式：{{priceUnit}}<br>
      购买时长：{{period}}<br>
    </span>
  </el-card>
  </div>
</template>

<script>
export default{
  data () {
    return {
      options_qy: [],
      options_slggz: [],
      options_slgg: [],
      options_yplx: [{
        value_yplx: 'cloud',
        label_yplx: '普通云盘'
      }, {
        value_yplx: 'cloud_efficiency',
        label_yplx: '高效云盘'
      }, {
        value_yplx: 'cloud_ssd',
        label_yplx: 'SSD云盘'
      }, {
        value_yplx: 'ephemeral_ssd',
        label_yplx: '本地SSD云盘'
      }, {
        value_yplx: 'cloud_essd',
        label_yplx: 'ESSD云盘'
      }],
      options_ffms: [{
        value_ffms: 'Year',
        label_ffms: '包年付费'
      }, {
        value_ffms: 'Month',
        label_ffms: '包月付费'
      }, {
        value_ffms: 'Week',
        label_ffms: '按周付费'
      }, {
        value_ffms: 'Hour',
        label_ffms: '按小时付费'
      }],
      resultObj: {
        rules: [],
        price: {
          originalPrice: '',
          discountPrice: '',
          tradePrice: '',
          currency: '',
          detailInfos: []
        }
      },
      regionId: '',
      instanceTypeFamily: '',
      instanceType: '',
      systemDiskCategory: '',
      systemDiskSize: 100,
      priceUnit: '',
      periodMax: 1,
      period: 1,
      displayPriceUnit: ''
    }
  },
  methods: {
    onInit () {
      var url1 = '/getRegions'
      var res1 = this.$axios.get(url1)
      res1.then((response) => { this.$data.options_qy = response.data }).catch()
    },
    onSubmit () {
      var url = '/describeInstancePrice?'
      url = url + 'regionId=' + this.$data.regionId + '&'
      url = url + 'instanceType=' + this.$data.instanceType + '&'
      url = url + 'systemDiskCategory=' + this.$data.systemDiskCategory + '&'
      url = url + 'systemDiskSize=' + this.$data.systemDiskSize + '&'
      url = url + 'priceUnit=' + this.$data.priceUnit + '&'
      url = url + 'period=' + this.$data.period
      var res = this.$axios.get(url)
      res.then((response) => { document.getElementById('resultarea').innerHTML = '原始价格：' + response.data.price.originalPrice + ' 元<br/>折扣价格：' + response.data.price.discountPrice + ' 元<br/>最终价格：' + response.data.price.tradePrice + ' 元' }).catch(document.getElementById('resultarea').innerHTML = ('请稍候...'))
    },
    onClick_RegionId () {
      var url = '/getInstanceTypeFamilies'
      var res = this.$axios.get(url)
      res.then((response) => { this.$data.options_slggz = response.data }).catch()
    },
    onClick_InstanceTypeFamily () {
      var url = '/getInstanceTypes?'
      url = url + 'instanceTypeFamily=' + this.$data.instanceTypeFamily
      var res = this.$axios.get(url)
      res.then((response) => { this.$data.options_slgg = response.data }).catch()
    },
    onclick_priceUnit () {
      var priceUnit = this.$data.priceUnit
      this.$data.period = 1
      if (priceUnit === 'Year') {
        this.$data.periodMax = 5
        this.$data.displayPriceUnit = ' 元/年'
      } else if (priceUnit === 'Month') {
        this.$data.periodMax = 9
        this.$data.displayPriceUnit = ' 元/月'
      } else if (priceUnit === 'Week') {
        this.$data.periodMax = 4
        this.$data.displayPriceUnit = ' 元/周'
      } else {
        this.$data.periodMax = 1
        this.$data.displayPriceUnit = ' 元/小时'
      }
    }
  }
}
window.onload = function () {
  document.getElementById('getSelectsBtn').click()
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.values-form-layout {
  position: absolute;
  left: 0;
  right: 0;
  width: 800px;
  margin: 15px auto;
  /* border-top: 8px solid #409eff; */
}
.values-form-layout11 {
  position: absolute;
  left: 0;
  right: 0;
  width: 800px;
  margin: 550px auto;
  border-top: 5px solid #ffaa00;
}
.values-form-layout22 {
  position: absolute;
  left: 0;
  right: 0;
  width: 400px;
  margin: 1000px auto;
  border-top: 5px solid #ffaa00;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
