<script setup lang="ts">
import { FilterForm } from "@lipnstar/component-library";
import '@lipnstar/component-library/dist/index.css'
import type { FilterFormConfig } from '@lipnstar/component-library'
import TuiAmountInwords from './components/tui-amount-inwords/tui-amount-inwords.vue'
import SlidVerify from './components/slid-verify/index.vue'

const fields:FilterFormConfig[] = [
  {
    key: "title",
    label: "推送标题",
    component: "input",
    defaultValue: "",
  },
];

const handleSearch = (values: any) => {
  console.log(values);
}
</script>

<template>
  <div>
    <div style="margin: 20px"><SlidVerify /></div>
    <TuiAmountInwords value="100.1101" />
    <FilterForm :fields="fields" @search="handleSearch" />
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
