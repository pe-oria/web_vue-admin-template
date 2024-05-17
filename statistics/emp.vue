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
        xueli: [0, 0, 0, 0],
      },
    };
  },
  mounted() {
    const params = {
      pageSize: 100,
    };
    this.axios
      .get("http://localhost:8080/student", { params })
      .then((response) => {
        const list = response.data.data.rows;
        list.forEach((element) => {
          if (element.gender === 1) this.genderCounts.male++;
          else if (element.gender === 2) this.genderCounts.female++;
          if (element.xueli >= 1 && element.xueli <= 4) {
            this.jobCounts.xueli[element.xueli - 1]++;
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
          data: ["学历1", "学历2", "学历3", "学历4"],
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            name: "学生学历统计",
            data: [
              this.jobCounts.xueli[0],
              this.jobCounts.xueli[1],
              this.jobCounts.xueli[2],
              this.jobCounts.xueli[3],
              this.jobCounts.xueli[4],
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
              { value: this.genderCounts.male, name: "男性学生" },
              { value: this.genderCounts.female, name: "女性学生" },
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
  
  