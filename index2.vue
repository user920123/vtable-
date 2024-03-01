<template>
  <div @click="handleClick" class="btn">点击这里更新columns（将横向滚动条拖到最末端 再点击此处进行更新 表格最后一列就会渲染不全 横向滚动条如果处于最左侧时 更新则没问题）</div>
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
  for (let i = 0; i < 30; i++) {
    arr.push({
      field: i + '',
      title: i
      // title: '第'+i+'列' 
    })
  }
  instance.updateColumns(arr)

  // instance.setScrollLeft(0)

}



const setData = () => {
  let cols = []
  for (let i = 0; i < 50; i++) {
    const obj={
      field:i+'',
      title:'第'+i+'列'

    }
    cols.push(obj)
  }
  let rows=[]
  for (let i = 0; i < 50; i++) {
    let obj={}
    for(let x=0;x< cols.length;x++){
      obj[cols[x].field]=x+','+i

    }

    rows.push(obj)
  }
  return{
    records:rows,
    columns:cols

  }

}
/**
 * @description 初始化表格
 */
const init = () => {
 const {records,columns}= setData()
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
.btn{
  text-align: center;
  margin: 100px 0 50px;
}
</style>

