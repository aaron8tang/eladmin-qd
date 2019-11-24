<template>
  <div class="app-container">
    <el-row :gutter="20">
      <!--<el-col :xs="24" :sm="24" :md="8" :lg="6" :xl="5">-->
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <span>车辆图片</span>
        </div>
        <div>
          <div style="text-align: center">
            <el-upload
              :show-file-list="false"
              :on-success="handleSuccess"
              :on-error="handleError"
              :headers="headers"
              :action="uploadVehicleApi"
              class="avatar-uploader">
              <img v-if="vehicle.url" :src="vehicle.url" title="点击上传图片" width="600" height="400">
              <i v-else class="el-icon-plus avatar-uploader-icon"/>
            </el-upload>
          </div>
          <ul class="user-info">
            <li><svg-icon icon-class="user1" /> 车牌号 <div class="user-right">{{ vehicle.plate }}</div></li>
            <li><svg-icon icon-class="phone" /> 车辆类型 <div class="user-right">{{ vehicle.type }}</div></li>
          </ul>
        </div>
      </el-card>
      <!--</el-col>-->
    </el-row>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import { getToken } from '@/utils/auth'
import store from '@/store'
import { parseTime } from '@/utils/index'
import initData from '@/mixins/initData'
export default {
  name: 'Center',
  components: {},
  mixins: [initData],
  data() {
    return {
      ico: 'el-icon-refresh',
      headers: {
        'Authorization': 'Bearer ' + getToken()
      },
      vehicle: {
        url: '',
        type: '',
        plate: ''
      }
    }
  },
  computed: {
    ...mapGetters([
      'user',
      'uploadVehicleApi'
    ])
  },
  created() {
    this.$nextTick(() => {
      this.init()
    })
    store.dispatch('GetInfo').then(() => {})
  },
  methods: {
    parseTime,
    handleSuccess(response, file, fileList) {
      debugger
      if (response) {
        this.vehicle = response
      }
    },
    // 监听上传失败
    handleError(e, file, fileList) {
      const msg = JSON.parse(e.message)
      this.$notify({
        title: msg.message,
        type: 'error',
        duration: 2500
      })
    },
    refresh() {
      this.ico = 'el-icon-loading'
      this.$refs.log.init()
      setTimeout(() => {
        this.ico = 'el-icon-refresh'
      }, 300)
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
  .avatar-uploader-icon {
    font-size: 28px;
    width: 120px;
    height: 120px;
    line-height: 120px;
    text-align: center
  }

  .avatar {
    width: 120px;
    height: 120px;
    display: block;
    border-radius: 50%
  }
  .user-info {
    padding-left: 0px;
    list-style: none;
    li{
      border-bottom: 1px solid #F0F3F4;
      border-top: 1px solid #F0F3F4;
      padding: 11px 0px;
      font-size: 13px;
    }
    .user-right {
      float: right;

      a{
        color: #317EF3;
      }
    }
  }
</style>
