<script lang="ts">
export default {
  name: "InstallGraph",
};
</script>

<template>
  <div class="chart-container">
    <div class="chart-header">
      <h2 class="chart-title">Install Growth Over Time</h2>
      <div class="accent-bar"></div>
    </div>
    <div class="chart-wrapper">
      <canvas ref="chartCanvas"></canvas>
    </div>
    <div class="chart-footer">
      <span class="gradient-text">Exponential growth: </span>
      336K → 44M installs (130x increase)
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import { Chart, registerables } from "chart.js";

Chart.register(...registerables);

const chartCanvas = ref(null);
let chartInstance = null;

const data = [
  { year: "2017", installs: 335781 },
  { year: "2019", installs: 2575950 },
  { year: "2021", installs: 9792349 },
  { year: "2023", installs: 24382077 },
  { year: "2025", installs: 44063971 },
];

const formatInstalls = (value) => {
  if (value >= 1000000) {
    return (value / 1000000).toFixed(1) + "M";
  } else if (value >= 1000) {
    return (value / 1000).toFixed(0) + "K";
  }
  return value.toString();
};

const createChart = () => {
  const ctx = chartCanvas.value.getContext("2d");

  // Create gradient for the line (GitKraken: purple → blue → aqua)
  const gradient = ctx.createLinearGradient(0, 0, 0, 400);
  gradient.addColorStop(0, "#9300F5");
  gradient.addColorStop(0.5, "#196FFF");
  gradient.addColorStop(1, "#01DFC5");

  // Create gradient for the area under the line
  const areaGradient = ctx.createLinearGradient(0, 0, 0, 400);
  areaGradient.addColorStop(0, "rgba(147, 0, 245, 0.3)");
  areaGradient.addColorStop(0.5, "rgba(25, 111, 255, 0.2)");
  areaGradient.addColorStop(1, "rgba(1, 223, 197, 0.1)");

  chartInstance = new Chart(ctx, {
    type: "line",
    data: {
      labels: data.map((d) => d.year),
      datasets: [
        {
          label: "Installs",
          data: data.map((d) => d.installs),
          borderColor: gradient,
          backgroundColor: areaGradient,
          borderWidth: 4,
          pointBackgroundColor: "#B657FF",
          pointBorderColor: "#1C1C1C",
          pointBorderWidth: 3,
          pointRadius: 8,
          pointHoverRadius: 12,
          pointHoverBorderWidth: 3,
          pointHoverBorderColor: "#B657FF",
          pointHoverBackgroundColor: "#1C1C1C",
          fill: true,
          tension: 0.4,
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          display: false,
        },
        tooltip: {
          backgroundColor: "#272727",
          titleColor: "#B657FF",
          bodyColor: "#FFFFFF",
          borderColor: "#3E3E3E",
          borderWidth: 1,
          cornerRadius: 12,
          displayColors: false,
          callbacks: {
            label: function (context) {
              return `Installs: ${formatInstalls(context.raw)}`;
            },
          },
        },
      },
      scales: {
        x: {
          grid: {
            color: "rgba(62, 62, 62, 0.5)",
            drawBorder: true,
            borderColor: "#3E3E3E",
          },
          ticks: {
            color: "#C9C9C9",
            font: {
              size: 13,
            },
          },
        },
        y: {
          grid: {
            color: "rgba(62, 62, 62, 0.5)",
            drawBorder: true,
            borderColor: "#3E3E3E",
          },
          ticks: {
            color: "#C9C9C9",
            font: {
              size: 13,
            },
            callback: function (value) {
              return formatInstalls(value);
            },
          },
        },
      },
      interaction: {
        intersect: false,
        mode: "index",
      },
    },
  });
};

onMounted(() => {
  createChart();
});

onUnmounted(() => {
  if (chartInstance) {
    chartInstance.destroy();
  }
});
</script>

<style scoped>
.chart-container {
  width: 100%;
  height: 400px;
  padding: 2rem;
  background: linear-gradient(135deg, #1C1C1C 0%, #272727 50%, #1C1C1C 100%);
  border-radius: 10px;
  border: 1px solid #3E3E3E;
}

.chart-header {
  margin-bottom: 2rem;
  text-align: center;
}

.chart-title {
  font-size: 1.875rem;
  font-weight: 700;
  font-family: 'Inter', sans-serif;
  background: linear-gradient(90deg, #9300F5 0%, #B657FF 50%, #196FFF 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  margin: 0 0 0.5rem 0;
}

.accent-bar {
  height: 4px;
  width: 6rem;
  background: linear-gradient(90deg, #7900C9, #196FFF);
  margin: 0 auto;
  border-radius: 2px;
}

.chart-wrapper {
  height: 280px;
  position: relative;
}

.chart-footer {
  margin-top: 1.5rem;
  text-align: center;
  font-size: 0.875rem;
  font-weight: 500;
  color: #C9C9C9;
}

.gradient-text {
  background: linear-gradient(90deg, #01FEE0, #EC7FFF);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>
