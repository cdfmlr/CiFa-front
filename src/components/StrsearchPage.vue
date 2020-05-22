<template>
  <div id="strsearchpage">
    <a-page-header title="子串搜索"
                   sub-title="给定两个字符串，搜索子串在父串中的位置。"/>

    <a-form-model :label-col="{ span: 4 }" :wrapper-col="{ span: 14 }">
      <a-form-model-item label="Text">
        <a-textarea placeholder="父字符串，在此字符串中搜索子串 pattern" allow-clear v-model="text"/>
      </a-form-model-item>

      <a-form-model-item label="Pattern">
        <a-textarea placeholder="子字符串，在父串 text 中搜索此字符串" allow-clear v-model="pattern"/>
      </a-form-model-item>

      <a-form-model-item label="算法">
        <a-radio-group v-model="algorithm">
          <a-radio-button value="0">
            regexp.FindAllIndex (go lib)
          </a-radio-button>
          <a-radio-button value="1">
            KMP 算法
          </a-radio-button>
          <a-radio-button value="2">
            Rabin-Karp 算法
          </a-radio-button>
          <a-radio-button value="3">
            暴力法
          </a-radio-button>
        </a-radio-group>
      </a-form-model-item>

      <a-form-model-item :wrapper-col="{ span: 14, offset: 9 }">
        <a-button type="primary" style="margin: 8px"
                  @click="onSearchSubmit">
          <a-icon type="file-search"/>
          开始匹配
        </a-button>
      </a-form-model-item>
    </a-form-model>

    <a-drawer
            title="搜索结果"
            :placement="'right'"
            :closable="false"
            :visible="resultVisible"
            @close="onResultClose">
      <a-statistic title="匹配次数:"
                   :value="resultIndex.length"
                   :value-style="{ color: '#3f8600' }" style="text-align: right"/>
      <p style="color: rgba(0, 0, 0, 0.45)">Pattern 在 Text 中的位置：</p>
      <ul v-for="i in resultIndex" :key="i">
        <li> {{ i }}</li>
      </ul>
    </a-drawer>
  </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "StrsearchPage",
        data: function () {
            return {
                text: "",
                pattern: "",
                algorithm: 0,
                resultIndex: [],
                resultVisible: false
            }
        },
        methods: {
            onSearchSubmit() {
                let thisVue = this;
                let reqData = {
                    algorithm: Number(this.algorithm),
                    text: this.text,
                    pattern: this.pattern
                };
                axios.post('http://localhost:9001/api/strsearch', JSON.stringify(reqData))
                    .then(res => {
                        console.log(res);
                        if (res.data && res.data.error) {
                            this.$message.error('搜索失败：' + res.data.error);
                        } else if (res.data.index) {
                            thisVue.resultIndex = res.data.index;
                            this.$message.info('搜索成功！用时' + res.data.time_cost);
                            thisVue.showResultDrawer()
                        } else {
                            this.$message.error('搜索失败');
                        }
                    })
                    .catch(reason => {
                        this.$message.error('无法连接服务器，搜索失败:\n' + reason);
                    })
            },
            showResultDrawer() {
                this.resultVisible = true;
            },
            onResultClose() {
                this.resultVisible = false;
            },
        },
    }
</script>

<style scoped>

</style>