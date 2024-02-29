<template>
  <div class="detail">
    <div class="card">
      <ECharts :option="option" />
    </div>
    <div class="card">
      <el-descriptions class="margin-top" title="分析结果" :column="1" border>
        <el-descriptions-item>
          <template #label>
            <div class="cell-item">峰宽占比</div>
          </template>
          70%
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>
            <div class="cell-item">峰重叠率</div>
          </template>
          70%
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>
            <div class="cell-item">峰数量</div>
          </template>
          10
        </el-descriptions-item>
      </el-descriptions>
      <el-image :src="img" />
    </div>
  </div>
</template>

<script setup lang="ts" name="useProTableDetail">
import { useRoute } from "vue-router";
import { ECOption } from "@/components/ECharts/config";
import ECharts from "@/components/ECharts/index.vue";
import img from "@/views/dataquality/list/pic1.png";

const route = useRoute();
const option: ECOption = {
  title: {
    text: "数据质量分析",
    textStyle: {
      color: "#a1a1a1"
    }
  },
  legend: {
    data: ["数据集"],
    textStyle: {
      color: "#a1a1a1"
    }
  },
  radar: {
    // shape: 'circle',
    indicator: [
      { name: "完整性", max: 1 },
      { name: "可靠性", max: 1 },
      { name: "准确性", max: 1 },
      { name: "一致性", max: 1 }
    ]
  },
  series: [
    {
      name: "数据质量分析",
      type: "radar",
      data: [
        {
          value: [0.7, 0.6, 0.4, 0.8],
          name: "Allocated Budget"
        }
      ]
    }
  ]
};
</script>

<style scoped lang="scss">
@import "./index.scss";
</style>
