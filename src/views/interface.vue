<template>
    <div>
        <h2>In interface.vue</h2>
        <BarChart :data=barchart_data></BarChart>
        <div class="row">
            <div class="col-md-6">
            </div>
            <div class="col-md-6">
            </div>      
        </div>
    </div>
</template>

<script>
// @ is an alias to /src
import BarChart from '../components/barChart.vue'
import * as d3 from "d3"
export default {
  name: 'interface',
  props: {
      data: Object
  },
  components: {
    BarChart
  },
  data(){
        // this.test_data = "bbbbbbbb"
        return {
            loadData: [],
            barchart_data: [],
            test_: "lalalalalalal"
        };
  },
  mounted() {
        this.fetchData();
        // this.test();
  },
  methods: {
      test: function () {
        // console.log("===>barchart_data: ", this.barchart_data);
        // console.log("===>this.test_: ", this.test_);
        // console.log("===>this.test_data: ", this.test_data);
      },
        fetchData() {
            console.log("Inside fetchData() !!!!");
            console.log("===>barchart_data: ", this.barchart_data);
            console.log("===>this.test_: ", this.test_);

            // d3.json('../barchart.json').then(data => {
            d3.json('barchart.json').then(data => {
                console.log("\nInside d3.json()!!!!\n");
                console.log("this: ", this);
                console.log("\nthis.barchart_data: ", this.barchart_data);
                console.log("\ndata: ", data);                   
                this.barchart_data = data;
                // debug
                console.log("\n-->this.barchart_data: ", this.barchart_data);
            });
        }
  }
}
</script>
