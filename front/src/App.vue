<template>
  <a-layout>
    <!-- 顶部页头 -->
    <a-layout-header
      style="background: #fff; height: 64px; line-height: 64px; box-shadow: 0 2px 8px #f0f1f2; position: fixed; top: 0; left: 0; width: 100%; z-index: 10;"
    >
      <div class="header-content">
        <img class="logo" src="https://www.henu.edu.cn/images/logo.png" alt="河南大学logo" />
        <div style="font-size: 20px; font-weight: bold; color: #1677ff;">
          河南大学社团管理系统
        </div>
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
                <div class="content-wrapper">
                  <!-- 搜索区域 -->
                  <div class="search-area">
                    <!-- 第一行：搜索框和查询按钮 -->
                    <a-row :gutter="16" style="margin-bottom: 16px">
                      <a-col :span="6">
                        <a-input-group compact>
                          <span class="search-label">成员编号</span>
                          <a-input
                            v-model:value="searchForm.memberCode"
                            style="width: calc(100% - 70px)"
                            allowClear
                          />
                        </a-input-group>
                      </a-col>
                      <a-col :span="6">
                        <a-input-group compact>
                          <span class="search-label">成员名称</span>
                          <a-input
                            v-model:value="searchForm.memberName"
                            style="width: calc(100% - 70px)"
                            allowClear
                          />
                        </a-input-group>
                      </a-col>
                      <a-col :span="6">
                        <a-input-group compact>
                          <span class="search-label">所属学院</span>
                          <a-select
                            v-model:value="searchForm.college"
                            style="width: calc(100% - 70px)"
                            allowClear
                          >
                            <a-select-option value="">全部</a-select-option>
                            <a-select-option value="computer">计算机学院</a-select-option>
                            <a-select-option value="math">数学学院</a-select-option>
                            <a-select-option value="physics">物理学院</a-select-option>
                          </a-select>
                        </a-input-group>
                      </a-col>
                      <a-col :span="6">
                        <a-button type="primary" @click="onSearch">
                          <search-outlined />
                          查询
                        </a-button>
                      </a-col>
                    </a-row>

                    <!-- 第二行：筛选器和重置按钮 -->
                    <a-row :gutter="16">
                      <a-col :span="6">
                        <a-input-group compact>
                          <span class="search-label">筛选方式</span>
                          <a-select
                            v-model:value="searchForm.filterType"
                            style="width: calc(100% - 70px)"
                            allowClear
                          >
                            <a-select-option value="">全部</a-select-option>
                            <a-select-option value="department">按部门</a-select-option>
                            <a-select-option value="role">按角色</a-select-option>
                          </a-select>
                        </a-input-group>
                      </a-col>
                      <a-col :span="6">
                        <a-input-group compact>
                          <span class="search-label">创建时间</span>
                          <a-range-picker
                            v-model:value="searchForm.createTime"
                            style="width: calc(100% - 70px)"
                            :placeholder="['开始', '结束']"
                          />
                        </a-input-group>
                      </a-col>
                      <a-col :span="6">
                        <a-input-group compact>
                          <span class="search-label">状态</span>
                          <a-select
                            v-model:value="searchForm.status"
                            style="width: calc(100% - 70px)"
                            allowClear
                          >
                            <a-select-option value="">全部</a-select-option>
                            <a-select-option value="active">在职</a-select-option>
                            <a-select-option value="inactive">离职</a-select-option>
                          </a-select>
                        </a-input-group>
                      </a-col>
                      <a-col :span="6">
                        <a-button @click="resetSearch">
                          <reload-outlined />
                          重置
                        </a-button>
                      </a-col>
                    </a-row>
                  </div>

                  <!-- 操作栏 -->
                  <div class="operation-bar">
                    <div class="left-buttons">
                      <a-button type="primary" @click="handleCreate">
                        <plus-outlined />
                        新建
                      </a-button>
                      <a-button @click="handleBatchImport">
                        <upload-outlined />
                        批量导入
                      </a-button>
                      <a-button 
                        :disabled="!selectedRowKeys.length"
                        @click="handleBatchDelete"
                      >
                        <delete-outlined />
                        批量删除
                      </a-button>
                    </div>
                    <div class="right-buttons">
                      <a-button @click="handleDownload">
                        <download-outlined />
                        下载
                      </a-button>
                    </div>
                  </div>
                </div>
                <!-- 批量操作栏 -->
                <div style="padding: 16px 24px; background: #fafafa; border-radius: 4px; margin-bottom: 16px;">
                  <a-space>
                    <a-button 
                      :disabled="!selectedRowKeys.length"
                      @click="handleBatchTransfer"
                    >
                      <swap-outlined />
                      批量转移
                    </a-button>
                    <a-button 
                      :disabled="!selectedRowKeys.length"
                      @click="handleBatchExport"
                    >
                      <export-outlined />
                      批量导出
                    </a-button>
                    <a-tag v-if="selectedRowKeys.length" style="margin-left: 8px;">
                      已选择 {{ selectedRowKeys.length }} 项
                    </a-tag>
                  </a-space>
                </div>

                <div style="text-align: left;">
                  <!-- 成员列表 -->
                  <a-table
                    :columns="columns"
                    :data-source="dataSource"
                    :pagination="pagination"
                    :loading="loading"
                    :row-selection="{ selectedRowKeys: selectedRowKeys, onChange: onSelectChange }"
                    :scroll="{ y: 'calc(100vh - 400px)' }"
                    rowKey="id"
                  >
                    <!-- 性别列自定义渲染 -->
                    <template #bodyCell="{ column, record }">
                      <template v-if="column.key === 'gender'">
                        {{ record.gender === 'male' ? '男' : '女' }}
                      </template>
                      
                      <!-- 操作列自定义渲染 -->
                      <template v-if="column.key === 'action'">
                        <a-space>
                          <a-button type="link" size="small" @click="handleView(record)">
                            查看
                          </a-button>
                          <a-button type="link" size="small" @click="handleDelete(record)">
                            删除
                          </a-button>
                        </a-space>
                      </template>
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

    <!-- 部门转移弹窗 -->
    <a-modal
      v-model:visible="transferModalVisible"
      title="批量转移部门"
      @ok="confirmTransfer"
      @cancel="cancelTransfer"
      okText="确认"
      cancelText="取消"
    >
      <a-form layout="vertical">
        <a-form-item
          label="选择目标部门"
          required
          :validateStatus="targetDepartment ? '' : 'error'"
          :help="targetDepartment ? '' : '请选择转移的目标部门'"
        >
          <a-select
            v-model:value="targetDepartment"
            style="width: 100%"
            placeholder="请选择部门"
          >
            <a-select-option
              v-for="dept in departmentOptions"
              :key="dept.value"
              :value="dept.value"
            >
              {{ dept.label }}
            </a-select-option>
          </a-select>
        </a-form-item>
        <p style="color: #666;">
          已选择 {{ selectedRowKeys.length }} 名成员，转移后将更改这些成员的所属部门
        </p>
      </a-form>
    </a-modal>
  </a-layout>
</template>

<script lang="ts" setup>
import { ref, reactive, h } from 'vue';
import {
  HomeOutlined,
  FileTextOutlined,
  DownloadOutlined,
  FileOutlined,
  UserOutlined,
  TeamOutlined,
  SettingOutlined,
  PlusOutlined,
  UploadOutlined,
  DeleteOutlined,
  SwapOutlined,
  ExportOutlined,
  SearchOutlined,
  ReloadOutlined,
} from '@ant-design/icons-vue';
import { message, Modal, Tag, DatePicker } from 'ant-design-vue';
const { RangePicker } = DatePicker;

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
    title: '成员编号',
    dataIndex: 'memberCode',
    key: 'memberCode',
    width: 120,
  },
  {
    title: '成员姓名',
    dataIndex: 'memberName',
    key: 'memberName',
    width: 120,
  },
  {
    title: '性别',
    dataIndex: 'gender',
    key: 'gender',
    width: 80,
  },
  {
    title: '所属学院',
    dataIndex: 'college',
    key: 'college',
    width: 150,
  },
  {
    title: '手机号',
    dataIndex: 'phone',
    key: 'phone',
    width: 120,
  },
  {
    title: '入社时间',
    dataIndex: 'joinTime',
    key: 'joinTime',
    width: 120,
  },
  {
    title: '职位',
    dataIndex: 'position',
    key: 'position',
    width: 120,
  },
  {
    title: '操作',
    key: 'action',
    width: 120,
    fixed: 'right',
  },
];

// 模拟数据
const dataSource = ref([
  {
    key: '1',
    memberCode: 'M001',
    memberName: '张三',
    gender: 'male',
    college: '计算机学院',
    phone: '13800138000',
    joinTime: '2023-09-01',
    position: '部长',
  },
  {
    key: '2',
    memberCode: 'M002',
    memberName: '李四',
    gender: 'female',
    college: '数学学院',
    phone: '13800138001',
    joinTime: '2023-09-02',
    position: '副部长',
  },
  // 更多数据...
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

// 搜索表单数据
const searchForm = reactive({
  memberCode: '',
  memberName: '',
  college: '',
  filterType: '',
  createTime: [],
  status: ''
});

// 搜索方法
const onSearch = () => {
  loading.value = true;
  console.log('搜索条件：', searchForm);
  setTimeout(() => {
    loading.value = false;
  }, 500);
};

// 重置搜索
const resetSearch = () => {
  searchForm.memberCode = '';
  searchForm.memberName = '';
  searchForm.college = '';
  searchForm.filterType = '';
  searchForm.createTime = [];
  searchForm.status = '';
  onSearch();
};

// 文件上传前的检查
const beforeUpload = (file: File) => {
  const isExcel = file.type === 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet' || 
                 file.type === 'application/vnd.ms-excel';
  if (!isExcel) {
    message.error('只能上传 Excel 文件！');
    return false;
  }
  const isLt2M = file.size / 1024 / 1024 < 2;
  if (!isLt2M) {
    message.error('文件必须小于 2MB！');
    return false;
  }
  return true;
};

// 处理文件上传
const handleUpload = ({ file, onSuccess, onError }: any) => {
  // TODO: 实现文件上传逻辑
  console.log('上传文件：', file);
  setTimeout(() => {
    onSuccess();
  }, 1000);
};

// 下载模板
const downloadTemplate = () => {
  // TODO: 实现模板下载逻辑
  console.log('下载模板');
};

// 表格选择相关
const selectedRowKeys = ref<string[]>([]);

const rowSelection = {
  selectedRowKeys: selectedRowKeys.value,
  onChange: (keys: string[]) => {
    selectedRowKeys.value = keys;
  }
};

// 批量操作方法
const handleBatchDelete = () => {
  Modal.confirm({
    title: '确认删除',
    content: `确定要删除选中的 ${selectedRowKeys.value.length} 条记录吗？`,
    okText: '确认',
    cancelText: '取消',
    onOk: () => {
      // TODO: 实现批量删除逻辑
      message.success(`已删除 ${selectedRowKeys.value.length} 条记录`);
      selectedRowKeys.value = [];
    }
  });
};

// 部门转移相关
const transferModalVisible = ref(false);
const targetDepartment = ref('');
const departmentOptions = [
  { label: '技术部', value: '技术部' },
  { label: '宣传部', value: '宣传部' },
  { label: '外联部', value: '外联部' },
  { label: '秘书处', value: '秘书处' },
  { label: '财务部', value: '财务部' }
];

const handleBatchTransfer = () => {
  transferModalVisible.value = true;
};

const confirmTransfer = () => {
  if (!targetDepartment.value) {
    message.warning('请选择目标部门');
    return;
  }
  
  // TODO: 实现转移逻辑
  message.success(`已将 ${selectedRowKeys.value.length} 名成员转移至 ${targetDepartment.value}`);
  transferModalVisible.value = false;
  targetDepartment.value = '';
  selectedRowKeys.value = [];
};

const cancelTransfer = () => {
  transferModalVisible.value = false;
  targetDepartment.value = '';
};

const handleBatchExport = () => {
  // TODO: 实现批量导出逻辑
  console.log('批量导出：', selectedRowKeys.value);
};

// 查看成员详情
const handleView = (record: any) => {
  console.log('查看成员:', record);
  // TODO: 实现查看详情逻辑
};

// 删除成员
const handleDelete = (record: any) => {
  console.log('删除成员:', record);
  // TODO: 实现删除逻辑
};

// 新建成员
const handleCreate = () => {
  console.log('新建成员');
  // TODO: 实现新建逻辑
};

// 批量导入
const handleBatchImport = () => {
  console.log('批量导入');
  // TODO: 实现批量导入逻辑
};

// 下载
const handleDownload = () => {
  console.log('下载');
  // TODO: 实现下载逻辑
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

/* 批量操作栏样式 */
.batch-operation-bar {
  margin-bottom: 8px;
  padding: 16px;
  background: #fafafa;
  border-radius: 4px;
}

/* 状态标签样式 */
:deep(.ant-tag) {
  min-width: 60px;
  text-align: center;
}

/* 表格滚动条样式 */
:deep(.ant-table-body) {
  overflow-y: auto !important;
  max-height: calc(100vh - 400px) !important;
}

/* 搜索标签样式 */
.search-label {
  display: inline-block;
  width: 70px;
  height: 32px;
  line-height: 32px;
  background: #fafafa;
  border: 1px solid #d9d9d9;
  border-right: none;
  border-radius: 2px 0 0 2px;
  text-align: center;
  color: rgba(0, 0, 0, 0.85);
}

:deep(.ant-input-group) {
  display: flex;
  align-items: center;
}

:deep(.ant-input-group .ant-input:first-child),
:deep(.ant-input-group .ant-select:first-child),
:deep(.ant-input-group .ant-range-picker:first-child) {
  border-radius: 0 2px 2px 0;
}

/* 操作按钮样式 */
:deep(.ant-btn-link) {
  padding: 0 8px;
  height: 24px;
  line-height: 24px;
}

/* 表格固定列阴影 */
:deep(.ant-table-cell-fix-right) {
  background: #fff;
}

/* 操作栏样式 */
.operation-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2px 24px;
  background: #fff;
  border-radius: 2px;
  margin: 4px;
}

.left-buttons {
  display: flex;
  gap: 8px;
}

.right-buttons {
  display: flex;
  gap: 8px;
}

/* 按钮图标对齐 */
:deep(.anticon) {
  vertical-align: -0.125em;
}

/* 区域间距调整 */
.search-area {
  padding: 24px;
  background: #fff;
  border-radius: 2px;
  margin: 2px;
}

:deep(.ant-table-wrapper) {
  background: #fff;
  border-radius: 2px;
  padding: 24px;
}

/* 确保表格内容不会顶到边框 */
:deep(.ant-table) {
  margin: -8px !important;
}

/* 内容区包装器 */
.content-wrapper {
  padding: 0 24px;
}

/* 搜索区域 */
.search-area {
  padding: 24px;
  background: #fff;
  border-radius: 2px;
  margin: 2px 0;
}

/* 操作栏样式 */
.operation-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2px 24px;
  background: #fff;
  border-radius: 2px;
  margin: 4px 0;
}

/* 表格容器样式 */
:deep(.ant-table-wrapper) {
  background: #fff;
  border-radius: 2px;
  padding: 24px;
  margin: 2px 0;
}

.header-content {
  display: flex;
  align-items: center;
  gap: 8px;
  height: 100%;
  padding: 0 16px;
}

.logo {
  height: 40px;
  width: auto;
  object-fit: contain;
  margin: 0px -100px -10px -50px;
}
</style>