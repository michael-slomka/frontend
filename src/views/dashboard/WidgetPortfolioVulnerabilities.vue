<script>
import { Line } from 'vue-chartjs';
import { CustomTooltips } from '@coreui/coreui-plugin-chartjs-custom-tooltips';
import { getStyle, hexToRgba } from '@coreui/coreui/dist/js/coreui-utilities';
import common from '../../shared/common';

export default {
  extends: Line,
  props: {
    height: Number,
    width: Number,
  },
  methods: {
    render: function (metrics) {
      const widgetColor = getStyle('--widget-1');
      let chartLabels = [];
      let chartData = [];
      for (let i = 0; i < metrics.length; i++) {
        chartLabels.push(metrics[i].firstOccurrence);
        chartData.push(metrics[i].vulnerabilities);
      }

      const datasets = [
        {
          label: this.$t('message.vulnerabilities'),
          backgroundColor: hexToRgba(widgetColor, 10),
          borderColor: widgetColor,
          pointHoverBackgroundColor: '#fff',
          data: chartData,
        },
      ];

      this.renderChart(
        {
          labels: chartLabels,
          datasets: datasets,
        },
        {
          tooltips: {
            enabled: false,
            custom: CustomTooltips,
            callbacks: {
              title: function (tooltipItems, data) {
                return common.formatTimestamp(
                  data.labels[tooltipItems[0].index],
                  true,
                );
              },
            },
          },
          maintainAspectRatio: false,
          legend: {
            display: false,
          },
          scales: {
            xAxes: [
              {
                gridLines: {
                  color: 'transparent',
                  zeroLineColor: 'transparent',
                },
                ticks: {
                  fontSize: 2,
                  fontColor: 'transparent',
                },
              },
            ],
            yAxes: [
              {
                display: false,
                ticks: {
                  display: false,
                  min: 0,
                  max: Math.max.apply(Math, datasets[0].data) + 5,
                },
              },
            ],
          },
          elements: {
            line: {
              tension: 0.00001,
              borderWidth: 2,
            },
            point: {
              radius: 0,
              hitRadius: 10,
              hoverRadius: 4,
            },
          },
        },
      );
    },
  },
};
</script>
