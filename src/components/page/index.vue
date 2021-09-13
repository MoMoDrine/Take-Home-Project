<template>
  <div style="display:flex;background-color:#f8f9f9;">
      <Side class="box_width" style="border-right: 1px #dcdfe6 solid;width:19%" @updateShowData="updateShowData" />
    
      <!-- <div class="box_width">{{baseData.data.allNotes.edges.length}}</div> -->
      <PatientLane class="box_width" title="Task" :patientData="filterData(groupedData.taskData)"/>
      <PatientLane class="box_width" title="Review" :patientData="filterData(groupedData.ReviewData)"/>
      <PatientLane class="box_width" title="Done" :patientData="filterData(groupedData.doneData)"/>
      

  </div>
</template>

<script>
import Side from "./Side"
import PatientLane from './PatientLane'
import _ from 'lodash'
import moment from "moment";
export default {
    components:{Side,PatientLane},
    mounted(){
        this.baseData = require("../../static/json/appointments.json")
        this.groupData();
    },
    data(){
        return {
            baseData:{},
            groupedData:{
                taskData:[],
                ReviewData:[],
                doneData:[]
            },
            condition:{
                searchData:'',
                date:null,
                type:'',
            },
        }
    },
    methods:{
        groupData() {
            this.baseData.data.allNotes.edges.forEach(item => {
                if (item.status==='PENDING') {
                    this.groupedData.taskData.push(item)
                }else if(item.status==='NEED_REVIEW') {
                    this.groupedData.ReviewData.push(item)
                }else {
                    this.groupedData.doneData.push(item)
                }
            });
        },
        updateShowData(condition){
            this.condition = _.cloneDeep(condition)
        },
        filterData(data){
            return data.filter(item=>{
                let name = item.patient.account.firstName+" "+item.patient.account.lastName
                if(name.toLowerCase().includes(this.condition.searchData.toLowerCase())){
                    return item
                }
            }).filter(item=>{
                if(this.condition.type===''||this.condition.type===item.type){
                    return item
                }
            }).filter(item=>{
                let startTime = moment(item.serviceStart).format("YYYY-MM-DD")
                let endTime = moment(item.serviceEnd).format("YYYY-MM-DD")
                if(this.condition.date===null||moment(this.condition.date).isBetween(startTime, endTime)){
                    return item
                }
            })
            
            
        },
       
    }
    
}
</script>

<style scoped>
.box_width{
    flex:1; 
    justify-content: space-between;
    flex-wrap: wrap;
}
</style>