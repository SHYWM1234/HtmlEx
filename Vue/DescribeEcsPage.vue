<template>
  <div class="hello" id="app">
    <h2>云服务器ECS最新资源价格查询</h2>
    <hr style="color:gray" size="2" width="60%">
    <el-row>
    <el-select v-model="valueQY" placeholder="请选择 区域">
      <el-option
        v-for="item in options_qy"
        :key="item.value_qy"
        :label="item.label_qy"
        :value="item.value_qy">
      </el-option>
    </el-select>
    </el-row>
    <br>
    <el-row>
    <el-select v-model="valueSLGG" placeholder="请选择 实例规格">
      <el-option
        v-for="item in options_slgg"
        :key="item.value_slgg"
        :label="item.label_slgg"
        :value="item.value_slgg">
      </el-option>
    </el-select>
    </el-row>
    <br>
    <el-row>
    <el-select v-model="valueYPLX" placeholder="请选择 云盘类型">
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
    <el-slider v-model="valueYPRL" :min="20" :max="500" show-input></el-slider>
    </el-row>
    <br>
    <el-row>
    <el-select v-model="valueFFMS" placeholder="请选择 付费模式">
      <el-option
        v-for="item in options_ffms"
        :key="item.value_ffms"
        :label="item.label_ffms"
        :value="item.value_ffms">
      </el-option>
    </el-select>
    </el-row>
    <br>
    <el-row>购买数量</el-row>
    <el-input-number v-model="num" @change="handleChange" :min="1" :max="10" label="文字描述"></el-input-number>
    <br><br>
    <br>

    <el-button type="primary" icon="el-icon-search" plain>查询</el-button>
    <el-button type="danger" icon="el-icon-setting" plain>清空</el-button>
    <br><br>
    <el-row><a href="https://help.aliyun.com/document_detail/25370.html?spm=a2c4g.11186623.6.566.70147609wZnUr5">计费详解</a></el-row>
    <br>
    <el-row>
      <span>
        区域：{{valueQY}}<br>
        实例规格：{{valueSLGG}}<br>
        云盘类型：{{valueYPLX}}<br>
        云盘容量：{{valueYPRL}}<br>
        付费模式：{{valueFFMS}}<br>
        购买数量：{{num}}<br>
      </span>
    </el-row>
  </div>
</template>

<script>
export default{
  data () {
    return {
      options_qy: [{
        value_qy: 'cn-hangzhou',
        label_qy: '杭州'
      }, {
        value_qy: 'cn-qingdao',
        label_qy: '青岛'
      }, {
        value_qy: 'cn-beijing',
        label_qy: '北京'
      }, {
        value_qy: 'cn-shanghai',
        label_qy: '上海'
      }],
      options_slgg: [{
        value_slgg: 'ecs.g5.large',
        label_slgg: '通用型g5基础版'
      }, {
        value_slgg: 'ecs.g5.xlarge',
        label_slgg: '通用型g5加强版'
      }, {
        value_slgg: 'ecs.g5.2xlarge',
        label_slgg: '通用型g5豪华版'
      }, {
        value_slgg: 'ecs.g5.3xlarge',
        label_slgg: '通用型g5旗舰版'
      }],
      options_yplx: [{
        value_yplx: 'cloud_efficiency',
        label_yplx: '高效云盘'
      }, {
        value_yplx: 'cloud_essd',
        label_yplx: 'ESSD云盘'
      }, {
        value_yplx: 'cloud_ssd',
        label_yplx: 'SSD云盘'
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
      valueQY: '',
      valueSLGG: '',
      valueYPLX: '',
      valueYPRL: 20,
      valueFFMS: '',
      num: 1
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
