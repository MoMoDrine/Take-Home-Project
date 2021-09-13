<template>
    <div>
        <div style="height:calc(12vh);border-bottom: 1px #dcdfe6 solid">
            <el-input 
                v-model="filterCondition.searchData" 
                prefix-icon="el-icon-search" 
                placeholder="Search..." 
                size="small"
                clearable
                style="height:100px;line-height:100px;width:75%;text-align:center;display:block;margin:0 auto"
            >
            </el-input>
        </div>
        <div style="height:calc(15vh);display: flex;border-bottom: 1px #dcdfe6 solid">
            <div style="display: flex;justify-content: center;align-items: center;flex-direction: column;width: 100%;">
                <div style="width:75%;margin:0 auto">
                    <p style="color:#606266;font-size:14px;font-family: system-ui;margin-top:0">PERIOD</p>
                </div>
                <el-date-picker
                    v-model="filterCondition.date"
                    type="month"
                    placeholder="All"
                    size="small"
                    value-format="yyyy-MM-15"
                    style="width:75%;display:block;margin:0 auto"
                >
                </el-date-picker>
            </div>
        </div>
        <div style="min-height:calc(70vh);border-bottom: 1px #dcdfe6 solid">
            <div >
                <div style="width:75%;margin:0 auto">
                    <p style="color:#606266;font-size:14px;font-family: system-ui">TYPE OF APPOINTMENT</p>
                </div>
                <el-select 
                    v-model="filterCondition.type" 
                    clearable  
                    placeholder="All" 
                    size="small" 
                    class="select"
                    style="width:75%;display:block;margin:0 auto"
                >
                    <el-option
                        v-for="item in dataType"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value">
                    </el-option>
                </el-select>
               
            </div>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
    created: function () {
      this.returnData = _.debounce(this.returnData, 500)
    },
    watch:{
        filterCondition:{
            deep:true,
            handler(condition) {
                this.returnData(condition)
            },
        }
    },
    data(){
        return {
            filterCondition:{
                searchData:"",
                date:null,
                type:'',
            },
            dataType:[
                {
                    label:"TypeA",
                    value:"TYPE_A"
                },
                {
                    label:"TypeB",
                    value:"TYPE_B"
                },{
                    label:"TypeC",
                    value:"TYPE_C"
                }]
        }
    },
    methods:{
        returnData(condition){
            this.$emit("updateShowData",condition)
        }
    }
}
</script>

<style scoped>
    /deep/.el-icon-search{
        color:#606266
    }
    /deep/input::-webkit-input-placeholder {
        color: #606266;
    }
    /deep/input::-moz-input-placeholder {
        color: #606266;
    }
    /deep/input::-ms-input-placeholder {
        color: #606266;
    }
    /deep/.el-icon-date{
        color: #606266;
    }
    .select /deep/.el-input__inner {
        background: url('../../static/icon/discount.svg') no-repeat; 
        background-size:16px 14px;  
        background-position: 4px 9px; 
        background-color: white;
        padding: 0 0 0 26px; 
        box-sizing: border-box;
        font-size: 14px;
    }
       
</style>