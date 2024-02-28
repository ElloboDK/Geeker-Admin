<template>
  <div class="card content-box">
    <ECharts :option="option" />
  </div>
</template>

<script setup lang="ts" name="useProTableDetail">
import { useRoute } from "vue-router";
import { ECOption } from "@/components/ECharts/config";
import ECharts from "@/components/ECharts/index.vue";

const route = useRoute();
const option: ECOption = {
  title: {
    text: "Basic Radar Chart",
    textStyle: {
      color: "#a1a1a1"
    }
  },
  legend: {
    data: ["Allocated Budget", "Actual Spending"],
    textStyle: {
      color: "#a1a1a1"
    }
  },
  radar: {
    // shape: 'circle',
    indicator: [
      { name: "完整性", max: 6500 },
      { name: "可靠性", max: 16000 },
      { name: "准确性", max: 30000 },
      { name: "一致性", max: 38000 }
    ]
  },
  series: [
    {
      name: "Budget vs spending",
      type: "radar",
      data: [
        {
          value: [4200, 3000, 20000, 35000],
          name: "Allocated Budget"
        },
        {
          value: [5000, 14000, 28000, 26000],
          name: "Actual Spending"
        }
      ]
    }
  ]
};
</script>
