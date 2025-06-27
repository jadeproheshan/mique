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
            <HomeOutlined />
            <span class="nav-text">不用写侧边栏</span>
          </a-menu-item>
          <a-menu-item key="2">
            <FileTextOutlined />
            <span class="nav-text">不用写侧边栏</span>
          </a-menu-item>
          <a-menu-item key="3">
            <DownloadOutlined />
            <span class="nav-text">不用写侧边栏</span>
          </a-menu-item>
          <a-menu-item key="4">
            <file-outlined />
            <span class="nav-text">不用写侧边栏</span>
          </a-menu-item>
          <a-menu-item key="5">
            <user-outlined />
            <span class="nav-text">不用写侧边栏</span>
          </a-menu-item>
          <a-menu-item key="6">
            <file-outlined />
            <span class="nav-text">不用写侧边栏</span>
          </a-menu-item>
          <a-menu-item key="7">
            <SettingOutlined />
            <span class="nav-text">不用写侧边栏</span>
          </a-menu-item>
        </a-menu>
      </a-layout-sider>
      <!-- 内容区 -->
      <a-layout style="margin-left: 200px; min-height: calc(100vh - 64px);">
        <a-layout-content style="margin: 0px 0px 0; overflow: initial;">
          
          <div style="padding: 24px; background: #fff; text-align: center;">

            //page-header
            <div class="page-header" style="height:160px;">
              <div style="font-size: 36px; font-weight: bold;">社团名称</div>
              <div style="font-size: 15px;">成员人数、指导老师、挂靠单位</div>
            </div>

            //tab与高级搜索
            <a-tabs v-model:activeKey="activeKey" class="manager-tab">
              <a-tab-pane key="basic-info" tab="基本信息">基本信息</a-tab-pane>
              <a-tab-pane key="member-management" tab="成员管理">成员管理</a-tab-pane>
              <a-tab-pane key="member-review" tab="成员审核">成员审核</a-tab-pane>
              <a-tab-pane key="supervisor" tab="指导老师?">指导老师</a-tab-pane>
              <a-tab-pane key="club-activities" tab="社团活动">社团活动</a-tab-pane>
              <a-tab-pane key="operation-record" tab="操作记录?">操作记录</a-tab-pane>
              <a-tab-pane key="advanced-search" tab="高级搜索">
                <a-form style="height: 200px; background-color: lightgray;">
                  高级搜索
                </a-form>
              </a-tab-pane>
            </a-tabs>

            //分割线
            <a-divider style="padding-top: 10px; padding-bottom: 10px;"/>

            //批量操作
            <a-space :size="16">
              <a-button class="manager-button">
                <PlusOutlined />
                新建
              </a-button class="manager-button">
              <a-button class="manager-button">批量导入</a-button>
              <a-button class="manager-button">批量删除</a-button>
              <a-button class="manager-button">
                <DownloadOutlined />下载</a-button>
            </a-space>

            //高级搜索下-成员列表页
            <a-table
              :columns="columns"
              :row-key="record => record.login.uuid"
              :data-source="dataSource"
              :pagination="pagination"
              :loading="loading"
              @change="handleTableChange"
            >
              <template #name="{ text }">{{ text.first }} {{ text.last }}</template>
            </a-table>
            <script lang="ts">
            import { TableState, TableStateFilters } from 'ant-design-vue/es/table/interface';
            import { usePagination } from 'vue-request';
            import { computed, defineComponent } from 'vue';
            const columns = [
              {
                title: '姓名',
                dataIndex: 'name',
                sorter: true,
                width: '20%',
                slots: { customRender: 'name' },
              },
              {
                title: '部门',
                dataIndex: 'department',
                width: '20%',
              },
              {
                title: '学/工号',
                dataIndex: 'studentId',
              },
              {
                title: '手机号',
                dataIndex: 'phonenumber',
                width: '20%',
              },
              {
                title: '还有别的栏 没写完',
                dataIndex: 'other',
                width: '20%',
              },
            ];

            type Pagination = TableState['pagination'];
            type APIParams = {
              results: number;
              page?: number;
              sortField?: string;
              sortOrder?: number;
              [key: string]: any;
            };
            type APIResult = {
              results: {
                gender: 'female' | 'male';
                name: string;
                email: string;
              }[];
            };

            const queryData = (params: APIParams) => {
              return `https://randomuser.me/api?noinfo&${new URLSearchParams(params)}`;
            };

            export default defineComponent({
              setup() {
                const { data: dataSource, run, loading, current, pageSize } = usePagination<APIResult>(
                  queryData,
                  {
                    formatResult: res => res.results,
                    pagination: {
                      currentKey: 'page',
                      pageSizeKey: 'results',
                    },
                  },
                );

                const pagination = computed(() => ({
                  total: 200,
                  current: current.value,
                  pageSize: pageSize.value,
                }));

                const handleTableChange = (pag: Pagination, filters: TableStateFilters, sorter: any) => {
                  run({
                    results: pag!.pageSize!,
                    page: pag?.current,
                    sortField: sorter.field,
                    sortOrder: sorter.order,
                    ...filters,
                  });
                };

                return {
                  dataSource,
                  pagination,
                  loading,
                  columns,
                  handleTableChange,
                };
              },
            });
            </script>

            //留白（看看还需要么？）
            <div style="height: 60px;"></div>

            </div>

                    
            </a-layout-content>
              <a-layout-footer style="text-align: center;">
                    河南大学社团管理系统
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
const activeKey = ref("basic-info");
const selectedKeys = ref<string[]>(['4']);
</script>

<style scoped>
//tab与高级搜索
.manager-tab{
  .ant-tabs-tab{
    font-size:16px;
  }
}

//批量操作
.manager-button {
  width: 48px;
  height: 32px;
  font-size: 20px;
}


.site-layout .site-layout-background {
  background: #fff;
}

[data-theme='dark'] .site-layout .site-layout-background {
  background: #141414;
}
</style>
