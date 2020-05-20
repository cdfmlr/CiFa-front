<template>
  <a-layout id="app">
    <!-- Sider Nav -->
    <a-layout-sider breakpoint="lg" collapsed-width="0" @collapse="onCollapse" @breakpoint="onBreakpoint"
                    style="background: #fff">
      <div class="logo">
        <a-icon type="loading"/>
        <span> CiFa</span>
        <hr/>
      </div>
      <a-menu theme="light" mode="inline" v-model="navSelected" @select="onNavMenuSelect">
        <a-menu-item key="wordfa">
          <a-icon type="file-search"/>
          <span class="nav-text">词频统计</span>
        </a-menu-item>
        <a-menu-item key="strsearch">
          <a-icon type="build"/>
          <span class="nav-text">子串搜索</span>
        </a-menu-item>
        <a-menu-item key="sortalgo">
          <a-icon type="sort-ascending"/>
          <span class="nav-text">排序算法</span>
        </a-menu-item>
        <a-menu-item key="about">
          <a-icon type="info-circle"/>
          <span class="nav-text">关于CiFa</span>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>

      <!--Header-->
      <a-layout-header :style="{ background: '#fff', padding: 0 }">
        <h1>CiFa</h1>
      </a-layout-header>

      <!-- Content -->
      <a-layout-content :style="{ margin: '24px 16px 0' }">
        <div :style="{ padding: '24px', background: '#fff', minHeight: '360px' }">
<!--          {{ navSelected }}-->
          <div v-if="navSelected[0]==='wordfa'">
            <word-fa-page/>
          </div>
          <div v-else-if="navSelected[0]==='about'">
            <about-page/>
          </div>
          <div v-else>
            Not Yet Implemented.
          </div>
        </div>
      </a-layout-content>

      <!-- Footer -->
      <a-layout-footer style="textAlign: center">CiFa ©2020 Created by CDFMLR</a-layout-footer>
    </a-layout>
  </a-layout>
</template>

<script>
    import WordFaPage from "./components/WordFaPage";
    import AboutPage from "./components/AboutPage";
    export default {
        name: "App",
        components: {AboutPage, WordFaPage},
        data: function () {
            return {
                navSelected: ["sortalgo"]
            };
        },
        methods: {
            onCollapse(collapsed, type) {
                console.log(collapsed, type);
            },
            onBreakpoint(broken) {
                console.log(broken);
            },
            onNavMenuSelect(i) {
                console.log(i);
            }
        }
    };
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
