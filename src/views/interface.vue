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
        return {
            loadData: [],
            barchart_data: []
        };
  },
  mounted() {
        this.fetchData();
  },
  methods: {
        fetchData() {
            console.log("Inside fetchData() !!!!");
            d3.json("../barchart.json", function(error, data) {
                console.log("\nInside d3.json()!!!!\n");
                if (error){
                    throw error;  
                }                        
                this.barchart_data = data;
                // debug
                console.log("this.barchart_data: ", this.barchart_data);
            });
        }
    }
}
</script>
