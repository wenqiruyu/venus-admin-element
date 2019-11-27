<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="iconfont icon-wen-book"></i> 图书管理</el-breadcrumb-item>
                <el-breadcrumb-item>店长推荐</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="handle-box">
                <el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10"></el-input>
                <el-button type="primary" icon="search" @click="search">搜索</el-button>
            </div>
            <el-table :data="tableData" border class="table" ref="multipleTable" @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55" align="center"></el-table-column>
                <el-table-column prop="name" label="推荐书名" width="180">
                </el-table-column>
                <el-table-column prop="author" label="作者" width="100">
                </el-table-column>
                <el-table-column prop="press" label="出版社" width="120">
                </el-table-column>
                <el-table-column prop="price" label="推荐理由">
                </el-table-column>
                <el-table-column label="操作" width="160" align="center">
                    <template slot-scope="scope">
                        <el-button type="text" icon="iconfont icon-wen-update1" @click="handleQuery(scope.$index, scope.row)">查看详情</el-button>
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
                // 搜索
                select_word: '',
                tableData: [],
                recommendDetail:{},
                queryVisible: false,
            }
        },
        methods:{
            // 分页导航
            handleCurrentChange(val) {
                this.cur_page = val;
                this.getData();
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            },
            handleQuery(index, row){
                const item = this.tableData[index]
                console.log(item)
                this.recommendDetail = {
                    name: item.product.name,
                    press: item.product.press,
                    format: item.format,
                    paper: item.paper,
                    pack: item.pack,
                    isSuit: item.isSuit === 1?'是':'否',
                    bookNumber: item.bookNumber,
                    createTime: item.createTime,
                    content: item.content
                }
                this.queryVisible = true
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
