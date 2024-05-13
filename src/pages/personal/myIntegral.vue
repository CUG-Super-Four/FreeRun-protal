
<template>
  <div class="myInterralWrapper">
    <div class="personalCards">
      <CardsTitle class="marg-bt-20" title="打卡日历" />
      <div class="title"></div>
      <!-- 打卡日历 -->
      <Calendar @pointsSign="pointsSignHandle"></Calendar>
      
    </div>
  </div>
</template>
<script setup>

/** 数据导入 **/
import { onMounted, ref } from "vue";
import { ElMessage } from "element-plus";
import { getSeasons, getTodayPoints, pointsSign } from "@/api/class.js";
import { useRoute } from "vue-router";
import { dataCacheStore } from "@/store"

// 组件导入
import CardsTitle from './components/CardsTitle.vue'
import Calendar from './components/Calendar.vue'

// mounted生命周期
onMounted(async () => {

});

/** 方法定义 **/
// 打卡 - 返回积分
const pointsSignHandle = async () => {
  await pointsSign()
    .then((res) => {
      if (res.code == 200 ){
        ElMessage({
          message: '签到成功！',
          type: 'success'
        });
      } else {
        ElMessage({
          message: res.msg,
          type: 'error'
        });
      }
    })
    .catch(() => {
      ElMessage({
        message: "学霸榜请求失败！",
        type: 'error'
      });
    });
}

</script>
<style lang="scss" src="./index.scss"> </style>
