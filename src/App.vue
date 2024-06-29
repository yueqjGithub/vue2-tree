<template>
  <div id="app">
    <lcc-tree
    :info="info"
    :isRoot="true"
    ></lcc-tree>
  </div>
</template>
<script>
import lccTree from './components/tree/index.vue';
import { mockData } from './datas/mock';
export default {
  components: {
    lccTree
  },
  data () {
    return {
      info: mockData.data.contract_info
    }
  }
}
</script>
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
