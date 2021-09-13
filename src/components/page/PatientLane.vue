<template>
    <div>
        <el-card shadow="always" style="width:94%;margin:0 auto;border-radius: 20px;height:calc(99vh);overflow-y:scroll" >
            <div slot="header" class="clearfix">
                <span style="font-weight:bold"><i>{{title}}</i></span>
            </div>
            <div v-for="(item,index) in patientData" :key="index">
                <div style="margin-top:5px">
                    <span style="font-family: Helvetica Neue;font-weight:bold;font-size:18px">{{item.patient.account.firstName}} {{item.patient.account.lastName}}</span>
                    <div class="basic_type" :style="chooseTypeStyle(item.type)" @click="beforeChangeVisible(item)"><span style="text-align:center;display:block;color:white">{{typeDict[item.type]}}</span></div>
                </div>
                <div style="margin-top:3px">
                    <span style="color:#909399;font-size:8px;font-weight:bold">SIGNED BY</span>
                </div>
                <div>
                    <el-avatar size="medium" style="margin-right:10px;vertical-align: middle"> user </el-avatar>
                    <span style="display:inline-block;vertical-align: middle;font-weight:bold;font-size:10px">{{item.signee.account.firstName}} {{item.signee.account.lastName}}</span>
                </div>
                <div style="margin-top:3px">
                    <span style="color:#909399;font-size:8px;font-weight:bold">SERVICE DATE</span>
                </div>
                <div style="margin-top:3px;border-bottom:1px #dcdfe6 solid">
                    <span style="font-size:8px;font-weight:500;margin-bottom:5px;display:block">{{formatDate(item.serviceStart,item.serviceEnd)}}</span>
                </div>
                
            </div>
        </el-card>
        <DetailDialog :visible="dialogVisible" :detail="detail" @changeVisible="changeVisible"/>
    </div>
</template>

<script>
import moment from "moment";
import DetailDialog from './DetailDialog'
export default {
    components:{DetailDialog},
    props:{
        title:{
            default:"",
            type:String,
        },
        patientData:{
            default:()=>[],
            type:Array,
        }
    },
    data(){
        return{
            dialogVisible: false,
            colorType:{
                TYPE_A:'green',
                TYPE_B:'#4061dd',
                TYPE_C:'#f3af66',
            },
            typeDict:{
                TYPE_A:'Type A',
                TYPE_B:'Type B',
                TYPE_C:'Type C',
            },
            detail:{
                title:'',
                date:'',
                content:'',
                color:''
            }
        }
    },
    methods:{
        chooseTypeStyle(type){
            return `background-color:${this.colorType[type]}`
        },
        formatDate(beginDate,endDate){
            return moment(beginDate).format("MMMM Do,YYYY") + " - " + moment(endDate).format("MMMM Do,YYYY")
        },
        beforeChangeVisible(item){
            this.detail.title = this.typeDict[item.type],
            this.detail.color = this.colorType[item.type],
            this.detail.date = this.formatDate(item.serviceStart,item.serviceEnd)
            this.detail.content = item.description
            this.changeVisible()
        },
        changeVisible(){
            this.dialogVisible = !this.dialogVisible
        }
    }
}
</script>

<style scoped>
/deep/.el-card__header{
    border: 0px;
}
.basic_type{
    float:right;
    width:90px;
    cursor:pointer;
}
</style>