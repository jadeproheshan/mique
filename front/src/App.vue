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
        width="260"
      >
        <a-menu style="margin-top: 24px;" v-model:selectedKeys="selectedKeys" theme="dark" mode="inline">
          <a-menu-item key="1">
            <HomeOutlined />
            <span class="nav-text">首页</span>
          </a-menu-item>
          <a-menu-item key="2">
            <FileTextOutlined />
            <span class="nav-text">文档</span>
          </a-menu-item>
          <a-menu-item key="3">
            <DownloadOutlined />
            <span class="nav-text">下载</span>
          </a-menu-item>
          <a-menu-item key="4">
            <FileOutlined />
            <span class="nav-text">社团管理</span>
          </a-menu-item>
          <a-menu-item key="5">
            <UserOutlined />
            <span class="nav-text">用户管理</span>
          </a-menu-item>
          <a-menu-item key="6">
            <TeamOutlined />
            <span class="nav-text">成员管理</span>
          </a-menu-item>
          <a-menu-item key="7">
            <SettingOutlined />
            <span class="nav-text">设置</span>
          </a-menu-item>
        </a-menu>
      </a-layout-sider>
      
      <!-- 内容区 -->
      <a-layout style="margin-left: 260px; min-height: calc(100vh - 64px);">
        <a-layout-content style="margin: 0px 0px 0; overflow: initial;">
          <div style="padding: 16px; background: #fff;">
            <!-- page-header -->
            <div class="page-header">
              <div style="font-size: 24px; font-weight: bold; padding-bottom: 18px;">社团名称</div>
              <div style="display: flex; margin-bottom: 16px;">
                <div style="font-size: 14px;">成员人数：xx人</div>
                <div style="padding-left: 24px;">指导老师：</div>
                <div style="padding-left: 24px;">挂靠单位：</div>
              </div>
              
            </div>

            <!-- tab与高级搜索 -->
            <a-tabs v-model:activeKey="activeKey" class="manager-tab">
              <a-tab-pane key="basic-info" tab="基本信息">基本信息</a-tab-pane>
              <a-tab-pane key="member-management" tab="成员管理">
                <div style="height: 140px; background-color: red; margin-bottom: 10px; border-bottom: 1px solid #eee; display: flex;">
                  <!-- 第一栏搜索 -->
                  <!-- <div style="height: auto; padding: 10px;">
                    <a-space style="width: 100%; justify-content: space-between;">
                      <a-input v-model:value="value" placeholder="Basic usage" />
                      <a-input v-model:value.lazy="value1" autofocus placeholder="Lazy usage" />
                    </a-space>
                    
                  </div> -->
                </div>
                <div style="height: 49px; background-color: blue; margin-top: 10px;"></div>
                <div style="text-align: left; margin-top: 10px;">

                  <!-- 成员列表 -->
                  <a-table
                    :columns="columns"
                    :data-source="dataSource"
                    :pagination="pagination"
                    :loading="loading"
                    @change="handleTableChange"
                    rowKey="id"
                  >
                    <template #action="{ record }">
                      <a-space>
                        <a-button size="small" @click="editMember(record)">编辑</a-button>
                        <a-button size="small" danger @click="deleteMember(record.id)">删除</a-button>
                      </a-space>
                    </template>
                  </a-table>
                </div>
              </a-tab-pane>
              <a-tab-pane key="member-review" tab="成员审核">成员审核</a-tab-pane>
              <a-tab-pane key="supervisor" tab="指导老师">指导老师</a-tab-pane>
              <a-tab-pane key="club-activities" tab="社团活动">社团活动</a-tab-pane>
              <a-tab-pane key="operation-record" tab="操作记录">操作记录</a-tab-pane>
              <a-tab-pane key="advanced-search" tab="高级搜索">
                <a-form layout="vertical" style="height: 200px; padding: 20px; background-color: #f5f5f5;">
                  <a-form-item label="姓名">
                    <a-input placeholder="请输入姓名" />
                  </a-form-item>
                  <a-form-item label="学号">
                    <a-input placeholder="请输入学号" />
                  </a-form-item>
                  <a-form-item>
                    <a-button type="primary">搜索</a-button>
                  </a-form-item>
                </a-form>
              </a-tab-pane>
            </a-tabs>

            <!-- 留白 -->
            <div style="height: 60px;"></div>
          </div>
        </a-layout-content>
        
        <a-layout-footer style="text-align: center;">
          河南大学社团管理系统 ©2023
        </a-layout-footer>
      </a-layout>
    </a-layout>
  </a-layout>
</template>

<script lang="ts" setup>
import { ref, reactive } from 'vue';
import {
  HomeOutlined,
  FileTextOutlined,
  DownloadOutlined,
  FileOutlined,
  UserOutlined,
  TeamOutlined,
  SettingOutlined,
  PlusOutlined,
} from '@ant-design/icons-vue';

// 搜索组件
import { watch } from 'vue';
const value = ref<string>('');
const value1 = ref<string>('');
watch(value, () => {
  console.log(value.value);
});
watch(value1, () => {
  console.log(value1.value);
});

// 标签页状态
const activeKey = ref("member-management");
const selectedKeys = ref<string[]>(['4']);

// 表格数据
const columns = [
  {
    title: '姓名',
    dataIndex: 'name',
    key: 'name',
  },
  {
    title: '部门',
    dataIndex: 'department',
    key: 'department',
  },
  {
    title: '学/工号',
    dataIndex: 'studentId',
    key: 'studentId',
  },
  {
    title: '手机号',
    dataIndex: 'phone',
    key: 'phone',
  },
  {
    title: '状态',
    dataIndex: 'status',
    key: 'status',
  },
  {
    title: '操作',
    key: 'action',
    slots: { customRender: 'action' },
  },
];

// 模拟数据
const dataSource = ref([
  {
    id: '1',
    name: '张三',
    department: '技术部',
    studentId: '20230001',
    phone: '13800138000',
    status: '已通过'
  },
  {
    id: '2',
    name: '李四',
    department: '宣传部',
    studentId: '20230002',
    phone: '13800138001',
    status: '待审核'
  },
  {
    id: '3',
    name: '王五',
    department: '外联部',
    studentId: '20230003',
    phone: '13800138002',
    status: '已通过'
  },
]);

const pagination = reactive({
  current: 1,
  pageSize: 10,
  total: 50,
  showSizeChanger: true,
  showQuickJumper: true,
});

const loading = ref(false);

const handleTableChange = (pag: any) => {
  pagination.current = pag.current;
  pagination.pageSize = pag.pageSize;
  // 这里可以添加加载数据的逻辑
  loading.value = true;
  setTimeout(() => {
    loading.value = false;
  }, 500);
};

const showAddModal = () => {
  // 显示添加成员的模态框
  console.log('显示添加成员模态框');
};

const editMember = (record: any) => {
  // 编辑成员
  console.log('编辑成员:', record);
};

const deleteMember = (id: string) => {
  // 删除成员
  console.log('删除成员:', id);
};
</script>

<style scoped>
/* tab样式 */
.manager-tab .ant-tabs-tab {
  font-size: 16px;
}

/* 按钮样式 */
.manager-button {
  height: 32px;
  font-size: 14px;
}

.site-layout .site-layout-background {
  background: #fff;
}

[data-theme='dark'] .site-layout .site-layout-background {
  background: #141414;
}
</style>