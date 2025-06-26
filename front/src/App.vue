<template>
  <a-layout>
    <!-- 顶部页头 -->
    <a-layout-header
      style="background: #fff; height: 64px; line-height: 64px; box-shadow: 0 2px 8px #f0f1f2; position: fixed; top: 0; left: 0; width: 100%; z-index: 10;"
    >
      <div style="font-size: 20px; font-weight: bold; color: #1677ff;">
        河南大学社团管理系统
      </div>
    </a-layout-header>
    <!-- 主体区域 -->
    <a-layout style="margin-top: 64px; min-height: calc(100vh - 64px);">
      <!-- 侧边栏 -->
      <a-layout-sider
        :style="{
          overflow: 'auto',
          height: 'calc(100vh - 64px)',
          position: 'fixed',
          left: 0,
          top: '64px',
          bottom: 0,
        }"
        width="200"
      >

        <a-menu style="margin-top: 24px;" v-model:selectedKeys="selectedKeys" theme="dark" mode="inline">
          <a-menu-item key="1">
            <user-outlined />
            <span class="nav-text">nav 1</span>
          </a-menu-item>
          <a-menu-item key="2">
            <video-camera-outlined />
            <span class="nav-text">nav 2</span>
          </a-menu-item>
          <a-menu-item key="3">
            <upload-outlined />
            <span class="nav-text">nav 3</span>
          </a-menu-item>
          <a-menu-item key="4">
            <bar-chart-outlined />
            <span class="nav-text">nav 4</span>
          </a-menu-item>
          <a-menu-item key="5">
            <cloud-outlined />
            <span class="nav-text">nav 5</span>
          </a-menu-item>
          <a-menu-item key="6">
            <appstore-outlined />
            <span class="nav-text">nav 6</span>
          </a-menu-item>
          <a-menu-item key="7">
            <team-outlined />
            <span class="nav-text">nav 7</span>
          </a-menu-item>
          <a-menu-item key="8">
            <shop-outlined />
            <span class="nav-text">nav 8</span>
          </a-menu-item>
        </a-menu>
      </a-layout-sider>
      <!-- 内容区 -->
      <a-layout style="margin-left: 200px; min-height: calc(100vh - 64px);">
        <a-layout-content style="margin: 0px 0px 0; overflow: initial;">
          <div style="padding: 24px; background: #fff; text-align: center;">
            ...
            <br />
            Really
            <br />
            ...
            <br />
            long
            <br />
            ...
            <br />
            content
            <br />
            ...
            <br />
            
          </div>
        </a-layout-content>
        <a-layout-footer style="text-align: center;">
          Ant Design ©2018 Created by Ant UED
        </a-layout-footer>
      </a-layout>
    </a-layout>
  </a-layout>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import {
  UserOutlined,
  VideoCameraOutlined,
  UploadOutlined,
  BarChartOutlined,
  CloudOutlined,
  AppstoreOutlined,
  TeamOutlined,
  ShopOutlined,
} from '@ant-design/icons-vue';
const selectedKeys = ref<string[]>(['4']);
</script>

<style scoped>
.site-layout .site-layout-background {
  background: #fff;
}

[data-theme='dark'] .site-layout .site-layout-background {
  background: #141414;
}
</style>
