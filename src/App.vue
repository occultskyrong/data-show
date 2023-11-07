<template>
  <div>
    <a-typography-title class="t-title">数据展示</a-typography-title>
    <div class="t-form">
      <a-form layout="inline" :model="formState">
        <a-form-item label="RomID">
          <a-input v-model:value="formState.romId" placeholder="input DomID" />
        </a-form-item>
        <a-form-item label="Model">
          <a-input v-model:value="formState.model" placeholder="input Model" />
        </a-form-item>
        <a-form-item label="OS">
          <a-input v-model:value="formState.os" placeholder="input OS" />
        </a-form-item>
        <a-form-item label="Region">
          <a-input v-model:value="formState.region" placeholder="input Region" />
        </a-form-item>
        <a-form-item>
          <a-button type="primary" @click="handleSubmit">搜索</a-button>
          <a-button style="margin-left: 10px" @click="resetForm">重置</a-button>
        </a-form-item>
      </a-form>
    </div>
    <a-table :data-source="data" :columns="columns" :pagination="{ pageSize: 50 }">
      <template #bodyCell="{ record, column }">
        <span v-if="column.key === 'link'">
          <a-button type="primary" :href="record.link" target="_black">FileList</a-button>
        </span>
      </template>
    </a-table>
  </div>
</template>

<script lang="ts" setup>

import { reactive, ref } from 'vue';
import orignalData from './data.json';

interface FormState {
  romId: string;
  model: string;
  os: string;
  region: string;
}

const formState: any = reactive<FormState>({
  romId: '',
  model: '',
  os: '',
  region: '',
});

const data = ref(orignalData);

const columns = [
  {
    title: 'RomId',
    dataIndex: 'romId',
    key: 'romId',
  },
  {
    title: 'Time',
    dataIndex: 'time',
    key: 'time',
  },
  {
    title: 'Model',
    dataIndex: 'model',
    key: 'model',
  },
  {
    title: 'OS',
    dataIndex: 'os',
    key: 'os',
  },
  {
    title: 'Region',
    dataIndex: 'region',
    key: 'region',
  },
  {
    title: 'FileList',
    dataIndex: 'link',
    key: 'link'
  },
];

const keys = ['romId', 'model', 'os', 'region'];

const handleSubmit = () => {
  let __resultData: any = [];
  const filters: any = [];

  for (let key of keys) {
    const v = formState[key]
    if (v) {
      filters.push([key, v]);
    }
  }

  if (filters.length) {
    orignalData.forEach((item: any) => {
      let rr = true;
      for (let filter of filters) {
        const key = filter[0];
        const v = filter[1].toString().toLowerCase();
        if (!(item[key].toString().toLowerCase().includes(v))) {
          rr = false;
          break;
        }
      }
      if (rr) {
        __resultData.push(item);
      }
    });
    data.value = __resultData;
  } else {
    data.value = orignalData;
  }
}

const resetForm = () => {
  keys.forEach(k => {
    formState[k] = '';
  })
};

</script>


<style scoped>
.highlight {
  background-color: rgb(255, 192, 105);
  padding: 0px;
}

.t-title {
  text-align: center;
}

.t-form {
  margin: 20px 40px;
}
</style>
