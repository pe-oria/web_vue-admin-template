/* eslint-disable */
<template>
  <div>
    <div
      class="echart"
      ref="mychart"
      id="mychart"
      :style="{ float: 'left', width: '50%', height: '600px' }"
    ></div>
    <div
      ref="jobchart"
      id="jobchart"
      :style="{ float: 'left', width: '50%', height: '600px' }"
    ></div>
  </div>
</template>
  
  <script>
import * as echarts from "echarts";

export default {
  data() {
    return {
      genderCounts: {
        male: 0,
        female: 0,
      },
      jobCounts: {
        job: [0, 0, 0, 0, 0],
      },
    };
  },
  mounted() {
    const params = {
      pageSize: 100,
    };
    this.axios
      .get("http://localhost:8080/emps", { params })
      .then((response) => {
        const list = response.data.data.rows;
        list.forEach((element) => {
          if (element.gender === 1) this.genderCounts.male++;
          else if (element.gender === 2) this.genderCounts.female++;
          if (element.job >= 1 && element.job <= 5) {
            this.jobCounts.job[element.job - 1]++;
          }
        });
        this.initEcharts();
        this.drawJob();
      });
  },
  // ... 其他选项

  methods: {
    drawJob() {
      const option = {
        xAxis: {
          type: "category",
          data: ["职位1", "职位2", "职位3", "职位4", "职位5"],
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            name: "员工职位统计",
            data: [
              this.jobCounts.job[0],
              this.jobCounts.job[1],
              this.jobCounts.job[2],
              this.jobCounts.job[3],
              this.jobCounts.job[4],
            ],
            type: "bar",
          },
        ],
      };
      const myChart = echarts.init(this.$refs.jobchart); // 图标初始化
      myChart.setOption(option); // 渲染页面
      //随着屏幕大小调节图表
      window.addEventListener("resize", () => {
        myChart.resize();
      });
    },
    initEcharts() {
      const option = {
        tooltip: {
          trigger: "item",
        },
        legend: {
          top: "5%",
          left: "center",
        },
        series: [
          {
            name: "员工性别比例",
            type: "pie",
            radius: ["40%", "70%"],
            avoidLabelOverlap: false,
            itemStyle: {
              borderRadius: 10,
              borderColor: "#fff",
              borderWidth: 2,
            },
            label: {
              show: false,
              position: "center",
            },
            emphasis: {
              label: {
                show: true,
                fontSize: 40,
                fontWeight: "bold",
              },
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: this.genderCounts.male, name: "男性员工" },
              { value: this.genderCounts.female, name: "女性员工" },
            ],
          },
        ],
      };
      const myChart = echarts.init(this.$refs.mychart); // 图标初始化
      myChart.setOption(option); // 渲染页面
      //随着屏幕大小调节图表
      window.addEventListener("resize", () => {
        myChart.resize();
      });
    },
  },
};
</script>
  
  