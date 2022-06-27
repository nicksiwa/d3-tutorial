<template>
  <div>
    <ChartsSimpleBar chart-id="req_amt_by_grp_name" chart-name="req_amt จำแนกตาม exp_grp_name" :data="reqAmtByGrpName" />
    <ChartsSimpleBar chart-id="budget_review_amt_by_grp_name" chart-name="budget_review_amt จำแนกตาม exp_grp_name" :data="budgetReviewAmtByGrpName" />
    <ChartsSimpleBar chart-id="req_amt_by_cate" chart-name="req_amt จำแนกตามด้าน" :data="reqAmtByCate" />
    <ChartsSimpleBar chart-id="budget_review_amt_by_cate" chart-name="budget_review_amt จำแนกตามด้าน" :data="budgetReviewAmtByCate" />
  </div>
</template>

<script>
import * as d3 from "d3";

export default {
  name: "IndexPage",
  data() {
    return {
      reqAmtByGrpName: [],
      budgetReviewAmtByGrpName: [],
      reqAmtByCate: [],
      budgetReviewAmtByCate: []
    };
  },
  mounted() {
    this.loadData();
  },
  methods: {
    async loadData() {
      const result = await d3.csv("/data.csv");
      this.reqAmtByGrpName = d3.rollups(
        result,
        (v) => d3.sum(v, (d) => +d["req_amt"]),
        (d) => d["exp_grp_name"]
      );
      this.budgetReviewAmtByGrpName = d3.rollups(
        result,
        (v) => d3.sum(v, (d) => +d["budget_review_amt"]),
        (d) => d["exp_grp_name"]
      );
      this.reqAmtByCate = d3.rollups(
        result,
        (v) => d3.sum(v, (d) => +d["req_amt"]),
        (d) => d["ด้าน"]
      );
      this.budgetReviewAmtByCate = d3.rollups(
        result,
        (v) => d3.sum(v, (d) => +d["budget_review_amt"]),
        (d) => d["ด้าน"]
      )
    },
  },
};
</script>

<style>

</style>
