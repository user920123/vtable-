<template>
  <div class="desc">
    <div>1.列宽计算模式设置为'autoWidth' </div>
    <div>2.列数多一点（大于30列）单元格为空,表头内容较少(比如只写一个数字，测试发现表头内容多一些时，渲染没有问题)</div>
    <div>3.将横向滚动条拖到最末端 再点击此处进行更新 表格渲染异常</div>
    <div> 4.横向滚动条如果处于最左侧时 更新则没问题）</div>
  </div>
  <div @click="handleClick" class="btn">点击这里更新columns</div>
  <div class="data_table" ref="tableRef" :id="id"></div>
</template>
<script setup>
import {
  ref, onMounted, defineProps, watch,
} from 'vue'
import * as VTable from '@visactor/vtable';

const id = 'data_table'

let container = null

let instance = null



/**
 * 更新列数
 */
const handleClick = () => {

  let arr = []
  for (let i = 1; i <= 31; i++) {
    arr.push({
      field: i + '',
      title: i + '',

    })
  }

  let rows = []
  for (let i = 0; i < 5; i++) {
    let obj = {}
    for (let x = 0; x < arr.length; x++) {
      obj[arr[x].field] = ''
    }
    rows.push(obj)
  }

  instance.updateColumns(arr)
  instance.setRecords(rows)
}


const setData = () => {
  let cols = []
  for (let i = 1; i <= 30; i++) {
    const obj = {
      field: i + '',
      title: i + '',
    }
    cols.push(obj)
  }
  let rows = []
  for (let i = 0; i < 5; i++) {
    let obj = {}
    for (let x = 0; x < cols.length; x++) {
      obj[cols[x].field] = ''

    }

    rows.push(obj)
  }

  return {
    records: rows,
    columns: cols

  }

}
/**
 * @description 初始化表格
 */
const init = () => {
  const { records, columns } = setData()
  const option = {
    container,
    records,
    columns,
    widthMode: 'autoWidth',
    columnResizeMode: 'header',
    defaultRowHeight: 40,
    defaultHeaderRowHeight: 40,
    // maxCharactersNumber: 20,
    defaultColWidth: 200,
    limitMinWidth: 200,

    select: {
      headerSelectMode: 'cell'
    },
    keyboardOptions: {
      moveEditCellOnArrowKeys: true,
      copySelected: true,
      pasteValueToCell: true
    },

  }
  instance = new VTable.ListTable(option);
}

onMounted(() => {
  container = document.getElementById(id)
  init()
})


</script>
<style>
.data_table {
  width: 800px;
  height: 400px;
  margin: 0 auto;
}

.btn {
  text-align: center;
  margin: 20px 0 50px;
}
.desc{
  width: fit-content;
  margin:20px auto 0;
}
</style>
