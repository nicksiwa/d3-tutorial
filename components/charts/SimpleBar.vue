<template>
  <div>
    <h5>{{ chartName }}</h5>
    <svg :id="chartId" width="1000" height="400"></svg>
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  props: ["chartId", "chartName", "data"],
  watch: {
    data() {
      this.createChart()
    },
  },
  methods: {
    createChart() {
      const margin = 100;
      const width = 1000 - margin * 2;
      const height = 400 - margin * 2;

      const group_scale = d3
        .scaleBand()
        .domain(this.data.map((d) => d[0]))
        .range([margin, margin + width])
        .paddingInner(0.1)
        .paddingOuter(0);

      const x_scale = d3
        .scaleBand()
        .domain(d3.range(this.data.length))
        .range([margin, margin + width])
        .paddingInner(0.1)
        .paddingOuter(0);

      const y_scale = d3
        .scaleLinear()
        .domain([0, d3.max(this.data, (d) => d[1])])
        .range([height + margin, margin]);

      const svg = d3
        .select(`#${this.chartId}`)
        .attr("transform", "translate(50,0)");

      svg
        .selectAll("rect")
        .data(this.data)
        .join((enter) => enter.append("rect"))
        .attr("x", (d, i) => x_scale(i))
        .attr("y", (d) => y_scale(d[1]))
        .attr("width", x_scale.bandwidth())
        .attr("height", (d) => height + margin - y_scale(d[1]));

      svg
        .append("g")
        .attr("transform", `translate(${margin},0)`)
        .call(d3.axisLeft(y_scale).ticks(5));

      svg
        .append("g")
        .attr("transform", `translate(0,${margin + height})`)
        .call(d3.axisBottom(group_scale));
    },
  },
};
</script>

<style>
svg rect {
  fill: steelblue;
}
</style>
