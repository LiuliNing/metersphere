<template>
  <!-- 操作按钮 -->
  <div style="background-color: white;">
    <el-row>
      <el-col>
        <!--操作按钮-->
        <div style="float: right;margin-right: 20px;margin-top: 20px">
          <el-button type="primary" size="small" @click="saveApi">{{$t('commons.save')}}</el-button>
          <el-button type="primary" size="small" @click="runTest">{{$t('commons.test')}}</el-button>
        </div>
      </el-col>
    </el-row>
    <!-- 基础信息 -->
    <p class="tip">{{$t('test_track.plan_view.base_info')}} </p>
    <br/>
    <el-row>
      <el-col>
        <ms-basis-api :moduleOptions="moduleOptions" :basisData="basisData" ref="basicForm" @callback="callback"/>
      </el-col>
    </el-row>

    <!-- 请求参数 -->
    <p class="tip">{{$t('api_test.definition.request.req_param')}} </p>
    <ms-basis-parameters :request="request"/>

  </div>
</template>

<script>
  import MsBasisApi from "./BasisApi";
  import MsBasisParameters from "../request/database/BasisParameters";

  export default {
    name: "MsApiSqlRequestForm",
    components: {
      MsBasisApi, MsBasisParameters
    },
    props: {
      request: {},
      basisData: {},
      moduleOptions: Array,
      isReadOnly: {
        type: Boolean,
        default: false
      }
    },

    data() {
      return {validated: false}
    },
    methods: {
      callback() {
        this.validated = true;
      },
      validateApi() {
        this.validated = false;
        this.$refs['basicForm'].validate();
      },
      saveApi() {
        this.validateApi();
        if (this.validated) {
          this.basisData.method = this.basisData.protocol;
          this.$emit('saveApi', this.basisData);
        }
      },
      runTest() {
        this.validateApi();
        if (this.validated) {
          this.basisData.request = this.request;
          this.$emit('runTest', this.basisData);
        }
      },
    },
  }
</script>

<style scoped>
  .tip {
    padding: 3px 5px;
    font-size: 16px;
    border-radius: 4px;
    border-left: 4px solid #783887;
    margin: 0px 20px 0px;
  }
</style>
