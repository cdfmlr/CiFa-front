<template>
  <a-layout id="app">
    <!-- Sider Nav -->
    <a-layout-sider breakpoint="lg" collapsed-width="0" @collapse="onCollapse" @breakpoint="onBreakpoint"
                    style="background: #fff">
      <div class="logo">
        <img alt="CiFa" src="./assets/logo.png" style="height: 56px; width: 117px"/>
        <hr/>
      </div>
      <a-menu theme="light" mode="inline" v-model="navSelected" @select="onNavMenuSelect">
        <a-menu-item key="home">
          <a-icon type="home"/>
          <span class="nav-text">欢迎使用</span>
        </a-menu-item>
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
          <div v-else-if="navSelected[0]==='sortalgo'">
            <sortalgo-page/>
          </div>
          <div v-else-if="navSelected[0]==='strsearch'">
            <strsearch-page/>
          </div>
          <div v-else>  <!--welcome home page-->
            <a-page-header title="CiFa" sub-title="Created By CDFMLR"/>
            <a-row :gutter="16" type="flex" justify="space-around" align="middle">
              <a-col :xs="16" :sm="10" :md="8" :lg="8" :xl="8">
                <!--词频统计-->
                <div class="nav-card">
                  <a-card style="width: 200px" hoverable>
                    <img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gf153ztau3j30zk0nptbs.jpg"
                         slot="cover"
                         height="150px"
                         width="180px"
                         alt="wordfa"/>
                    <template slot="actions" class="ant-card-actions">
                      <a-icon type="file-search"/>
                    </template>
                    <a-card-meta title="词频统计" description="给定关键词，统计其在文件中出现的频数"/>
                  </a-card>
                </div>
              </a-col>
              <a-col :xs="16" :sm="10" :md="8" :lg="8" :xl="8">
                <!--子串搜索-->
                <div class="nav-card">
                  <a-card class="nav-card" style="width: 200px" hoverable>
                    <img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gf159evrzqj30hs0dcjsd.jpg"
                         slot="cover"
                         height="150px"
                         width="180px"
                         alt="wordfa"/>
                    <template slot="actions" class="ant-card-actions">
                      <a-icon type="build"/>
                    </template>
                    <a-card-meta title="子串搜索" description="给定两个字符串，搜索子串在父串中的位置。"/>
                  </a-card>
                </div>

              </a-col>
              <a-col :xs="16" :sm="10" :md="8" :lg="8" :xl="8">
                <!--排序算法-->
                <div class="nav-card">
                  <a-card class="nav-card" style="width: 200px" hoverable>
                    <img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gf15ikgoqvj30d70dcwfd.jpg"
                         slot="cover"
                         height="150px"
                         width="180px"
                         alt="wordfa"/>
                    <template slot="actions" class="ant-card-actions">
                      <a-icon type="sort-ascending"/>
                    </template>
                    <a-card-meta title="排序算法" description="给定浮点数序列，以从小到小的顺序排序。"/>
                  </a-card>

                </div>
              </a-col>

            </a-row>
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
    import SortalgoPage from "./components/SortalgoPage";
    import StrsearchPage from "./components/StrsearchPage";

    export default {
        name: "App",
        components: {StrsearchPage, SortalgoPage, AboutPage, WordFaPage},
        data: function () {
            return {
                navSelected: ["home"]
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
  .nav-card {
    margin: 8px 8px 12px;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
