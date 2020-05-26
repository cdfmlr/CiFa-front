<template>
  <div id="wordfapage">
    <!--任务提交界面-->
    <div id="task-post" v-if="!showResultPage">
      <a-page-header title="词频分析"
                     sub-title="给定关键词，选择文本文件，统计关键词在文件中出现的频数，以频数从大到小的顺序进行输出。"/>

      <!--任务表单输入-->
      <a-card title="新建任务" :bordered="false" hoverable>
        <a slot="extra" href="#">
          <a-icon type="question-circle"/>
        </a>

        <a-form-model :model="wordfaForm" :label-col="wordfaLabelCol" :wrapper-col="wordfaWrapperCol">
          <a-form-model-item label="* 文件">
            <a-input type="file" @change="onWordfaFileChange"/>
          </a-form-model-item>

          <a-form-model-item label="* 关键词">
            <a-textarea placeholder="要检测的关键词，多个词间用逗号(',' 或 '，')隔开"
                        v-model="wordfaForm.keywords" auto-size/>
          </a-form-model-item>

          <a-form-model-item label="搜索算法">
            <a-radio-group v-model="wordfaForm.search_by">
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

          <a-form-model-item label="排序算法">
            <a-radio-group v-model="wordfaForm.sort_by">
              <a-radio-button value="0">
                sort.Sort (go lib)
              </a-radio-button>
              <a-radio-button value="1">
                sort.Stable (go lib)
              </a-radio-button>
              <a-radio-button value="2">
                快速排序
              </a-radio-button>
              <a-radio-button value="3">
                堆排序
              </a-radio-button>
              <a-radio-button value="4">
                归并排序
              </a-radio-button>
              <a-radio-button value="5">
                希尔排序
              </a-radio-button>
              <a-radio-button value="6">
                希尔排序(并发)
              </a-radio-button>
              <a-radio-button value="7">
                插入排序
              </a-radio-button>
              <a-radio-button value="8">
                选择排序
              </a-radio-button>
            </a-radio-group>
          </a-form-model-item>

          <a-form-model-item :wrapper-col="{ span: 14, offset: 18 }">
            <a-button type="primary" @click="onWordfaFormSubmit">
              提交
            </a-button>
            <!--            <a-button style="margin-left: 10px;">-->
            <!--              重置-->
            <!--            </a-button>-->
          </a-form-model-item>
        </a-form-model>

        <!--        <form>-->
        <!--          <div class="input-item">-->
        <!--            <span>要搜索的文件：</span>-->
        <!--            <a-input type="file"/>-->
        <!--          </div>-->
        <!--          <div class="input-item">-->
        <!--            <span>关键词：</span>-->
        <!--          </div>-->
        <!--        </form>-->

      </a-card>
      <a-popconfirm
              title="立即查看处理结果?"
              ok-text="是"
              cancel-text="否"
              @confirm="showResultPage=true">
        <a-icon slot="icon" type="question-circle-o"/>
        <a>我已经提交过任务?</a>
      </a-popconfirm>
    </div>

    <!--任务进度/结果界面-->
    <div id="task-result" v-if="showResultPage">
      <a-page-header title="任务结果"
                     sub-title="已提交的词频分析任务执行结果。"
                     @back="showResultPage=false">
        <template slot="tags">
          <a-tag color="blue" v-if="justPosted">
            Submitting
          </a-tag>
          <a-tag color="blue" v-else-if="isWordfaRunning">
            Running
          </a-tag>
          <a-tag color="green" v-else-if="isWordfaResulted">
            Done
          </a-tag>
          <a-tag color="red" v-else>
            Error
          </a-tag>
        </template>
        <template slot="extra">
          <a-button key="refresh"
                    @click="fetchWordfaTaskResult">
            <a-icon type="sync"/>
            刷新
          </a-button>
        </template>
      </a-page-header>

      <!--      <a @click="fetchWordfaTaskResult()">(DEBUG)fetchWordfaTaskResult</a>-->

      <!--出现error，显示404-->
      <div v-if="isWordfaError && !justPosted">
        <a-result
                status="404"
                title="没有找到结果"
                v-bind:sub-title="wordfaResult.error"
        >
          <template #extra>
            <a-button key="task-post" type="primary" @click="showResultPage=false">
              返回提交任务
            </a-button>
          </template>

          <div class="desc">
            <p style="font-size: 16px;">
              <strong>我们没有发现由您提交的词频统计任务，这可能是由于:</strong>
            </p>
            <p>
              <a-icon :style="{ color: 'red' }" type="close-circle"/>
              您还没有提交任务或提交的任务不符合要求。
              <a @click="showResultPage=false">重新提交任务 &gt;</a>
            </p>
            <p>
              <a-icon :style="{ color: 'red' }" type="close-circle"/>
              系统出现了故障。
              <a @click="notYetImplemented">联系管理员 &gt;</a>
            </p>
          </div>
        </a-result>
      </div>
      <!--任务未完成，显示进度-->
      <div v-if="isWordfaRunning || justPosted">
        <a-card :bordered="false" hoverable style="text-align: center"
                title="词频统计中">
          <a-progress v-if="wordfaResult && wordfaResult.progress && wordfaResult.progress>0"
                      type="circle"
                      :stroke-color="{
                        '0%': '#108ee9',
                        '100%': '#87d068',
                      }"
                      style="text-align: center"
                      :percent="wordfaResult.progress"
                      status="active"
          />
          <a-spin v-if="!wordfaResult || !wordfaResult.progress || wordfaResult.progress<=0" size="large"/>
          <p style="text-align: center">任务正在执行中，请耐心等候...</p>
          <p style="text-align: center"><span style="color: bisque">我的token: {{ getToken() }}</span></p>

        </a-card>
      </div>
      <!--任务完成，显示结果-->
      <div v-if="isWordfaResulted && !justPosted">
        <a-table :columns="wordfaResultColumns" :data-source="wordfaResult.result">
          <a slot="name" slot-scope="text">{{ text }}</a>
        </a-table>
      </div>
    </div>

  </div>
</template>

<script>
    import axios from 'axios'
    import {baseUrl} from "../config";

    export default {
        name: "WordFaPage",
        data: function () {
            return {
                showResultPage: false,
                wordfaProgress: -1,
                wordfaResult: null,
                wordfaResultColumns: wordfaResultColumns,
                wordfaForm: {
                    token: this.getToken(),
                    keywords: "",
                    file: null,
                    sort_by: 0,
                    search_by: 0
                },
                wordfaLabelCol: {span: 4},
                wordfaWrapperCol: {span: 14},
                wordfaResultRefreshTimer: null,
                justPosted: false
            }
        },
        methods: {
            getToken() {
                let token = getCookie('wordfa-token');
                if (token != null) {
                    return token;
                } else {
                    token = Math.random().toString(36).slice(-8);
                    setCookie('wordfa-token', token);
                }
                return token;
            },
            fetchWordfaTaskResult() {
                let thisVue = this;
                // TODO: Production URL
                axios.get(baseUrl + '/api/wordfa', {
                    params: {
                        token: this.getToken()
                    }
                }).then(function (response) {
                    thisVue.wordfaResult = response.data;
                    if (thisVue.wordfaResult.progress) {
                        thisVue.wordfaResult.progress = Number(
                            (thisVue.wordfaResult.progress * 100)
                                .toFixed(2)
                        );
                    }
                })
            },
            refreshResult() {
                if (this.showResultPage === true) {
                    this.fetchWordfaTaskResult()
                }
            },
            notYetImplemented() {
                alert("Not Yet Implemented.")
            },
            onWordfaFileChange(e) {
                let files = e.target.files || e.dataTransfer.files;
                if (!files.length)
                    return;
                this.wordfaForm.file = files[0];
            },
            onWordfaFormSubmit() {
                this.wordfaForm.token = this.getToken();
                console.log("submit:", this.wordfaForm);

                let thisVue = this;

                if (thisVue.wordfaForm.keywords === '' || thisVue.wordfaForm.file === null) {
                    this.$message.error("提交被拒绝：请正确填写文件、关键词。");
                    return
                }

                const data = new FormData();

                data.append("token", thisVue.getToken());
                data.append("keywords", thisVue.wordfaForm.keywords);
                data.append("file", thisVue.wordfaForm.file);
                data.append("sort_by", thisVue.wordfaForm.sort_by);
                data.append("search_by", thisVue.wordfaForm.search_by);

                thisVue.wordfaResult = {progress: 0};
                thisVue.justPosted = true;
                thisVue.showResultPage = true;

                axios({
                    method: 'post',
                    headers: {'Content-Type': 'multipart/form-data'},
                    url:  baseUrl + '/api/wordfa',
                    data: data
                }).then(function (response) {
                    console.log(response.data);
                    thisVue.justPosted = false;
                    if (response.data.error) {
                        thisVue.$message.error('词频统计失败:\n' + response.data.error);
                    }
                });
            }
        },
        computed: {
            isWordfaError: function () {
                return (this.wordfaResult && this.wordfaResult.error !== undefined)
            },
            isWordfaRunning: function () {
                return (this.wordfaResult && this.wordfaResult.error === undefined && (this.wordfaResult.result === null || this.wordfaResult.result === undefined))
            },
            isWordfaResulted: function () {
                return (this.wordfaResult && this.wordfaResult.result && this.keywordsMatched)
            },
            keywordsMatched: function () {
                if (!this.wordfaResult || !this.wordfaResult.result) {
                    return false;
                }
                if (!this.wordfaForm || !this.wordfaForm.keywords) {
                    return true;
                }
                let re = /\s*[,，]\s*/g;
                let arrKeyInForm = this.wordfaForm.keywords.split(re);
                let arrKeyInResult = [];
                for (let i of this.wordfaResult.result) {
                    arrKeyInResult.push(i.keyword);
                }
                arrKeyInForm.sort();
                arrKeyInResult.sort();
                return (JSON.stringify(arrKeyInResult) === JSON.stringify(arrKeyInForm));
            }
        },
        mounted() {
            this.wordfaResultRefreshTimer = setInterval(this.refreshResult, 2000);
        }
    }

    // 显示结果的表格的表头
    const wordfaResultColumns = [
        {
            title: 'Keyword',
            dataIndex: 'keyword',
            key: 'keyword',
            ellipsis: true
        },
        {
            title: 'Frequency',
            dataIndex: 'frequency',
            key: 'frequency',
        }
    ];

    function setCookie(name, value) {
        let Days = 30;
        let exp = new Date();
        exp.setTime(exp.getTime() + Days * 24 * 60 * 60 * 1000);
        document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString();
    }

    function getCookie(name) {
        let arr, reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
        arr = document.cookie.match(reg);
        if (arr)
            return unescape(arr[2]);
        else
            return null;
    }

</script>

<style>
  #wordfapage {
    text-align: start;
  }

</style>
