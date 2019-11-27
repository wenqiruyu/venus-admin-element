<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="iconfont icon-wen-lihuoshangjia"></i> 订单管理</el-breadcrumb-item>
                <el-breadcrumb-item>订单列表</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10"></el-input>
                <el-button type="primary" icon="search" @click="search">搜索</el-button>
            </div>
            <el-table :data="tableData" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55" align="center"></el-table-column>
                <el-table-column prop="orderNumber" label="订单号" width="180">
                </el-table-column>
                <el-table-column prop="user" label="下单用户名" width="120">
                    怎么这样子
                </el-table-column>
                <el-table-column prop="product[0].author" label="作者" width="120">
                </el-table-column>
                <el-table-column prop="product[0].name" label="书名">
                </el-table-column>
                <el-table-column prop="orderMoney" label="订单金额" sortable width="80" sortable>
                </el-table-column>
                <el-table-column prop="" label="订单状态" sortable width="80" sortable>
                    未发货
                </el-table-column>
                <el-table-column label="操作" width="160" align="center">
                    <template slot-scope="scope">
                        <el-button type="text" icon="iconfont icon-wen-update1" @click="handleQuery(scope.$index, scope.row)">发货</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <div class="pagination">
                <el-pagination background @current-change="handleCurrentChange" layout="prev, pager, next" :total="1000">
                </el-pagination>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                select_word: '',
                tableData: [],
                multipleSelection: [],
            }
        },
        created() {
            var self = this
            this.$axios.get("/order-server//order/group/1").then((res) => {
                if(res.data.status == 1){
                    // 数组复制
                    self.tableData = res.data.data.slice()
                    console.log(res.data.data)
                    this.$message({
                        message: '恭喜你，这是一条成功消息',
                        type: 'success'
                    })
                }else{
                    this.$message.error(res.data.msg)
                }
            })
        },
        methods:{
            handleQuery(){

            },
            // 分页导航
            handleCurrentChange(val){
                this.cur_page = val;
                this.getData();
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            },
            search(){

            }
        }
    }
</script>

<style>
    .handle-box {
        margin-bottom: 20px;
    }
    .handle-input {
        width: 300px;
        display: inline-block;
    }
    .table{
        width: 100%;
        font-size: 14px;
    }
    .red{
        color: #ff0000;
    }
    .mr10{
        margin-right: 10px;
    }
</style>
