<template>
  <div id="sortalgopage">
    <a-page-header title="排序算法"
                   sub-title="给定一个浮点数序列，以从小到小的顺序进行输出。"/>
    <!-- Data-->
    <a-card>
      <template v-for="(d, index) in data">
        <!--👇这行代码里 :key="index+d+getRandom()" 是为了防止删除的时候 key 变化，导致显示出的删除和真正 data 的删除不同 -->
        <a-tag :key="index+d+getRandom()" closable @close="() => delDataAt(index)">
          {{ d }}
        </a-tag>
      </template>
      <a-input
              v-if="inputVisible"
              ref="input"
              type="text"
              size="small"
              :style="{ width: '78px' }"
              :value="inputValue"
              @change="handleInputChange"
              @blur="handleInputConfirm"
              @keyup.enter="handleInputConfirm"
      />
      <a-tag v-else style="background: #fff; borderStyle: dashed;" @click="showInput">
        <a-icon type="plus"/>
        添加数据
      </a-tag>
    </a-card>

    <div id="algorithm-choice">
      <a-radio-group v-model="algorithm">
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
    </div>

    <a-button type="danger" style="margin: 8px"
              @click="onSortClean">
      <a-icon type="delete"/>
      清空
    </a-button>
    <a-button type="primary" style="margin: 8px"
              @click="onSortSubmit">
      <a-icon type="sort-ascending"/>
      排序
    </a-button>
  </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: "SortalgoPage",
        data: function () {
            return {
                data: [2, 1, 3, 7, 4],
                algorithm: 0,
                inputVisible: false,
                inputValue: ''
            }
        },
        methods: {
            delDataAt(index) {
                this.data.splice(index, 1)
            },
            showInput() {
                this.inputVisible = true;
                this.$nextTick(function () {
                    this.$refs.input.focus();
                });
            },
            handleInputChange(e) {
                this.inputValue = e.target.value;
            },
            handleInputConfirm() {
                if (this.inputValue || this.inputValue === '0') {
                    const inputValue = Number(this.inputValue);
                    if (!isNaN(inputValue)) {
                        this.data.push(inputValue);
                        this.inputVisible = false;
                        this.inputValue = '';
                        console.log(this.data);
                    } else {
                        this.$message.warning('输入必须是数字哦');
                    }
                }
            },
            onSortSubmit() {
                let thisVue = this;
                let reqData = {
                    algorithm: Number(this.algorithm),
                    data: this.data
                };
                axios.post('http://localhost:9001/api/sort/float', JSON.stringify(reqData))
                    .then(res => {
                        console.log(res);
                        if (res.data && res.data.error) {
                            this.$message.error('排序失败：' + res.data.error);
                        } else if (res.data.result) {
                            thisVue.data = res.data.result;
                            this.$message.info('排序成功！用时' + res.data.time_cost);
                        } else {
                            this.$message.error('排序失败');
                        }
                    })
                    .catch(reason => {
                        this.$message.error('无法连接服务器，排序失败:\n' + reason);
                    })
            },
            onSortClean() {
                this.data = []
            },
            getRandom() {
                return Math.random().toString(36).slice(-8)
            }
        }
    }
</script>

<style scoped>

</style>