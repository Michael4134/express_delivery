<template>
    <div class="allWoker-page">
        <person-title>当前位置 :: 所有用户 >> <em>工作人员</em></person-title>
        <div class="allWoker-page-cont">
            <personList
            :titleList='titleList'
            :userList='wokerList'
            @DeletePerson='handleDeletePerson'></personList>
        </div>
    </div>
</template>

<script>
import personList from './personList'
import axios from 'axios'
export default {
    components:{
        personList
    },
    data(){
        return{
            titleList:[
                {id:1,columnTitle:'注册时间',prop:'regDate',sortable:true},
                {id:2,columnTitle:'名字',prop:'userName',sortable:false},
                {id:3,columnTitle:'账号',prop:'userId',sortable:false}
            ],
            wokerList:[]
        }
    },
    created() {
        this.getAllWoker();
    },
    methods:{
         getAllWoker(){
            let loading = this.$loading({lock:true,text:'玩命加载中...'});
            axios.get(`/admin/allWoker`).then(response=>{
                let res = response.data;
                loading.close();
                if(res.status=='0'){
                    this.wokerList = res.result;
                }else{
                    console.log(res.msg);    
                }
            }).catch(err=>{
                console.log(err);
                
            })
        },
        handleDeletePerson(data){
            // 删除订单
            this.$confirm('此操作将永久删除该用户, 是否继续?', 
                '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'error'
            }).then(() => {
                // axios.get(``).then(response=>{
                //     let res = response.data;
                //     if(res.status=='0'){
                        this.$message({
                            type: 'success',
                            message: '删除成功!'
                        });
                //         this.getHistoryOrder();
                //     }else{
                //         this.$message({
                //             type: 'error',
                //             message:res.msg
                //         });  
                //     }
                // }).catch(err=>{
                //     console.log(err);     
                // });  
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消删除'
                 });          
            });  
        }
    }
}
</script>

<style>

</style>

