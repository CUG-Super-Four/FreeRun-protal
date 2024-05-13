<!-- 首页 -->
<template>
  <div class="mainWrapper">
    <div class="container banner fx" style="overflow: hidden;">
      <div class="categorys bg-wt">
        <!-- 头部分类 -->
        <ClassCategory :data="classCategorys"></ClassCategory>
      </div>
      <!-- 头部幻灯片 -->
      <Swiper :data="imags"></Swiper>
    </div>

    <!-- 新课推荐 -->
    <div class="pd-tp-30">
      <OpenClass
        title="新课推荐"
        class="container"
        :data="freeClassData"
      ></OpenClass>
    </div>

    <!-- 精品好课 -- start -->
    <div class="pd-tp-30">
      <OpenClass
        title="精品好课"
        class="container"
        :data="goodClassData"
      ></OpenClass>
    </div>
    <!-- 精品好课 -- end -->
    <!-- 精品新课 -- start -->
    <div class="pd-tp-30">
      <OpenClass
          title="精品新课"
          class="container"
          :data="newClassData"
      ></OpenClass>
    </div>
    <!-- 精品新课 -- end -->
  </div>
</template>
<script setup>
/** 数据导入 **/

import { onMounted, ref } from "vue";
import { dataCacheStore } from "@/store";
import { ElMessage } from "element-plus";
import { getClassCategorys, getRecommendClassList } from "@/api/class.js";
import ClassCategory from "@/components/ClassCategory.vue";
import OpenClass from "./components/OpenClass.vue";
import Swiper from "./components/Swiper.vue";
import banner1 from "@/assets/banner1.jpg";
import banner2 from "@/assets/banner2.jpg";
import banner3 from "@/assets/banner3.jpg";
const dataCache = dataCacheStore();
// 分类数据
const classCategorys = ref([]);
// banner幻灯片图片
const imags = [banner1, banner2, banner3];
// 精品公开课的数据
const freeClassData = ref([]);
// 兴趣弹窗
const interestDialog = ref(false);
// 精品好课数据
const goodClassData = ref([]);
// 精品新课数据
const newClassData = ref([]);
// mounted生命周期
onMounted(async () => {
  // 获取三级分类信息
  getClassCategoryData();
  // 获取精品公开课
  getFreeClassListData();
  getGoodClassListData();
  getNewClassListData();

});

/** 方法定义 **/

// 获取一、二、三级分类信息
const getClassCategoryData = async () => {
  await getClassCategorys()
    .then((res) => {
      if (res.code === 200) {
        classCategorys.value = res.data;
        dataCache.setCourseClassDataes(res.data)
      } else {
        ElMessage({
          message: res.data.msg,
          type: "error",
        });
      }
    })
    .catch(() => {
      ElMessage({
        message: "分类请求出错！",
        type: "error",
      });
    });
};

// 精品公开课接口 - 公开课取消
const getFreeClassListData = async () => {
  await getRecommendClassList("free")
    .then((res) => {
      if (res.code === 200) {
        freeClassData.value = res.data;
      } else {
        ElMessage({
          message: res.data.msg,
          type: "error",
        });
      }
    })
    .catch(() => {
      ElMessage({
        message: "分类请求出错！",
        type: "error",
      });
    });
};
// 精品公开课接口
const getGoodClassListData = async () => {
  await getRecommendClassList('best')
    .then((res) => {
      if (res.code === 200) {
        goodClassData.value = res.data;
      } else {
        ElMessage(res.meg);
      }
    })
    .catch(() => {
      ElMessage("分类请求出错！");
    });
};
// 新课推荐
const getNewClassListData = async () => {
  await getRecommendClassList('new')
    .then((res) => {
      if (res.code === 200) {
        newClassData.value = res.data;
      } else {
        ElMessage(res.meg);
      }
    })
    .catch(() => {
      ElMessage({
        message: "分类请求出错！",
        type: "error",
      });
    });
};



</script>
<style lang="scss" src="./index.scss">
</style>
