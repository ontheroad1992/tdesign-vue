<template>
  <div class="tdesign-demo-block-column tdesign-demo__table" style="max-width: 1200px">
    <div>
      <t-radio-group v-model="leftFixedColumn" variant="default-filled">
        <t-radio-button :value="1">左侧固定一列</t-radio-button>
        <t-radio-button :value="2">左侧固定两列</t-radio-button>
      </t-radio-group>
    </div>

    <div>
      <t-radio-group v-model="rightFixedColumn" variant="default-filled">
        <t-radio-button :value="1">右侧固定一列</t-radio-button>
        <t-radio-button :value="2">右侧固定两列</t-radio-button>
      </t-radio-group>
    </div>

    <div>
      <t-radio-group v-model="tableLayout" variant="default-filled">
        <t-radio-button value="fixed">table-layout: fixed</t-radio-button>
        <t-radio-button value="auto">table-layout: auto</t-radio-button>
      </t-radio-group>
      <t-checkbox v-model="emptyData" style="margin-left: 16px; vertical-align: middle">空数据</t-checkbox>
    </div>

    <!--
      如果希望表格列宽自适应，设置 `table-layout: auto`。
      `table-layout: auto` 模式下的固定列，必须指定 tableContentWidth，表示：表格总宽度固定，列宽随内容自适应。
      注意：`table-layout: auto` 模式下，不允许使用 resizable
    -->
    <t-table
      ref="tableRef"
      rowKey="index"
      :data="emptyData ? [] : data"
      :columns="columns"
      :table-layout="tableLayout"
      :table-content-width="tableLayout === 'fixed' ? undefined : '1000px'"
      bordered
      resizable
    >
      <template #operation="{ row }">
        <t-link theme="primary" hover="color" @click="rehandleClickOp(row)">
          {{ row.status === 0 ? '查看详情' : '再次申请' }}
        </t-link>
      </template>
    </t-table>
  </div>
</template>
<script lang="jsx">
import { ErrorCircleFilledIcon, CheckCircleFilledIcon, CloseCircleFilledIcon } from 'tdesign-icons-vue';

const data = [];
for (let i = 0; i < 5; i++) {
  data.push({
    index: i + 1,
    applicant: ['贾明', '张三', '王芳'][i % 3],
    status: i % 3,
    channel: ['电子签署', '纸质签署', '纸质签署'][i % 3],
    detail: {
      email: ['w.cezkdudy@lhll.au', 'r.nmgw@peurezgn.sl', 'p.cumx@rampblpa.ru'][i % 3],
    },
    matters: ['宣传物料制作费用', 'algolia 服务报销', '相关周边制作费', '激励奖品快递费'][i % 4],
    time: [2, 3, 1, 4][i % 4],
    createTime: ['2022-01-01', '2022-02-01', '2022-03-01', '2022-04-01', '2022-05-01'][i % 4],
  });
}
export default {
  data() {
    return {
      data,
      tableLayout: 'fixed',
      leftFixedColumn: 2,
      rightFixedColumn: 1,
      emptyData: false,
    };
  },
  computed: {
    columns() {
      return [
        {
          colKey: 'applicant',
          title: '申请人',
          width: 100,
          fixed: 'left',
        },
        {
          colKey: 'status',
          title: '审批状态',
          width: 120,
          fixed: this.leftFixedColumn >= 2 ? 'left' : undefined,
          cell: (h, { row }) => {
            const statusNameListMap = {
              0: { label: '审批通过', theme: 'success', icon: <CheckCircleFilledIcon /> },
              1: { label: '审批失败', theme: 'danger', icon: <CloseCircleFilledIcon /> },
              2: { label: '审批过期', theme: 'warning', icon: <ErrorCircleFilledIcon /> },
            };
            return (
              <t-tag shape="round" theme={statusNameListMap[row.status].theme} variant="light-outline">
                {statusNameListMap[row.status].icon}
                {statusNameListMap[row.status].label}
              </t-tag>
            );
          },
        },
        { colKey: 'detail.email', title: '邮箱地址', width: 180 },
        { colKey: 'matters', title: '申请事项', width: 200 },
        {
          colKey: 'createTime',
          title: '申请日期',
          width: 180,
          fixed: this.rightFixedColumn >= 2 ? 'right' : undefined,
        },
        {
          colKey: 'operation',
          title: '操作',
          width: 120,
          fixed: 'right',
        },
      ];
    },
  },
  methods: {
    rehandleClickOp(data) {
      console.log(data);
    },
    // eslint-disable-next-line
    scrollToCreateTime() {
      // 横向滚动到指定列
      this.$refs.tableRef.scrollColumnIntoView('matters');
    },
  },
};
</script>

<style lang="less" scoped>
.tdesign-demo-block-column {
  width: 100%;
}
</style>
