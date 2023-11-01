<template>
  <a-table :data-source="data" :columns="columns" :pagination="{ pageSize: 50 }">
    <template #headerCell="{ column }">
      <template v-if="column.key === 'romId'">
        <span style="color: #1890ff">RomId</span>
      </template>
    </template>
    <template #customFilterDropdown="{ setSelectedKeys, selectedKeys, confirm, clearFilters, column }">
      <div style="padding: 8px">
        <a-input ref="searchInput" :placeholder="`Search ${column.dataIndex}`" :value="selectedKeys[0]"
          style="width: 188px; margin-bottom: 8px; display: block"
          @change="(e: any) => setSelectedKeys(e.target.value ? [e.target.value] : [])"
          @pressEnter="handleSearch(selectedKeys, confirm, column.dataIndex)" />
        <a-button type="primary" size="small" style="width: 90px; margin-right: 8px"
          @click="handleSearch(selectedKeys, confirm, column.dataIndex)">
          <template #icon>
            <SearchOutlined />
          </template>
          Search
        </a-button>
        <a-button size="small" style="width: 90px" @click="handleReset(clearFilters)">
          Reset
        </a-button>
      </div>
    </template>
    <template #customFilterIcon="{ filtered }">
      <search-outlined :style="{ color: filtered ? '#108ee9' : undefined }" />
    </template>
    <template #bodyCell="{ text, record, column }">
      <span v-if="state.searchText && state.searchedColumn === column.dataIndex">
        <template v-for="(fragment, i) in text
          .toString()
          .split(new RegExp(`(?<=${state.searchText})|(?=${state.searchText})`, 'i'))">
          <mark v-if="fragment.toLowerCase() === state.searchText.toLowerCase()" :key="i" class="highlight">
            {{ fragment }}
          </mark>
          <template v-else>{{ fragment }}</template>
        </template>
      </span>
      <span v-else-if="column.key === 'link'">
        <a-button type="primary" :href="record.link" target="_black">FileList</a-button>
      </span>
    </template>
  </a-table>
</template>

<script lang="ts" setup>
import { SearchOutlined } from '@ant-design/icons-vue';
import { reactive, ref } from 'vue';
import data from './data.json';

const state = reactive({
  searchText: '',
  searchedColumn: '',
});

const searchInput = ref();

const columns = [
  {
    title: 'RomId',
    dataIndex: 'romId',
    key: 'romId',
    customFilterDropdown: true,
    onFilter: (value: any, record: any) => record.romId.toString().toLowerCase().includes(value.toLowerCase()),
    onFilterDropdownOpenChange: (visible: any) => {
      if (visible) {
        setTimeout(() => { searchInput.value.focus(); }, 100);
      }
    },
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
    customFilterDropdown: true,
    onFilter: (value: any, record: any) => record.address.toString().toLowerCase().includes(value.toLowerCase()),
    onFilterDropdownOpenChange: (visible: any) => {
      if (visible) {
        setTimeout(() => { searchInput.value.focus(); }, 100);
      }
    },
  },
  {
    title: 'OS',
    dataIndex: 'os',
    key: 'os',
    customFilterDropdown: true,
    onFilter: (value: any, record: any) => record.address.toString().toLowerCase().includes(value.toLowerCase()),
    onFilterDropdownOpenChange: (visible: any) => {
      if (visible) {
        setTimeout(() => { searchInput.value.focus(); }, 100);
      }
    },
  },
  {
    title: 'Region',
    dataIndex: 'region',
    key: 'region',
    customFilterDropdown: true,
    onFilter: (value: any, record: any) => record.address.toString().toLowerCase().includes(value.toLowerCase()),
    onFilterDropdownOpenChange: (visible: any) => {
      if (visible) {
        setTimeout(() => { searchInput.value.focus(); }, 100);
      }
    },
  },
  {
    title: 'FileList',
    dataIndex: 'link',
    key: 'link'
  },
];

const handleSearch = (selectedKeys: any, confirm: any, dataIndex: any) => {
  confirm();
  state.searchText = selectedKeys[0];
  state.searchedColumn = dataIndex;
};

const handleReset = (clearFilters: any) => {
  clearFilters({ confirm: true });
  state.searchText = '';
};
</script>


<style scoped>
.highlight {
  background-color: rgb(255, 192, 105);
  padding: 0px;
}
</style>
