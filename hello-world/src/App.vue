<template>
  <div>
    <div @click="exportExcel">点我下载</div>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import XLSX from 'xlsx'
import data from './tpldata';
export default {
  name: 'App',
  components: {
    HelloWorld
  },
  created () {
    // let tplData = '';
    console.log('datadatadata', data);
    let arr = [
      ['单元格A1内容', '单元格A2内容', '单元格A3内容'],
      ['单元格B1内容', '单元格B2内容', '单元格B3内容'],
      ['单元格B1内容', '单元格B2内容', '单元格B3内容'],
    ]
    var wb = XLSX.utils.book_new();
    var ws = XLSX.utils.aoa_to_sheet(arr);
    XLSX.utils.book_append_sheet(wb, ws, "file");
    console.log('wbbb', wb);
    // XLSX.writeFile(wb, '自定义名称1.xlsx');
    // this.transData();
  },
  methods: {
    transData () {
      let tempArr = [];
      let arr = [];
      tempArr = data.children.map(item => {
        return item.containerProps.y;
      }).sort((a, b) => {return b - a});
      console.log('tempArrtempArrtempArr', tempArr);
      arr = new Array(Math.ceil(tempArr[0] / 30)).fill('1');
      arr = arr.map(() => {
        return [];
      })
      console.log('sss', arr);
      data.children.forEach(item => {
        let row = Math.ceil(item.containerProps.y / 30) - 1;
        row = row >= 0? row : 0;
        arr[row].push(item.props.label);
      })
      return arr;
    },
    exportExcel () {
      let data = this.transData();
      console.log(data);
      var wb = XLSX.utils.book_new();
      var ws = XLSX.utils.aoa_to_sheet(data);
      XLSX.utils.book_append_sheet(wb, ws, "file");
      XLSX.writeFile(wb, '自定义名称2.xlsx');
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
