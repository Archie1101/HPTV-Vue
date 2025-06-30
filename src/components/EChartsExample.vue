<template>
  <div ref="chartContainer" style="width: 100%; height: 400px;"></div>
</template>

<script>
import { defineComponent, onMounted, ref } from 'vue';
import * as echarts from 'echarts';

export default defineComponent({
  name: 'EChartsExample',
  setup() {
    const chartContainer = ref(null);
    const myChart = ref(null);

    onMounted(() => {
      // 初始化图表
      myChart.value = echarts.init(chartContainer.value);

      // 设置初始的空配置项
      const initialOption = {
        title: {
          text: '简单柱状图示例',
          left: 'center',
        },
        tooltip: {},
        xAxis: {
          data: [],
        },
        yAxis: {},
        series: [
          {
            name: '销量',
            type: 'bar',
            data: [],
          },
        ],
      };

      // 设置图表初始配置
      myChart.value.setOption(initialOption);

      // 获取数据并更新图表
      fetchDataAndUpdateChart();

      // 监听窗口大小变化，动态调整图表大小
      window.addEventListener('resize', () => {
        myChart.value.resize();
      });
    });

    const fetchDataAndUpdateChart = async () => {
      try {
        // 使用 fetch 替代 axios 来获取数据
        const response = await fetch('http://localhost:8080/api/test');
        
        // 检查响应状态是否为 200
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }

        // 解析 JSON 数据
        const data = await response.json();

        // 获取 API 返回的数据
        const { fruits, counts } = data;

        // 更新图表配置
        const option = {
          title: {
            text: '水果销量',
            left: 'center',
          },
          tooltip: {},
          xAxis: {
            data: fruits,  // 使用从 API 获取的水果数据
          },
          yAxis: {},
          series: [
            {
              name: '销量',
              type: 'bar',
              data: counts,  // 使用从 API 获取的销量数据
            },
          ],
        };

        // 更新图表
        myChart.value.setOption(option);
      } catch (error) {
        console.error('获取数据失败', error);
      }
    };

    return {
      chartContainer,
    };
  },
});
</script>

<style scoped>
/* 你可以为图表容器添加样式 */
</style>
