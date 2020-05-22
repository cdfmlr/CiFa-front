<template>
  <div id="aboutpage">
    <a-page-header title="CiFa" sub-title="Created By CDFMLR" />
    <!--  <h1>About CiFa</h1>-->
    <div id="readme">
      <p><img alt="CiFa" src="../assets/logo.png" style="height: 12%; width: 12%"/> 是一个词频统计工具，提供一套 Web API。后端使用 Golang 实现，完成计算密集型任务，并维持 http 服务。前端基于 Vue.js，实现了一套 Ant Design 风格的用户界面。</p>
      <h2>功能概览</h2>
      <ul>
        <li>
          <p>以《数据结构与算法》课程中所学的“串”结构为基础，实现了如下字符串模式匹配的算法：</p>
          <ol start>
            <li>暴力匹配算法</li>
            <li>KMP算法</li>
            <li>Rabin-Karp 算法</li>
          </ol>
        </li>
        <li>
          <p>以《数据结构与算法》课程中所学的“排序”算法为基础，实现了排序算法：</p>
          <ol start>
            <li>快速排序</li>
            <li>堆排序</li>
            <li>归并排序</li>
            <li>希尔排序</li>
            <li>插入排序</li>
            <li>选择排序</li>
          </ol>
        </li>
        <li>
          <p>
            在“字符串模式匹配算法”与“排序算法”实现的基础上，实现了词频统计工具。即给定关键词，选择另外一个文本文件，程序会统计关键词在该文件中出现的频数，完毕后以频数从大到小的顺序进行输出。或者也可以选择一个目录打包成的
            zip 文件，统计关键词在该目录下所有文本文件中出现的频数。
          </p>
        </li>
      </ul>
      <h2>开发进度</h2>
      <ul style="list-style: none;">
        <li>
          <input type="checkbox" disabled="disabled" checked />
          <span>排序算法</span>
        </li>
        <li>
          <input type="checkbox" disabled="disabled" checked />
          <span>字符串搜索算法</span>
        </li>
        <li>
          <input type="checkbox" disabled="disabled" checked />
          <span>词频统计实现</span>
        </li>
        <li>
          <input type="checkbox" disabled="disabled" checked />
          <span>服务框架</span>
        </li>
        <li>
          <input type="checkbox" disabled="disabled" checked />
          <span>词频统计 API、UI</span>
        </li>
        <li>
          <input type="checkbox" disabled="disabled" checked />
          <span>搜索 API、UI</span>
        </li>
        <li>
          <input type="checkbox" disabled="disabled" checked />
          <span>排序 API、UI</span>
        </li>
      </ul>
      <h2>开放源代码</h2>
      <p>Copyright 2020 CDFMLR</p>
      <p>
        Licensed under the Apache License, Version 2.0 (the &quot;License&quot;);
        you may not use this file except in compliance with the License.
        You may obtain a copy of the License at
      </p>
      <p>
        <a
          href="http://www.apache.org/licenses/LICENSE-2.0"
          target="_blank"
          class="url"
        >http://www.apache.org/licenses/LICENSE-2.0</a>
      </p>
      <p>
        Unless required by applicable law or agreed to in writing, software distributed under the License is
        distributed
        on an &quot;AS IS&quot; BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See
        the
        License for the specific language governing permissions and limitations under the License.
      </p>
      <p>&nbsp;</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "AboutPage"
};
</script>

<style scoped>
  #aboutpage {
    text-align: start;
  }
</style>