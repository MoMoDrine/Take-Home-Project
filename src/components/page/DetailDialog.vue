<template>
    <el-dialog
        :visible.sync="visible"
        width="50%"
        :show-close="false"
        :modal="false"
        @close="cancel"
    >
        <span slot="title">
            <span :style="'background-color:'+detail.color">&nbsp;</span>
            <i style="font-weight:bold;"> {{detail.title}}</i>
        </span>
        <span style="font-size:8px;font-weight:500">{{detail.date}}</span>
        <p style="font-weight:500">{{detail.content}}</p>
    </el-dialog>
</template>

<script>
export default {
    props:{
        visible:{
            default:false,
            type:Boolean,
        },
        detail:{
            default:() => {},
            type:Object
        }
    },
    methods:{
        cancel(){
            this.$emit("changeVisible")
        }
    }
}
</script>

<style scoped>
/deep/.el-dialog__body{
    padding-top: 0;
}
</style>