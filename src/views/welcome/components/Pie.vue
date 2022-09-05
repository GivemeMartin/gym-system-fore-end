<script setup lang="ts">
import { ref, computed, type Ref } from "vue";
import { useDark, useECharts, type EchartOptions } from "@pureadmin/utils";

const { isDark } = useDark();

let theme: EchartOptions["theme"] = computed(() => {
  return isDark.value ? "dark" : "light";
});

const pieChartRef = ref<HTMLDivElement | null>(null);
const { setOptions } = useECharts(pieChartRef as Ref<HTMLDivElement>, {
  theme
});

setOptions(
  {
    tooltip: {
      trigger: "item"
    },
    legend: {
      orient: "vertical",
      // @ts-expect-error
      right: true
    },
    series: [
      {
        name: "消费信息",
        type: "pie",
        radius: "60%",
        center: ["40%", "50%"],
        data: [
          { value: 1079, name: "会员充值" },
          { value: 1079, name: "新用户充值" },
          { value: 204, name: "会员消费" },
          { value: 3, name: "新用户消费" }
        ],
        emphasis: {
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: "rgba(0, 0, 0, 0.5)"
          }
        }
      }
    ]
  },
  {
    name: "click",
    callback: params => {
      console.log("click", params);
    }
  },
  // 点击空白处
  {
    type: "zrender",
    name: "click",
    callback: params => {
      console.log("点击空白处", params);
    }
  }
);
</script>

<template>
  <div ref="pieChartRef" style="width: 100%; height: 35vh" />
</template>
